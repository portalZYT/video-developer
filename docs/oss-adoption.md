# 开源候选审计与采用记录 (oss-adoption)

> **目的：** 记录本项目所用 / 已审计 / 明确拒绝的开源候选，统一追踪 Star 快照、许可证、复用范围、Linux 部署方式与拒绝理由。
> **维护人：** 项目负责人 / 架构师
> **更新日期：** 2026-08-05
> **依据：** `设计及任务.md` §4.1 选型硬约束 + §4.2 免费 API 与成本边界

---

## 0. 审计硬约束

- 默认 Star 门槛：**GitHub Star ≥ 6,000**（快照日期需附在条目里）
- 必须满足：未归档、近 12 个月有维护、许可证允许当前分发方式、Linux 可自动化部署、无未解决的高危 CVE
- Star 只是初筛，不替代代码/许可证审计
- 低于 6000 的候选**必须**含 `exception_owner / reason / no_alternative / expiry` 四个字段才能采用
- 所有第三方语言不限（Python / Go / Rust / C/C++），但仅以版本锁定的 CLI 或独立容器接入
- 选用 / 拒绝决定必须写本文件，并随主设计文档同步

---

## 1. 已采用（首版核心依赖）

| 工具 / 库 | 仓库 | Star (快照日期) | 许可证 | 复用范围 | Linux 部署 | 风险 | 替代方案 |
|---|---|---|---|---|---|---|---|
| **React** | facebook/react | ~230k (2026-08) | MIT | UI 框架 | npm + Vite | 生态庞大,需锁定版本 | Preact(降低体积但生态不全) |
| **Vite** | vitejs/vite | ~70k (2026-08) | MIT | 前端构建/HMR | npm | — | webpack(更慢) |
| **Ant Design** | ant-design/ant-design | ~93k (2026-08) | MIT | 唯一 UI 组件库 | npm | — | 严禁并行引入第二套通用 UI 库 |
| **TanStack Query** | TanStack/query | ~42k (2026-08) | MIT | 数据获取/缓存 | npm | — | SWR(同等) |
| **Express 5** | expressjs/express | ~66k (2026-08) | MIT | 后端 API 框架 | npm | — | NestJS(对 MVP 装饰器偏多) |
| **TypeScript** | microsoft/TypeScript | ~100k+ (2026-08) | Apache-2.0 | 前后端统一语言 | npm | — | — |
| **Zod** | colinhacks/zod | ~33k (2026-08) | MIT | 全部 schema/校验 | npm | — | io-ts/Valibot |
| **Sharp** (libvips) | lovell/sharp-libvips | ~29k+sharp / libvips 1.5k+ (2026-08) | Apache-2.0 | 图片 EXIF/色彩/尺寸 | npm + native | native 编译 | jimp(纯 JS,慢) |
| **Busboy** | mscdex/busboy | ~3k | MIT | multipart 流式解析 | npm | — | formidable(回调式) |
| **file-type** | sindresorhus/file-type | ~6.5k (2026-08) | MIT | 文件魔数验证 | npm | — | — |
| **undici** | nodejs/undici | ~6k+ (2026-08) | MIT | HTTP 客户端(DNS 固定) | npm 内置 | — | node:fetch |
| **Vitest** | vitest-dev/vitest | ~13k (2026-08) | MIT | 测试框架 | npm | — | Jest |
| **Supertest** | visionmedia/supertest | ~14k (2026-08) | MIT | API 集成测试 | npm | — | — |
| **Playwright** | microsoft/playwright | ~68k (2026-08) | Apache-2.0 | E2E 测试 | npm + browser | 浏览器体积大 | Cypress |
| **FFmpeg / ffprobe** | FFmpeg/FFmpeg | ~45k (2026-08) | LGPL/GPL | 探测/抽帧/转码/合成/字幕烧录 | 系统包 | 编译选项影响编码 | 不可替代 |
| **yt-dlp** | yt-dlp/yt-dlp | ~110k (2026-08) | Unlicense | 视频元数据/下载 | Python pip / 二进制 | 平台解析变化 | 单平台 adapters |
| **OpenCV** | opencv/opencv | ~80k+ (2026-08) | Apache-2.0 | 色调/亮度/饱和度/运动 | pip + native | 资源占用 | 纯 FFmpeg 统计降级 |
| **PySceneDetect** | Breakthrough/PySceneDetect | 需复核 ≥6k | BSD | 镜头边界 | pip | — | FFmpeg scene change |
| **librosa** | librosa/librosa | ~7k (2026-08) | ISC | BPM/能量/onset | pip | — | essentia(C++ 重) |
| **PaddleOCR** | PaddlePaddle/PaddleOCR | ~43k (2026-08) | Apache-2.0 | 字幕区域/密度(可选) | pip | CPU 成本高,允许关闭 | 关闭后由用户确认 |
| **OpenAI CLIP / OpenCLIP** | openai/CLIP / mlfoundations/open_clip | ~25k / ~10k (2026-08) | MIT / Apache-2.0 | 主题语义标签(可选) | pip | 模型体积大,允许关闭 | 关闭后由用户确认主题 |
| **阿里云 OSS SDK** | aliyun/aliyun-oss-nodejs-sdk | ~1.3k (2026-08) | MIT | 可选对象存储 | npm | 较低 Star,审计通过;仅作可选后端 | local(默认) |

> **Snap 评估原则**：snapshot 日期附在 Star 后；采纳时必须重新核对 Star / 维护状态 / 许可证 / 已知 CVE；变更需在本表更新。

---

## 2. 低于 6000 Star 但审计通过 / 部分复用

