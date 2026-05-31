# Android and AOSP Agent Skills Notes

面向 Android 系统开发、传统 Android View 项目、设备调试、性能分析和动画链路排查的 Agent Skills 清单。

本文暂不关注 Jetpack Compose。Skill 的内容会持续变化，安装前请检查上游仓库的最新说明，并审阅 `SKILL.md` 和附带脚本。

更新日期：2026-05-31

## 推荐顺序

建议先安装 Google 官方 Skill，再按需要补充社区 Skill 和设备自动化工具。AOSP 环境差异较大，最后再沉淀团队自己的系统开发 Skill。

### 第一批：Google 官方 Skills

官方仓库：[android/skills](https://github.com/android/skills)

| Skill | 用途 | 推荐场景 |
| --- | --- | --- |
| `android-cli` | 让 Agent 正确使用 Android CLI 和相关工作流 | Android 开发基础能力 |
| `perfetto-trace-analysis` | 分析 Perfetto Trace，定位延迟、卡顿和内存问题 | 系统动画、启动、滑动和性能分析 |
| `perfetto-sql` | 编写和解释 Perfetto SQL | 深入分析线程、Binder、Frame Timeline 等数据 |
| `testing-setup` | 为 Android 项目建立测试策略 | 应用和模块改动后的回归验证 |
| `edge-to-edge` | 处理状态栏、导航栏、Insets 和沉浸式布局 | 传统 View 页面、系统栏适配 |
| `r8-analyzer` | 分析 R8 配置、裁剪和包体积问题 | Android App 构建优化 |
| `agp-9-upgrade` | 升级到 Android Gradle Plugin 9 | App 或 SDK 工程升级 |

查看官方完整清单：

```bash
android skills list
```

安装核心 Skill：

```bash
android skills add --skill=android-cli --project=.
android skills add --skill=perfetto-trace-analysis --project=.
android skills add --skill=perfetto-sql --project=.
android skills add --skill=testing-setup --project=.
android skills add --skill=edge-to-edge --project=.
```

### 第二批：社区 Skills

#### Android Testing Skills

仓库：[skydoves/android-testing-skills](https://github.com/skydoves/android-testing-skills)

这套 Skill 覆盖 Android 测试、ADB 和设备端验证。非 Compose 场景优先挑选：

| 分类 | 用途 |
| --- | --- |
| `adb/` | 安装 APK、启动 Activity、输入事件、截图、录屏和端到端验证 |
| `instrumentation/` | AndroidX Test 和设备端测试 |
| `jvm/` | JVM 单元测试 |
| `fundamentals/` | 测试组织、稳定性和常见陷阱 |
| `shell/` | 面向 CI 和设备验证的脚本流程 |

不需要一次性加载整个目录。按项目实际测试面挑选即可。

#### Awesome Android Agent Skills

仓库：[new-silvermoon/awesome-android-agent-skills](https://github.com/new-silvermoon/awesome-android-agent-skills)

这个仓库同时包含 Compose 和非 Compose Skill。对于传统 Android、设备调试和构建优化，优先挑选：

| Skill | 用途 | 推荐度 |
| --- | --- | --- |
| `android-emulator-skill` | 使用 ADB 和 UIAutomator 做语义化导航、构建、日志监控和模拟器生命周期管理 | 高 |
| `gradle-build-performance` | 分析和优化 Gradle、Android 构建耗时 | 高 |
| `android-coroutines` | 检查结构化并发、生命周期、取消和测试模式 | 中 |
| `rxjava-to-coroutines` | 将 RxJava 逐步迁移到 Coroutines 和 Flow | 按需 |
| `accessibility` | 检查可访问性、触摸目标、描述和对比度 | 按需 |
| `android-gradle-logic` | 使用 Convention Plugins、Version Catalogs 和 Composite Builds | App 或 SDK 多模块工程按需 |

其中 `android-emulator-skill` 最值得先试。它附带构建、测试、Logcat 过滤和基于 UI 语义的设备操作脚本，适合补足 `adb-device-verification` 的通用部分。

安装示例：

```bash
npx skills add https://github.com/new-silvermoon/awesome-android-agent-skills --skill android-emulator-skill
npx skills add https://github.com/new-silvermoon/awesome-android-agent-skills --skill gradle-build-performance
npx skills add https://github.com/new-silvermoon/awesome-android-agent-skills --skill android-coroutines
```

#### TestMu AI Testing Skills

仓库：[LambdaTest/agent-skills](https://github.com/LambdaTest/agent-skills)

这是一套通用测试自动化 Skill。传统 Android 开发可按需使用：

| Skill | 用途 | 推荐场景 |
| --- | --- | --- |
| `espresso-skill` | Android View 层 UI 自动化测试 | App 页面和交互回归 |
| `appium-skill` | 跨平台、跨设备端到端测试 | 需要设备矩阵或云真机时 |
| `junit-5-skill` | JVM 单元测试 | Java、Kotlin 工具层和部分业务逻辑 |
| `cicd-pipeline-skill` | 测试流水线配置 | 将 Android 测试接入 CI |

`espresso-skill` 与 `skydoves/android-testing-skills` 有重叠。前者适合集中学习 Espresso，后者更贴近 Android 项目的完整验证流程。通常不必同时全部安装。

#### QA Testing Android

仓库：[vasilyu1983/ai-agents-public](https://github.com/vasilyu1983/ai-agents-public)

| Skill | 用途 | 推荐场景 |
| --- | --- | --- |
| `qa-testing-android` | Espresso、UIAutomator、ADB、Gradle Managed Devices 和测试稳定性建议 | 传统 View 项目、系统 UI 跨应用测试、CI |

这个 Skill 强调 UIAutomator、Gradle Managed Devices、AndroidX Test Orchestrator 和减少 flaky test。与其他测试 Skill 重叠较多，可把它作为测试策略参考，不必默认安装。

安装示例：

```bash
npx skills add https://github.com/vasilyu1983/ai-agents-public --skill qa-testing-android
```

### 第三批：安全审计 Skill

仓库：[DragonJAR/Android-Pentesting-Skill](https://github.com/DragonJAR/Android-Pentesting-Skill)

| Skill | 用途 | 注意事项 |
| --- | --- | --- |
| `Android-Pentesting-Skill` | APKTool、JADX、APKiD、Semgrep、Frida、MASVS 映射和报告生成 | 仅用于你拥有权限的 App、测试设备和实验环境 |

这个 Skill 内容较完整，包含静态分析、动态验证、Frida 脚本、评分和报告模板。它不是日常编码必装项，但适合发布前安全自查或授权安全评估。

安装前应重点检查脚本，并只在隔离环境中使用：

```bash
git clone https://github.com/DragonJAR/Android-Pentesting-Skill.git
```

## 配套工具

下面这些不是 Skill，但很适合和 Agent 搭配使用。

| 工具 | 用途 |
| --- | --- |
| [skydoves/android-skills-mcp](https://github.com/skydoves/android-skills-mcp) | 将 Google 官方 Android Skills 暴露给 Claude Code、Cursor、Codex、Windsurf 等支持 MCP 的客户端 |
| [callstackincubator/agent-device](https://github.com/callstackincubator/agent-device) | 可安装的 `agent-device` Skill 和 CLI。让 Agent 操作 Android 设备，读取 UI、点击、输入、滑动、截图并收集验证证据 |
| `adb` | 安装、启动、输入事件、日志、截图和设备状态检查 |
| `perfetto` | 动画、卡顿、线程调度和渲染性能分析 |
| `dumpsys` | 查看 Window、Surface、Activity、内存和系统服务状态 |
| `atest` | AOSP 局部测试 |
| `repo`、`m`、`mm`、`mmm` | AOSP 仓库管理和构建 |

## 建议自建的 AOSP Skills

以下名称是待自建清单，不是 Google 官方或已验证的社区 Skill。

| 自定义 Skill | 适用范围 |
| --- | --- |
| `aosp-build-and-test` | `lunch`、`m`、`mm`、`mmm`、`atest`、刷机和增量验证 |
| `systemui-animation-debug` | SystemUI 动画入口、状态机、生命周期和中断处理 |
| `shell-transitions-trace` | WindowManager Shell Transitions、Transition Handler 和 `SurfaceControl.Transaction` |
| `surfaceflinger-latency-analysis` | SurfaceFlinger、FrameTimeline、掉帧和合成延迟 |
| `launcher-recents-animation` | Launcher、Quickstep、Recents 手势与动画链路 |
| `adb-device-verification` | 安装、启动、输入、截图、录屏、Logcat 和 `dumpsys` 验收 |

## 初始配置建议

先保持 Skill 数量克制：

```text
Google 官方:
  android-cli
  perfetto-trace-analysis
  perfetto-sql
  testing-setup
  edge-to-edge

社区:
  skydoves/android-testing-skills 中的 adb、instrumentation、shell 部分
  new-silvermoon/awesome-android-agent-skills 中的 android-emulator-skill
  new-silvermoon/awesome-android-agent-skills 中的 gradle-build-performance

工具:
  agent-device
  android-skills-mcp

优先自建:
  aosp-build-and-test
  systemui-animation-debug
```

等实际遇到 Shell Transitions、SurfaceFlinger 或 Launcher Recents 任务时，再补对应 Skill。

## Skill 与项目约束的分工

- `AGENTS.md`：记录每次任务都应该遵守的项目约束，例如构建目标、模块边界、代码风格和禁止执行的命令。
- `SKILL.md`：按需加载复杂流程，例如 Perfetto 分析、SystemUI 动画排查和设备验收。

## 安全检查

安装社区 Skill 前：

1. 阅读 `SKILL.md`。
2. 检查 `scripts/` 和引用的外部命令。
3. 留意网络上传、删除文件、刷机和高权限 `adb` 操作。
4. 先在测试设备和隔离分支中验证。

## 参考资料

- [Android skills overview](https://developer.android.com/tools/agents/android-skills)
- [Google 官方 Android Skills 仓库](https://github.com/android/skills)
- [Android Testing Skills](https://github.com/skydoves/android-testing-skills)
- [Awesome Android Agent Skills](https://github.com/new-silvermoon/awesome-android-agent-skills)
- [TestMu AI Testing Skills](https://github.com/LambdaTest/agent-skills)
- [QA Testing Android](https://github.com/vasilyu1983/ai-agents-public)
- [Android Pentesting Skill](https://github.com/DragonJAR/Android-Pentesting-Skill)
- [Android Skills MCP](https://github.com/skydoves/android-skills-mcp)
- [agent-device](https://github.com/callstackincubator/agent-device)
