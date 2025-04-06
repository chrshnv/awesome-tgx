# awesome-tgx — a fork of Telegram X.

![Telegram X](/images/feature.png)

This is the complete source code and the build instructions for the official alternative Android client for the Telegram messenger, based on the [Telegram API](https://core.telegram.org/api) and the [MTProto](https://core.telegram.org/mtproto) secure protocol via [TDLib](https://github.com/TGX-Android/tdlib).

* [**awesome-tgx** on Telegram](https://t.me/awesometgx)
* [APKs and Build Info](https://t.me/awesometgxbuilds)

<details>
<summary>Other sources</summary>

* [**GitHub Releases**](https://github.com/chrshnv/awesome-tgx/releases)

</details>

## Build instructions

### Prerequisites

* At least **5,34GB** of free disk space: **487,10MB** for source codes and around **4,85GB** for files generated after building all variants
* **4GB** of RAM
* **macOS** or **Linux**-based operating system. **Windows** platform is supported by using [MSYS](https://www.msys2.org/) (e.g., [Git Bash](https://gitforwindows.org/)).

#### macOS

* [Homebrew](https://brew.sh)
* git with LFS, wget and sed: `$ brew install git git-lfs wget gsed && git lfs install`

#### Ubuntu

* git with LFS: `# apt install git git-lfs`
* Run `$ git lfs install` for the current user, if you didn't have `git-lfs` previously installed

#### Windows

* Shell with `git`, `wget`, and `make` utilities:
    * **MSYS**: `$ pacman -S make git mingw-w64-x86_64-git-lfs`
    * **Git Bash**: 
        1. Download [wget](https://eternallybored.org/misc/wget/), unzip `wget.exe` and move to your `Git\mingw64\bin\`
        2. Download [make](https://sourceforge.net/projects/ezwinports/files/make-4.3-without-guile-w32-bin.zip), unzip and copy the contents to your `Git\mingw64\` merging the folders, but do **NOT** overwrite any existing files
* Run `$ git lfs install` for the current user, if you didn't have `git lfs` previously initialized

### Building

1. `$ git clone --recursive --depth=1 --shallow-submodules https://github.com/chrhsnv/awesome-tgx awesome-tgx` — clone **Telegram X** with submodules
2. In case you forgot the `--recursive` flag, `cd` into `tgx` directory and: `$ git submodule init && git submodule update --init --recursive --depth=1`
3. Create `keystore.properties` file outside of source tree with the following properties:<br/>`keystore.file`: absolute path to the keystore file<br/>`keystore.password`: password for the keystore<br/>`key.alias`: key alias that will be used to sign the app<br/>`key.password`: key password.<br/>**Warning**: keep this file safe and make sure nobody, except you, has access to it. For production builds one could use a separate user with home folder encryption to avoid harm from physical theft
4. `$ cd tgx`
5. Run `$ scripts/./setup.sh` and follow up the instructions
6. If you specified package name that's different from the one Telegram X uses, [setup Firebase](https://firebase.google.com/docs/android/setup) and replace `google-services.json` with the one that's suitable for the `app.id` you need
7. Now you can open the project using **[Android Studio](https://developer.android.com/studio/)** or build manually from the command line: `./gradlew assembleUniversalRelease`.

#### Available flavors

* `arm64`: **arm64-v8a** build with `minSdkVersion` set to `21` (**Lollipop**)
* `arm32`: **armeabi-v7a** build
* `x64`: **x86_64** build with `minSdkVersion` set to `21` (**Lollipop**)
* `x86`: **x86** build
* `universal`: universal build that includes native bundles for all platforms.

### Quick setup for development

If you are developing a [contribution](https://github.com/chrshnv/awesome-tgx/blob/main/docs/PULL_REQUEST_TEMPLATE.md) to the project, you may follow the simpler building steps:

1. `$ git clone --recursive https://github.com/chrshnv/awesome-tgx awesome-tgx`
2. `$ cd tgx`
3. [Obtain Telegram API credentials](https://core.telegram.org/api/obtaining_api_id)
4. Create `local.properties` file in the root project folder using any text editor:<br/><pre># Location where you have Android SDK installed
sdk.dir=YOUR_ANDROID_SDK_FOLDER
\# Telegram API credentials obtained at previous step
telegram.api_id=YOUR_TELEGRAM_API_ID
telegram.api_hash=YOUR_TELEGRAM_API_HASH</pre>
5. Run `$ scripts/./setup.sh` — this will download required Android SDK packages and build native dependencies that aren't part of project's [CMakeLists.txt](/app/jni/CMakeLists.txt)
6. Open and build project via [Android Studio](https://developer.android.com/studio) or by using one of `./gradlew assemble` commands in terminal

After submitting a pull request and its initial review, special build including your contribution will be published in [@tgx_prs](https://t.me/tgx_prs) channel, where it can be tested by the community. In case any issues or bugs found, you may push more commits to an existing PR that address them and request to publish a newer build by using comments section of pull request or in [@tgx_dev](https://t.me/tgx_dev) chat.

Here's a list of related PR-welcome TODOs:

* Project path must not affect the resulting `.so` files, so user & project location requirement could be removed
* When building native binaries on **macOS**, `.comment` ELF section differs from the one built with **Linux** version of NDK. It must be removed or made deterministic without any side-effects like breaking `native-debug-symbols.zip` (or should be reported to NDK team?)
* Checksums of cold APK builds always differ, even though the same keystore applied and generated inner APK contents do not differ. Real cause must be investigated and fixed, if possible.<br/>To generate cold build, invoke `$ scripts/./reset.sh` and `$ scripts/./setup.sh --skip-sdk-setup`.<br/>**Warning**: this will also reset changes inside some of the submodules ([ffmpeg](/app/jni/thirdparty/ffmpeg), [libvpx](/app/jni/thirdparty/libvpx), [webp](/app/jni/thirdparty/webp), [opus](/app/jni/thirdparty/opus) and [ExoPlayer](/app/jni/thirdparty/exoplayer))
* 
<i>PS: [Docker](https://www.docker.com) is not considered an option, as it just hides away these tasks, and requires that all published APKs must be built using it.</i>

## License

`awesome-tgx` is licensed under the terms of the GNU General Public License v3.0.

For more information, see [LICENSE](/LICENSE) file.

License of components and third-party dependencies it relies on might differ, check `LICENSE` file in the corresponding folder.

### Third-party dependencies

List of third-party components used in **awesome-tgx** can be found [here](/docs/THIRDPARTY.md). Additionally you can check the specific commit of the third-party component used, for example, [here](/app/jni/thirdparty) and [here](/thirdparty).

## Contributions

**awesome-tgx** welcomes contributions. Check out [pull request template](/docs/PULL_REQUEST_TEMPLATE.md) and [guide for contributors](/docs/GUIDE.md) to learn more about awesome-tgx internals before creating the first pull request.

Please do not use this repository to ask questions: if you have general issue with Telegram, refer to [FAQ](http://telegram.org/faq) or contact [Telegram Support](https://telegram.org/faq#telegram-support).
