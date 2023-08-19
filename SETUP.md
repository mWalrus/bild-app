# Android development in Rust

## Setup
### Core Dependencies
- Rust
- `cargo`

### Android Setup
#### Packages
```bash
paru -S android-tools android-sdk-platform-tools android-udev android-sdk android-ndk mtpfs gvfs-mtp gvfs-gphoto2 jre8-openjdk jre17-openjdk jdk17-openjdk
```
NOTE: the mtpfs and gvfs packages are for connecting your android device to your computer so that you can run the app on your phone.
#### USB Debugging

In order for adb to find your device you need to enable developer mode on your phone:
1. Open settings app
2. Navigate to About phone -> Software information
3. Tap Build number 7 times
4. Connect your device
5. Check if it was recognized with `adb devices`

#### ADB Issues
If you cannot see your device in adb, do this:
1. `adb kill-server`
2. `adb start-server`

### PATH
Add these to your bash-/zshrc:
```
export ANDROID_HOME=/opt/android-sdk
export NDK_HOME=/opt/android-ndk
# Finds the latest version you have installed on your system
# export JAVA_LATEST_VERSION="$(archlinux-java status | awk -F'-' '/java-/ {print $2}' | sort -g | tail -n 1)"
export JAVA_HOME="/usr/lib/jvm/java-17-openjdk"
export PATH="$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:$JAVA_HOME/bin"
```
NOTE: The JAVA_LATEST_VERSION command only works for arch linux.
NOTE 2: I tried installing the latest jre/jdk (which is 20 at the time of writing) but compiling the app would generate errors. jre/jdk 17 seems to work though so we go with that.

## Tauri mobile
- Run: `cargo install --git https://github.com/tauri-apps/tauri-mobile`

You can now use tauri mobile as a subcommand to cargo. For more info go [here](https://github.com/tauri-apps/tauri-mobile).

### Running your application
- To run on pc just do `cargo run`
- To run on android device:
  - Connect android device to PC over USB
  - Run `cargo android run`

## License Issues
### Attempt 1
If when running `cargo android run`, you get an error regarding the license of your device you should run this command:
`JAVA_HOME=/usr/lib/jvm/java-8-openjdk sdkmanager --licenses`

### Attempt 2
If you still get the error after youve run the above command, you can run the above as sudo. This will however make
`cargo android run` unable to read the licenses but you can "fix" that by changing ownership of the android-sdk directory.

Steps:
1. `JAVA_HOME=/usr/lib/jvm/java-8-openjdk sudo sdkmanager --licenses`
2. `sudo chown -R <your_username> /opt/android-sdk`
3. `cargo android run`

I don't know if this is a good idea but I was pretty frustrated by licenses at this point and it worked so I went with it anyways.
