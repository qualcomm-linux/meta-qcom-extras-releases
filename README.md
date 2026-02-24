# meta-qcom-extras-releases

This repository holds lock.yml for meta-qcom-extras milestone releases.
Each milestone release is tagged uniquely and tag follows the format `qli-2.0-rc1`

## Host requirements

* Configuration
  - x86 machine
  - Quad-core CPU, for example, Intel i7-2600 at 3.4 GHz (equivalent or better)
  - 300 GB free disk space (swap partition > 32 GB)
  - 16 GB RAM
  - Ubuntu 22.04

* Tools
  - Git 1.8.3.1 or later versions
  - Tar 1.28 or later versions
  - Python 3.10.2 or later versions
  - GCC 10.1 or later versions
  - GNU Make 4.0 or later versions

* Permissions
  - A `sudo` permission is required to run a few commands

### Note
```
Code compilation on a VM is a slow process and can take a few hours. Qualcomm recommends using an Ubuntu host computer for compilation. To set up a virtual machine (VM) running Ubuntu 22.04 on Microsoft® Windows® or Apple® macOS®, see Qualcomm Linux Virtual Machine Setup Guide.
```

## Build Instructions

To create and build a Yocto Docker image:


1. Download Qualcomm Yocto and the supporting meta-layers.
```
git clone https://github.com/qualcomm-linux/meta-qcom-extras-releases.git -b <meta-qcom-extras-releases-tag>
kas checkout meta-qcom-extras-releases/lock.yml 
```

2. Set up the Yocto build:
```
# Customer ID is needed to download the proprietary source repositories from the chipcode
# export CUST_ID using the following command.
export CUST_ID=<Customer ID>

# Set the environment variable to pick up the firmware prebuilts (example shown below):
export FWZIP_PATH="<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/<product>/common/build/ufs/bin"

# Populate meta-qcom-extras kas fragment
meta-qcom-extras/setup_extras_config.sh

# Copy generated meta-qcom-extras kas fragment to meta-qcom
cp meta-qcom-extras/ci/extras.yml meta-qcom/ci/extras.yml

# kas configuration files need to be part of the same repository
# copy the kas lock file to meta-qcom repository
cp meta-qcom-extras-releases/lock.yml meta-qcom/ci/lock.yml
```

3. Build the software image. Build targets are defined based on machine and distro combinations.
```
kas build meta-qcom/ci/<machine>:meta-qcom/ci/<distro>:meta-qcom/ci/extras.yml:meta-qcom/ci/lock.yml
```

4. After a successful build, check that the rootfs.img file is in the build artifacts:
```
cd <workspace-dir>/build/tmp/deploy/images/<MACHINE>/<IMAGE>-<MACHINE>.rootfs-<DATE>.qcomflash/
ls -al rootfs.img
```

For more info, please refer to the official qualcomm-linux [build guide](https://docs.qualcomm.com/doc/80-70022-254/topic/build_landing_page.html?product=895724676033554725&version=1.6)


## Getting in Contact

If you need help, have questions, or want to report a problem:

**GitHub Issues**  
  Use the issue tracker below to report bugs, request features, or ask technical questions related to this repository.  
  👉 [Open an Issue](https://github.com/qualcomm-linux/meta-qcom/issues)


## License
This layer is licensed under the [BSD-3-clause License](https://spdx.org/licenses/BSD-3-Clause.html). See [LICENSE.txt](LICENSE.txt) for the full license text.