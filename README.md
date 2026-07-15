# 桌面智慧屏(Smart Screen T113)

基于全志 **T113** 主控的桌面智慧屏项目,包含完整的硬件设计文件与基于 **LVGL** 的应用程序源码。功能页面包括:时钟、天气、闹钟、番茄钟、联动控制、系统设置等。

## 仓库结构

```
├── app4/                        # LVGL 应用程序(CMake 工程)
│   ├── main.c                   # 入口:LVGL 初始化 + 页面加载
│   ├── page_test.c              # 页面逻辑
│   └── res/                     # 应用资源(字体 / 图片)
├── page_main.c                  # 主页面
├── page_alarm.c                 # 闹钟页面
├── page_dialog.c                # 弹窗组件
├── res/                         # UI 资源
├── 素材/ 图片资源/               # 设计素材与切图
├── UI.jsd                       # UI 设计工程文件
│
├── ProPrj_Smart_Screen_T113_PCB6.epro          # 立创 EDA 工程(最新)
├── ProPrj_Smart_Screen_T113_end_2025-07-25.epro # 立创 EDA 工程(历史版本)
├── SCH_Schematic_2026-05-21.pdf                # 原理图
├── BOM_Board1_MIPI_PCB_2025-10-30.xlsx         # 物料清单
├── Gerber_PCB_2025-10-30.zip                   # 生产文件
├── InteractiveBOM_PCB_1_2025-11-8.html         # 交互式 BOM
├── T113_硬件设计指南V1_0.pdf                    # 主控硬件设计参考
│
├── bootlogo/ bootlogo.bmp       # 开机 Logo
└── 智慧屏项目QA问答.docx         # 项目 QA 文档
```

## 开发环境

以下工具体积较大,未包含在仓库中(见 `.gitignore`),需自行获取:

| 工具 | 用途 |
|------|------|
| `app_sdk_v2` | 应用 SDK(含 LVGL、屏幕/输入驱动移植层) |
| `toolchain-sunxi-glibc-gcc-830` | 全志 T113 交叉编译工具链(gcc 8.3.0, glibc) |
| CMake ≥ 3.15 | 构建系统 |
| PhoenixSuit | 全志固件烧录工具(Windows) |
| ADB (platform-tools) | 调试与文件传输 |

## 编译

`app4` 需要放入 SDK 源码树中构建(工程引用了 SDK 内的 `lvgl`、`component/font` 等目录):

1. 将 `app4/` 拷贝到 `app_sdk_v2` 的应用目录下,并在 SDK 顶层 `CMakeLists.txt` 中加入 `add_subdirectory(app4)`;
2. 使用 sunxi 交叉编译工具链配置 CMake 并编译,产物为可执行文件 `demo4`;
3. 构建时 `res/` 会被自动拷贝到 build 目录,与可执行文件一同部署到设备。

> 定义 `SIMULATOR_LINUX` 宏可在 Linux 模拟器环境下编译运行,便于脱离硬件调试 UI。

## 烧录与部署

- 整机固件使用 **PhoenixSuit** 通过 USB 烧录(驱动见 `usbdriver_V1.0.0.3`);
- 应用与资源可通过 **adb push** 推送到设备调试;
- 开机 Logo 替换 `bootlogo.bmp` 后重新打包固件。
