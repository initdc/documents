# 概述
本文介绍了如何使用 Linux Yocto 构建环境下载和编译 TH1520 Linux SDK。Linux SDK 包含了源码和二进制文件，支持用户开发 Linux 应用程序，并可以构建运行在 evt 开发板上的完整镜像。

- SDK 代码仓库：[https://gitee.com/thead-yocto](https://gitee.com/thead-yocto)
- SDK 代码仓库 tag：**Linux_SDK_V1.1.2**
# 搭建编译环境
Linux SDK 使用 Yocto 构建镜像。Yocto 编译环境使用 Ubuntu 18.04，推荐使用Linux + docker 的方式部署，也可以直接在 Ubuntu 系统搭建编译环境。
具体搭建环境方法，请查看文档《[T-Head曳影1520Yocto用户指南.pdf](https://gitee.com/thead-yocto/documents/blob/master/zh/user_guide/T-Head曳影1520Yocto用户指南.pdf)》。

# 下载
0. 下载开源软件包（仅在第一次获取 SDK 时下载）：

   构建时会从网络下载开源软件包，下载的时间依不同的网络和网速而不同切差异很大；有些开源软件位于 GitHub 仓库，受限于国内网络环境会下载失败。为了加速这一过程，可以到 gitee 下载离线开源软件包（假设将开源软件包下载到用户目录）：

   ```bash
   cd ~
   git clone https://gitee.com/thead-yocto/yocto-downloads.git
   ```

1. 下载 Yocto 构建包（不含 SDK 源码）：

   ```bash
   git clone https://gitee.com/thead-yocto/xuantie-yocto.git -b Linux_SDK_V1.1.2
   ```
   
2. 加载目标设备的配置文件，加载环境变量：

    ```bash
    cd xuantie-yocto
    source openembedded-core/oe-init-build-env thead-build/light-fm
    ```

3. 将前面下载的开源软件包通过共享 downloads 目录的方式软链接到 SDK 目录：

    ```bash
    ln -s ~/yocto-downloads ../downloads
    ```

# Machine/Target支持列表

设置完环境变量后，可以看到如下打印信息：

```
### Shell environment set up for builds. ###

You can now run 'bitbake <target>'

Common targets are:
    thead-image-linux
    thead-image-multimedia
    thead-image-gui

machines:
    light-beagle
    light-b-product
    light-a-val
    light-lpi4a
```

其中，'targets' 表示 SDK 支持的镜像列表，'machines' 表示 SDK 支持的板级配置。关于支持的板级配置和镜像列表，说明如下：

<u>target：</u>

| **命名**               | **描述**                                                     |
| ---------------------- | ------------------------------------------------------------ |
| thead-image-linux      | 典型linux系统配置，最小系统加上必要的相关基础组件            |
| thead-image-multimedia | 典型linux系统+视频视觉配置，加上视频子系统的组件（Gstreamer等） |
| thead-image-gui        | 加上GUI相关组件的完整配置版本，包括Gnome桌面、weston、QT等应用组件等等 |

<u>machine：</u>

| **命名**        | **描述**            |
| --------------- | ------------------- |
| light-a-val     | TH1520-A EVB板      |
| light-b-product | TH1520-B EVB板      |
| light-beagle    | beagleV-Ahead开发板 |
| light-lpi4a     | Lichee Pi 4A开发板  |

# 构建固件

构建命令如下：
```bash
MACHINE={machine} bitbake {target}
```

其中，将 {machine} 和 {target} 替换为上一节支持的列表中的真实名称。例如，要编译一个典型 Linux 系统镜像并运行在 TH1520-A EVB 开发板上，则编译命令如下：

```bash
MACHINE=light-a-val bitbake thead-image-linux
```



# 其他
**light_deploy_images 仓库：**

- 包含已经构建好的 Linux Image，以及其他相关工具，可以打开仓库后直接下载使用
- 仓库地址：[https://gitee.com/thead-yocto/light_deploy_images](https://gitee.com/thead-yocto/light_deploy_images)

**documents 仓库：**

- 包含所有发布文档
- 仓库地址：[https://gitee.com/thead-yocto/documents](https://gitee.com/thead-yocto/documents)
