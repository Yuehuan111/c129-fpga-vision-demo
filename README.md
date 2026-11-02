# C129 FPGA 多模式图像矫正系统 · 演示指标美化版

基于紫光同创 MES50HP / PGL50H 和 OV5640 的 720p 图像矫正工程，用于比赛工程提交与复现。

## 下载完整工程

**当前版本：2026-11-02 拍照录像更新版。**

**[点击下载完整工程 ZIP](https://github.com/Yuehuan111/c129-fpga-vision-demo/archive/refs/tags/C129-beautified-recording-20261102.zip)**

也可以点击 **Code → Download ZIP**，或进入 [最新发布版本](https://github.com/Yuehuan111/c129-fpga-vision-demo/releases/tag/C129-beautified-recording-20261102) 下载 **Source code (zip)**。

本仓库包含完整交付内容，包括 PDS 工程、RTL、IP、上位机、标定、烧录文件、编译数据库及报告。GitHub 自动附件虽然显示为 “Source code”，下载内容也包括上述已编译产物。

解压后进入 `C129_compiled_final_20261031/`，按照其中的 `使用说明.txt` 配置环境，再运行 `start_demo.cmd`。工程目录保留原名称，当前包的更新日期为 2026-11-02。

仓库中的 [`C129_compiled_final_20261031/`](C129_compiled_final_20261031/) 与用户提供的 `C129_演示指标美化版_拍照录像更新_20261102.zip` 中的 **493 个文件逐个字节一致**。本次上传不再修改包内代码、参数、说明或构建产物，也没有重新编译。

### 本次更新

- 上位机新增 **F8 拍照、F9 开始／停止录像**，以及照片／录像文件夹入口和录像状态显示。
- 切换视频源及关闭软件时完成录像保存收尾；具体操作见包内 [`更新说明_20261102.txt`](C129_compiled_final_20261031/更新说明_20261102.txt)。
- 相较 2026-10-31 美化版，新增 8 个文件、修改 3 个文件、删除 0 个文件；FPGA 工程、参数与烧录文件保持一致。
- 包内自带 7 项离线测试通过记录及离线录像样例；本次发布仅核对交付内容，未新增硬件测试结论。样例由本地 PNG 重复输入生成，其录像文件帧率不代表 FPGA 每秒新矫正帧率。

[上一版归档（2026-10-31）](https://github.com/Yuehuan111/c129-fpga-vision-demo/releases/tag/C129-beautified-20261031) 保留供回溯。

原始交付 ZIP 的 SHA256（用于核对用户原包，不是 GitHub 自动 ZIP 的校验值）：

```text
41405211a6b1eabe8253b91e1f5ac5e42f7c2729dd5d15966ae8be681480a8c1
```

## 系统功能与分工

- 鱼眼畸变矫正与双线性插值。
- IMU 姿态参数驱动的动态矫正。
- 自适应 Gamma 图像增强。
- 棋盘平面定位、扫描与正视提取。
- 上位机控制的镜像、旋转与工作模式切换。
- 演示指标显示及已有测量结果导入、日志导出。

FPGA 完成逐像素坐标运算、映射生成、取样、插值、图像处理和输出。上位机用于姿态或平面矩阵计算、参数下发、预览与测量结果管理；不以 PC 预计算整帧坐标上传替代 FPGA 算法。

## 主要入口

| 内容 | 路径 |
|---|---|
| 上位机启动 | [`start_demo.cmd`](C129_compiled_final_20261031/start_demo.cmd) |
| 上位机源码 | [`host/`](C129_compiled_final_20261031/host/) |
| FPGA / IP / PDS / 约束 | [`fpga_reference/project/`](C129_compiled_final_20261031/fpga_reference/project/) |
| PDS 工程文件 | [`camera_board50.pds`](C129_compiled_final_20261031/fpga_reference/project/board/build_cmos1/camera_board50.pds) |
| 已有烧录文件 | [`C129_720p_cmos1.sbit`](C129_compiled_final_20261031/C129_720p_cmos1.sbit) |
| 标定文件 | [`host/calibration/`](C129_compiled_final_20261031/host/calibration/) |
| 构建记录 | [`output/c129/`](C129_compiled_final_20261031/fpga_reference/project/output/c129/) |
| 原始使用说明 | [`使用说明.txt`](C129_compiled_final_20261031/使用说明.txt) |

## 使用环境

- PDS：`2022.2-SP6.4`，器件 `PGL50H / FBG484 / -6`。
- 上位机：Windows，Python 3.10+，需包含 tkinter。
- 视频处理尺寸：1280 × 720，RGB565；摄像头通道 CMOS1。

下载完整工程并解压到英文路径。在工程目录执行：

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\host\setup_host.ps1 -Python python
```

然后运行 `start_demo.cmd`，按实际电脑选择串口和采集设备。

已有编译、综合、器件映射、布局布线与时序数据库均按原包保留。读取报告不需要重新编译。不要运行 `prepare_board.py` 覆盖已准备的工程；重新构建的详细要求以原使用说明为准。

## 数据与验证说明

本仓库发布的是指定日期的原始交付版本，本次归档不会新增实板验证结论。参数状态和测试限制保留原包记录，包括 IMU 安装矩阵的待独立验证标记。画质测量、采集帧率、每秒新矫正结果与 HDMI 刷新率应分别按对应实际记录引用。

## 第三方文件

包内厂商 IP、模型和其他第三方文件的版权与使用条件以其原始声明为准。本次归档保留这些声明，不统一改授 MIT、Apache 等许可证。
