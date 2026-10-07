# FluidMatter — Status

更新时间：2026-10-07。

**Overall: real-time water rendering Research Prototype / 持续研发中。**

## Accepted / evidence-backed

W3.0 boundary field：15 gates × 2 passes 全 PASS，3,343 条 graded rows，0 FAIL，`isolation_unresolved = 0`。
这是**有界测试**的通过，不是所有场景、分辨率与硬件的认证。

Accepted 的模块：Linear Eye Depth 与 validity、thickness/SurfaceData 契约、screen-space refraction、
Beer–Lambert transmittance、reflection **preview**、Gerstner 位移（最多 4 slot）、Profile / binding /
shared-material-cache / MPB、debug views 与自动化验证。

**关键限定（容易被误读，明确写出）：** boundary 目前是**数据**，accepted 配置里该特性为关闭状态
（`_boundaryEnabled: 0`），没有任何 production 外观消费它；仓库中出现的两种 boundary 视觉都是诊断视图。
reflection 是 preview 路径，不是 SSR，也不是 planar reflection。

## In development

W3.1 shoreline / foam consumer：precheck 研究（岸边衰减曲线选择、foam 噪声覆盖与细节预算）与 binding
代码正在推进。源文件存在、precheck 文档存在、roadmap 写了某功能，都**不等于**验收通过。W3.1 在其自身
gate 与回归证据完成前不发布任何 PASS 声明。

## Not implemented / not claimed

shoreline appearance、shoreline foam、open-water crest foam、wake、interaction foam、underwater system、
caustics、FFT / infinite ocean、SSR、planar reflection、flow map、buoyancy、GPU benchmark、跨 API 认证。

## Visual status

**NEEDS_CAPTURE（真实水景封面）**：仓库不使用 debug mesh 或诊断色块充当项目主视觉；README 顶部图片是
accepted build 的真实 final composite 捕获，并明确标注为验证 rig，而非美术成品。
**NEEDS_CAPTURE（短动态演示）**：尚无 actual-run 视频。
未使用任何生成图像冒充引擎运行结果。

## Validation scope

Tuanjie 1.10.4（Unity 兼容版本 2022.3.62t16）、URP 14.2.0-t1、D3D11、透视验证 rig、1280×720 为主。
不声称跨 API 支持、orthographic 支持，也不提供 FPS 或 GPU per-pass 成本结论。

## Roadmap

完成并验证 W3.1 consumers → 补真实 cover 与短动态 → 如需性能结论则单独设计 Player/GPU 测量 →
W3.1 通过后才考虑 W4.0 caustics。

## Distribution

Source/project distribution is not currently provided。本仓库只发布选定文档与渲染证据，未分发完整 Unity
项目、场景、shader 或引擎/包内容；公开可见不构成开源许可，也未新建 LICENSE。
