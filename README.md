# 一加 Turbo 6（PLU110）TWRP 16 GitHub Actions 构建器

这个仓库通过 GitHub Actions 编译 Android 16 / TWRP 16 的
`recovery.img`，目标是国行一加 Turbo 6（型号 `PLU110`，设备代号
`volkswagen` / `vw`）。

## 使用方法

1. 新建一个 GitHub 仓库，把本压缩包中的文件按原目录上传到仓库根目录。
2. 打开仓库的 **Actions** 页面并允许工作流运行。
3. 上传工作流的那次提交会自动开始构建；也可以进入
   **Build TWRP 16 for OnePlus Turbo 6**，选择 **Run workflow** 手动重跑。
4. 等待约 40–90 分钟。构建成功后，在运行页面底部下载
   `TWRP-16-OnePlus-Turbo6-PLU110-<run id>`。
5. 解压产物，先使用 `SHA256SUMS.txt` 校验镜像。

构建包内还包含：

- `BUILD_INFO.txt`：关键源码版本和运行链接；
- `pinned-manifest.xml`：本次构建实际使用的全部源码版本；
- `SHA256SUMS.txt`：镜像的 SHA-256 校验值。

## 固定的上游版本

- TWRP 16 manifest：`TWRP-Test/platform_manifest_twrp_aosp`，分支
  `twrp-16.0`，manifest 提交
  `512614e74d5a65f7b11fbd0b424f0d68b5ef7fe1`；
- 设备树：`kmiit/twrp_device_oplus_sm87xx`，分支 `twrp-16.0`，提交
  `74d075623b35f685fcde82efb5a43548697d1a6e`；
- Turbo 6 触控支持：提交
  `dd7a43fdb13c4440df99fbe145bead37e5435aef`，来源为 PLU110
  ColorOS `16.0.3.501` 的 BOE 触控固件，并注明已在 PLU110 上测试。

工作流会验证设备树中包含上述触控提交；校验不通过时会停止构建。

## 重要安全说明

这是社区设备树生成的非官方 TWRP，不是 OnePlus 或 Team Win 的官方发布。
构建成功只说明镜像成功生成，不等于已经在你的具体系统版本上验证可启动。

- 仅用于国行 `PLU110`；不要用于 Turbo 6V、Ace 6 或其他型号。
- 刷入前备份数据和原厂 `recovery`，确认 Bootloader 已解锁，并准备可用的
  线刷/救砖方案。
- 第一次应优先临时启动或只刷当前活动槽进行验证；具体是否支持
  `fastboot boot` 取决于设备 Bootloader，不能保证。
- 不要照搬网上的 `vbmeta`、禁用 AVB 或双槽刷写命令；应先核实你当前
  ColorOS 版本、活动槽和分区布局。

设备树声明 recovery 分区大小为 `0x6400000`（100 MiB）。工作流会检查
输出镜像不超过该大小，并确认它是 Android boot image。

## 已知成功基线

上游同一工作流在 2026-06-21 成功构建过 `sm87xx` 镜像，耗时约
1 小时 15 分钟；其 Actions 压缩包 SHA-256 为
`47411e59ab2af6c991cbc9a61f8f44f7d1d98490ed87abe33bff50bfce07ff22`。
该次运行选择的是 `nord6` 设备树分支，因此这里只把它作为构建环境基线；
本仓库固定使用含国行 Turbo 6 触控支持的 `twrp-16.0` 分支。
