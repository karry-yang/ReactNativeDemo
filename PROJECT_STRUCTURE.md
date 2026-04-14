# ReactNativeDemo 项目结构与文件说明

## 一、项目概览

- **项目名称**: ReactNativeDemo
- **框架版本**: React Native 0.85.0 + React 19.2.3
- **语言**: TypeScript 5.8.3
- **最低 Android 版本**: Android 7.0 (API 24)
- **目标 Android 版本**: Android 16 (API 36)
- **JDK 要求**: 21+
- **Node 要求**: >= 22.11.0
- **Gradle 版本**: 9.3.1
- **Kotlin 版本**: 2.1.20
- **NDK 版本**: 27.1.12297006

---

## 二、顶层目录结构

```
ReactNativeDemo/
├── App.tsx                  应用入口组件（React 根组件）
├── index.js                 React Native 注册入口，将 App 组件注册到原生端
├── app.json                 App 全局配置（应用名称、图标路径等）
├── package.json             Node.js 依赖管理与 npm scripts 定义
├── package-lock.json        依赖版本锁定文件
├── tsconfig.json            TypeScript 编译器配置
├── babel.config.js          Babel 转译配置（JSX/ES6+ → 兼容代码）
├── metro.config.js          Metro Bundler 配置（React Native 打包工具）
├── jest.config.js           Jest 测试框架配置
├── .eslintrc.js             ESLint 代码规范检查规则
├── .prettierrc.js           Prettier 代码格式化规则
├── .watchmanconfig          Watchman 文件监控配置
├── .gitignore               Git 忽略文件规则
├── Gemfile                  Ruby 依赖（iOS CocoaPods 相关）
├── android/                 Android 原生工程目录
├── ios/                     iOS 原生工程目录
├── node_modules/            NPM 安装的第三方依赖包
├── __tests__/               Jest 单元测试文件
└── .bundle/                 Metro 打包器缓存配置
```

---

## 三、核心文件详解

### 3.1 入口与组件文件

| 文件 | 作用 |
|------|------|
| `index.js` | RN 启动入口，调用 `AppRegistry.registerComponent()` 将根组件注册到原生端 |
| `App.tsx` | React 根组件，使用 `SafeAreaProvider` 包裹，支持安全区域和深色模式，当前展示默认欢迎页 |

### 3.2 配置文件

| 文件 | 作用 |
|------|------|
| `package.json` | 定义项目元信息、npm scripts（`start`/`android`/`ios`/`test`/`lint`）、运行时和开发依赖 |
| `app.json` | App 级配置，包括应用名称、入口文件路径等 |
| `tsconfig.json` | TypeScript 配置，定义编译选项和路径别名 |
| `babel.config.js` | Babel 配置，使用 `@react-native/babel-preset` 预设处理 JSX 和新语法 |
| `metro.config.js` | Metro 打包器配置，处理模块解析、转换、资源打包等 |
| `jest.config.js` | Jest 测试配置，使用 `@react-native/jest-preset` 预设 |
| `.eslintrc.js` | ESLint 规则配置，使用 `@react-native/eslint-config` 预设 |
| `.prettierrc.js` | Prettier 格式化规则（单引号、分号、缩进等） |
| `.watchmanconfig` | Watchman 文件监控排除规则，提升开发监听性能 |

### 3.3 主要依赖

**运行时依赖:**
- `react` (19.2.3) - React 核心库
- `react-native` (0.85.0) - React Native 框架
- `@react-native/new-app-screen` (0.85.0) - 默认欢迎页组件
- `react-native-safe-area-context` (^5.5.2) - 安全区域适配

**开发依赖:**
- `@babel/core` / `@babel/preset-env` - Babel 转译
- `@react-native-community/cli` - RN 命令行工具
- `typescript` (^5.8.3) - TypeScript 编译器
- `jest` - 测试框架
- `eslint` / `prettier` - 代码规范

---

## 四、Android 原生工程 (`android/`)

### 4.1 目录结构

```
android/
├── build.gradle                根级构建配置（全局版本变量、仓库、插件声明）
├── settings.gradle             项目设置（模块声明、React Native 插件集成）
├── gradle.properties           Gradle 属性配置（JVM 参数、架构、新架构开关等）
├── local.properties            本地环境配置（SDK/NDK 路径，不提交 Git）
├── gradlew                     Linux/Mac 下的 Gradle Wrapper 启动脚本
├── gradlew.bat                 Windows 下的 Gradle Wrapper 启动脚本
├── gradle/
│   └── wrapper/
│       └── gradle-wrapper.properties   Gradle Wrapper 版本配置（当前 9.3.1）
└── app/                        App 主模块
    ├── build.gradle            App 构建配置（签名、构建类型、混淆、依赖）
    ├── debug.keystore          Debug 签名密钥库
    ├── proguard-rules.pro      ProGuard 代码混淆规则
    └── src/main/
        ├── AndroidManifest.xml         Android 清单文件（包名、权限、Activity 声明）
        ├── res/                        Android 资源文件
        │   ├── drawable/               可绘制资源（矢量图等）
        │   │   └── rn_edit_text_material.xml  文本输入框样式
        │   ├── mipmap-hdpi/            App 图标（高密度 ~240dpi）
        │   ├── mipmap-mdpi/            App 图标（中密度 ~160dpi）
        │   ├── mipmap-xhdpi/           App 图标（超高密度 ~320dpi）
        │   ├── mipmap-xxhdpi/          App 图标（超超高密度 ~480dpi）
        │   ├── mipmap-xxxhdpi/         App 图标（超超超高密度 ~640dpi）
        │   └── values/                 值资源
        │       ├── strings.xml         字符串常量（App 名称等）
        │       └── styles.xml          主题和样式定义
        └── java/com/reactnativedemo/   Java/Kotlin 源码
            ├── MainActivity.kt         主 Activity（应用页面宿主）
            └── MainApplication.kt      Application 类（RN 运行时初始化）
```

