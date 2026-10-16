# FluidMatter — Status

更新时间：2026-10-16。

**Overall: real-time water rendering Research Prototype / 持续研发中。**

## Accepted / evidence-backed

W0 – W3.1 chain accepted。最近一个 accepted milestone 是 **W3.1 shoreline & foam**：
`SHORELINE_TEST.csv` 342 条 graded rows 全部 `PASS`（HARNESS + SHF-01…12 + CAL + BEAUTY + PERF），
16-gate regression × 2 个独立 pass（sequential + isolated）全部 `PASS`，exit 0，
`isolation_unresolved = 0`、`errorCS = 0`、`shaderError = 0`、`ioError = 0`、`files_created_by_gate = 0`，
isolated pass 中 `drifted 36 == restored 36`，每个 gate 快照 265 个工程文件。
W3.0 boundary field：15 gates × 2 passes 全 PASS，3,343 条 graded rows，0 FAIL。
这是**有界测试**的通过，不是所有场景、分辨率与硬件的认证。

Accepted 的模块：Linear Eye Depth 与 validity、thickness/SurfaceData 契约、screen-space refraction、
Beer–Lambert transmittance、reflection **preview**、Gerstner 位移（最多 4 slot）、boundary 字段、
**shoreline 形状与 foam 消费者**、Profile / binding / shared-material-cache / MPB、debug views 与自动化验证。

**关键限定（容易被误读，明确写出）：**

- reflection 是 preview 路径，**不是 SSR，也不是 planar reflection**。
- shoreline/foam 的外观来自**验证 rig**：无美术资产、单一方向光、1280×720。accepted 不等于好看。
- foam `coverage` 是**场级**标定，不是某一段岸线的**带状**承诺（见 README 限制 §5）。
- 所有结论限定在实测的视角斜坡、分辨率与弧距范围内。

## Implemented, validation pending

**W4.0 caustics**：working tree 中存在 caustics 消费者（shader、Core 模块、profile/binding、debug mode 与
CAU 自动化系列），其验收序列仍有未通过的 failing rows。**本仓库不发布任何 W4.0 图像，也不声称焦散的
物理正确性、flux 守恒或水下渲染。** "implemented" 与 "accepted" 是两个不同的词，这里刻意分开使用。

## Portfolio scene rendered, art pass in progress

`FMS_Showcase_Island` 是项目第一个作品集展示场景（岛屿海湾、水体表面、cinematic profile、三台相机）。
它已被引擎导入、通过结构化校验，并在真实 Tuanjie 编辑器（D3D11、RTX 4070）中以 1920×1080 渲染出三台相机的
画面。公开发布了**四张**：两张绑定反射输入后的最新水面帧、一张同机位单变量 A/B 对照图，以及较早那张
**technical** 捕获（以 **Development Showcase / Work in Progress** 的名义，现在作为 A/B 的 before 状态保留）。
README 首图仍然不使用它们，仍是 accepted W3.1 技术捕获。

**最新两张帧的准确边界（由 A/B 当帧的材质回读给出，不是事后描述）：** `_FMS_ShorelineEnabled = 1`、
`_FMS_FoamEnabled = 1`、`_FMS_EnvironmentReflectionAvailable = 1`、`envCube = FMS_Showcase_SkyCube`、
`_FMS_DebugMode = 0`、**`_FMS_CausticsEnabled = 0`**。画面里有已接受的岸线与泡沫、有环境反射输入，
没有任何焦散。反射输入是**本项目程序生成的天空 cubemap**，不是摄影 HDRI；反射路径仍是 **preview**，
不是 SSR，也不是 planar reflection，水面不镜像岛屿本体。

该结果由一次受控 A/B 支撑：**只有一个 shader 属性不同**（`stateDiff_keys = _FMS_EnvironmentReflectionAvailable`），
差异像素在两台相机下都**完全落在水体遮罩内**（遮罩外 `0` 个），全部水体像素上的平均通道差 `1.021229` /
`0.834653`；
debug mode 6 的独立读数显示天空方向误差比地面方向低 **4.77× / 8.01×**，即"反射天空"而不是"整体变亮"。
较早那张 technical 帧的边界保持不变，也不被追溯修改：**`_FMS_EnvironmentReflectionAvailable = 0` 且
`envCube = null`**，也就是说那张图里没有完成天空环境反射输入。

Hero 封面候选**已渲染但暂不公开**：视觉复审分数低于封面门槛。反射输入这一项有界美术修复**已完成并测过**，
剩下的差距是场景非水体美术仍是 blockout（billboard 地面、平色背景带、占位岩石）。
水下相机为 **DIAGNOSTIC ONLY**，状态标记 `UNSUPPORTED_CAMERA_VIEW`，其画面作为失败证据保留，不公开发布，
本仓库也不声称完整水下渲染。

## Not implemented / not claimed

physically accurate caustics、flux-conserving caustics、ray-traced water、underwater system、
open-water crest foam、wake、interaction foam、FFT / infinite ocean、SSR、planar reflection、
flow map、buoyancy、GPU benchmark、跨 API 认证。

## Visual status

**封面：已渲染，暂不公开（NOT PUBLISHED）**。首页图片仍是 accepted W3.1 技术捕获并明确标注为验证 rig；
展示场景的水面帧放在 README 画廊的 **Art pass / Work in Progress** 标签下，较早的 technical 帧单独放在
Development Showcase 段落。仓库不使用 debug mesh 或诊断色块充当项目主视觉，也不使用生成图像。
**NEEDS_CAPTURE（短动态演示）**：尚无 actual-run 视频。
**水下视图：DIAGNOSTIC ONLY / `UNSUPPORTED_CAMERA_VIEW`**，不发布、不声称功能。
**焦散图像：不发布**；`_FMS_CausticsEnabled = 0` 写在每张已发布图片的当帧回读里。
未使用任何生成图像冒充引擎运行结果；若使用程序生成的环境素材，会明确标注为程序生成，而不宣称为摄影 HDRI。
最新两张水面帧的反射输入就是这种素材：`FMS_Showcase_SkyCube` 由本项目编辑器内的生成器程序生成，
其 SHA-256 与生成时记录的 accepted run 一致，并且只作为天空内容被采样。

## Validation scope

Tuanjie 1.10.4（Unity 兼容版本 2022.3.62t16）、URP 14.2.0-t1、D3D11、透视验证 rig、1280×720 为主。
不声称跨 API 支持、orthographic 支持，也不提供 FPS 或 GPU per-pass 成本结论。

## Roadmap

完成 W4.0 焦散验证 → showcase 场景美术收口（已渲染；反射输入已绑定并测过，封面候选的差距现在是非水体
blockout 美术）→ 补真实 cover 与短动态
→ 如需性能结论则单独设计 Player/GPU 测量。

## Distribution

Source/project distribution is not currently provided。本仓库只发布选定文档与渲染证据，未分发完整 Unity
项目、场景、shader 或引擎/包内容；公开可见不构成开源许可，也未新建 LICENSE。
