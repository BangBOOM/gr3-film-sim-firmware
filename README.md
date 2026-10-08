# GR III Film Simulation — 胶片模拟实验固件

[中文](#中文) · [English](#english)

## 中文

理光 GR III 影像控制与胶片风格的非官方研究项目。本仓库仅提供研究说明和实验固件 Release，不提供逆向源码、原厂源码、构建环境或商业服务。

**最新版本：[V9.1 · RAW 崩溃修复（实验预发布）](https://github.com/BangBOOM/gr3-film-sim-firmware/releases/tag/v9.1-raw-fix-experimental)。项目维护者于 2026-10-08 报告，更新后此前触发崩溃的机内 RAW 操作已验证正常。本版本仍为实验预发布，不应视为官方升级包或全面稳定性认证。**

旧 [V9](https://github.com/BangBOOM/gr3-film-sim-firmware/releases/tag/v9-four-films-experimental) 存在 RAW 影像控制列表花屏、关机问题，原厂与新增风格均可能受影响；请使用 V9.1 修复版。

## 适用范围

**仅支持 RICOH GR III（GR3）和 RICOH GR III HDF（GR3 HDF）。** 不支持 GR IIIx、GR IIIx HDF、GR IV、GR DIGITAL III 或其他机型。

本包基于官方 **2.10** 固件制作，其他基础版本或第三方补丁的迁移兼容性未评估。支持范围不代表每个机型/硬件修订均有独立验收记录。相机内的版本显示仍为 `2.10`，不能用它判断是否已安装 V9.1。

## V9.1 做了什么

修复 V9 在机内 RAW 处理中打开或切换影像控制、调节参数时可能花屏并自动关机的问题：列表最多需要 19 行，原构造路径只创建 17 行；V9.1 将构造容量提高到 19 行。相对 V9，四款风格颜色、参数、菜单顺序与图标保持原样。

在保留原厂影像控制的基础上，增加四个独立菜单项：

| 影像控制 | 风格方向 | 图标 |
| --- | --- | --- |
| Gold 200 | 温暖、日常、怀旧 | G200 |
| Tungsten 800 / 800T | 钨丝灯、夜景与冷暖色彩对比 | T800 |
| Ektar 100 | 鲜艳、清晰的户外色彩 | E100 |
| Metropolis | 低饱和、冷灰的城市色彩 | MTRO |

这些是受胶片启发的数字色彩配方，**不保证精确复现真实胶片**。这里的 E100 图标指 Ektar 100，不是 Ektachrome E100。

- 四款支持各自的饱和度、色相、明暗调、对比度、高光对比度、阴影对比度、锐度、阴影和清晰度，共九项 `−4…+4` 调节。
- 接入机内 RAW 显影、独立参数记录及参数保存/恢复路径。
- 菜单顺序：Nega → Posi → Gold → 800T → Ektar → Metropolis → Standard → Vivid → Monotone → Soft Monotone → Hard Monotone → Hi-Contrast B&W → Bleach Bypass → Retro → HDR → Cross Processing → Custom 1 → Custom 2。RAW 的 Original 选项可用时置顶。
- Gold 与 800T 保留前一研究版本的数值色彩数据；四款图标统一为紧凑文字标识。
- 支持本项目 V7 的 Gold 参数、V8 的 Gold/800T 参数迁移；不保证任意其他补丁、重新绑定风格或降级后的参数与选择状态兼容。
- Movie 保持原厂路径。没有加入颗粒或光晕，选择风格不会自动改变白平衡或 ISO。

## 下载与文件核验

请从 [Releases](https://github.com/BangBOOM/gr3-film-sim-firmware/releases) 下载对应版本，并同时保留该版本的第三方许可附件。

V9.1 文件：`gr3-v210-four-films-menu-v9-1.raw-fix.experimental.bin`

大小：`30,410,436` 字节

SHA256：

```text
58135687e2b990636369d80ef36475cab0ea28a5128184322f2cd94ba46352d5
```

若自行决定在上述目标设备上实验，安装文件须放在 SD 卡根目录，命名为 `fwdc239b.bin`，再通过相机固件更新入口操作。不要将解包镜像、配方或 JSON 当成升级文件。文件名正确与校验一致只证明文件身份，**不证明设备兼容或刷写安全**。

## 如何使用

### 1. 确认机型与准备

仅支持 **RICOH GR III（GR3）和 RICOH GR III HDF（GR3 HDF）**。不支持 GR IIIx、GR IIIx HDF、GR IV、GR DIGITAL III 或其他机型。

本包基于官方 **2.10** 固件制作，未评估其他基础版本或第三方补丁的迁移兼容性。先备份照片和重要设置，准备相机格式化过的 SD 卡与充满电的电池；格式化会清空卡内数据。

### 2. 下载、校验与拷贝

从 [V9.1 Release](https://github.com/BangBOOM/gr3-film-sim-firmware/releases/tag/v9.1-raw-fix-experimental) 下载 `.bin`、`SHA256SUMS.txt` 和第三方许可附件。核验 `.bin` 的 SHA256 与本页一致。macOS 可使用：

```sh
shasum -a 256 gr3-v210-four-films-menu-v9-1.raw-fix.experimental.bin
```

将 `.bin` **改名为 `fwdc239b.bin`**，放到 SD 卡**根目录**，不要放进 DCIM，不要使用 `.bin.bin` 或带编号的文件名。根目录只保留本次使用的更新包，然后安全弹出 SD 卡。

```text
SD 卡根目录 /
├── fwdc239b.bin
└── DCIM/
```

### 3. 更新相机

相机关机后插入 SD 卡。**按住 MENU，同时开机**；在更新界面选择 **Execute / 执行**，按 OK。更新过程中不要断电、取出电池或 SD 卡。看到 **Update completed / 更新完成** 后关机，再取出更新卡。

这一操作流程参考 [理光官方 GR III 更新说明](https://www.ricoh-imaging.co.jp/english/support/digital/gr3_s.html)，不表示本实验包获得官方认可。若没有出现更新界面，先复查机型、文件名、根目录和文件校验，不要尝试绕过相机检查。

### 4. 选择胶片风格

重新开机，在静态照片的 **影像控制 / Image Control** 菜单选择 Gold 200、800T、Ektar 100 或 Metropolis；进入对应详细调节页即可调节九项参数。白平衡、曝光与 ISO 仍由你自行设置。

直出 JPEG 会应用所选风格。已有相机 DNG 可在回放菜单的 **RAW 显影 / RAW Development** 中选择上述影像控制并输出 JPEG；拍 RAW 时，不应把预览风格当作原始传感器数据被永久改写。

先确认菜单滚动、四款切换、拍摄、RAW 显影和参数重启保存正常。相机版本显示仍为 **2.10**；新增菜单项只能确认已安装本项目的风格固件，不能区分 V9 与 V9.1；请在更新前用本页文件名和 SHA256 确认所用修复包。更新后删除卡上的 `fwdc239b.bin`，避免误用旧包；若格式化卡，请先备份照片。降级/恢复兼容性未保证，本项目不承诺回滚或救砖服务。

## 验证范围与已知限制

V9.1 已完成七套最终 ROM 的离线检查（色彩后端、存储、菜单、RAW、RAW UI、图标、文本），以及两个独立解码器的一致性复核、补丁范围与容器校验。额外回归验证真实列表构造、18/19 行边界与共用 RAW 参数页面的容量契约。部分测试使用服务、文件系统或硬件替身。

**实机反馈（2026-10-08）：项目维护者刷入本次 V9.1 包后报告，之前触发故障的机内 RAW 操作验证正常。** 尚未附逐项测试清单、设备硬件修订或长期运行记录，因此此反馈不代表所有设备或全部功能均已验证。离线检查和单台设备反馈不能替代完整异步调度、ISP、真实文件系统、内存/栈余量、功耗及长期稳定性的评估。

- 不支持从 SD 卡直接加载任意 `.cube` LUT，也不提供无限扩展的影像控制槽。
- 四个风格身份固定，任意换包和降级的配置兼容性未保证。
- 自定义数值参数的完整文本导入/导出未实现。
- 新增的私有 ImageTone 标识可能无法被第三方照片软件识别。
- 实机已获作者验证反馈；精确色彩准确性、长期稳定性及全部功能兼容性仍未全面评估。

## 研究用途与免责声明

1. **用途与关系。** 本项目仅用于固件、影像控制和数字色彩的学习、实验与研究。它不是官方固件、消费级产品、维修工具或适用于关键工作的解决方案；作者与 RICOH/PENTAX、Kodak、CineStill、Lomography 及其他品牌无隶属、授权、赞助或背书关系。
2. **按现状提供。** 所有文档、配方与二进制以“现状”和“可用时”提供。在适用法律允许的范围内，不提供明示或默示保证，包括安全性、稳定性、准确性、适销性、特定用途适用性、兼容性或不侵权保证。
3. **自行承担实验风险。** 下载、修改、刷写、降级或使用实验固件均由使用者自行判断并承担风险。可能发生无法启动、卡死、功能异常、配置或照片丢失、存储损坏、硬件损坏、维修费用及保修影响。项目不承诺可恢复、可回滚或可由官方服务修复。
4. **责任范围。** 在适用法律允许的最大范围内，作者、贡献者与发布者不承担因本项目或其使用产生的直接、间接、附带、特殊或后果性损失，包括设备、数据、收入、业务或时间损失。不得依法排除或限制的责任，以适用法律为准；本声明不改变第三方许可，也不剥夺依法不可放弃的权利。
5. **不提供服务承诺。** 本项目不承担刷机指导、适配、售后、修复、数据恢复、赔偿、更新维护或响应期限义务。研究成果可能随时变更或停止维护。
6. **权利与许可。** 研究用途并不自动授予对原厂固件、第三方作品、专利或商标的使用或再分发权。相应权利归各权利人；使用者应自行确认所在地法规及适用许可。本仓库未将完整固件统一授予 MIT、CC BY-SA 或其他开源许可。下载或再分发时不得移除适用署名和许可说明。

## 第三方来源与署名

Gold 色彩数据的研究来源包含 **spektrafilm** 的 `kodak_gold_200` profile，由 **Andrea Volpato** 创作：<https://github.com/andreavolpato/spektrafilm>。该 profile 及其直接衍生色彩数据采用 **CC BY-SA 4.0**；[上游许可原文](https://github.com/andreavolpato/spektrafilm/blob/main/SPEKTRAFILM_LICENSE.txt)。

BangBOOM / 本研究项目对其色彩数据进行了烘焙、拟合、机内矩阵/曲线适配及固件集成，属于修改后的研究实现。V9 引入四款风格集成；V9.1 延续 V9 的数值色彩数据、存储编码和集成路径，仅修复 RAW 列表容量。Release 附带 `third-party-notices-v9-1.zip`，包含所保留的来源说明、原样许可及本次修改记录；其许可范围不因此扩展到原厂固件或其他独立作品。

胶片与相机名称仅用于描述研究对象和风格方向，商标归各自权利人所有。

---

## English

### About and supported cameras

An unofficial research project exploring GR III Image Control and film-inspired colour. This repository contains only this README; experimental firmware and supporting notices are distributed through [Releases](https://github.com/BangBOOM/gr3-film-sim-firmware/releases).

**Supported models only: RICOH GR III and RICOH GR III HDF.** GR IIIx, GR IIIx HDF, GR IV, GR DIGITAL III and other models are not supported. V9.1 is based on the official **2.10** firmware; migration from other base versions or third-party patches has not been evaluated.

**Current release: [V9.1 — RAW crash fix (experimental prerelease)](https://github.com/BangBOOM/gr3-film-sim-firmware/releases/tag/v9.1-raw-fix-experimental).** On **2026-10-08**, the maintainer reported that the previously failing in-camera RAW workflow worked normally after installing this package. No per-model test matrix, detailed test log or long-term run record has been published. This remains an experimental prerelease, not an official update or a comprehensive stability certification.

The older [V9 release](https://github.com/BangBOOM/gr3-film-sim-firmware/releases/tag/v9-four-films-experimental) has a known RAW Image Control list crash affecting factory and added styles. Use V9.1 instead.

### What V9.1 changes

Fixes the garbled display and automatic shutdown when opening or changing Image Control or adjusting parameters during in-camera RAW development. The list needs up to 19 rows, while the previous construction path created only 17; V9.1 creates 19. Compared with V9, the four styles, numerical colour data, parameters, menu order and icons remain unchanged.

| Added Image Control | Intended look | Icon |
| --- | --- | --- |
| Gold 200 | Warm, nostalgic everyday colour | G200 |
| Tungsten 800 / 800T | Tungsten/night scenes and cool–warm contrast | T800 |
| Ektar 100 | Vivid outdoor colour | E100 |
| Metropolis | Muted, cool-grey urban colour | MTRO |

These are digital recipes inspired by film, not guaranteed reproductions of physical film. The E100 icon refers to **Ektar 100**, not Ektachrome E100.

- Four independent menu entries, each with nine adjustments from −4 to +4: saturation, hue, high/low key, contrast, highlight contrast, shadow contrast, sharpness, shading and clarity.
- Integrated into in-camera RAW development and separate parameter storage/save/restore paths.
- Still-menu order: Nega → Posi → Gold → 800T → Ektar → Metropolis → Standard → Vivid → Monotone → Soft Monotone → Hard Monotone → Hi-Contrast B&W → Bleach Bypass → Retro → HDR → Cross Processing → Custom 1 → Custom 2. RAW Original appears first when available.
- Preserves earlier Gold/800T numerical colour data and uses compact icons.
- Migrates this project's V7 Gold and V8 Gold/800T records. Arbitrary style rebinding, other patches and downgrade compatibility are not guaranteed.
- Leaves the factory Movie path in place. No added grain or halation; selecting a style does not change WB or ISO automatically.

### How to use

1. **Prepare.** Confirm that the camera is a GR III or GR III HDF. Back up photos and important settings. Prepare a camera-formatted SD card and a fully charged battery. Formatting erases the card.
2. **Download and verify.** Download `gr3-v210-four-films-menu-v9-1.raw-fix.experimental.bin`, `SHA256SUMS.txt` and the third-party notices from the [V9.1 Release](https://github.com/BangBOOM/gr3-film-sim-firmware/releases/tag/v9.1-raw-fix-experimental). The firmware is **30,410,436 bytes**, with SHA256:

   ```text
   58135687e2b990636369d80ef36475cab0ea28a5128184322f2cd94ba46352d5
   ```

   On macOS, run `shasum -a 256 gr3-v210-four-films-menu-v9-1.raw-fix.experimental.bin` and compare the result.
3. **Copy.** Rename the firmware to **`fwdc239b.bin`** and place it in the SD card **root**, outside DCIM. Avoid duplicate extensions or numbered filenames. Keep only the intended update package in the root, then safely eject the card.
4. **Update.** With the camera off, insert the card. Hold **MENU while powering on**, select **Execute**, and press **OK**. Do not interrupt power or remove the battery/card. Once **Update completed** appears, turn the camera off and remove the update card. See [Ricoh's official update procedure](https://www.ricoh-imaging.co.jp/english/support/digital/gr3_s.html); this reference does not imply endorsement of this experimental package. If no update screen appears, recheck the model, filename, location and checksum rather than bypassing camera checks.
5. **Use a style.** Open the still-photo **Image Control** menu and select Gold 200, 800T, Ektar 100 or Metropolis. Use its detail page for the nine adjustments; set exposure, WB and ISO yourself. Camera JPEGs apply the selected look. Existing camera DNGs can be processed through playback **RAW Development** with the new styles to produce JPEGs. A RAW preview look does not mean the original sensor data has been permanently recoloured.
6. **Check and clean up.** Check menu scrolling, style switching, capture, RAW development and settings retention after restart. The firmware version display remains **2.10**; the added entries identify this project's firmware but do not distinguish V9 from V9.1. Verify the download filename and SHA256 before updating to identify this repair package. Delete `fwdc239b.bin` from the card afterward. Back up photos before any formatting. Downgrade/recovery compatibility and unbricking assistance are not promised.

A matching filename or checksum identifies a file; it does not prove hardware safety or compatibility.

### Validation and limitations

Seven final-ROM offline checks passed: colour backend, storage, menu, RAW, RAW UI, icons and text. Two independent decoders agreed, and container/checksum and patch-boundary checks passed. Additional regressions exercised native list construction, the 18/19-row boundaries and shared RAW page capacity contracts. Some tests use service, filesystem or hardware substitutes.

The maintainer's successful RAW-workflow feedback on 2026-10-08 does not establish coverage of every device or feature. Full hardware behaviour, asynchronous scheduling, real filesystem behaviour, stack/memory margins, power consumption and long-term stability have not been comprehensively evaluated. Exact colour accuracy is not certified.

V9.1 does not load arbitrary `.cube` LUTs from SD or provide unlimited slots. Four style identities are fixed. Complete numeric custom-control text import/export is not implemented. Private ImageTone identifiers may be unrecognised by third-party photo software.

### Research-only disclaimer

1. **Purpose and affiliation.** This project is for learning, experimentation and research into firmware and digital colour. It is not an official firmware, consumer product, repair tool or solution for critical work. The author is not affiliated with, authorised by, sponsored by or endorsed by RICOH/PENTAX, Kodak, CineStill, Lomography or other referenced brands.
2. **No warranties.** Documentation, recipes and binaries are provided **“as is” and “as available”**. To the extent permitted by applicable law, no express or implied warranties are provided, including safety, stability, accuracy, merchantability, fitness for a particular purpose, compatibility or non-infringement.
3. **Your decision and risk.** Downloading, modifying, flashing, downgrading or using experimental firmware is your own decision and risk. Possible consequences include failure to boot, freezes, malfunctions, lost settings/photos, corrupted storage, hardware damage, repair costs and warranty implications. Recovery, rollback and official repair are not guaranteed.
4. **Limitation of liability.** To the maximum extent permitted by applicable law, authors, contributors and distributors accept no liability for direct, indirect, incidental, special or consequential losses arising from this project or its use, including device, data, income, business or time losses. Liability that cannot legally be excluded remains governed by applicable law. This disclaimer does not override third-party licences or waive non-waivable rights.
5. **No service commitment.** No obligation is undertaken to provide flashing assistance, adaptations, support, repairs, data recovery, compensation, updates, maintenance or response deadlines. The project may change or stop being maintained.
6. **Rights and licences.** Research use does not itself grant rights to use or redistribute manufacturer firmware, third-party works, patents or trademarks. Rights remain with their respective owners; users must check applicable laws and licences. No blanket MIT, CC BY-SA or other open-source licence is granted to the complete firmware. Preserve applicable attribution and licence notices when downloading or redistributing.

### Third-party attribution

Gold colour research includes a modified derivative of the **spektrafilm `kodak_gold_200` profile by Andrea Volpato**: <https://github.com/andreavolpato/spektrafilm>. The profile and its direct colour-data derivatives are **CC BY-SA 4.0**; see the [upstream licence](https://github.com/andreavolpato/spektrafilm/blob/main/SPEKTRAFILM_LICENSE.txt).

BangBOOM / this research project baked, fitted and adapted colour data to camera matrices/curves and integrated it into the firmware. V9 introduced the four-style integration. V9.1 preserves V9 numerical colour data, storage encoding and integration, and repairs only the RAW list capacity. The Release includes `third-party-notices-v9-1.zip` with retained provenance, the unchanged licence and a modification log. That licence does not thereby extend to manufacturer firmware or other independent works.

Camera and film names describe research targets and intended looks. Trademarks belong to their respective owners.
