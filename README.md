### Requirements
- Around 75G disk space
- 20G or more usable internet
- A computer with at least 16G RAM running Linux or MacOS
- A brain
- Patience

### Instructions
- Preparing the SERVER
    1. To prepare your server, i recommend using [**Akhil Narang**](https://github.com/akhilnarang/scripts) scripts.
    2. Make directory for the repo binary
        ```
        mkdir ~/bin
        ```
    3. Add directory for the repo binary to its path
        ```
        PATH=~/bin:$PATH
        ```
    4. Downloading repo binary and placing it in the proper directory
        ```
        curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
        ```
    5. Giving the repo binary the proper permissions
        ```
        chmod a+x ~/bin/repo
        ```
    6. Creating directory for where the ROM repo will be stored and synced
        ```
        mkdir ~/DroidROM
        cd ~/DroidROM
        ```

- Preparing the ROM
    1. Make sure you have a build environment setup.
    2. Make a new directory, cd to it and run
        ```
        repo init --depth=1 -u https://github.com/DroidROM/manifest -b eleven
        ```
    3. Sync!
        ```
        repo sync -c -j$(nproc --all) --no-clone-bundle --no-tags --force-sync
        ```

- OpenGApps
    1. **NOTE** If you are not going to use the `build.sh` script, read below:
    2. Due to the excessive size of the .apk, some of them are not synchronized correctly only in the sync of the manifest. To correct this problem, we use a script at the root of the ROM, called `opengapps.sh`.
    3. In order for it to work correctly, it is necessary to have installed the `git-lfs` package in your distribution.
    4. After that, run the script:
        ```
        ./opengapps.sh
        ```

- Preparing device
    1. Clone your tree repositories
        Example:
          ```
          git clone https://github.com/YourUser/device_xiaomi_beryllium -b eleven device/xiaomi/beryllium
          ```
    2. Move/copy your `<ROM>.mk` (Example: `lineage.mk` or `lineage_beryllium.mk`) file to `aosp_beryllium.mk`.
    3. Open this file and
        - Set PRODUCT_NAME to `aosp_<device>` (Example: `aosp_beryllium`)
        - For a Phone or tablet with a SIM Card, add
            ```
            # Inherit from aosp vendor
            $(call inherit-product, vendor/aosp/config/common_full_phone.mk)
            ```
        - For a WiFi-only tablet, add
            ```
            # Inherit from aosp vendor
            $(call inherit-product, vendor/aosp/config/common_full_tablet_wifionly.mk)
            ```
    4. Save and exit

- Building
    1. Run
        ```
        ./build.sh <device>
        ```
    2. If you want to do it manually, run:
        ```
        . build/envsetup.sh
        lunch aosp_<device>-userdebug
        make -j$(nproc --all) bacon
        ```
        Example:
        ```
        . build/envsetup.sh
        lunch aosp_beryllium-userdebug
        make -j$(nproc --all) bacon
        ```
        For detailed logs, use:
        ```
        make -j$(nproc --all) bacon 2>&1 | tee log.txt
        ```
    3. This will start compiling the build.
    4. Resolve errors if any and continue building.
    5. Remember to `make clobber && make clean` every now and then!

### Credits
- This project aims to be the purest [**AOSP**](https://android.googlesource.com/) experience, with the improvements of [**LineageOS**](https://github.com/LineageOS)
- [**OpenGApps**](https://github.com/opengapps) included

### Reporting compilation issues
- For common porting related errors, visit [**Android Building Help**](https://t.me/AndroidBuildersHelp)
- Make sure you provide relevant logs, screenshots and details with all sources you used.