# FluidMatter — Status

更新时间：2026-10-15。

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

## Scene asset created, visual review pending

`FMS_Showcase_Island` 是项目第一个作品集展示场景（岛屿海湾、水体表面、cinematic profile、三台相机）。
它已被引擎导入并通过结构化校验（引用完整、无编译错误、无缺失引用），但**尚未渲染、尚未人工审阅**，
因此没有任何该场景的图像公开发布。

## Not implemented / not claimed

physically accurate caustics、flux-conserving caustics、ray-traced water、underwater system、
open-water crest foam、wake、interaction foam、FFT / infinite ocean、SSR、planar reflection、
flow map、buoyancy、GPU benchmark、跨 API 认证。

## Visual status

**NEEDS_CAPTURE（真实水景封面）**：首页图片是 accepted W3.1 技术捕获，并明确标注为验证 rig，而非美术成品。
仓库不使用 debug mesh 或诊断色块充当项目主视觉。
**NEEDS_CAPTURE（短动态演示）**：尚无 actual-run 视频。
未使用任何生成图像冒充引擎运行结果。

## Validation scope

Tuanjie 1.10.4（Unity 兼容版本 2022.3.62t16）、URP 14.2.0-t1、D3D11、透视验证 rig、1280×720 为主。
不声称跨 API 支持、orthographic 支持，也不提供 FPS 或 GPU per-pass 成本结论。

## Roadmap

完成 W4.0 焦散验证 → 渲染并审阅 showcase 场景 → 补真实 cover 与短动态 → 如需性能结论则单独设计
Player/GPU 测量。

## Distribution

Source/project distribution is not currently provided。本仓库只发布选定文档与渲染证据，未分发完整 Unity
项目、场景、shader 或引擎/包内容；公开可见不构成开源许可，也未新建 LICENSE。
