# C129 FPGA 多模式图像矫正系统 · 演示指标美化版

基于紫光同创 MES50HP / PGL50H 和 OV5640 的 720p 图像矫正工程，用于比赛工程提交与复现。

## 下载完整工程

完整压缩包放在本仓库的 [**Releases / 完整工程下载**](https://github.com/Yuehuan111/c129-fpga-vision-demo/releases/tag/C129-beautified-20261031) 中，文件名为 `C129_beautified_20261031.zip`。它与原始交付包 `C129_演示指标美化版_20261031.zip` 内容完全相同，仅改用英文附件名方便下载。

SHA256：

```text
adf5fa12f38296ff8132d72a4cb5498f32f0b8517b7b9f976aeb1955aef69be3
```

仓库中的 [`C129_compiled_final_20261031/`](C129_compiled_final_20261031/) 也保留了完整解压内容，便于直接浏览代码、PDS 工程、位流和已有构建报告。本次上传没有修改上位机、FPGA 代码或参数，也没有重新编译。

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
