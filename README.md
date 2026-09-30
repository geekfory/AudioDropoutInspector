# 音频断流检测工具 / Audio Dropout Inspector

<p align="center">
  <img src="https://img.shields.io/badge/Version-v0.1.5.4-blue.svg" alt="Version">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%20%2F%2011%20(64--bit)-0078D6.svg?logo=windows" alt="Platform">
  <img src="https://img.shields.io/badge/Release-Portable%20Exe%20(Standalone)-success.svg" alt="Release">
  <img src="https://img.shields.io/badge/Architecture-x86__64-orange.svg" alt="Architecture">
  <img src="https://img.shields.io/badge/License-CC%20BY--NC--ND%204.0-lightgrey.svg" alt="License">
</p>

<p align="center">
  <b>面向音频后期、配音制作与游戏音频 QA 的精准音频断流、丢字与静音异常质检工具</b><br>
  <i>A professional inspection tool for detecting silent dropouts, clipped speech, and glitch gaps after DAW/Audition batch processing.</i>
</p>

---

点击前往 [Releases](https://github.com/geekfory/AudioDropoutInspector/releases) 页面下载 (Click on the [Releases](https://github.com/geekfory/AudioDropoutInspector/releases) to download)   [![GitHub release](https://img.shields.io/github/v/release/geekfory/AudioDropoutInspector?label=Download%20Release)](https://github.com/geekfory/AudioDropoutInspector/releases/latest)

---

<img width="2034" height="1419" alt="UI" src="https://github.com/user-attachments/assets/7816964f-c165-4935-946b-a73683db1a1d" />


---
## 目录 / Table of Contents

- [简体中文说明 (Chinese)](#简体中文说明)
  - [1. 软件背景与核心作用](#1-软件背景与核心作用)
  - [2. 核心特性与技术亮点](#2-核心特性与技术亮点)
  - [3. 下载与运行](#3-下载与运行)
  - [4. 界面分区与每个功能/按钮使用指南](#4-界面分区与每个功能按钮使用指南)
    - [4.1 顶部状态栏 (Header Bar)](#41-顶部状态栏-header-bar)
    - [4.2 路径配置区 (Path Configuration)](#42-路径配置区-path-configuration)
    - [4.3 参数微调与控制区 (Control & Parameters)](#43-参数微调与控制区-control--parameters)
    - [4.4 左侧：已扫描文件列表 (Scanned File List)](#44-左侧已扫描文件列表-scanned-file-list)
    - [4.5 右侧：断流异常详情与日志明细 (Detailed Diagnostic View)](#45-右侧断流异常详情与日志明细-detailed-diagnostic-view)
    - [4.6 关于与激活对话框 (About & License Modal)](#46-关于与激活对话框-about--license-modal)
  - [5. 推荐质检工作流](#5-推荐质检工作流)
  - [6. 参数调优参考指南](#6-参数调优参考指南)
  - [7. 常见问题 (FAQ)](#7-常见问题-faq)
  - [8. 软件许可与版权协议 (License)](#8-软件许可与版权协议-license)
- [English Documentation](#english-documentation)
  - [1. Overview & Problem Solved](#1-overview--problem-solved)
  - [2. Key Highlights](#2-key-highlights)
  - [3. Download & Run](#3-download--run)
  - [4. UI Tour & Feature Manual](#4-ui-tour--feature-manual)
  - [5. Recommended Workflow](#5-recommended-workflow)
  - [6. Parameter Tuning Guide](#6-parameter-tuning-guide)
  - [7. FAQ](#7-faq)
  - [8. License & Terms of Use](#8-license--terms-of-use)
- [免责声明 / Disclaimer](#免责声明--disclaimer)

---

# 简体中文说明

## 1. 软件背景与核心作用

在从事配音后期、播客制作、有声书录制、游戏音频（Game Audio）以及影视对白母带批量处理时，音频工程师通常会借助 **Adobe Audition、iZotope RX、Reaper 或各类第三方批处理插件链**（降噪、去齿音、门限切除、压限响度标准化等）对成百上千条音频执行自动化渲染导出。

然而，由于音频引擎缓冲吞吐瓶颈、插件内部丢帧、CPU 调度瞬时延迟或自动化切除算法 Bug，批处理导出的音频中**偶发性地会出现中间某处发音突然静音几十毫秒的“吞字/丢字/跳字”现象**，甚至发生开头吃字或尾部截断。此类瑕疵在宏观波形概览中极难察觉，而依靠人工逐条整轨复听不仅耗时耗力，而且极易因听觉疲劳而产生漏检，导致严重的成品交付质量事故。

**音频断流检测工具 (Audio Dropout Inspector)** 正是为此痛点量身打造：
- 采用 **短时能量包络（RMS/Peak）高精度滑动互相关与双门限检测机制**；
- 逐帧智能对比原始录音（Original）与批处理导出音频（Processed）；
- 毫秒级捕获并定位所有异常静音、断音断流、中间剪接错位及首尾削头截断；
- 提供可视化的诊断日志与毫秒级时间戳一键跳转查听功能。

---

## 2. 核心特性与技术亮点

- **双门限双重特征过滤 (Dual-Threshold Filtering)**：
  同时结合有效语音能量门限 (dBFS) 与瞬时真实峰值 (Peak)，严格过滤自然停顿、换气呼吸声和环境微弱底噪，消除对正常静音的伪报。
- **抗静音修剪自适应时间对齐 (Adaptive Resync & Auto-Alignment)**：
  - 智能感知批处理过程中因“修剪开头静音 (Trim Lead-in)”造成的整轨全局微小时间轴偏差；
  - 独家集成 **最优变点检测 (Change Point Detection) 与正向探针双向融合算法**：当音频中途因剪辑使得时间轴发生跳跃时，在发音停顿处自动重对齐，精确定位字中被缝合错位的起始时刻，绝不发生后续雪崩式虚假误报与末端伪截断。
- **多层级智能文件配对 (Smart Hierarchical Matcher)**：
  - 自动递归扫描多级嵌套子文件夹；
  - 严格遵循跨层级匹配策略（精准相对路径匹配 > 同目录跨格式扩展名转换匹配 > 同名子工程父目录上下文穿透匹配）；
  - 采用自然数字排序（Natural Sort：1, 2, ..., 10 而非 1, 10, 2）。
- **极速虚拟化卡片列表 (High-Performance Virtual List)**：
  针对上千条甚至上万条音频工程设计，毫秒级即时筛选与滚动，零内存暴涨与界面假死。
- **高集成单文件分发 (Portable Green Software)**：
  无需预装 Python、无需配置庞大的运行依赖环境，双击即可运行。

---

## 3. 下载与运行

1. 进入本项目 GitHub 仓库的 **Releases** 页面；
2. 下载最新版本的打包可执行文件（例如 `AudioDropoutInspector_v0.1.5.4.exe`）；
3. 将下载的 `.exe` 放置在电脑上任意无只读权限的目录（如 `D:\Tools\`）；
4. 双击运行即可。

> **系统要求**：Windows 10 / Windows 11 (64位操作系统)。  
> **音频格式支持**：`.wav`, `.mp3`, `.ogg`, `.flac`, `.aiff`, `.m4a`。

---

## 4. 界面分区与每个功能/按钮使用指南

软件主界面采用现代深色卡片扁平化设计，整体划分为四大区域：

```
+-----------------------------------------------------------------------------------+
|  [顶部状态栏] 软件标题与副标题                              [状态胶囊徽标 / 激活按钮]  |
+-----------------------------------------------------------------------------------+
|  [路径配置区]                                                                       |
|    - 原始音频路径:   [ 输入框 (支持拖拽/自动净化) ]                [ 浏览 ]  [ 打开 ]  |
|    - 处理后的路径:   [ 输入框 (支持拖拽/自动净化) ]                [ 浏览 ]  [ 打开 ]  |
|    - 报告日志路径:   [ 智能推导txt文件路径 ]                      [ 浏览 ]  [ 打开 ]  |
+-----------------------------------------------------------------------------------+
|  [参数与控制区]                                                                     |
|    - [参数微调]: 原版RMS门限 | 原版峰值门限 | 静音RMS门限 | 最小判定时长 | 自动对齐     |
|    - [操作按钮]: [ 开始检测比对 (大蓝胶囊) ]        [ 停止 (红) ]   [ 清除 (灰) ]     |
|    - [进度展示]: [ 胶囊进度条 0%~100% ]  当前进度/比对文件名提示                      |
+-----------------------------------------------------------------------------------+
|  [主内容分栏区 (左右 1:1 等宽)]                                                     |
|  [左侧: 已扫描文件列表]                     | [右侧: 断流异常详情与日志明细]          |
|  - 分段按钮: [全部] [异常] [缺失] [正常]    | - 诊断标题与双路径说明                   |
|  - 虚拟卡片行:                             | - 异常明细清单 (异常区间/持续时间/能量)   |
|    [序号] [文件名] -------- [状态胶囊按钮]  | - [时间码一键交互]: 点击复制毫秒时间点   |
+-----------------------------------------------------------------------------------+
```

---

### 4.1 顶部状态栏 (Header Bar)

- **主标题 & 副标题**：显示当前程序名称与设计用途。
- **状态胶囊徽标 (Status Badge)**：
  - **状态指示**：
    - `[验证授权中...]`：启动时后台异步校验授权。
    - `状态: 就绪`（灰蓝底色）：当前环境已就绪，可随时开始比对。
    - `状态: 扫描中`（深蓝底色）：多线程比对正在进行。
    - `状态: 已停止`（深灰底色）：扫描被用户手动终止。
    - `全部正常`（绿色高亮）：扫描结束且全部文件均未发现断流。
    - `异常: X个、缺失: Y个`（鲜红高亮）：明确提示发现瑕疵或文件缺失。
    - `未激活，点此激活`（淡红警示底色）：表示尚未激活。
  - **点击交互**：在就绪或未激活状态下，**直接单击该胶囊徽标**，即可调出软件的「关于与激活」窗口。

---

### 4.2 路径配置区 (Path Configuration)

包含三行规范的输入卡片，均配备快速操作按键：

#### (1) 原始音频路径
- **作用**：指定未经批处理的原始录音文件夹。
- **输入框交互**：
  - 支持手动输入或粘贴系统路径；
  - **支持鼠标直接拖拽**：可从 Windows 文件资源管理器中将文件夹直接拖入输入框，程序会自动净化系统路径中的空格、双引号与花括号；
  - **智能联动推导**：当您填入或拖入此路径并失焦时，程序会自动计算最佳的日志报告保存路径。
- **`[ 浏览 ]` 按钮**：调出系统文件夹选择弹窗选择原始文件夹。
- **`[ 打开 ]` 按钮**：一键在 Windows 资源管理器中弹出打开该文件夹。

#### (2) 处理后的路径
- **作用**：指定经过 Audition 或宿主批处理导出的目标音频文件夹。
- **输入框交互**：同样支持手动输入、粘贴、拖拽文件夹放入与失焦自动推导日志路径。
- **`[ 浏览 ]` 按钮**：弹窗选择批处理后的文件夹。
- **`[ 打开 ]` 按钮**：一键在资源管理器中定位打开该文件夹。

#### (3) 报告日志路径
- **作用**：指定将详细比对结果导出为磁盘 `.txt` 文本报告的绝对路径。
- **智能推导逻辑**：
  - 若原路径和处理后路径有公共父目录，自动存放在公共文件夹下名为 `AudioDropout_Error_Log.txt`；
  - 若跨盘符或无深层公共目录，自动存放在处理后路径的上一级目录；
  - **留空机制**：若清空此输入框，扫描仍然正常进行并将报告保存在内存中，不会在磁盘产生多余垃圾文件。
- **`[ 浏览 ]` 按钮**：弹出保存文件对话框，选择存放路径。
- **`[ 打开 ]` 按钮**：
  - 强制校验后缀名必须为 `.txt`；
  - **内存自愈写出**：若此前扫描时留空（或文件尚未生成），此时只要内存中存在检测报告，点击此按钮会自动先将内存中的完整报告写入该文件，然后立即以系统默认文本程序（如 Windows 记事本、VS Code、Sublime）打开。

---

### 4.3 参数微调与控制区 (Control & Parameters)

#### 核心检测门限参数表

| 参数项名称 | 界面默认值 | 作用与判定原理 | 建议调整场景 |
| :--- | :---: | :--- | :--- |
| **原版RMS门限(dB)** | `-26.0` | 原版音频短时有效语音能量下限。低于此值视为呼吸、自然停顿或环境底噪，不纳入异常对比。 | 若录音人声音量偏小或咬字较轻，可下调至 `-30.0`；若环境噪声较大，可上调至 `-22.0`。 |
| **原版峰值门限** | `0.15` | 结合 RMS 能量双重过滤。原版瞬时波形绝对值峰值需达到该值，才视为有声发音。 | 过滤细微喷麦、吸气声与摩擦杂音。通常保持在 `0.10 ~ 0.20`。 |
| **静音RMS门限(dB)** | `-45.0` | 处理后音频被判定为“异常静音”的上限阈值。低于此值视为进入静音状态。 | 若批处理插件保留了明显底噪，可适当提高到 `-40.0`；若是干净静音，保持 `-45.0`。 |
| **最小判定时长(ms)** | `40` | 异常静音需连续持续达到此毫秒数才报警，过滤极微小的瞬态切音。 | 默认 `40ms` 契合人耳对断字的感知阈值。*(注：强语音信号丢失 >=25ms 会自动捕获)* |
| **自动时间对齐** | `开启(勾选)` | 开启基于互相关的自适应重对齐。自动容忍开头静音切除，并在中途剪接点自动纠正时间轴。 | **强烈建议始终保持开启**；仅在明确确定原版与处理版严格样点锁相时可关闭。 |

> **悬停气泡提示 (Tooltip)**：将鼠标悬停在任意参数标签或输入框上，即可查看带有详细算法说明的高质感气泡提示。

#### 控制操作按钮
- **`[ 开始检测比对 ]`（宽大蓝色高亮胶囊）**：
  - 校验路径与门限有效性；
  - 消耗授权点数并进行底层安全校验；
  - 启动多线程后台异步扫描，主界面保持丝滑响应。
- **`[ 停止 ]`（红色胶囊）**：
  - 扫描进行中生效；
  - 瞬间中断后台分析线程并清空界面排队消息，即时刹车，无需卡顿等待。
- **`[ 清除 ]`（灰色胶囊）**：
  - 清空当前内存中暂存的所有分析数据、列表卡片、日志详情；
  - 恢复进度条、筛选计数器与初始状态徽标；
  - 自动清空路径并唤醒输入框的默认占位提示（Placeholder），将软件一键还原到初始启动状态。

---

### 4.4 左侧：已扫描文件列表 (Scanned File List)

#### (1) 筛选分段按钮 (Segmented Filter)
提供四个带有实时数量统计的胶囊标签：
- `全部 (X个)`：展示所有被检索到的文件；
- `异常 (X个)`：仅筛选存在丢字、静音或剪接瑕疵的文件；
- `缺失 (X个)`：仅筛选在目标文件夹中未找到对应导出的文件；
- `正常 (X个)`：仅筛选完全契合通过的文件。

**进阶交互手势**：
- **单击任意分类**：瞬间（耗时 < 1ms）切换列表呈现内容，且**保留您当前选中的高亮文件及其右侧详情**；
- **快速双击「全部」**：清空左侧单项的高亮选中状态，并在右侧文本框中**还原展示整个工程的全局完整汇总简报**。

#### (2) 虚拟卡片行 (VirtualCardRow)
每一行均呈现为圆角卡片，包含三部分：
1. **独立序号徽标**：左侧精致深色圆角小方块，标示文件扫描次序；
2. **文件名**：中间展示音频文件相对路径名称，过长自动省略不挤占按钮；
3. **状态胶囊 Badge 按钮**：
   - 绿色 `正常`：未发现断流；
   - 红色 `异常 (N)`：明确标明检测到的断流/剪切处数量；
   - 深灰 `缺失`：未在输出目录找到匹配文件。

**卡片三大特色操作**：
- **单击卡片行主体**：当前行呈现科技深蓝背景与亮天蓝描边，右侧立即展开该文件的详细断流时间区间；
- **单击右侧状态胶囊 Badge**：
  - **一键双路径成对复制**：直接将原文件与比对文件的绝对物理路径以带引号的格式（`"D:\Orig\01.wav" "D:\Proc\01.wav"`）写入系统剪贴板！方便直接在 DAW（如 Audition 多轨工程）、播放器或对比工具中粘贴载入；
  - **微动画反馈**：点击后胶囊文字瞬间变为 `已复制路径`，宽度随文字自适应撑宽，1.2 秒后自动恢复原状；
  - 若文件状态为“缺失”，则自动只复制原始文件的绝对路径。

---

### 4.5 右侧：断流异常详情与日志明细 (Detailed Diagnostic View)

右侧为大面积的富文本诊断报告区：

#### (1) 全局扫描简报模式
在扫描过程中实时流式输出检测日志；扫描结束后若双击「全部」筛选按钮，此处将呈现整个工程的完整总览统计：
```
==================================================
扫描结束！共检查了 128 个文件，125 个文件正常。
【处理异常】: 共有 3 个文件存在断流或异常！
详细报告已生成至: D:\AudioProject\AudioDropout_Error_Log.txt
```

#### (2) 单文件诊断报告模式
单击左侧任意文件卡片，右侧立即切换呈现该文件的精准诊断：
- 文件的完整原始路径与处理后绝对路径；
- **异常静音区间**：精准显示原版对应时间码与处理版时间码（例如：`原版约 0:17.550 - 0:18.230 (处理版约 0:17.230 - 0:17.910, 持续约 680ms, 原版峰值 0.72, RMS -15.4dB)`）；
- **音频剪接/位错警报**：精确指出发音剪切切口发生在何时（例如：`约在原版 0:42.150 处发音产生剪接位错，处理版相对原版提前约 0:00.320`）；
- **时间对齐重同步标记**：标明在何处停顿空白点重新咬合对齐；
- **尾部截断预警**：提示处理后音频是否在末尾存在未发完即被裁断的声音。

#### (3) 时间码一键交互复制 (Interactive Clickable Timecodes)
- 文本报告中出现的所有分秒毫秒时间码（形如 `0:23.263`、`1:05.120`）均会自动添加**天蓝色下划线交互标识**；
- **悬停跟随提示 (Tooltip)**：鼠标悬停在时间码上时，光标自动变为手型，并弹出深色浮动提示框 `点击快速复制时间码: 0:23.263`；
- **单击一键复制**：直接将该纯文本时间戳复制入剪贴板，同时时间码**闪烁高亮绿色反馈**，方便您直接切换到 Adobe Audition、Premiere 或播放器的时间轴输入框中粘贴（Ctrl+V），秒级定位问题波形！

---

### 4.6 关于与激活对话框 (About & License Modal)

- **呼出方式**：在右上角胶囊显示为 `未激活，点此激活` 或 `状态: 就绪` 时，单击该徽标即可弹出。
- **对话框组件**：
  - **软件基础信息**：展示制作人声明与版本信息；
  - **当前软件与授权状态**：展示当前软件名、版本号以及实时在线/离线授权有效天数或剩余可用计次；
  - **本地机器码 (Machine ID)**：
    - 读取 Windows 底层硬件指纹，断网、插拔网卡绝不漂移；
    - 附带 `[ 复制 ]` 按钮，一键复制包含软件名与 25 位纯数字机器码的信息；
  - **激活码输入框**：
    - 附带 `[ 粘贴授权码 ]` 快捷按钮；
    - 点击 `[ 立即注册/激活 ]` 即可完成激活。【软件自带100次免费试用次数，直接点击此按钮可使用】

---

## 5. 推荐质检工作流

1. **导出准备**：在 Audition 或 DAW 中完成整批音频的批量导出渲染；
2. **载入路径**：
   - 打开本检测工具；
   - 将装有原始录音的文件夹直接拖入「原始音频路径」；
   - 将批处理导出的目标文件夹直接拖入「处理后的路径」；
3. **确认门限**：保持默认门限参数不变，确保「自动时间对齐」处于勾选状态；
4. **一键开始**：点击大蓝按钮 `[ 开始检测比对 ]`；
5. **快速排查**：
   - 观察右上角状态徽标变为绿色（通过）或红色（发现异常）；
   - 若发现异常，点击左侧分段按钮 `[ 异常 (N个) ]`；
   - 单击异常卡片，在右侧查看断流时间码；
   - 单击卡片右侧 Badge 按钮一键复制双路径；或者直接**点击右侧下划线时间码**复制时间戳；
   - 切换至 Audition，粘贴时间码精确定位复听并补录修复。

---

## 6. 参数调优参考指南

- **场景 A：常规语音播客 / 广播剧配音 (对白饱满)**
  - 保持全默认参数即可（原版 RMS `-26.0dB`，峰值 `0.15`，静音 `-45.0dB`，持续 `40ms`）。
- **场景 B：低语轻声 / ASMR / 语气非常微弱的对白**
  - **调整**：将「原版RMS门限」调低至 `-32.0dB`，并将「原版峰值门限」调低至 `0.08`；
  - **效果**：防止轻微的气息尾音被判定为背景底噪而忽略对比。
- **场景 C：降噪插件底噪残留较大 (处理后静音处有嘶嘶声)**
  - **调整**：将「静音RMS门限」由 `-45.0dB` 放宽调整至 `-38.0dB`；
  - **效果**：即便处理后的音频在断流处保留了 `-40dB` 的嘶嘶底噪，依然能被准确定位为断流静音。

---

## 7. 常见问题 (FAQ)

#### Q1: 处理后的音频转换了格式（如 WAV 转成了 MP3），软件能对比吗？
**A**: **完全支持**。软件内置分层匹配引擎，只要主文件名相同（例如 `voice_01.wav` 与 `voice_01.mp3`），即会自动优先跨扩展名配对分析。

#### Q2: 批处理插件切掉了开头的空白静音，会不会导致整首歌报异常？
**A**: **不会**。软件默认启用了「自动时间对齐」，能通过互相关包络自动探测开头被切除的时长，毫秒级校正时间基准。

#### Q3: 为什么有的文件只断了 25ms 也会报警？
**A**: 软件针对能量强劲的有效发音（峰值 $\ge 0.35$ 或 RMS $\ge -20\text{dB}$）内置了增强保护机制。强发音中途哪怕缺失 25ms 也会严重影响听感（听感为明显卡顿或爆字），因此会被严格捕获并标明。

---

## 8. 软件许可与版权协议 (License)

本项目发布的可执行二进制程序受 **知识共享 署名-非商业性使用-禁止演绎 4.0 国际许可协议 (CC BY-NC-ND 4.0)** 保护，并附带以下严格的专有版权与使用限制条款：

1. **保留出处 (Attribution)**：
   - 任何个人或机构在允许的非商业范围内传播、推荐或引用本软件时，**必须明确标注原作者（Fory）姓名及本项目的 GitHub 官方原始发布地址**。
2. **严格禁止商业使用 (Non-Commercial Use Only)**：
   - 本软件仅供个人学习、研究、交流以及非营利性工程复核使用。
   - **严禁任何个人、团队、电商卖家或商业机构将本软件（包括可执行文件、文档、界面资产、算法逻辑等）用于任何商业盈利目的**，包括但不限于：单独销售、付费下载、捆绑销售、作为收费插件/收费课程附赠工具、商业软件转售或任何形式的有偿分发。
3. **严格禁止反编译出源码与逆向工程 (Strict Prohibition of Decompilation & Reverse Engineering)**：
   - **本软件当前仅以编译打包后的独立二进制程序（.exe）形式发布，未公开任何源代码。作者保留全部源代码的著作权与专有知识产权**；
   - **严禁任何个人或实体使用反编译器（如 pyinstxtractor、uncompyle6、decompyle++、IDA Pro、Ghidra、x64dbg、dnSpy 等工具）对本程序的二进制文件、字节码、资源包或内存数据进行反编译、反汇编、反混淆、内存 Dump 或逆向工程提取原始 Python 代码与算法**；
   - **严禁尝试破解、绕过、篡改或移除本软件的机器码硬件绑定机制、RSA-2048 数字签名验签逻辑、动态配额完整性校验（HMAC-SHA256）及任何内置的安全防护机制**；
   - **严禁基于逆向或修改后的代码二次打包、分发任何“破解版”、“绿色去授权版”、“修改版”或衍生软件产品。任何反编译、逆向提取或未经授权的二次分发行为均构成对作者著作权的严重侵权，作者保留依法追究法律责任的全部权利**。

---
---

# English Documentation

## 1. Overview & Problem Solved

In audio post-production, podcast editing, audiobook mastering, and game audio localization, audio engineers frequently process hundreds or thousands of dialogue clips in bulk using **Adobe Audition Batch Process, iZotope RX, or complex DAW master chains** (denoising, gating, limiting, normalization, etc.).

However, due to DAW buffer underruns, plugin rendering glitches, CPU scheduling hiccups, or automated trimming algorithm bugs, **speech dropouts—where 30–100ms of spoken syllables suddenly drop to complete silence—can occasionally plague rendered files**. These micro-dropouts are nearly invisible on macro waveforms and extremely tedious to catch through manual QC listening, often causing embarrassing client rejections.

**Audio Dropout Inspector** is designed to solve this quality control headache:
- Utilizes **dual-threshold RMS & Peak short-time energy tracking with cross-correlation time-alignment**;
- Frame-by-frame compares original recordings against batch-processed renders;
- Detects silent dropouts, syllable clippings, mid-speech splice phase slips, and tail truncations;
- Provides instant diagnostic reports with **one-click clickable timecode copying** for rapid DAW verification.

---

## 2. Key Highlights

- **Dual-Threshold Acoustic Energy Filtering**:
  Evaluates both RMS speech floor (dBFS) and instant true peak, reliably filtering out natural speech pauses, breaths, and ambient background room tone.
- **Adaptive Lead-in Trimming Resync & Auto-Alignment**:
  - Automatically identifies lead-in offset caused by automated "Trim Silence" operations.
  - Implements **Optimal Change Point Detection & Forward Probe algorithms**: when middle edits shift the timeline, it automatically locks back on during subsequent pauses, tracing exact splice joints while preventing cascading false positives.
- **Hierarchical Smart File Matcher**:
  Recursively traverses nested subfolders, following an intuitive matching priority (Strict Relative Path > Cross-Format Extensions like WAV to MP3 > Subdirectory Context Matching) with natural alphanumeric sorting.
- **High-Performance Virtual Card List**:
  Smoothly navigates through thousands of files without UI lags or memory bloat.
- **Single Portable Executable**:
  No Python installation or complex third-party dependencies required. Ready to run out of the box on Windows.

---

## 3. Download & Run

1. Head over to the **Releases** page of this repository.
2. Download the latest pre-compiled binary (e.g., `AudioDropoutInspector_v0.1.5.4.exe`).
3. Place the `.exe` in any accessible local folder (e.g., `D:\Tools\`).
4. Double-click to launch.

> **System Requirements**: Windows 10 / Windows 11 (64-bit).  
> **Supported Audio Formats**: `.wav`, `.mp3`, `.ogg`, `.flac`, `.aiff`, `.m4a`.

---

## 4. UI Tour & Feature Manual

### 4.1 Header Bar
- **Title & Subtitle**: Overview of tool purpose.
- **Status Badge**:
  - Displays statuses: `Ready`, `Scanning...`, `Stopped`, `All Normal` (Green), `Issues: X` (Red), or `Unactivated`.
  - **Click to Register**: Clicking this badge in `Ready` or `Unactivated` mode opens the About & License dialog.

### 4.2 Path Configuration
- **Original Audio Path**:
  - Drag & drop folders directly from Windows Explorer into the entry box (path quotes and brackets are sanitized automatically).
  - `[ Browse ]`: Open folder picker.
  - `[ Open ]`: Open the directory in Windows Explorer.
- **Processed Audio Path**:
  - Path for exported batch files. Supports drag & drop and auto log path derivation.
  - `[ Browse ]` / `[ Open ]` buttons available.
- **Report Log Path**:
  - Automatically suggests a common parent path named `AudioDropout_Error_Log.txt`.
  - If left empty, results are kept in memory without creating disk clutter.
  - `[ Open ]`: Verifies `.txt` extension, flushes memory report to disk if needed, and opens it using the default system text editor.

### 4.3 Control & Parameters

| Parameter | Default | Function & Principle |
| :--- | :---: | :--- |
| **Orig RMS Floor (dB)** | `-26.0` | RMS threshold for valid speech in the original audio. Levels below this are treated as pauses/breaths. |
| **Orig Peak Floor** | `0.15` | Minimum true peak threshold to weed out mic bumps and low-level noise. |
| **Silence RMS (dB)** | `-45.0` | Threshold in processed audio below which frames are flagged as anomalous silence. |
| **Min Duration (ms)** | `40` | Minimum duration required to trigger a dropout flag (strong speech loss $\ge 25\text{ms}$ is also captured). |
| **Auto Time-Align** | `Enabled` | Compensates for lead-in silence trimming and mid-stream splice time shifts. |

#### Control Buttons
- **`[ Start Inspection ]` (Large Blue Capsule)**: Validates settings and launches inspection in a background worker thread.
- **`[ Stop ]` (Red Capsule)**: Instantly interrupts processing and flushes queued tasks.
- **`[ Clear ]` (Gray Capsule)**: Clears all cached memory, UI cards, input entries, and resets controls to initial launch state.

### 4.4 Scanned File List (Left Pane)
- **Segmented Filter Buttons**: Switch between `All (X)`, `Issues (X)`, `Missing (X)`, and `Normal (X)`.
  - *Single click*: Instant (<1ms) filter update keeping the active item selected.
  - *Double click on "All"*: Restores the right-side pane to the global summary report.
- **Virtual Card Rows**:
  - Displays index badge, filename, and status pill badge (`Normal`, `Issues (N)`, or `Missing`).
  - **Clicking the status badge**: Copies both original and processed absolute paths formatted with quotes (`"D:\Orig\a.wav" "D:\Proc\a.wav"`) to the clipboard, providing visual confirmation `Path Copied`.
  - **Clicking the card body**: Highlights the item and shows detailed diagnostic findings in the right pane.

### 4.5 Detailed Diagnostic View (Right Pane)
- **Comprehensive Log**: Lists exact timecodes, dropout durations, peak/RMS metrics, and splice phase slip notices.
- **Interactive Timecodes**:
  - Every timecode (e.g., `0:23.263`) is rendered with a cyan underline.
  - **Hovering**: Displays a tooltip: `Click to copy timecode: 0:23.263`.
  - **Clicking**: Instantly copies the timestamp to the clipboard with a green flash indicator, ready to be pasted directly into Adobe Audition / DAW transport bars.

### 4.6 About & License Modal
- Displays machine hardware GUID (stable across reboots and network card changes) with a dedicated `[ Copy ]` button.
- Validates RSA-2048 cryptographically signed activation keys.

---

## 5. Recommended Workflow

1. Perform batch rendering in Adobe Audition / your DAW.
2. Drag the source folder into **Original Audio Path**, and render folder into **Processed Audio Path**.
3. Keep default threshold parameters and ensure **Auto Time-Align** is checked.
4. Click **`[ Start Inspection ]`**.
5. Once complete, filter by **`[ Issues ]`**, click an abnormal file, and inspect the timestamps.
6. Click the interactive timecodes to copy timestamps, paste them into your DAW, review the spot, and correct any dropouts.

---

## 6. Parameter Tuning Guide

- **Standard Dialogue / Audiobooks**: Keep defaults (`-26.0dB` RMS, `0.15` Peak, `-45.0dB` Silence, `40ms`).
- **Whispering / Soft Speech**: Lower **Orig RMS Floor** to `-32.0dB` and **Orig Peak** to `0.08`.
- **Noisy Processing (High Hiss Floor)**: Increase **Silence RMS Floor** from `-45.0dB` to `-38.0dB`.

---

## 7. FAQ

#### Q: Does it support comparing files converted across different formats (e.g., WAV to MP3)?
**A**: **Yes**. The smart matching engine matches by base filename across supported formats (`.wav`, `.mp3`, `.ogg`, `.flac`, `.aiff`, `.m4a`).

#### Q: Will trimming lead-in silence cause false alarms?
**A**: **No**. Auto Time-Align measures envelope cross-correlation to align global timebases seamlessly.

---

## 8. License & Terms of Use

This software binary distribution is protected under the **Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License (CC BY-NC-ND 4.0)** with additional strict proprietary restrictions:

1. **Attribution**:
   - Any authorized non-commercial sharing, redistribution, or reference to this software must clearly credit the original author (**Fory**) and link to the official project repository.
2. **Strictly Non-Commercial**:
   - This software is provided solely for personal, research, evaluation, and non-profit audio engineering quality check purposes.
   - **Commercial exploitation of any kind is strictly prohibited**, including but not limited to selling, monetizing downloads, bundling into commercial toolkits, or offering as paid add-ons.
3. **Strict Prohibition of Reverse Engineering & Decompilation**:
   - **This project is currently distributed exclusively as a standalone compiled executable binary (.exe). No source code is publicly released, and the author explicitly reserves all proprietary rights, copyrights, and trade secrets in the source code and underlying algorithms.**
   - **Any attempt to decompile, disassemble, deobfuscate, reverse engineer, dump memory contents, or extract Python source code and bytecode from the executable binary (using tools such as pyinstxtractor, uncompyle6, decompyle++, IDA Pro, Ghidra, x64dbg, dnSpy, etc.) is strictly and unequivocally forbidden.**
   - **Attempting to bypass, tamper with, patch, or remove hardware machine ID binding, RSA-2048 verification, dynamic token checks, or any anti-tampering protection is strictly prohibited.**
   - **Creating, distributing, or repacking cracked, modified, or derivative versions of this software constitutes willful copyright infringement. The author reserves all legal rights and remedies under applicable copyright laws.**

---

## 免责声明 / Disclaimer

- 本软件诊断报告及分析结果仅供工程复核与参考，不构成法定质检凭证。
- 本工具为独立开发的音频分析软件，与 Adobe Systems Incorporated 或 Audition 官方无任何附属或背书关系。
- This software and diagnostic outputs are provided for engineering quality check purposes only. Not affiliated with or endorsed by Adobe Systems Inc.