| 工具 / 库 | Star | 许可证 | 状态 | 理由 | 失效日期 |
|---|---|---|---|---|---|
| **MoneyPrinterTurbo** | ~101k (注:高于 6000,但独立项目,< 1k 维护) | MIT | 仅复用**独立分镜/模板模块** | 不整体接管其账号/数据库/Agent 系统 | — |
| **OpenMontage** | ~44.8k | — | 仅复用**独立算子** | 同上 | — |
| **Pixelle-Video** | ~26.4k | — | 仅复用**独立算子** | 同上 | — |
| **hyperframes** | ~39.3k | — | 仅复用**独立算子** | 同上 | — |
| **video-use** | ~18.6k | — | 仅复用**独立剪辑算子** | 必须有确定输入输出和无模型降级 | — |
| **NarratoAI** | ~10.6k | — | 同上 | 同上 | — |
| **autoclip** | ~6.3k | — | 临界,需负责人书面批准 | 同上 | — |
| **Subtitle Edit** | ~13.7k | — | **仅参考** ASS 格式行为测试 | 不部署 C# 桌面应用,运行时由 ASS 生成器 + FFmpeg 完成 | — |

> **审计步骤**：每次采纳前必须执行 `npm exec -- tsx scripts/audit-oss.ts --check --min-stars 6000`；低于门槛条目必须含 `exception_owner / reason / no_alternative / expiry`。

---

## 3. 明确拒绝（拒绝原因已记录）

| 工具 / 库 | Star | 拒绝日期 | 拒绝原因 | 替代方案 |
|---|---|---|---|---|
| **tnfe/FFCreator** | ~2.6k (2026-08) | 2026-08-05 | Node.js + Puppeteer/Chromium 渲染;**与 §4 选型表"Remotion/HTML 渲染"明确拒绝方案冲突**;Chromium 200-400MB 内存/任务在 4 vCPU / 8GB 单机不可行;Puppeteer 难做到严格沙箱;Chromium 版本差异破坏 §9.5 渲染输入冻结与历史可重现;Star 低于 6000 门槛 | 维持 FFmpeg + Sharp + text-cards 渲染路线;如需 HTML→视频模板引擎,优先评估 Remotion 自身(不引入其 Chromium 渲染) |
| **drawcall/inkpaint** | <1k (2026-08) | 2026-08-05 | 服务端 Canvas 2D 库(PixiJS Node 端口);**低于 6000 Star 门槛**;与现有 Sharp + FFmpeg 渲染路线重叠;不解决 StoryboardService 已用结构化数组描述场景的问题;引入额外 Scene Graph 抽象增加复杂度 | 维持 Sharp + FFmpeg + 程序化背景/text-cards 路线 |
| **flycut-caption** | ~1.7k | v0.1 评估期 | 低于 6000 Star;字幕烧录已由 FFmpeg + libass 覆盖 | FFmpeg + libass |
| **videoflow** | 极低 | v0.1 评估期 | 低于 6000 Star;维护停滞 | — |
| **short-video-factory** | ~5.1k | v0.1 评估期 | 低于 6000 Star;整体依赖重 | 复用其独立算子(spike 后再决定) |
| **NestJS** | (框架类) | v0.1 评估期 | 对当前单体 MVP 装饰器偏多,不符合"简洁易维护"目标 | Express 5 |
| **Remotion** | ~21k | v0.1 评估期 | Chromium 渲染资源占用高;音视频细节仍依赖 FFmpeg;**结论为"仅作为后续模板引擎实验"** | 维持 FFmpeg + Sharp |

---

## 4. 待评估 / 后备

| 工具 / 库 | Star | 状态 | 评估要点 |
|---|---|---|---|
| **res-downloader (putyy)** | ~18.9k | 后备 | 抖音备用解析;仅审计通过后作为 platform adapter |
| **MediaCrawler** | ~59.7k | 后备 | 同上;**不采集评论/账号** |
| **node-canvas** | ~10k | 后备 | 若未来需 server-side canvas 才评估 |
| **OGV.js / mp4box.js** | 各 1-2k | 暂不评估 | v0.1 不做浏览器内解码,服务端 FFmpeg 已覆盖 |

---

## 5. 字体与音乐资产

| 资产 | 来源 | 许可证 | 登记位置 |
|---|---|---|---|
| **Noto Sans SC** | Google Fonts | SIL OFL 1.1 | `assets/fonts/LICENSES.md` |
| **BGM 曲库(6 类)** | 自有 / 商用授权 / CC0 | 见各文件 | `assets/music/LICENSES.md` |

> 任何字体 / 音乐添加必须先登记 SHA-256 与许可证条目摘要,运行 `scripts/check-assets.ts` 通过。

---

## 6. 审计脚本

```bash
# 拒绝 Star<6000、未维护、许可证不兼容
npm exec -- tsx scripts/audit-oss.ts --check --min-stars 6000 docs/oss-adoption.md

# 校验工具/容器可用性 + JSON schema
npm exec -- tsx scripts/check-tools.ts --contract-only

# 校验字体/音乐资产 SHA-256 + 许可证摘要
npm exec -- tsx scripts/check-assets.ts --font assets/fonts/NotoSansSC-Regular.otf \
                                            --music-root assets/music \
                                            --min-tracks-per-style 1 \
                                            --verify-digests

# 校验 FFmpeg 版本与必备 filter
npm exec -- tsx scripts/check-ffmpeg.ts
```

---

## 7. 变更日志

| 日期 | 变更 | 操作人 |
|---|---|---|
| 2026-08-05 | 初始化本文件;记录首版审计结果;新增 `tnfe/FFCreator` 与 `drawcall/inkpaint` 拒绝条目 | — |
