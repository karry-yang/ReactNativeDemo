# ReactNativeDemo

基于 **React Native 0.85.0 + TypeScript 5.8.3** 的移动应用开发项目。

## 环境要求

| 工具 | 最低版本 | 说明 |
|------|----------|------|
| Node.js | >= 22.11.0 | JavaScript 运行时 |
| JDK | 21+ | Java 开发工具包（Android 构建必需） |
| Android SDK | API 36 | 编译目标 SDK |
| Android Build Tools | 36.0.0 | Android 构建工具 |
| NDK | 27.1.12297006 | C/C++ 原生开发（新架构必需） |
| Kotlin | 2.1.20 | Android 原生代码 |
| Gradle | 9.3.1 | 通过 Wrapper 自动管理 |

> **提示**: 首次使用前请确保已完成 [React Native 环境配置](https://reactnative.dev/docs/set-up-your-environment)。

---

## 项目运行步骤

### 第一步：安装依赖

```bash
npm install
```

### 第二步：启动 Metro 开发服务器

Metro 是 React Native 的 JavaScript 打包工具，负责编译和提供 JS Bundle。

```bash
npm start
```

启动成功后，终端会显示 Metro 服务运行信息（默认端口 `8081`）。**保持此窗口不要关闭**。

### 第三步：运行应用

在**新的终端窗口**中，根据目标平台执行以下命令：

#### Android

```bash
npm run android
```

此命令会：
1. 编译 Android 原生代码（Gradle 构建）
2. 将 JS Bundle 打包并安装到设备/模拟器
3. 启动应用

> 前提：已配置 Android 模拟器或连接真机，且 `ANDROID_HOME` 环境变量已设置。

#### iOS（仅 macOS）

iOS 需要先安装 CocoaPods 依赖：

```bash
# 首次克隆项目时，安装 CocoaPods 本身
bundle install

# 每次更新原生依赖后执行
bundle exec pod install
```

然后运行：

```bash
npm run ios
```

---

## 开发工作流

### 热重载（Fast Refresh）

修改 `App.tsx` 或任何 `.ts/.tsx` 文件并保存后，应用会**自动刷新**，无需手动重新加载。

### 手动刷新

如果需要强制全量重载（重置应用状态）：

- **Android**: 连按 <kbd>R</kbd> 键两次，或通过 <kbd>Ctrl</kbd> + <kbd>M</kbd> 打开开发菜单选择 "Reload"
- **iOS**: 在模拟器中按 <kbd>R</kbd> 键

### 调试技巧

- **摇动设备**或按 <kbd>Ctrl</kbd> + <kbd>M</kbd>（Android）/ <kbd>Cmd ⌘</kbd> + <kbd>D</kbd>（iOS）打开开发者菜单
- 可开启 **Reload on Debug** 实现调试时自动重载

---

## 其他常用命令

```bash
# 运行单元测试
npm test

# 代码规范检查
npm run lint

# 直接通过 Android Studio / Xcode 运行
# - Android: 用 AS 打开 android/ 目录
# - iOS: 用 Xcode 打开 ios/ReactNativeDemo.xcworkspace
```

---

## 技术栈

| 技术 | 版本 | 用途 |
|------|------|------|
| React | 19.2.3 | UI 框架 |
| React Native | 0.85.0 | 跨平台移动开发框架 |
| TypeScript | 5.8.3 | 类型安全的 JavaScript 超集 |
| Hermes | 内置 | 高性能 JS 引擎 |
| TurboModules / Fabric | 已启用 | 新架构原生模块系统 |

## 项目结构概览

```
ReactNativeDemo/
├── App.tsx              # 应用根组件
├── index.js             # RN 注册入口
├── tsconfig.json        # TypeScript 配置
├── babel.config.js      # Babel 转译配置
├── metro.config.js      # Metro 打包器配置
├── android/             # Android 原生工程
├── ios/                 # iOS 原生工程
└── __tests__/           # 单元测试
```

详细说明见 [PROJECT_STRUCTURE.md](./PROJECT_STRUCTURE.md)

---

## Android 项目详解

### Android Studio 打开方式

用 Android Studio 打开 `android/` 文件夹时，会提示选择打开模式：

| 选择方式 | 说明 |
|---------|------|
| **Android** (推荐) | 以 Android 项目方式打开，自动识别 Gradle 构建脚本，正确处理源码、资源、依赖。**React Native 项目应选这个。** |
| **Project** | 以标准 IntelliJ 项目打开，不会自动识别 Gradle 构建，源码/依赖可能识别不正确，不推荐。 |
| **Projects** | 新版本的窗口管理视图，用于同时管理多个项目窗口，不是一种打开模式。 |

### android/ 目录结构说明

```
android/
├── app/                        # 应用主模块（你自己的代码）
│   ├── build.gradle            #   模块构建脚本（applicationId、依赖等）
│   ├── src/main/
│   │   ├── AndroidManifest.xml #   应用清单（权限、Activity 声明等）
│   │   ├── java/               #   Java/Kotlin 源码
│   │   └── res/                #   Android 资源文件（图标、布局等）
│   └── build/                  #   编译输出（可安全删除）
├── build.gradle                # 项目级构建脚本（全局 Gradle 配置）
├── settings.gradle             # 声明模块 + autolink 原生库
├── gradle.properties           # Gradle 属性（JVM 内存等）
├── gradle/                     # Gradle Wrapper（确保统一 Gradle 版本）
│   └── wrapper/
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew / gradlew.bat       # Gradle Wrapper 启动脚本
└── build/                      # 项目级编译输出（可安全删除）
```

### 关键组件说明

| 组件 | 位置 | 作用 |
|------|------|------|
| **app 模块** | `android/app/` | 你的 Android 应用主模块，包含应用清单、原生源码、资源、签名配置 |
| **@react-native/gradle-plugin** | `node_modules/@react-native/gradle-plugin/` | React Native Gradle 插件，负责构建流程：JS Bundle 打包、Hermes 配置、自动链接等 |
| **第三方原生库**（如 react-native-safe-area-context） | `node_modules/` 中各自目录 | 每个含原生代码的 RN 库都有自己的 Android 模块，通过 `settings.gradle` 的 autolink 自动链接到项目中 |
| **Gradle Wrapper** | `android/gradle/` | 确保团队使用相同版本的 Gradle，无需手动安装 |
| **build/ 目录** | `android/build/`、`android/app/build/` | 编译产物（APK、中间文件），可随时删除，下次构建会重新生成 |

### 构建流程简述

1. Gradle 读取 `settings.gradle`，识别 `app` 模块和通过 `includeBuild` 引入的 React Native Gradle 插件
2. 执行 `app/build.gradle`，应用 `com.facebook.react` 插件
3. React 插件通过 autolink 扫描所有已安装的 RN 原生库（如 safe-area-context），自动链接其 Android 模块
4. 编译 Kotlin/Java 原生代码 + Metro 打包 JS Bundle → 生成 APK

### Gradle Scripts 详解

React Native Android 项目的 Gradle 构建体系由 4 个核心文件组成，按 Gradle 执行顺序依次介绍：

#### (1) `settings.gradle` — 项目初始化，最先执行

```groovy
pluginManagement { includeBuild("../node_modules/@react-native/gradle-plugin") }
plugins { id("com.facebook.react.settings") }
extensions.configure(com.facebook.react.ReactSettingsExtension){ ex -> ex.autolinkLibrariesFromCommand() }
rootProject.name = 'ReactNativeDemo'
include ':app'
includeBuild('../node_modules/@react-native/gradle-plugin')
```

| 配置项 | 作用 | 为什么需要 |
|--------|------|-----------|
| `pluginManagement { includeBuild(...) }` | 告诉 Gradle 去哪里找 `com.facebook.react.settings` 插件 | React Native 的 Gradle 插件不在 Maven 仓库中，而是通过 `includeBuild` 从 `node_modules` 本地引入 |
| `plugins { id("com.facebook.react.settings") }` | 加载 React Native Settings 提供的 autolink 扩展 | 提供自动链接原生库的能力，是整个 autolink 机制的入口 |
| `autolinkLibrariesFromCommand()` | 扫描 `package.json` 中所有依赖，找到含原生 Android 代码的库并自动 include | **不需要手动添加每个原生库**，`npm install` 后自动生效 |
| `rootProject.name` | 项目名称 | 用于 IDE 识别和构建日志显示 |
| `include ':app'` | 声明 app 为子模块 | Gradle 是多项目构建工具，必须显式声明包含哪些子模块 |
| `includeBuild(...)` | 将 React Native Gradle 插件作为复合构建引入 | 插件本身的代码需要被编译并参与构建过程 |

#### (2) `build.gradle`（项目根级）— 全局版本与依赖管理

```groovy
buildscript {
    ext {
        buildToolsVersion = "36.0.0"
        minSdkVersion = 24
        compileSdkVersion = 36
        targetSdkVersion = 36
        ndkVersion = "27.1.12297006"
        kotlinVersion = "2.1.20"
    }
    repositories {
        google()
        mavenCentral()
    }
    dependencies {
        classpath("com.android.tools.build:gradle")
        classpath("com.facebook.react:react-native-gradle-plugin")
        classpath("org.jetbrains.kotlin:kotlin-gradle-plugin")
    }
}
apply plugin: "com.facebook.react.rootproject"
```

| 配置项 | 当前值 | 作用 | 为什么这么配置 |
|--------|--------|------|---------------|
| `buildToolsVersion` | `36.0.0` | Android 构建工具版本 | React Native 0.85 要求，需与 SDK Platform 版本匹配 |
| `minSdkVersion` | `24` | 最低支持的 Android 版本（Android 7.0） | React Native 0.76+ 最低要求为 24，过低会导致某些 API 不可用 |
| `compileSdkVersion` | `36` | 编译时使用的 SDK 版本 | 应使用最新的稳定版以获得最新的 API 和构建工具优化 |
| `targetSdkVersion` | `36` | 目标 SDK 版本 | Google Play 要求 targetSdk 不超过 2 年前的版本；设为最新版可确保应用适配最新系统行为 |
| `ndkVersion` | `27.1.12297006` | NDK 版本 | **新架构（TurboModules/Fabric）必需**，用于编译 C++ 代码；版本必须与 React Native 要求一致 |
| `kotlinVersion` | `2.1.20` | Kotlin 编译器版本 | React Native 0.85 要求，过低可能导致编译失败 |
| `google()` / `mavenCentral()` | — | 仓库源 | `google()` 提供 Android 构建工具和 Jetpack 库；`mavenCentral()` 提供通用 Java/Kotlin 依赖 |
| `classpath` 依赖 | — | 构建时需要的插件 | `gradle` 提供 Android 构建能力，`react-native-gradle-plugin` 提供 RN 构建流程，`kotlin-gradle-plugin` 提供 Kotlin 编译支持 |
| `com.facebook.react.rootproject` | — | React Native 根项目插件 | 在项目级别提供 React Native 构建所需的基础配置和任务 |

> **`ext {}` 块的作用**：集中定义版本号，`app/build.gradle` 通过 `rootProject.ext.xxx` 引用，避免版本号分散在各处。

#### (3) `app/build.gradle` — 应用模块构建配置

```groovy
apply plugin: "com.android.application"   // Android 应用模块
apply plugin: "org.jetbrains.kotlin.android" // Kotlin 支持
apply plugin: "com.facebook.react"          // React Native 构建支持
```

**三大插件的作用：**

| 插件 | 作用 |
|------|------|
| `com.android.application` | 标识这是一个 Android 应用模块（区别于 library），提供 APK 打包、签名等能力 |
| `org.jetbrains.kotlin.android` | 启用 Kotlin 编译，使项目支持 `.kt` 文件 |
| `com.facebook.react` | React Native 核心插件，负责：autolink 原生库、调用 Metro 打包 JS Bundle、Hermes 字节码编译、Debug/Release 差异化构建 |

**`react {}` 配置块（React Native 专属）：**

```groovy
react {
    autolinkLibrariesWithApp()
}
```

这个块默认几乎不需要配置，所有选项都有合理默认值。常用可选项：

| 配置项 | 默认值 | 说明 | 使用场景 |
|--------|--------|------|---------|
| `root` | `../..` | 项目根目录（`package.json` 所在） | 非标准目录结构时修改 |
| `entryFile` | `index.js` | JS 入口文件 | 入口文件改名时修改 |
| `bundleAssetName` | `index.android.bundle` | 生成的 JS Bundle 文件名 | 需要自定义 Bundle 名时 |
| `hermesCommand` | `hermesc` | Hermes 编译器路径 | 使用自定义 Hermes 版本时 |
| `debuggableVariants` | `["debug", "debugOptimized"]` | 可调试的构建变体 | 添加 productFlavors 时需要扩展 |

> **关键：`autolinkLibrariesWithApp()`** — 这是 autolink 的第二步（第一步在 `settings.gradle`），它将之前扫描到的原生库的 Android 模块作为 `implementation` 依赖添加到 app 模块中。

**`android {}` 配置块：**

| 配置项 | 当前值 | 作用 | 常见调整场景 |
|--------|--------|------|-------------|
| `namespace` | `com.reactnativedemo` | 应用的 R 资源类包名 | 修改包名时需同步更新 |
| `applicationId` | `com.reactnativedemo` | 应用唯一标识（发布到应用商店的 ID） | **重要**：上线前应设为正式包名 |
| `versionCode` | `1` | 版本号（整数，每次发布递增） | 每次发版 +1 |
| `versionName` | `"1.0"` | 版本名（用户可见） | 如 `"2.1.0"` |
| `signingConfigs.debug` | 默认 keystore | Debug 签名配置 | 通常不需要修改 |
| `signingConfigs.release` | — | Release 签名配置 | **生产环境必须**替换为自有 keystore |
| `minifyEnabled` | `false` | 是否启用代码混淆/压缩 | Release 建议设为 `true` 以减小 APK 体积 |

**`dependencies {}` 依赖块：**

```groovy
dependencies {
    implementation("com.facebook.react:react-android")
    if (hermesEnabled.toBoolean()) {
        implementation("com.facebook.react:hermes-android")  // Hermes 引擎
    } else {
        implementation jscFlavor                              // JSC 引擎（备选）
    }
}
```

| 依赖 | 作用 | 为什么 |
|------|------|--------|
| `react-android` | React Native Android 运行时核心 | 提供所有 React Native 基础组件和桥接机制 |
| `hermes-android` | Hermes JS 引擎 | 相比 JSC 启动更快、内存占用更小、预编译字节码，**RN 默认推荐** |
| `jsc-android` | JavaScriptCore 引擎（备选） | Hermes 不可用时的回退方案；国际版（`jsc-android-intl`）约大 6MiB，支持完整 i18n |

#### (4) `gradle.properties` — Gradle 运行时属性

```properties
org.gradle.jvmargs=-Xmx2048m -XX:MaxMetaspaceSize=512m
android.useAndroidX=true
reactNativeArchitectures=armeabi-v7a,arm64-v8a,x86,x86_64
newArchEnabled=true
hermesEnabled=true
edgeToEdgeEnabled=false
```

| 属性 | 值 | 作用 | 为什么这么配置 |
|------|-----|------|---------------|
| `org.gradle.jvmargs` | `-Xmx2048m -XX:MaxMetaspaceSize=512m` | Gradle Daemon 的 JVM 内存 | 默认 512m 太小，RN 项目依赖多，构建时容易 OOM，2048m 是推荐值 |
| `android.useAndroidX` | `true` | 启用 AndroidX | 所有现代 Android 库都基于 AndroidX，不启用则无法使用新版本依赖 |
| `reactNativeArchitectures` | `armeabi-v7a,arm64-v8a,x86,x86_64` | 编译哪些 CPU 架构 | 开发时构建全部架构确保模拟器（x86）和真机（ARM）都能运行；Release 时可只保留 `arm64-v8a` 减小 APK 体积 |
| `newArchEnabled` | `true` | 启用新架构（TurboModules + Fabric） | React Native 0.76+ 默认启用，提供更好的性能和类型安全的原生模块 |
| `hermesEnabled` | `true` | 启用 Hermes 引擎 | RN 推荐的 JS 引擎，启动速度比 JSC 快约 2 倍 |
| `edgeToEdgeEnabled` | `false` | 边到边显示（沉浸式状态栏） | 需要配合 `ReactActivity` 使用；设为 `true` 可让应用绘制到系统栏后面 |

#### (5) `gradle/wrapper/gradle-wrapper.properties` — Gradle 版本锁定

```properties
distributionUrl=https\://services.gradle.org/distributions/gradle-8.13-bin.zip
```

| 属性 | 说明 |
|------|------|
| `distributionUrl` | 指定 Gradle 版本（当前 8.13）。通过 Wrapper 机制，**团队无需手动安装 Gradle**，执行 `gradlew` 时自动下载对应版本 |
| `networkTimeout=10000` | 下载超时时间（毫秒） |
| `validateDistributionUrl=true` | 验证下载 URL 的 HTTPS 证书 |

> **为什么用 Wrapper 而不是全局安装？** 确保所有开发者和 CI 环境使用完全相同的 Gradle 版本，避免"在我机器上能跑"的问题。

#### 文件执行顺序总结

```
settings.gradle          → 初始化项目，autolink 扫描原生库
    ↓
build.gradle (根级)      → 定义全局版本号，加载构建插件
    ↓
app/build.gradle         → 配置 app 模块：SDK、签名、依赖、react 插件
    ↓
gradle.properties        → 设置运行时属性（JVM 内存、新架构等）
    ↓
gradle-wrapper.properties → 确定使用的 Gradle 版本
```

### app 模块详解

app 模块是 Android 应用的主体，包含源码、资源、清单和构建配置。其内部结构：

```
app/
├── build.gradle                # 模块构建配置（已在上方详解）
├── proguard-rules.pro          # ProGuard 混淆规则
├── debug.keystore              # Debug 签名文件
├── src/main/
│   ├── AndroidManifest.xml     # 应用清单文件
│   ├── java/com/reactnativedemo/
│   │   ├── MainActivity.kt     # 主 Activity（应用入口页面）
│   │   └── MainApplication.kt  # Application 类（应用全局初始化）
│   └── res/
│       ├── drawable/
│       │   └── rn_edit_text_material.xml  # TextInput 底线修复
│       ├── mipmap-*/           # 应用图标（多分辨率）
│       │   ├── ic_launcher.png
│       │   └── ic_launcher_round.png
│       └── values/
│           ├── strings.xml     # 字符串资源
│           └── styles.xml      # 主题样式
└── build/                      # 编译输出（可安全删除）
```

#### (1) `AndroidManifest.xml` — 应用清单

清单文件是 Android 系统认识应用的"身份证"，声明应用的基本信息和组件。

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <uses-permission android:name="android.permission.INTERNET" />

    <application
      android:name=".MainApplication"
      android:label="@string/app_name"
      android:icon="@mipmap/ic_launcher"
      android:roundIcon="@mipmap/ic_launcher_round"
      android:allowBackup="false"
      android:theme="@style/AppTheme"
      android:usesCleartextTraffic="${usesCleartextTraffic}"
      android:supportsRtl="true">
      <activity
        android:name=".MainActivity"
        android:label="@string/app_name"
        android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|screenSize|smallestScreenSize|uiMode"
        android:launchMode="singleTask"
        android:windowSoftInputMode="adjustResize"
        android:exported="true">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
      </activity>
    </application>
</manifest>
```

**权限声明 `<uses-permission>`：**

| 权限 | 作用 | 为什么需要 |
|------|------|-----------|
| `INTERNET` | 允许网络访问 | React Native 调试时需要连接 Metro 开发服务器（`localhost:8081`），且大多数应用都需要网络请求 |

> **常见扩展权限**：开发中按需添加，如 `CAMERA`（相机）、`ACCESS_FINE_LOCATION`（定位）、`READ_EXTERNAL_STORAGE`（存储读取）、`WRITE_EXTERNAL_STORAGE`（存储写入）等。

**`<application>` 属性：**

| 属性 | 值 | 作用 | 为什么这么配置 |
|------|-----|------|---------------|
| `android:name` | `.MainApplication` | 指定自定义 Application 类 | RN 需要在 Application 初始化阶段加载 ReactHost、注册原生模块等 |
| `android:label` | `@string/app_name` | 应用在桌面显示的名称 | 引用 `strings.xml` 中的值，方便国际化 |
| `android:icon` / `roundIcon` | `@mipmap/ic_launcher` | 应用图标 | 引用多分辨率图标资源，系统自动选择适配当前屏幕密度的版本 |
| `allowBackup` | `false` | 禁止自动备份 | **安全考虑**：防止应用数据被 adb backup 导出；生产环境建议保持 |
| `android:theme` | `@style/AppTheme` | 应用主题 | 定义在 `styles.xml` 中，控制全局视觉风格 |
| `usesCleartextTraffic` | `${usesCleartextTraffic}` | 是否允许 HTTP 明文传输 | Debug 时需要连接 Metro（HTTP），Release 时自动禁用；由 React Gradle Plugin 注入此变量 |
| `supportsRtl` | `true` | 支持从右到左（RTL）布局 | 支持阿拉伯语、希伯来语等 RTL 语言；RN 内部已处理 RTL 布局 |

**`<activity>` 属性（MainActivity）：**

| 属性 | 值 | 作用 | 为什么需要 |
|------|-----|------|-----------|
| `configChanges` | `keyboard\|...\|uiMode` | 声明哪些配置变化时**不重建** Activity | RN 需要自己处理这些变化（如屏幕旋转、键盘弹出），如果系统重建 Activity 会导致 JS 上下文丢失，应用白屏重启 |
| `launchMode` | `singleTask` | Activity 启动模式 | 确保应用只有一个实例，从通知栏或深度链接打开时不会创建新的 Activity 栈 |
| `windowSoftInputMode` | `adjustResize` | 软键盘弹出时的调整方式 | 键盘弹出时自动调整布局（缩小内容区域），避免输入框被键盘遮挡 |
| `exported` | `true` | 允许被其他应用启动 | 作为 LAUNCHER Activity 必须设为 `true`，否则无法从桌面启动 |

**`<intent-filter>` — 桌面入口标识：**

```xml
<intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
</intent-filter>
```

这告诉 Android 系统：`MainActivity` 是应用的启动入口，应在桌面显示图标。没有这个声明，应用安装后桌面上不会出现图标。

#### (2) `MainActivity.kt` — 应用主页面

```kotlin
class MainActivity : ReactActivity() {

  override fun getMainComponentName(): String = "ReactNativeDemo"

  override fun createReactActivityDelegate(): ReactActivityDelegate =
      DefaultReactActivityDelegate(this, mainComponentName, fabricEnabled)
}
```

| 成员 | 作用 | 为什么 |
|------|------|--------|
| `ReactActivity` | RN 提供的基类 Activity | 内部处理了 React 生命周期管理、开发菜单、Back 键处理等，不需要自己实现 |
| `getMainComponentName()` → `"ReactNativeDemo"` | 返回 JS 端注册的根组件名 | 必须与 JS 端 `AppRegistry.registerComponent('ReactNativeDemo', ...)` 的第一个参数完全一致，否则无法渲染 |
| `createReactActivityDelegate()` | 创建 Activity 委托 | `DefaultReactActivityDelegate` 是 RN 默认实现，`fabricEnabled` 参数控制是否启用新架构的 Fabric 渲染器（由 `newArchEnabled` 属性自动控制） |

> **简单理解**：`MainActivity` 是一个"容器"，它把整个屏幕交给 React Native 渲染。应用启动 → 系统创建 MainActivity → 加载 JS Bundle → `AppRegistry.registerComponent` 注册的根组件接管整个界面。

#### (3) `MainApplication.kt` — 应用全局初始化

```kotlin
class MainApplication : Application(), ReactApplication {

  override val reactHost: ReactHost by lazy {
    getDefaultReactHost(
      context = applicationContext,
      packageList = PackageList(this).packages.apply {
        // Packages that cannot be autolinked yet can be added manually here
      },
    )
  }

  override fun onCreate() {
    super.onCreate()
    loadReactNative(this)
  }
}
```

| 成员 | 作用 | 为什么 |
|------|------|--------|
| `Application()` | Android Application 基类 | 在应用进程启动时最先创建，生命周期贯穿整个应用 |
| `ReactApplication` | RN 的 Application 接口 | 要求实现 `reactHost` 属性，提供 React 运行时环境 |
| `reactHost` (lazy) | RN 运行时宿主 | `lazy` 延迟初始化，在真正需要时才创建，避免启动时阻塞；包含所有原生模块和 JS Bundle 配置 |
| `PackageList(this).packages` | 所有已安装的原生模块列表 | 由 autolink 自动生成，包含每个原生库提供的 Package（如 SafeAreaView、网络库等） |
| `getDefaultReactHost(...)` | 创建默认 ReactHost | 封装了 React Native 的初始化逻辑，包括 TurboModule 绑定、Bridge/Fabric 配置等 |
| `onCreate()` | 应用启动时执行 | `loadReactNative(this)` 执行 RN 引擎的底层初始化（加载 `.so` 库、配置线程等） |

> **手动添加原生模块**：如果某个库不支持 autolink，可在 `packages.apply {}` 块中手动添加：
> ```kotlin
> packages.apply {
>   // add(MyCustomReactPackage())
> }
> ```

#### (4) 资源文件

**`res/values/strings.xml` — 字符串资源：**

```xml
<resources>
    <string name="app_name">ReactNativeDemo</string>
</resources>
```

| 资源 | 作用 |
|------|------|
| `app_name` | 应用在桌面和标题栏显示的名称，被 `AndroidManifest.xml` 的 `@string/app_name` 引用 |

> **国际化**：可创建 `values-zh/strings.xml`、`values-en/strings.xml` 等目录，系统根据设备语言自动选择。

**`res/values/styles.xml` — 主题样式：**

```xml
<style name="AppTheme" parent="Theme.AppCompat.DayNight.NoActionBar">
    <item name="android:editTextBackground">@drawable/rn_edit_text_material</item>
</style>
```

| 配置 | 作用 | 为什么 |
|------|------|--------|
| `Theme.AppCompat.DayNight.NoActionBar` | 继承 AppCompat 日/夜模式主题，无 ActionBar | RN 自己管理导航栏（React Navigation），不需要系统 ActionBar；DayNight 自动适配深色模式 |
| `editTextBackground` → `rn_edit_text_material` | 自定义 TextInput 底线样式 | 修复了原版 Material Design EditText 在 RN 中的 NullPointerException 崩溃问题 |

**`res/drawable/rn_edit_text_material.xml` — TextInput 修复：**

这是一个 React Native 内置的 bug 修复文件。原版 Android Material 组件的 `abc_edit_text_material` 在特定状态下会导致 `NullPointerException`，RN 通过覆盖这个 drawable 来规避问题。

**`res/mipmap-*/` — 应用图标：**

| 目录 | 目标屏幕密度 | 典型设备 |
|------|-------------|---------|
| `mipmap-mdpi` | ~160dpi | 低端安卓机 |
| `mipmap-hdpi` | ~240dpi | 较旧手机 |
| `mipmap-xhdpi` | ~320dpi | 中端手机 |
| `mipmap-xxhdpi` | ~480dpi | 主流手机 |
| `mipmap-xxxhdpi` | ~640dpi | 高端/旗舰手机 |

> 系统自动选择最匹配当前屏幕密度的图标，无需代码处理。替换图标时需要同时替换所有分辨率版本。

#### (5) 其他文件

**`proguard-rules.pro` — 代码混淆规则：**

当前为空（仅包含注释）。当 `app/build.gradle` 中 `enableProguardInReleaseBuilds = true` 时生效。用于：
- 缩小 APK 体积（移除未使用的代码）
- 混淆代码（增加逆向难度）
- 防止特定类被混淆（通过 `-keep` 规则）

> **注意**：启用 ProGuard 后如果应用崩溃，通常是因为反射调用的类被混淆了。需要为 RN 相关类添加 `-keep` 规则。

**`debug.keystore` — Debug 签名文件：**

React Native 自动生成的 Debug 签名文件，用于开发调试。**Release 构建必须替换为自有 keystore**（参见 [官方签名指南](https://reactnative.dev/docs/signed-apk-android)）。

#### app 模块文件职责总览

| 文件 | 类别 | 核心职责 | 修改频率 |
|------|------|---------|---------|
| `AndroidManifest.xml` | 清单 | 权限、组件声明、应用元信息 | 中等（添加权限、Activity 时） |
| `MainActivity.kt` | 源码 | RN 渲染容器，桥接原生与 JS | 低（通常不需要改动） |
| `MainApplication.kt` | 源码 | 应用全局初始化，原生模块注册 | 低（手动添加模块时） |
| `res/values/strings.xml` | 资源 | 应用名称等字符串 | 低（改应用名时） |
| `res/values/styles.xml` | 资源 | 主题、视觉风格 | 中等（自定义主题时） |
| `res/drawable/rn_edit_text_material.xml` | 资源 | RN 内置 bug 修复 | 不需要修改 |
| `res/mipmap-*/` | 资源 | 应用图标 | 低（换图标时） |
| `proguard-rules.pro` | 配置 | Release 代码混淆规则 | 低（启用混淆调试时） |
| `debug.keystore` | 签名 | Debug 签名 | 不需要修改 |
| `build.gradle` | 构建 | 模块构建配置（见上文详解） | 中等（添加依赖、改版本时） |

---

## 学习资源

- [React Native 官方文档](https://reactnative.dev)
- [React Native 入门指南](https://reactnative.dev/docs/getting-started)
- [环境搭建指南](https://reactnative.dev/docs/environment-setup)
- [Fast Refresh 热重载](https://reactnative.dev/docs/fast-refresh)

## 故障排查

遇到问题可参考 [官方 Troubleshooting 页面](https://reactnative.dev/docs/troubleshooting)。常见问题：

- **Metro 端口被占用**: 关闭占用 8081 端口的进程，或修改端口 `npm start -- --port 8082`
- **Android 构建失败**: 检查 JDK 版本 >= 21、`ANDROID_HOME` 环境变量是否正确设置
- **iOS 构建失败**: 确保 Xcode 版本兼容，并已执行 `pod install`
