# FluidMatter — Status

更新时间：2026-10-07。

**Overall: Unity URP water rendering Research Prototype / WIP.**

**Implemented**：Linear Eye Depth、screen-space refraction、Beer–Lambert transmittance、reflection preview、Gerstner wave displacement、SurfaceData、boundary field、Profile / binding / cache。

**W3.0 = evidence-backed**：既有阶段报告、reference/capture 对照与回归记录支持边界数据契约；这是有界测试结果，不是所有场景和硬件的认证。

**W3.1 = WIP**：shoreline / foam consumer 在研。源文件或 precheck 的存在不等于完成验收。

**Roadmap**：完成并验证 W3.1 consumers；补真实 cover 和短动态；若需性能结论，单独设计 Player/GPU 测量。

**Not completed / not claimed**：shoreline foam、underwater system、FFT ocean、SSR、planar reflection、GPU benchmark。既有 reflection 是 preview；既有 boundary colour 是 diagnostic view。

**NEEDS_CAPTURE**：老师可理解的真实水景封面与波面/Debug 短动态。没有使用生成图片冒充运行证据。

**Validation scope**：Tuanjie 2022.3.62t16、URP 14.2.0-t1、D3D11、perspective rig；不声称跨 API、orthographic 支持或 GPU per-pass 性能。

Source/project distribution is not currently provided. 没有完整 Unity 项目或源码发行，没有新建 LICENSE。
