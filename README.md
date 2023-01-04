# 概述
本文介绍了如何使用 Linux Yocto 构建环境下载和编译 TH1520 Linux SDK。Linux SDK 包含了源码和二进制文件，支持用户开发 Linux 应用程序，并可以构建运行在 evt 开发板上的完整镜像。

- SDK 代码仓库：[https://gitee.com/thead-yocto](https://gitee.com/thead-yocto)
- SDK 代码仓库 tag：**Linux_SDK_V1.0.3**
# 搭建编译环境
Linux SDK 使用 Yocto 构建镜像。Yocto 编译环境使用 Ubuntu 18.04，推荐使用Linux + docker 的方式部署，也可以直接在 Ubuntu 系统搭建编译环境。
具体搭建环境方法，请查看文档《[T-Head曳影1520Yocto用户指南.pdf](https://gitee.com/thead-yocto/documents/blob/master/zh/user_guide/T-Head曳影1520Yocto用户指南.pdf)》。

# 下载
下载 Yocto 构建包（不含 SDK 源码）：
```
git clone https://gitee.com/thead-yocto/xuantie-yocto.git -b Linux_SDK_V1.0.3
```

下载开源软件包（仅在第一次获取 SDK 时下载）：
构建时会从网络下载开源软件包，下载的时间依不同的网络和网速而不同切差异很大；有些开源软件位于 GitHub 仓库，受限于国内网络环境会下载失败。为了加速这一过程，可以到 gitee 下载离线开源软件包：
```
git clone https://gitee.com/thead-yocto/yocto-downloads.git
```

加载目标设备的配置文件，加载环境变量：
```
cd xuantie-yocto
source openembedded-core/oe-init-build-env build/light-fm
```

将前面下载的开源软件包通过共享 downloads 目录的方式软链接到 SDK 目录（假设 yocto-downloads 被下载到根目录下）：
```
ln -s /yocto-downloads ../downloads
```

# 构建固件

构建命令如下：
```bash
MACHINE=light-a-public-release bitbake light-fm-image-linux
```

MACHINE 支持列表：

* light-a-public-release：TH1520 light-a 开发板
* light-b-public：TH1520 light-b 开发板
* light-beagle：beagle-board 开发板

# 手动清除 data 分区
当前版本引入了 root 分区和 data 分区的 overlay 机制 -- 如果 root 分区和 data 分区有重名文件，系统会优先选择 data 分区文件。
为了避免 data 分区影响，fastboot 烧录后，在 u-boot 模式下，请使用以下命令手动清除 data 分区：
```c
part start mmc 0 data start_blk
part size mmc 0 data size_blk
mmc erase ${start_blk} ${size_blk}
```
# deb 包安装
默认工具类应用不会编译进镜像，以 deb 包的形式提供。deb 包会生成在编译后的镜像目录：build/light-fm/tmp-glibc/deploy/deb。

安装 deb 包时，需要先将 deb 包拷贝（通过 adb 或者网络方式）到板子上，然后使用如下命令：
```c
dpkg -i "package_name"
```
如果该包依赖其他包，请根据提示依次安装。

# 其他
**light_deploy_images 仓库：**

- 包含已经构建好的 Linux Image，以及其他相关工具，可以打开仓库后直接下载使用
- 仓库地址：[https://gitee.com/thead-yocto/light_deploy_images](https://gitee.com/thead-yocto/light_deploy_images)

**documents 仓库：**

- 包含所有发布文档
- 仓库地址：[https://gitee.com/thead-yocto/documents](https://gitee.com/thead-yocto/documents)
