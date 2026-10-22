# OCR 小软件

<img src="assets/icon.png" width="88" align="right" alt="应用图标">

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.9%20~%203.13-blue.svg)](https://www.python.org/)
[![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11-0078D4.svg)](#前置要求)
[![Release](https://img.shields.io/github/v/release/qiwanyang/offline-ocr)](https://github.com/qiwanyang/offline-ocr/releases/latest)

> 一个本地离线运行的 OCR 图片识别小软件，专注「垂直场景」，从普通图片识别起步。

**不想装 Python 环境？** 直接下载 [Windows 免安装包](https://github.com/qiwanyang/offline-ocr/releases/latest)（319 MB），
解压双击 `OCR小软件.exe` 就能用 —— 模型已内置，全程离线。

## 简介

把图片里的文字提取出来，拖图、点击、出文字、可复制可导出。全程本地运行，数据不出本机。

内置三个可切换的功能模块：

| 模式 | 做什么 | 适合 |
| --- | --- | --- |
| **普通识别** | 选图 / 拖图，识别全部文字 | 截图、扫描件、照片里的文字 |
| **即时识别** | 按 `Ctrl+Shift+A` 框选屏幕，松手立刻识别 | 网页、聊天记录、报错弹窗里的字 |
| **表格识别** | 识别表格结构并导出 Excel | 论文表格、对账单、报表截图 |

> 模式是「注册表驱动」的：新增模块只要在 `src/main.py` 的 `MODES` 里加一项、补对应处理方法，
> 工具栏会自动出现入口，不需要改界面布局。

- **技术栈**：Python + PySide6 + PaddleOCR
- **引擎**：PaddleOCR（PP-OCRv5 mobile 轻量模型）+ SLANet_plus（表格结构）
- **平台**：Windows

## 功能

- [x] 普通图片 / 截图文字识别
- [x] 支持 17 种图片格式（png / jpg / jpeg / jfif / bmp / webp / tif / tiff / gif / ico / tga / ppm / pgm / pbm / pnm / xbm / xpm）
- [x] 命令行识别：图片 → 文本
- [x] 可视化：生成标注图 / HTML 报告页
- [x] GUI 图形界面（拖拽/选图、标注框、复制、导出）
- [x] 图片滚轮缩放 / 鼠标拖动平移 / 适应窗口 / 原始大小
- [x] 拖拽图片文件进窗口（带高亮反馈）
- [x] 原图 ↔ 标注图一键切换
- [x] 识别进度反馈（进度条 + 忙碌光标，不再「以为卡死」）
- [x] 模型预下载脚本（可离线使用，首次无需联网）
- [x] 边界处理：中文路径 / 超大图自动压缩 / 损坏文件友好报错
- [x] 应用图标（由脚本生成，可复现）
- [x] 打包为 Windows 可执行文件（PyInstaller，内置模型、开箱即用、完全离线）
- [x] 快捷键（Ctrl+O / Ctrl+Enter / Ctrl+S …），可在「设置」中**重新绑定**，保存即生效
- [x] 三模式切换：普通识别 / 即时识别（截图）/ 表格识别
- [x] 截图即时识别：框选屏幕区域，松手自动识别（不落临时文件）
- [x] 表格识别：识别表格结构，可复制为制表符文本、导出 `.xlsx`
- [x] 记住窗口大小、上次目录与上次模式（可在设置里关闭）
- [ ] 批量识别（规划中，已有模式机制可直接挂载）
- [ ] 多语言（中/英/繁体，需额外模型）

## 界面预览

| 普通识别 | 表格识别 |
| :---: | :---: |
| <img src="docs/screenshot_main.png" width="520" alt="普通识别"> | <img src="docs/screenshot_table.png" width="520" alt="表格识别"> |
| 拖入图片 → 识别 → 右侧文字 / 左侧识别框 | 识别表格结构，右侧预览，可导出 Excel |

| 设置面板 |
| :---: |
| <img src="docs/screenshot_settings.png" width="360" alt="设置面板"> |
| 快捷键可改键 / 解绑 / 恢复默认；可开关「记住窗口与目录」 |

## 支持的图片格式

支持 **17 种**常见图片格式，**不限 PNG**：

| 类型 | 扩展名 |
| --- | --- |
| 最常用 | `.png` `.jpg` `.jpeg` `.jfif` `.bmp` `.webp` |
| 扫描 / 传真件 | `.tif` `.tiff` |
| 其他 | `.gif` `.ico` `.tga` `.ppm` `.pgm` `.pbm` `.pnm` `.xbm` `.xpm` |

设计上有一点值得说明：**「载入显示」与「文字识别」共用同一套解码逻辑（PIL）**。
这样不会出现「界面能显示、识别引擎却打不开」的格式错位（例如 SVG、PDF —— Qt 能渲染，OCR 无法识别）。
一句话：**界面上能看到的图，一定能识别。**

- 选择文件时若遇到不常见格式，对话框里的「所有文件 (*)」可强制尝试
- 拖拽时若拖入的文件都不在支持列表内，会明确提示，而不是静默失败
- 格式清单的唯一来源是 `src/ocr_engine.py` 的 `SUPPORTED_EXTS`，文件选择框的过滤器由它自动生成，不会两处不一致

> 注：`.ico` 受格式规范限制最大 256×256，更大的图会被降采样；`.gif` / `.tiff` 若为多帧 / 多页，只读取第一帧。

## 获取代码

```bash
git clone https://github.com/qiwanyang/offline-ocr.git
cd offline-ocr
```

不想装 Python 环境的话，可以直接到 [Releases](https://github.com/qiwanyang/offline-ocr/releases)
下载打包好的 Windows 压缩包，解压后双击 `OCR小软件.exe` 即可运行（模型已内置，全程离线）。

## 从零搭建（环境可复现）

### 前置要求

- Windows 10/11（64 位）
- Python 3.9 ~ 3.13（64 位）

### 方式一：一键脚本（推荐）

双击项目根目录的 `setup.bat`，自动完成环境搭建。

### 方式二：手动搭建

```bat
REM 1. 创建虚拟环境
python -m venv .venv

REM 2. 安装 PaddlePaddle（必须用官方镜像，PyPI 无最新 CPU 版）
.venv\Scripts\python.exe -m pip install paddlepaddle==3.2.1 -i https://www.paddlepaddle.org.cn/packages/stable/cpu/

REM 3. 补装遗漏依赖（PaddlePaddle 3.2.1 的坑）
.venv\Scripts\python.exe -m pip install "setuptools>=84.0.0" "chardet>=7.6.0"

REM 4. 安装其余依赖
.venv\Scripts\python.exe -m pip install paddleocr==3.2.0 "PySide6>=6.6" "opencv-python>=4.8"
```

> 如需完全一致的环境，可用 `requirements.lock`（pip freeze 全量锁定）：
> `.venv\Scripts\python.exe -m pip install -r requirements.lock`

### 预下载 OCR 模型（可选，推荐）

```bat
.venv\Scripts\python.exe src\download_models.py
```

把模型提前缓存到 `~/.paddlex/official_models/`（约 20MB），之后**完全离线**也能识别。
不预下载也可以 —— 首次识别时会自动下载，这个脚本只是把这步提前。
`setup.bat` 已自动包含此步。

## 使用

### 1. 图形界面（推荐）

```bat
.venv\Scripts\python.exe src\main.py
```

把图片拖进窗口 → 点「开始识别」→ 右侧显示文字，左侧图片上画出识别框。
支持一键复制结果、导出 TXT / Markdown。

**三种模式：**

- **普通识别**：选图 / 拖图 → 「开始识别」→ 纯文本结果。
- **即时识别**：点「截取屏幕」或按 `Ctrl+Shift+A` → 主窗口自动隐藏 → 拖拽框选 → 松手即开始识别。
  选区小于 8×8 视为误点，按 `Esc` 或右键可取消。适合抓网页、聊天记录、报错框里的字。
- **表格识别**：选图 → 「识别表格」→ 右侧以表格形式预览 → 「导出文件」存成 `.xlsx`，
  或「复制结果」得到制表符分隔文本（可直接粘进 Excel）。

**界面操作：**

| 操作 | 说明 |
| --- | --- |
| 左上角「模式」 | 切换 普通识别 / 即时识别 / 表格识别；按钮与结果区会跟着变 |
| 拖入图片 | 把图片文件从文件夹拖进窗口即可载入（悬停时边框变绿提示可松手），也可点「选择图片」 |
| 滚轮 | 缩放图片（5% ~ 800%） |
| 按住鼠标拖动 | 放大后可按住左键拖动画面看细节（光标变手型），不用去拖滚动条 |
| 「显示识别框」 | 勾选显示标注图（绿框 + 文字），取消显示原图，切换时保留缩放 |
| 「适应窗口」/「原始大小」 | 一键切换查看比例，右侧实时显示当前百分比 |
| 识别中 | 按钮变「识别中…」，状态栏出现进度条，光标变忙碌态 |
| 右上角「设置」 | 打开设置面板，查看并重新绑定快捷键（等价于按 F1） |

**快捷键：**

| 按键 | 功能 |
| --- | --- |
| `Ctrl+O` | 选择图片（即时识别模式下为「截取屏幕」） |
| `Ctrl+Shift+A` | 截取屏幕（任意模式可用，会自动切到即时识别） |
| `Ctrl+Enter` | 开始识别 / 识别表格（主键盘 / 小键盘回车都支持） |
| `Ctrl+Shift+C` | 复制识别结果（`Ctrl+C` 留给结果框自身复制） |
| `Ctrl+S` | 导出为 TXT / Markdown / Excel |
| `Ctrl+0` | 适应窗口 |
| `Ctrl+1` | 原始大小（1:1） |
| `Ctrl+B` | 切换原图 / 识别框图 |
| `F1` | 打开设置面板 |

> 快捷键在整个窗口内生效；未载入图片或无识别结果时不会触发（与按钮可用状态一致）。
> 快捷键清单的唯一来源是 `src/main.py` 的 `DEFAULT_SHORTCUTS`，默认值与绑定都由它驱动，
> 并由 `test/test_shortcuts.py` 守护「定义 / 绑定 / 面板 / 持久化」四者一致。

### 自定义快捷键

点右上角「设置」（或按 F1）→ 点进对应功能的输入框 → **直接按下新的组合键** → 「保存」，立即生效，无需重启。

- **一个功能可填多个组合键**（如 `Ctrl+Enter, Ctrl+Return`），按任一都触发；点右侧「清除」可解除绑定
- **冲突校验**：同一个组合键不能绑给两个功能，保存时会给提示
- **裸键校验**：不允许只绑一个普通字母/数字（如 `S`）——否则在结果框里打字就会误触发；`F1~F12` 这类功能键可以单独使用
- **恢复默认**：面板左下角一键还原，保存后生效
- 配置保存在本机（Windows 为注册表 `HKCU\Software\QIWANYANG\ocrapp`），不随程序卸载自动清除，也不影响其他程序

> 识别结果只输出图片中的文字本身，不附带置信度等额外信息。

### 表格识别说明

表格识别用 **SLANet_plus** 出结构（单元格坐标 + HTML），文字仍由同一套轻量 OCR 逐格识别，
不额外加载版面检测 / 去扭曲 / server 档模型，因此只多了约 8MB 模型。

- 导出的 `.xlsx` 带边框、首行加粗、列宽按内容估算
- 合并单元格（rowspan / colspan）**不还原样式**，被覆盖的位置留空，保证文字落在正确的行列
- 一张图里有多个表格时会依次拼接
- 只适合「有框线的规整表格」；无线表、跨页表、嵌套表结构可能不准

> 表格链路会牵入 `scikit-learn` / `scipy`（paddlex 内部依赖检查与 KMeans），
> 这是打包体积比只做文字识别大约多 150MB 的原因。

### 2. 命令行识别

```bat
.venv\Scripts\python.exe test\test_ocr.py test\test_demo.png
```

`test/` 目录下放着两张示例图，可以直接拿来试：

| 文件 | 内容 |
| --- | --- |
| `test_image.png` | 基础测试图（2 行中文 + 数字） |
| `test_demo.png` | 混合内容测试图（7 行：标题 / 中文正文 / 数字 / 邮箱 / 中英混合 ID） |

输出（只输出文本本身，不带置信度等附加信息）：

```text
产品周报 · 第二季度
本季度累计新增用户 12,480 人，环比增长 23.5%。
其中移动端占比 68%，PC 端占比 32%。
```

### 3. 生成标注图

```bat
.venv\Scripts\python.exe test\visualize_ocr.py test\test_demo.png
```

生成 `result_vis.jpg`：绿框标出文字位置，红字显示识别到的文字。

### 4. 生成 HTML 可视化报告

```bat
.venv\Scripts\python.exe test\report_ocr.py test\test_demo.png
```

生成 `ocr_report.html`：原图 / 标注图对比 + 识别文本表 + 结构化 JSON，图片以 base64 内嵌，单文件可直接分享。

> 上面两个脚本的输出文件默认生成在**当前工作目录**；想直接放进 `test/`，把输出路径作为第二个参数传入即可，例如：
> `.venv\Scripts\python.exe test\visualize_ocr.py test\test_demo.png test\result_vis.png`
>
> 首次运行会自动下载 OCR 模型（约 20MB）到 `~/.paddlex/official_models/`，之后离线可用。

### 5. 边界场景测试

```bat
.venv\Scripts\python.exe test\test_edge_cases.py
```

覆盖 5 类场景：正常图 / 无文字图 / 损坏文件 / 超大图 / 中文路径，全部通过会打印 `5/5 项通过`。

### 6. 格式支持测试

```bat
.venv\Scripts\python.exe test\test_formats.py
```

逐一验证 `SUPPORTED_EXTS` 里每种格式的「载入 + 识别」都正常，并检查扩展名判定（含大小写不敏感），全部通过会打印 `36/36 项通过`。

### 7. 快捷键一致性测试

```bat
.venv\Scripts\python.exe test\test_shortcuts.py
```

校验「定义 → 绑定 → 设置面板 → 持久化」四者一致，避免出现「面板里写着某个键、实际按了没反应」，
并验证改键 / 解绑 / 冲突检测 / 裸键拦截 / 保存后立即生效。

### 8. 表格识别测试

```bat
.venv\Scripts\python.exe test\test_table.py
```

覆盖 HTML 表格解析（含 rowspan / colspan 不错位）、Excel 导出往返，以及一张合成 3×3 表格的真实识别。
把「解析逻辑」和「模型能力」分开验证——解析错位看起来很像识别不准，混在一起很难归因。

### 9. 界面模式测试

```bat
.venv\Scripts\python.exe test\test_ui_modes.py
```

校验模式注册表字段完整、切模式时按钮文字与结果页同步、按钮可用性随结果变化、
窗口状态持久化与「关闭记忆」、以及高 DPI 下截图选区的坐标换算。

## 打包与分发（生成 .exe）

项目已配置好 PyInstaller 打包脚本 `ocrapp.spec`，一条命令即可产出可执行程序：

```bat
.venv\Scripts\python.exe -m PyInstaller ocrapp.spec --noconfirm
```

产物位于 `dist\OCR小软件\`，双击其中的 `OCR小软件.exe` 即可运行。

### 为什么是「文件夹」而不是单文件 exe？

本项目采用 PyInstaller 的 `--onedir`（目录）形态，而非 `--onefile`（单文件），原因有二：

1. **启动更快**：单文件 exe 每次运行都要先把上百 MB 内容解包到临时目录，首启明显变慢；目录形态直接加载。
2. **LGPLv3 合规**：界面框架 PySide6 采用 LGPLv3，以独立 `.dll/.pyd` 文件形式分发，便于用户按许可证要求替换 Qt 库文件。

### 内置模型，开箱即用

`PP-OCRv5_mobile_det` / `PP-OCRv5_mobile_rec`（约 22MB）+ `SLANet_plus`（表格结构，约 8MB）已随包分发。
首次运行时程序会把它们释放到标准缓存目录 `~/.paddlex/official_models/`，因此：

- **无需联网**：解压即用，断网也能识别
- **不重复占用**：若该目录已有同名模型则跳过，不覆盖用户自行更新的版本
- 想自定义缓存位置，可设置环境变量 `PADDLE_PDX_CACHE_HOME`

### 打包后自检

打包产物提供无界面自检开关，用于确认「模型能否加载、识别是否正常」：

```bat
dist\OCR小软件\OCR小软件.exe --selftest
```

它会程序化生成一张文字图和一张 3×3 表格图，跑通「文字识别 + 表格识别」两条链路，
结果写入 `%TEMP%\ocr_selftest.txt`。退出码：`0` 全部通过 / `1` 未识别到文字 / `2` 异常 / `3` 表格未识别。

> **体积说明**：完整产物约 830MB，主要来自 PaddlePaddle 推理框架（约 300MB）、
> Qt 运行库（约 90MB），以及表格识别牵入的 scipy + scikit-learn（约 160MB）。
> 这是「本地离线深度学习 OCR」的固有成本 —— 换来的是数据不出本机、无需 API Key、无调用次数限制。

## 目录结构

```text
ocrapp/
├── src/                   # 正式代码
│   ├── main.py            # GUI 主程序（PySide6）
│   ├── ocr_engine.py      # OCR 引擎封装
│   └── download_models.py # 模型预下载脚本
├── test/                  # 测试脚本、测试数据与运行产物
│   ├── test_ocr.py        # 命令行识别
│   ├── visualize_ocr.py   # 生成标注图
│   ├── report_ocr.py      # 生成 HTML 报告
│   ├── test_edge_cases.py # 边界场景测试（5 类）
│   ├── test_formats.py    # 格式支持测试（17 种扩展名）
│   ├── test_shortcuts.py  # 快捷键测试（定义 / 绑定 / 面板 / 持久化）
│   ├── test_table.py      # 表格测试（HTML 解析 / Excel 往返 / 真实识别）
│   ├── test_ui_modes.py   # 界面模式测试（切换 / 状态 / 截图坐标）
│   ├── test_image.png     # 示例图（基础，2 行）
│   ├── test_demo.png      # 示例图（7 行混合内容）
│   ├── result_vis.png     # 标注图产物
│   ├── ocr_report.html    # HTML 报告产物
│   └── gui_*.png          # GUI 界面截图
├── assets/                # 应用图标
│   ├── icon.ico           # 多尺寸 Windows 图标（打包时使用）
│   ├── icon.png           # 256×256 PNG（README / 非 Windows 平台）
│   └── make_icon.py       # 图标生成脚本（保证图标可复现）
├── docs/                  # README 配图（普通识别 / 表格识别 / 设置面板）
├── ppocr_img/             # 本地真实场景测试图集（不入库，自行下载放于此）
├── requirements.txt       # 依赖清单（含版本说明）
├── requirements.lock      # 全量锁定版本（pip freeze，96 个包）
├── setup.bat              # 一键安装脚本
├── ocrapp.spec            # PyInstaller 打包配置（生成 dist/OCR小软件）
├── LICENSE                # MIT 许可证
├── THIRD-PARTY-NOTICES.md # 第三方组件许可证声明
├── OCR小软件可行性计划.md   # 技术方案与四阶段路线图
├── OCR项目进度跟踪.md       # 进度、待完善项、优化方向
├── .gitignore             # 忽略虚拟环境 / 运行产物 / 模型缓存
├── .gitattributes         # 统一换行符策略（bat 用 CRLF，其余 LF）
└── README.md
```

> 说明：`test/` 目录同时存放测试脚本、示例图片和运行产物。其中示例图片会被提交到仓库（便于复现识别结果），运行产物（`result_vis.*`、`ocr_report.html`、`gui_*.png`）已在 `.gitignore` 中排除。

## 已知问题 / 踩坑

- `paddleocr==3.2.1` 版本不存在，正确版本是 `3.2.0`
- PaddlePaddle 3.2.1 遗漏 `setuptools` 依赖，`import paddle` 会报错，需手动补装
- `paddlex` 依赖 `chardet`，官方镜像无此包，需先装
- PaddleOCR 3.x 使用新 API（`text_detection_model_name` + `predict()`），网上旧教程的 `use_angle_cls`/`ocr()` 已废弃

### 打包（PyInstaller）相关踩坑

- **包元数据必须一起打包**：paddlex 在运行时用 `importlib.metadata` 判断依赖是否「已安装」，而 PyInstaller 默认不打包 `*.dist-info`，
  会让 OCR 管线把依赖误判为缺失，报 `A dependency error occurred during pipeline creation`。
  解法：在 spec 中收集全部已安装分发的元数据（`copy_metadata`）。
- **排除 modelscope 的重型子包**：`collect_submodules('paddlex')` 会牵连 `modelscope.models` 等大量 CV/NLP 模型定义；
  实测 OCR 只用到 `modelscope.hub`/`utils`，故排除其余子包，并排除 `paddle.jit.sot`
  （后者在打包依赖分析阶段会导致隔离子进程崩溃）。
- **界面库换用 PySide6**：PyQt6 为 `GPL-3.0-only`（强传染），迁移到 PySide6（`LGPLv3`）后，本项目才能以 MIT 开源。

### 已主动处理的边界情况

- **中文 / 含空格路径**：统一「PIL 读图 → ndarray」再送入识别，规避路径兼容问题
- **超大图**（最长边 > 4000px）：自动等比压缩后识别，识别框坐标会还原回原图，标注位置依然准确
- **损坏文件 / 非图片**：抛出可读提示「无法识别的图片格式」，程序不会崩溃
- **无文字图片**：提示「未识别到文字」，界面状态正常
- **标注图生成失败**：降级为「只显示文字」，不影响识别结果的使用
- **格式错位**：载入与识别共用同一套 PIL 解码，杜绝「能显示却识别不了」的格式（如 SVG、PDF）
- **拖入非图片**：拖拽时先按扩展名筛选，没有可识别的图片就明确提示，不会静默失败

## 许可证

本项目以 **MIT 许可证** 发布，详见 [LICENSE](LICENSE)。

界面框架选用 **PySide6**（LGPLv3）而非 PyQt6（GPL-3.0-only），从而允许本项目以宽松的 MIT 许可证开源。
所有第三方组件的署名与许可证信息，见 [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md)。

## 进度

开发过程与踩坑记录详见 [OCR项目进度跟踪.md](OCR项目进度跟踪.md)。
