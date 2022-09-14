# 1 Overview

This article describes how to download and compile the TH1520 Linux SDK using the Linux Yocto build environment. The Linux SDK contains source code and binary files, which support users to develop Linux applications and build a complete image running on the evt development board.

- SDK code repository: https://gitee.com/thead-yocto
- SDK code repository tag: ***\*Linux_SDK_V0.9.5\****

# 2 Build the Compilation Environment

The Linux SDK uses Yocto to build images. The Yocto compilation environment uses Ubuntu 18.04. It is recommended to use Linux + docker for deployment. You can also build a compilation environment directly on the Ubuntu system. For the specific method of setting up the environment, please visit the repository at https://gitee.com/thead-yocto/documents and view the document "Yocto User Guide".

# 3 Download

1. Download the Yocto buildpack (without SDK source code):

   ```
   git clone https://gitee.com/thead-yocto/xuantie-yocto.git -b Linux_SDK_V0.9.5
   ```



2. Download open source software packages (only download when you get the SDK for the first time): 

   Open source software packages are downloaded from the network when building. The download time varies greatly depending on different networks and network speeds. Some open source software is located in GitHub repositories, and download may fail due to the domestic network environment. To speed up the process, you can go to gitee to download offline open source packages:

   ```
   git clone https://gitee.com/thead-yocto/yocto-downloads.git
   ```

   

3. Load the configuration file of the target device and load the environment variables:

   ```
   cd xuantie-yocto
   
   source openembedded-core/oe-init-build-env build/light-fm
   ```

   

4. Soft link the open source packages downloaded earlier to the SDK directory by sharing the downloads directory (assuming yocto-downloads is downloaded to the root directory):

   ```
   ln -s /yocto-downloads ../downloads
   ```

# 4 Build the Firmware

The build command is as follows:

```
MACHINE=light-a-public-release bitbake light-fm-image-linux
```

For more details on SDK build, please refer to "Linux SDK User Guide".

# 5 Manually Clear the Data Partition

The current version introduces the overlay mechanism of the root partition and the data partition -- if the root partition and the data partition have files with the same name, the system preferentially selects the file in data partition. In order to avoid the influence of the data partition, please use the following command to manually clear the data partition in u-boot mode after fastboot is burned:

```
part start mmc 0 data start_blk
part size mmc 0 data size_blk
mmc erase ${start_blk} ${size_blk}
```

# 6 deb Package Installation

The default utility application is not compiled into the image and is provided as a deb package. The deb package will be generated in the compiled image directory: build/light-fm/tmp-glibc/deploy/deb.

When installing the deb package, you need to first copy the deb package (through adb or network) to the board, and then use the following command:

```
dpkg -i "package_name"
```

If the package depends on other packages, please install them in sequence according to the prompts.

# 7 Other

***light_deploy_images repository:***

- Contains the built Linux Image and other related tools, which can be downloaded and used directly after opening the repository
- Repository address: https://gitee.com/thead-yocto/light_deploy_images

***Documents repository:***

- Contains all published documentation
- Repository address: https://gitee.com/thead-yocto/documents

 