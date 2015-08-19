---
layout: blog
title: xen安装coreos
---

## 环境信息

- System: Debian GNU/Linux 8.1
- Kernel: 3.16.0-4-amd64
- Xen Version: 4.5.1
- CoreOS Version: stable(723.3.0) 

这里不再赘述xen4.5.1的安装,以及lvm的配置.

## 步骤

1.下载所需文件
    
- [coreos_production_iso_image.iso](http://stable.release.core-os.net/amd64-usr/current/coreos_production_iso_image.iso)

- [coreos_production_image.bin.bz2](http://stable.release.core-os.net/amd64-usr/current/coreos_production_image.bin.bz2)

- [coreos_production_image.bin.bz2.sig](http://stable.release.core-os.net/amd64-usr/current/coreos_production_image.bin.bz2.sig)
    

2.安装配置nginx

``` bash
    # 因为安装coreos过程中,安装脚本会去下载coreos_production_image.bin.bz2
    # 以及coreos_production_image.bin.bz2.sig文件,速度较慢,还可能需要翻墙
    # 所以此步骤是为了安装过程中直接从本地下载所需文件
    # 如果不嫌弃长时间下载,或者自己网络环境好,可以省略该步骤

    # 安装nginx
    apt-get install nginx
    # 新建主目录 /srv/coreos/723.3.0
    # 723.3.0 为当前的coreos stable 版本号,根据情况而定
    # 如果stable更新到725.1.2, 则相应创建/srv/coreos/725.1.2
    mkdir -p /srv/coreos/723.3.0
    # 将步骤1中下载的文件拷贝到主目录
    cp coreos* /srv/coreos/723.3.0
    # 配置nginx, 将主目录设置到以上步骤创建的目录
    location /coreos {
                root /srv/;
        }
    # 重启nginx,使配置生效
```

3.创建cloud-config.yaml, 并放入nginx的主目录中

注意: 将配置文件中的$private_ipv4 替换成你配置的静态ip

原因如下:
```bash
    Note: The $private_ipv4 and $public_ipv4 substitution variables referenced in other documents are only supported on Amazon EC2, Google Compute Engine, OpenStack, Rackspace, DigitalOcean, and Vagrant.
```

```bash
    # 这里这是简单配置,更多的配置信息参考配置文档
    # 注意网卡名字,根据自己的实际情况来, 我的是eth0
    # 以及ssh-rsa

    #cloud-config

    hostname: coreos

    coreos:
      etcd:
        addr: $private_ipv4:4001
        peer-addr: $private_ipv4:7001
      units:
        - name: etcd.service
          command: start
        - name: fleet.service
          command: start
        - name: static.network
          content: |
            [Match]
            Name=eth0

            [Network]
            Address=192.168.1.115/24
            Gateway=192.168.1.254
            DNS=223.5.5.5
            DNS=223.6.6.6
    users:
      - name: core
        ssh-authorized-keys:
          - ssh-rsa zaC1yc2EAAAADAQABAAABAQDOL2qJA26+qaM8RWf8qlM8NHDy.......

      - groups:
          - sudo
          - docker 
```

 
4.创建coreos使用的磁盘

``` bash
    # 视自己情况调整
    lvcreate -n CoreOS -L 100G vg0
```


5.创建coreos安装配置

``` bash
        # 该文件位置 /etc/xen/CoreOS.cfg
        # 安装xen时需要安装pygrub支持
        # 注意extra中的cloud-config-url,将localhost替换成nginx所在server的ip,
        # 或者整个替换成可以下载到的url 

        #
        # Configuration file for the Xen instance Search, created
        # by xen-tools 4.6 on Mon Aug 19 10:47:09 2015.
        #

        bootloader = "/usr/local/lib/xen/bin/pygrub"
        bootloader_args = "--kernel=/coreos/vmlinuz --ramdisk=/coreos/cpio.gz --output-directory=/mnt"
        extra = " cloud-config-url=http://localhost/coreos/cloud-config.yaml console=tty0"

        vcpus       = '1'
        memory      = '1000'


        
          Disk device(s).
        
        disk        = [
                          'file:/srv/coreos/723.3.0/coreos_production_iso_image.iso,xvdd:cdrom,r',
                          'phy:/dev/vg0/CoreOS,xvda,w',
                      ]

        #disk        = [
        #      'phy:/dev/vg0/CoreOS,xvda,rw'
        #                ]

        #
        #  Hostname
        #
        name        = 'CoreOS'

        #
        #  Networking
        #
        vif         = [ 'mac=00:16:3E:4A:BE:D2,bridge=xenbr0' ]

        #
        #  Behaviour
        #
        on_poweroff = 'destroy'
        on_reboot   = 'restart'
        on_crash    = 'restart'
```


6.启动虚拟机

``` bash
    xl create -c /etc/xen/CoreOS.cfg
```

我启动时,console日志卡在如下位置,不再打印虚拟机的启动日志

``` bash
    root@unix:~/VM/xen-4.5.1# xl create -c /etc/xen/CorsOs.cfg
    Parsing config from /etc/xen/CorsOs.cfg
    WARNING: Specifying "bootloader_args" as a string is deprecated. Please use a list of arguments.
    xenconsole: Could not open tty '/dev/pts/4': No such file or directory
```

然后我通过按下`Control+]`键跳出console界面时,日志开始打印,我也没在查原因,日志卡住,就通过 按下`Control+]`键跳出console界面解决
启动成功后,会停在如下界面,同时会显示eth0的ip

```
    This is coreos (Linux x86_64 4.0.5) 07:18:26
    SSH host key: 76:8c:be:83:51:9e:0d:b0:3d:9a:54:51:91:5d:6b:c8 (DSA)
    SSH host key: d2:14:fa:61:ab:c7:dc:1b:98:b1:ac:c4:b4:b8:44:c8 (ED25519)
    SSH host key: fc:6d:4b:90:85:a6:ef:8a:83:85:bf:fe:c6:ab:dd:d2 (RSA)
    eth0: 192.168.1.115 fe80::216:3eff:fe4a:bed2

    coreos login:
```

7.完成安装
    
因为虚拟机启动时已经从webserver上下载了cloud-config.yaml(步骤5)
现在可以直接通过无密码连接到服务器
登陆后如下所示:
``` bash
    ➜  ~  ssh core@192.168.67.115
    Last login: Wed Aug 19 05:33:42 2015 from 192.168.67.41
    CoreOS stable (723.3.0)
    core@coreos ~ $

```

切换到root用户,查看磁盘信息
``` bash
    core@coreos ~ $ sudo su - root
    coreos ~ #
    coreos ~ #
    coreos ~ # fdisk -l
    Disk /dev/xvda: 100 GiB, 107374182400 bytes, 209715200 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 4096 bytes
    I/O size (minimum/optimal): 4096 bytes / 4096 bytes
    Disklabel type: dos
    Disk identifier: 0x00000000

    Device     Boot Start    End Sectors  Size Id Type
    /dev/xvda1 *     4096 266239  262144  128M  c W95 FAT32 (LBA)
    /dev/xvda2          1   4095    4095    2M ee GPT

    Partition 2 does not start on physical sector boundary.
    Partition table entries are not in disk order.
    coreos ~ #
```

下载cloud-config.yaml文件,修改coreos-install脚本,进行硬盘安装
``` bash
    # 修改coreos-install 脚本 位置 /usr/bin/coreos-install
    # 做好备份

    # 找到以下位置
    if [[ -z "${BASE_URL}" ]]; then
        BASE_URL="http://${CHANNEL_ID}.release.core-os.net/amd64-usr"
    fi
    # 将 http://${CHANNEL_ID}.release.core-os.net/amd64-usr 替换为nginx服务器地址
    # 我的地址如下
    http://192.168.67.111/coreos
    # 替换完成后,下载cloud-config.yaml
    wget http://192.168.67.111/coreos/cloud-config.yaml
    # 开始安装
    coreos-install -d /dev/xvda -C stable -c ./cloud-config.yaml
    # 安装成功后,既可关闭虚拟机
```

8.修改coreos启动文件,完成正式启动

``` bash
    # 修改 /etc/xen/CoreOS.cfg
    #
        # Configuration file for the Xen instance Search, created
        # by xen-tools 4.6 on Mon Aug 19 10:47:09 2015.
        #

        bootloader = "/usr/local/lib/xen/bin/pygrub"

        vcpus       = '1'
        memory      = '1000'

        disk        = [
              'phy:/dev/vg0/CoreOS,xvda,rw'
                        ]

        #
        #  Hostname
        #
        name        = 'CoreOS'

        #
        #  Networking
        #
        vif         = [ 'mac=00:16:3E:4A:BE:D2,bridge=xenbr0' ]

        #
        #  Behaviour
        #
        on_poweroff = 'destroy'
        on_reboot   = 'restart'
        on_crash    = 'restart'
```

重新启动coreos虚拟机,至此,xen安装CoreOS完成. 有疑问请邮件: wheatsliu#gmail.com