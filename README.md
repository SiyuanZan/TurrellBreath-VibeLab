# 呼吸天穹 · Vibe Lab Android 工程

此目录是 Unity 6000.0.73f1 导出的 Android Gradle 工程，作为独立 GitHub 仓库提交。Unity 源工程另行保留，不应放进本仓库。

- 应用：呼吸天穹 / Turrell Breath
- 包名：`com.vibelab.turrellbreath`
- 版本：`0.1.0`（versionCode 1，平台可覆盖）
- Backend / ABI：IL2CPP / arm64-v8a
- compileSdk / targetSdk / minSdk：36 / 36 / 29
- 工具链：Unity 自带 Gradle 8.13、JDK 17、NDK 27.2.12479018
- `libil2cpp.so` 已本地预编译，`buildIl2Cpp` 已禁用，原任务体保留。
- `.vibelab-prebuilt.json` 记录预编译文件的 SHA-256。

## 提交

在 GitHub 创建空仓库，将**本目录内容直接作为仓库根目录**上传。不要在外面再套一层目录。用 Git 提交全部工程文件（包括 `.so`、AAR、JAR、`gradle/`、`.vibelab-prebuilt.json`），然后推送：

```bash
git init -b main
git add .
git commit -m "Prepare Turrell Breath Android project for Vibe Lab"
git remote add origin https://github.com/OWNER/REPOSITORY.git
git push -u origin main
git rev-parse HEAD
```

在 Vibe Lab 填入仓库 HTTPS 地址与上述完整 40 位 commit SHA，技术栈选择 Unity。私有仓库按平台界面授予访问权限。不要把本机 token、私钥、keystore 或签名密码提交到仓库。每次更新需重新导出、预编译、验证并提交新 commit。

## 本地构建

使用与导出一致的 Unity Android 工具链，设好 `JAVA_HOME`、`ANDROID_HOME` 和 `ANDROID_SDK_ROOT`，只构建 `:launcher:assembleRelease`。本工程按 Unity 标准导出保留 `gradle/wrapper/gradle-wrapper.properties`，不补写 wrapper 启动脚本；可使用 Unity 自带的同版本 Gradle launcher：

```bash
export JAVA_HOME="/Applications/Unity/Hub/Editor/6000.0.73f1/PlaybackEngines/AndroidPlayer/OpenJDK"
export ANDROID_HOME="/Applications/Unity/Hub/Editor/6000.0.73f1/PlaybackEngines/AndroidPlayer/SDK"
export ANDROID_SDK_ROOT="$ANDROID_HOME"
"$JAVA_HOME/bin/java" -cp "/Applications/Unity/Hub/Editor/6000.0.73f1/PlaybackEngines/AndroidPlayer/Tools/gradle/lib/gradle-launcher-8.13.jar" org.gradle.launcher.GradleMain :launcher:assembleRelease --console=plain --no-daemon
```

`gradle.properties` 与 module 中的 Unity 工具链路径按官方要求保持导出原样，由平台构建时处理。`unityProjectPath` 和 `unity.projectPath` 已清空。不要添加 `skipIl2CppBuild`；它会产生假 so。release 引用 debug signingConfig 为 Unity 默认行为，最终由 Vibe Lab 统一签名。

本机构建验证不代表平台端到端构建或头显运行验证。部分 PICO 插件可能仅具备 4 KB LOAD 对齐，详见独立转换报告，不能据此声明全量 16 KB 兼容。

官方要求：https://developer-cn.picoxr.com/document/distribute/pico-vibelab-requirements/
