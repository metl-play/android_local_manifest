### Device specific configuration to build AOSP Android 14 for Raspberry Pi 4 and Raspberry Pi 5.

***

### How to build:

1. Establish [Android build environment](https://source.android.com/setup/initializing) and install [repo](https://source.android.com/docs/setup/develop#installing-repo).

2. Install additional packages:

```
sudo apt-get install bc coreutils dosfstools e2fsprogs fdisk kpartx mtools ninja-build pkg-config python3-pip
sudo pip3 install meson mako jinja2 ply pyyaml dataclasses
mkdir ~/bin/
curl https://storage.googleapis.com/git-repo-downloads/repo-1 > ~/bin/repo
chmod a+x ~/bin/repo
```

3. Initialize repo:

```
sudo python3 ~/bin/repo init -u https://android.googlesource.com/platform/manifest -b android-14.0.0_r17
sudo curl -o .repo/local_manifests/manifest_brcm_rpi.xml -L https://raw.githubusercontent.com/raspberry-vanilla/android_local_manifest/android-14.0/manifest_brcm_rpi.xml --create-dirs
```

Or optionally, you can reduce download size by creating a shallow clone and removing unneeded projects:

```
sudo python3 ~/bin/repo init -u https://android.googlesource.com/platform/manifest -b android-14.0.0_r17 --depth=1
sudo curl -o .repo/local_manifests/manifest_brcm_rpi.xml -L https://raw.githubusercontent.com/raspberry-vanilla/android_local_manifest/android-14.0/manifest_brcm_rpi.xml --create-dirs
sudo curl -o .repo/local_manifests/remove_projects.xml -L https://raw.githubusercontent.com/raspberry-vanilla/android_local_manifest/android-14.0/remove_projects.xml
```

4. Sync source code:

```
sudo python3 ~/bin/repo sync
```

5. Setup Android build environment:

```
. build/envsetup.sh
```

6. Select the device (`rpi4` or `rpi5`) and build target (tablet UI, `tv` for Android TV, or `car` for Android Automotive):

```
lunch aosp_rpi4-userdebug
```
```
lunch aosp_rpi4_tv-userdebug
```
```
lunch aosp_rpi4_car-userdebug
```
```
lunch aosp_rpi5-userdebug
```
```
lunch aosp_rpi5_tv-userdebug
```
```
lunch aosp_rpi5_car-userdebug
```

7. Compile:

```
make bootimage systemimage vendorimage -j$(nproc)
```

8. Make flashable image for the device (`rpi4` or `rpi5`):

```
./rpi4-mkimg.sh
```
```
./rpi5-mkimg.sh
```

Also look into [Linux kernel build instructions](https://github.com/raspberry-vanilla/android_kernel_manifest/tree/android-14.0).

***

### Issues:

- [Android](https://github.com/raspberry-vanilla/android_local_manifest/issues)
- [Linux kernel](https://github.com/raspberry-vanilla/android_kernel_manifest/issues)

***

### Wiki:

- [Audio](https://github.com/raspberry-vanilla/android_local_manifest/wiki/Audio)
- [DSI display](https://github.com/raspberry-vanilla/android_local_manifest/wiki/DSI-display)
- [HDMI display](https://github.com/raspberry-vanilla/android_local_manifest/wiki/HDMI-display)
- [HDMI-CEC](https://github.com/raspberry-vanilla/android_local_manifest/wiki/HDMI-CEC)
- [Interfaces](https://github.com/raspberry-vanilla/android_local_manifest/wiki/Interfaces)
- [USB boot](https://github.com/raspberry-vanilla/android_local_manifest/wiki/USB-boot)
- [Utilities](https://github.com/raspberry-vanilla/android_local_manifest/wiki/Utilities)
- [Video decoding & encoding](https://github.com/raspberry-vanilla/android_local_manifest/wiki/Video-decoding-&-encoding)
