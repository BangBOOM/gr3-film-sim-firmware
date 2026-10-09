# GR III Film Simulation — 胶片模拟实验固件

[中文](#中文) · [English](#english)

## 中文

理光 GR III Image Control 与胶片风格的非官方研究项目。本仓库提供研究说明和实验固件 Release；它与 GR Studio / GR Firmware 工具仓库分开，不发布工具网站、桌面应用或私有研究源码。

**最新版本：[V9.2 · 参数重启保存修复（实验预发布）](https://github.com/BangBOOM/gr3-film-sim-firmware/releases/tag/v9.2-persistence-fix-experimental)。项目维护者于 2026-10-09 报告，这份固件的参数在关机重开后可以保留。该反馈不代表完整功能或长期稳定性验收。**

## 适用范围

**仅支持 RICOH GR III（GR3）和 RICOH GR III HDF（GR3 HDF），基于官方 2.10。** 不支持 GR IIIx、GR IIIx HDF、GR IV、GR DIGITAL III 或其他机型。其他基础版本、第三方补丁和降级的兼容性未保证。相机内版本仍显示 `2.10`，请用下载文件 SHA256 判断版本。

## V9.2 做了什么

V9.2 修复新增风格的饱和度等参数在重启后归零的问题。旧自定义存档占用了原厂功能使用的 APData11；V9.2 改用独立 APData12，并完整扩展原生记录生命周期到 13 个文件。另一条 Still 全量刷新路径现在恢复九项自定义参数，避免参数页取消将默认值写回存档。

V9.2 保留 V9.1 的 RAW 列表容量修复，以及四款参考配方、菜单顺序和图标：

| Image Control | 风格方向 | 图标 |
| --- | --- | --- |
| Gold 200 | 温暖、日常、怀旧 | G200 |
| Tungsten 800 / 800T | 钨丝灯、夜景与冷暖对比 | T800 |
| Ektar 100 | 鲜艳、清晰的户外色彩 | E100 |
| Metropolis | 低饱和、冷灰的城市色彩 | MTRO |

这些是受胶片启发的数字配方，不保证精确复现真实胶片。E100 指 Ektar 100，不是 Ektachrome E100。

- 四款分别支持饱和度、色相、明暗调、对比度、高光对比度、阴影对比度、锐度、阴影和清晰度，共九项 `−4…+4` 调节。
- 支持静态照片 Image Control、机内 RAW Development，以及独立参数存储。
- Still 菜单顺序：Nega → Posi → Gold → 800T → Ektar → Metropolis → Standard → Vivid → Monotone → Soft Monotone → Hard Monotone → Hi-Contrast B&W → Bleach Bypass → Retro → HDR → Cross Processing → Custom 1 → Custom 2。RAW Original 可用时置顶。
- Movie 保持原厂路径；没有新增颗粒或光晕；选择风格不自动改变 WB 或 ISO。
- 此参考包不从 SD 加载任意 `.cube`，不提供无限扩展槽位或完整参数文本导入/导出。私有 ImageTone 标识可能无法被第三方照片软件识别。

## 下载与升级

从 [V9.2 Release](https://github.com/BangBOOM/gr3-film-sim-firmware/releases/tag/v9.2-persistence-fix-experimental) 下载固件、`SHA256SUMS.txt`、第三方许可附件和验证记录。

| 文件身份 | 值 |
| --- | --- |
| 文件名 | `gr3-v210-four-films-v9-2.persistence-fix.experimental.bin` |
| 大小 | 30,410,988 字节 |
| SHA256 | `0e946a4570c5a2a33430e5821d062179f2472efac727b417e66f0b3c175c3911` |

1. 确认机型，备份照片和重要设置，准备相机格式化过的 SD 卡及充满电的电池。格式化会清空卡内数据。
2. 核验 SHA256。macOS 可运行：

   ```sh
   shasum -a 256 gr3-v210-four-films-v9-2.persistence-fix.experimental.bin
   ```

3. 将固件改名为 **`fwdc239b.bin`**，放在 SD 卡根目录、DCIM 外；根目录只保留本次更新包，避免 `.bin.bin` 或带编号的文件名。安全弹出 SD 卡。
4. 相机关机后插卡，**按住 MENU，同时开机**。选择 **Execute / 执行**，按 OK，等待 **Update completed / 更新完成**。期间不要断电、取出电池或 SD 卡；完成后关机。流程参考[理光官方更新说明](https://www.ricoh-imaging.co.jp/english/support/digital/gr3_s.html)，不表示实验包获得官方认可。未出现更新界面时请复查机型、路径和校验，不要绕过相机检查。
5. **首次升级会初始化新的自定义存档。请重新设置参数，再测试关机重开。旧版已经被覆盖的数值无法恢复。**
6. 在静态照片 Image Control 选择四款风格并调节参数。JPEG 应用所选风格；已有 DNG 可通过回放 RAW Development 输出 JPEG。RAW 预览风格不意味着原始传感器数据被永久改写。
7. 复测菜单、风格切换、拍摄、RAW 显影和重启保留。更新后移除卡上的升级文件，避免误用旧包。降级和恢复兼容性未保证，本项目不承诺回滚或救砖服务。

## 验证范围

| 检查 | 结果 |
| --- | --- |
| 参考包：11 个既有 ARM 套件＋2 项最终载荷持久化回归 | PASS |
| 网页 reference / custom 及连续自定义构建：13 个 ARM 套件 | PASS |
| 原生 setter 组合 | 972 例 PASS |
| 两种写入顺序、八个 bank、864 个非中性参数位置、13 文件打包/导入/缓存恢复 | PASS |
| 八种存档失败场景、54 次 Still 切换、18 次原厂回退、6 次参数页取消 | PASS |
| 独立双解码器、完整载荷白名单及容器校验 | PASS |
| 真机重启保留参数 | 维护者于 2026-10-09 报告通过 |

真机反馈仅确认重启后参数保留。未提供重启次数、逐风格/槽位/模式清单、硬件修订或长期运行记录。完整真机矩阵、断电和长期稳定性测试未完成。离线测试替代部分文件系统、堆分配和消息传输；它们不能证明真实闪存、完整调度、ISP、任务栈余量或所有设备均正常。

## 历史版本

- [V9.1](https://github.com/BangBOOM/gr3-film-sim-firmware/releases/tag/v9.1-raw-fix-experimental) 修复 V9 的 RAW 列表崩溃，但存在本次修复的参数重启保存缺陷。请使用 V9.2。
- [V9](https://github.com/BangBOOM/gr3-film-sim-firmware/releases/tag/v9-four-films-experimental) 有 RAW Image Control 花屏和关机问题，原厂与新增风格均可能受影响。

## 研究用途与免责声明


1. **用途与关系。** 本项目仅用于固件、影像控制和数字色彩的学习、实验与研究。它不是官方固件、消费级产品、维修工具或适用于关键工作的解决方案；作者与 RICOH/PENTAX、Kodak、CineStill、Lomography 及其他品牌无隶属、授权、赞助或背书关系。
2. **按现状提供。** 所有文档、配方与二进制以“现状”和“可用时”提供。在适用法律允许的范围内，不提供明示或默示保证，包括安全性、稳定性、准确性、适销性、特定用途适用性、兼容性或不侵权保证。
3. **自行承担实验风险。** 下载、修改、刷写、降级或使用实验固件均由使用者自行判断并承担风险。可能发生无法启动、卡死、功能异常、配置或照片丢失、存储损坏、硬件损坏、维修费用及保修影响。项目不承诺可恢复、可回滚或可由官方服务修复。
4. **责任范围。** 在适用法律允许的最大范围内，作者、贡献者与发布者不承担因本项目或其使用产生的直接、间接、附带、特殊或后果性损失，包括设备、数据、收入、业务或时间损失。不得依法排除或限制的责任，以适用法律为准；本声明不改变第三方许可，也不剥夺依法不可放弃的权利。
5. **不提供服务承诺。** 本项目不承担刷机指导、适配、售后、修复、数据恢复、赔偿、更新维护或响应期限义务。研究成果可能随时变更或停止维护。
6. **权利与许可。** 研究用途并不自动授予对原厂固件、第三方作品、专利或商标的使用或再分发权。相应权利归各权利人；使用者应自行确认所在地法规及适用许可。本仓库未将完整固件统一授予 MIT、CC BY-SA 或其他开源许可。下载或再分发时不得移除适用署名和许可说明。

## 第三方来源与署名

Gold 色彩数据包含 **spektrafilm `kodak_gold_200` profile by Andrea Volpato** 的修改衍生数据：[上游项目](https://github.com/andreavolpato/spektrafilm)。该 profile 与直接衍生色彩数据采用 **CC BY-SA 4.0**。Release 的 `third-party-notices-v9-2.zip` 保留原样许可、署名及修改记录。

本项目烘焙、拟合并适配色彩数据到相机矩阵和曲线。V9 引入四款风格；V9.1 修复 RAW 列表容量；V9.2 修复参数存储和刷新，参考颜色数值保持不变。第三方许可不扩展到原厂固件或其他独立作品；完整固件未统一授予开源许可。相机及胶片名称只描述兼容性或风格方向，不表示品牌授权或背书。

---

## English

### About and compatibility

An unofficial GR III Image Control and film-look research project. This repository distributes experimental firmware and notices through Releases. It is separate from the GR Studio / GR Firmware tool repository. It does not distribute those applications or private research source.

**Current release: [V9.2 — Parameter Persistence Fix (Experimental)](https://github.com/BangBOOM/gr3-film-sim-firmware/releases/tag/v9.2-persistence-fix-experimental). Supported models: GR III and GR III HDF only, based on official 2.10.** GR IIIx, IIIx HDF, GR IV, GR DIGITAL III and other models are not supported. Migration from other bases, third-party patches and downgrades is not guaranteed. The camera still displays 2.10; identify the package by SHA256.

### Changes

V9.2 fixes custom parameters resetting after restart. APData11 already belongs to a stock feature; custom data now uses APData12 with a complete native thirteen-record lifecycle. The Still bulk-refresh snapshot also restores nine custom controls before property commit. Detail-page backup/cancel no longer writes neutral values back through that missing synchronization path.

V9.2 retains the V9.1 RAW list capacity fix, four reference looks (Gold 200, Tungsten 800, Ektar 100 and Metropolis), menu order and compact icons. These are film-inspired digital recipes, not certified film reproductions. E100 identifies Ektar 100, not Ektachrome E100.

Each look has nine independent −4…+4 controls. The package supports still Image Control and in-camera RAW Development. It retains the factory Movie path and adds no grain or halation. It does not change WB/ISO automatically, load arbitrary SD `.cube` files, add unlimited slots or implement complete numeric-control text import/export. Private ImageTone IDs may be unrecognized by third-party photo software.

### Download and installation

Download the BIN, checksums, third-party notices and JSON evidence from the [V9.2 Release](https://github.com/BangBOOM/gr3-film-sim-firmware/releases/tag/v9.2-persistence-fix-experimental).

- File: `gr3-v210-four-films-v9-2.persistence-fix.experimental.bin`
- Size: **30,410,988 bytes**
- SHA256: `0e946a4570c5a2a33430e5821d062179f2472efac727b417e66f0b3c175c3911`

Confirm the camera model. Back up photos and important settings. Prepare a camera-formatted SD card and a fully charged battery; formatting erases the card. Verify SHA256, rename the BIN to **`fwdc239b.bin`**, and place it in the SD root outside DCIM. Keep only the intended update package and safely eject the card.

With the camera off, insert the card. Hold **MENU while powering on**, select **Execute**, and press **OK**. Do not interrupt power or remove the card/battery. Wait for **Update completed**, then power off. See [Ricoh's update procedure](https://www.ricoh-imaging.co.jp/english/support/digital/gr3_s.html); it does not endorse this experimental package. If no update screen appears, recheck the model, location, name and checksum instead of bypassing checks.

**The first upgrade initializes the new custom record. Set parameters again before testing restart retention. Overwritten old values cannot be recovered.** Select the four looks in still Image Control, or use playback RAW Development for existing DNGs. A RAW preview look does not permanently recolor sensor data. Test capture, menus, RAW development and restart retention, then remove the upgrade file. Recovery, downgrade compatibility and unbricking support are not promised.

### Validation and release history

The reference package passes eleven existing ARM suites and two final-decoded persistence regressions. Production Web reference/custom and repeated custom builds pass thirteen ARM suites. Checks cover 972 setters, both writer orders, eight banks and 864 non-neutral values, thirteen-file bundle/import/cache restoration, eight failure cases, 54 Still transitions, 18 factory cases and six detail cancellations. Independent decoders, complete payload ownership and container checks agree.

On **2026-10-09**, the maintainer reports that this exact V9.2 package retains parameters after camera restart. The feedback does not include a restart count, per-style/slot/mode matrix, hardware revision or long-term log. Full device acceptance and power-interruption tests remain incomplete. Offline tests substitute selected services; they do not execute real flash hardware or complete camera scheduling and do not certify ISP output, task-stack margin or all devices.

[V9.1](https://github.com/BangBOOM/gr3-film-sim-firmware/releases/tag/v9.1-raw-fix-experimental) repairs the V9 RAW list crash but has the parameter-persistence defects fixed here. Use V9.2. The older [V9](https://github.com/BangBOOM/gr3-film-sim-firmware/releases/tag/v9-four-films-experimental) can crash during RAW Image Control operations for both stock and added styles.

### Research-only disclaimer


1. **Purpose and affiliation.** This project is for learning, experimentation and research into firmware and digital colour. It is not an official firmware, consumer product, repair tool or solution for critical work. The author is not affiliated with, authorised by, sponsored by or endorsed by RICOH/PENTAX, Kodak, CineStill, Lomography or other referenced brands.
2. **No warranties.** Documentation, recipes and binaries are provided **“as is” and “as available”**. To the extent permitted by applicable law, no express or implied warranties are provided, including safety, stability, accuracy, merchantability, fitness for a particular purpose, compatibility or non-infringement.
3. **Your decision and risk.** Downloading, modifying, flashing, downgrading or using experimental firmware is your own decision and risk. Possible consequences include failure to boot, freezes, malfunctions, lost settings/photos, corrupted storage, hardware damage, repair costs and warranty implications. Recovery, rollback and official repair are not guaranteed.
4. **Limitation of liability.** To the maximum extent permitted by applicable law, authors, contributors and distributors accept no liability for direct, indirect, incidental, special or consequential losses arising from this project or its use, including device, data, income, business or time losses. Liability that cannot legally be excluded remains governed by applicable law. This disclaimer does not override third-party licences or waive non-waivable rights.
5. **No service commitment.** No obligation is undertaken to provide flashing assistance, adaptations, support, repairs, data recovery, compensation, updates, maintenance or response deadlines. The project may change or stop being maintained.
6. **Rights and licences.** Research use does not itself grant rights to use or redistribute manufacturer firmware, third-party works, patents or trademarks. Rights remain with their respective owners; users must check applicable laws and licences. No blanket MIT, CC BY-SA or other open-source licence is granted to the complete firmware. Preserve applicable attribution and licence notices when downloading or redistributing.

### Third-party attribution

Gold includes modified profile-derived color data from **spektrafilm `kodak_gold_200` by Andrea Volpato**, under **CC BY-SA 4.0**. Preserve the verbatim attribution and license in `third-party-notices-v9-2.zip`. [Upstream project](https://github.com/andreavolpato/spektrafilm).

The project bakes, fits and adapts color data to native matrices/curves. V9 integrates four styles; V9.1 repairs RAW list capacity; V9.2 repairs parameter storage and refresh while retaining the reference color values. The upstream grant does not extend to manufacturer firmware or independent works. No blanket open-source license is granted to the complete firmware. Camera and film names describe compatibility and intended looks, not endorsement.
