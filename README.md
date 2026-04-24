<div align="center">
  <img src="./docs/logo-220.png" alt="Gait-PaperClaw Logo" width="120" />
# Gait-PaperClaw🚶
### 步态论文自动追踪与分析流水线
[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)](#)
[![Status](https://img.shields.io/badge/Status-MVP-2EA043)](#)
[![Workflow](https://img.shields.io/badge/Workflow-arXiv%20%E2%86%92%20Issue%20%E2%86%92%20Digest-8A2BE2)](#)
**arXiv → 单篇论文 Issue → 每日汇总 → 面向步态方向的本地优先阅读流**
English version: **[README_EN.md](./README_EN.md)**
</div>
---
## 这个项目现在做什么
Gait-PaperClaw 是在 RS-PaperClaw 基础上做的一个**步态方向本地优先 MVP 改造版**。
当前第一版目标是：
- 抓取 gait 相关 arXiv 候选论文
- 用关键词 + LLM 做二级筛选
- 同时覆盖两大类内容：
  - 身份识别相关 gait 论文
  - 临床 / 康复 / 生物力学 gait 论文
- 生成或更新单篇论文 GitHub Issue
- 生成每日 digest issue
- 同步日报到 `daily_reports/YYYYMM/YYYYMMDD.md`
---
## 第一版收录策略
当前 MVP 采用**分层收录**思路：
### 1. 核心层：身份识别相关 gait 论文
- gait recognition
- gait identification
- gait authentication
- gait retrieval
- gait re-identification
- cross-view / cross-domain gait recognition
### 2. 扩展层：步态分析相关论文
- gait analysis
- gait assessment
- abnormal gait
- gait disorder
- clinical gait
- rehabilitation gait
- biomechanics gait
- walking pattern analysis
### 3. 排除 / 降权层
- gait 不是核心问题的通用动作识别论文
- 只是顺带提到 gait 的姿态估计 / 视频理解 / 行为分析论文
- 泛运动分析但不是以 gait 为主要研究对象的论文
---
## 为什么要这样拆
步态方向内部其实差异很大。
如果不分层，很容易把：
- 步态身份识别
- 临床步态评估
- 康复分析
- 生物力学研究
全部混在一起，阅读体验会很差。
所以后续日报理想结构会逐步走向：
1. **身份识别相关**
2. **临床 / 康复 / 生物力学相关**
3. **边缘但值得关注**
目前这个仓库还处在 **MVP 改造阶段**：筛选规则和项目定位已经切到 gait，但更深的报告风格、标签体系、页面文案还会继续收口。
---
## 目前最关键的可改文件
如果你继续往下改，优先看这几个：
- `skills/rs-paper-pipeline/scripts/config/filter_keywords.json`
- `skills/rs-paper-pipeline/scripts/prompts/filter_cross_prompt.md`
- `skills/rs-paper-pipeline/.env.example`
它们分别控制：
- 候选论文检索词
- 本地硬过滤 regex
- LLM 二次筛选逻辑
- 目标仓库 / 模型 / 通知配置
---
## 快速开始
```bash
cd skills/rs-paper-pipeline
./bootstrap.sh
python3 scripts/cli.py doctor
python3 scripts/cli.py filter --dry-run --date 20260317
```
确认筛选质量没问题后，再跑：
```bash
python3 scripts/cli.py run --date 20260317 --no-notify
```
---
## 当前状态
这是一个**先本地改、先跑通筛选**的版本。
已经改动的部分：
- gait 关键词配置
- gait LLM 筛选 prompt
- 项目定位与 README 文案
下一步建议继续改：
- 单篇论文标签提取更贴近 gait 子方向
- 日报分区输出
- repo 内更多文案从遥感切到步态
- 未来按需要补 arXiv 之外的数据源
---
## 备注
- 目前流水线骨架仍继承自 `RS-PaperClaw`
- 内部部分路径名暂时还保留 `rs-paper-pipeline`
- 当前目标不是一次性全量重命名，而是先确保 **步态筛选逻辑可工作**
