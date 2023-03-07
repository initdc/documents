# Overview

This article describes how to download and compile the TH1520 Linux SDK using the Linux Yocto build environment. The Linux SDK contains source code and binary files, which support users to develop Linux applications and build a complete image running on the evt development board.

- SDK code repository: https://gitee.com/thead-yocto
- SDK code repository tag: **Linux_SDK_V1.1.2**

# Build the Compilation Environment

The Linux SDK uses Yocto to build images. The Yocto compilation environment uses Ubuntu 18.04. It is recommended to use Linux + docker for deployment. You can also build a compilation environment directly on the Ubuntu system. For the specific method of setting up the environment, please check the document: "[T-Head_Yeying1520_Yocto_User_Guide.pdf](https://gitee.com/thead-yocto/documents/blob/master/en/user_guide/T-Head_Yeying1520_Yocto_User_Guide.pdf)".

# Download

0. Download open source software packages (only download when you get the SDK for the first time): 

   Open source software packages are downloaded from the network when building. The download time varies greatly depending on different networks and network speeds. Some open source software is located in GitHub repositories, and download may fail due to the domestic network environment. To speed up the process, you can go to gitee to download offline open source packages:

    ```
    cd ~
    git clone https://gitee.com/thead-yocto/yocto-downloads.git
    ```

1. Download the Yocto buildpack (without SDK source code):

   ```
   git clone https://gitee.com/thead-yocto/xuantie-yocto.git -b Linux_SDK_V1.1.2
   ```

2. Load the configuration file of the target device and load the environment variables:

   ```
   cd xuantie-yocto
   source openembedded-core/oe-init-build-env thead-build/light-fm
   ```
   
3. Soft link the open source packages downloaded earlier to the SDK directory by sharing the downloads directory (assuming yocto-downloads is downloaded to the root directory):

   ```
   ln -s /yocto-downloads ../downloads
   ```

# Machine/Target List

After setting  the configuration file, you can see the following logs:

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

Among them, 'targets' indicates the image list supported by the SDK, and 'machines' indicates the board configuration supported by the SDK. Board configurations and images are listed as follows:

<u>target：</u>

| Name                   | Description                                                  |
| ---------------------- | ------------------------------------------------------------ |
| thead-image-linux      | 典型linux系统配置，最小系统加上必要的相关基础组件            |
| thead-image-multimedia | 典型linux系统+视频视觉配置，加上视频子系统的组件（Gstreamer等） |
| thead-image-gui        | 加上GUI相关组件的完整配置版本，包括Gnome桌面、weston、QT等应用组件等等 |

<u>machine：</u>

| Name            | Description         |
| --------------- | ------------------- |
| light-a-val     | TH1520-A EVB板      |
| light-b-product | TH1520-B EVB板      |
| light-beagle    | beagleV-Ahead开发板 |
| light-lpi4a     | Lichee Pi 4A开发板  |

# Build the Firmware

The build command is as follows:

```
MACHINE={machine} bitbake {target}
```

Replace {machine} and {target} with the real names from the supported list in the previous section. For example, to compile a typical Linux system image and run it on TH1520-A EVB development board, the compilation command is as follows:

```
MACHINE=light-a-val bitbake thead-image-linux
```

# Other

***light_deploy_images repository:***

- Contains the built Linux Image and other related tools, which can be downloaded and used directly after opening the repository
- Repository address: https://gitee.com/thead-yocto/light_deploy_images

***Documents repository:***

- Contains all published documentation
- Repository address: https://gitee.com/thead-yocto/documents

 