### 4.2 构建配置文件详解

#### `build.gradle`（根级）

```groovy
buildscript {
    ext {
        // 全局版本变量，供子模块引用
        buildToolsVersion = "36.0.0"       // Android 构建工具版本
        minSdkVersion = 24                  // 最低支持 Android 7.0
        compileSdkVersion = 36              // 编译 SDK 版本（Android 16）
        targetSdkVersion = 36               // 目标 SDK 版本（Android 16）
        ndkVersion = "27.1.12297006"       // NDK 版本（C/C++ 编译）
        kotlinVersion = "2.1.20"           // Kotlin 版本
    }
    repositories {
        google()         // Google Maven 仓库（AGP、AndroidX 等）
        mavenCentral()   // Maven 中央仓库（Kotlin 插件等）
    }
    dependencies {
        // 构建插件（非运行时依赖）
        classpath("com.android.tools.build:gradle")                // AGP 插件
        classpath("com.facebook.react:react-native-gradle-plugin") // RN Gradle 插件
        classpath("org.jetbrains.kotlin:kotlin-gradle-plugin")     // Kotlin 插件
    }
}

apply plugin: "com.facebook.react.rootproject"  // 启用 RN 根项目插件
```

#### `app/build.gradle`（App 级）

- **插件**: `com.android.application`、`org.jetbrains.kotlin.android`、`com.facebook.react`
- **React 配置**: 自动链接、Hermes 引擎、JS Bundle 配置等
- **Android 配置**: 包名 `com.reactnativedemo`、签名配置、构建类型（debug/release）
- **依赖**: `react-android`（RN 核心原生库）、`hermes-android`（Hermes JS 引擎）

#### `gradle.properties`（重要属性）

| 属性 | 值 | 说明 |
|------|------|------|
| `org.gradle.jvmargs` | `-Xmx2048m ...` | Gradle JVM 内存配置 |
| `android.useAndroidX` | `true` | 启用 AndroidX |
| `reactNativeArchitectures` | `armeabi-v7a,arm64-v8a,x86,x86_64` | 支持的 CPU 架构 |
| `newArchEnabled` | `true` | 启用新架构（TurboModules + Fabric） |
| `hermesEnabled` | `true` | 启用 Hermes JS 引擎 |
| `edgeToEdgeEnabled` | `false` | 沉浸式状态栏（关闭） |

### 4.3 Android 原生代码

#### `MainActivity.kt`
- 继承 `ReactActivity`，是应用的页面入口
- 配置组件注册和 JS Bundle 路径

#### `MainApplication.kt`
- 继承 `ReactApplication`，管理 React Native 生命周期
- 初始化 `ReactHost`，配置 TurboModule 和组件注册

#### `AndroidManifest.xml`
- 声明应用包名、权限（INTERNET、SYSTEM_ALERT_WINDOW 等）
- 注册 `MainActivity` 和 `MainApplication`

---

## 五、iOS 原生工程 (`ios/`)

```
ios/
├── Podfile                     CocoaPods 依赖管理配置
├── .xcode.env                  Xcode 环境变量
├── ReactNativeDemo.xcodeproj/  Xcode 项目文件
└── ReactNativeDemo/            Xcode 工程源码目录
```

---

## 六、框架内部文件说明

### `node_modules/react-native/ReactAndroid/cmake-utils/default-app-setup/OnLoad.cpp`

这是 **React Native 框架内部**的 C++ 文件（非项目自有代码），属于新架构的核心组件。

**作用**: Android JNI 入口与 TurboModule/Fabric 组件注册中心。

**核心函数:**

| 函数 | 作用 |
|------|------|
| `JNI_OnLoad` | `.so` 库加载时自动调用，初始化三个注册回调：C++ TurboModule 提供者、Java TurboModule 提供者、Fabric 组件注册器 |
| `registerComponents` | 注册 Fabric 渲染组件（自定义组件 + autolinking 自动链接的第三方组件） |
| `javaModuleProvider` | 查找并实例化 Java/Kotlin 编写的 TurboModule（优先级：自定义 → 核心模块 → 自动链接） |
| `cxxModuleProvider` | 查找并实例化纯 C++ 编写的 TurboModule（通过 autolinking 自动链接） |

**自定义扩展**: 如需添加自定义 C++ 原生模块，可将此文件和对应 CMake 配置复制到 `android/app/src/main/jni/` 目录下进行修改。

---

## 七、开发环境要求汇总

| 工具 | 最低版本 | 说明 |
|------|----------|------|
| Node.js | 22.11.0+ | JavaScript 运行时 |
| JDK | 21+ | Java 开发工具包（Gradle 9.x 要求） |
| Android SDK | API 36 | 编译目标 SDK |
| Android Build Tools | 36.0.0 | Android 构建工具 |
| NDK | 27.1.12297006 | C/C++ 原生开发工具包 |
| Kotlin | 2.1.20 | Kotlin 编译器 |
| Gradle | 9.3.1 | 通过 Wrapper 自动管理 |

---

## 八、常用命令

```bash
# 启动 Metro 开发服务器
npm start

# 运行 Android
npm run android

# 运行 iOS
npm run ios

# 运行测试
npm test

# 代码检查
npm run lint
```
