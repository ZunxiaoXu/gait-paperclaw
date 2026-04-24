# Gait-PaperClaw Pipeline Overview
## 1. 这是什么
这个目录现在是一个从 `RS-PaperClaw` 改出来的**步态论文追踪 MVP 流水线**。
当前目标不是做一个完美通用系统，而是先把下面这条链路改成可工作：
- 从 arXiv 拉取 gait 相关候选论文
- 用关键词和 LLM 做二次筛选
- 同时收录：
  - 身份识别相关 gait 论文
  - 临床 / 康复 / 生物力学 gait 论文
- 为每篇论文生成结构化 GitHub Issue
- 汇总当天论文，生成“日报 YYYYMMDD”
- 把日报同步回目标仓库 `daily_reports/YYYYMM/YYYYMMDD.md`
## 2. 当前有效的数据流
主入口：
```bash
python3 scripts/cli.py run
```
执行链路仍沿用原项目骨架：
1. `scripts/run_rs_daily_workday.py`
  负责总调度、锁文件、防重入、GitHub 连通性检查、状态落盘、可选通知。
2. `scripts/daily_arxiv_cross_filter.py`
   从 arXiv API 拉取候选论文，按 gait 关键词初筛，再用 LLM 做二次筛选，并跳过已存在于 GitHub Issues 的论文。
3. `scripts/paper_processor.py`
   单篇论文处理核心：抓元信息、下载 PDF、生成预览图、调 LLM 做摘要/标签/十问分析、创建或更新 GitHub Issue。
4. `scripts/daily_digest_llm_upgrade.py`
  读取当天论文 issue，生成“日报 YYYYMMDD” issue。
5. `scripts/sync_daily_reports_to_repo.py`
   将日报 issue 内容同步到目标仓库 `daily_reports/` 目录，并维护 `README.md`。
## 3. 当前 gait 收录范围
### 核心层
- gait recognition
- gait identification
- gait authentication
- gait retrieval
- gait re-identification
- cross-view / cross-domain gait recognition
### 扩展层
- gait analysis
- gait assessment
- abnormal gait
- gait disorder
- clinical gait
- rehabilitation gait
- biomechanics gait
- walking pattern analysis
### 排除 / 降权层
- gait 不是核心问题的通用动作识别
- 只是顺带提到 gait 的姿态估计 / 视频理解 / 行为分析
- 泛运动分析但不是以 gait 为主要研究对象的论文
## 4. 已完成的 MVP 改造点
当前已经切换的重点包括：
- `scripts/config/filter_keywords.json`
  已从遥感关键词改成 gait 三层相关词表
- `scripts/prompts/filter_cross_prompt.md`
  已从“遥感 x AI”筛选改成 gait 分层筛选
- 根目录 `README.md` / `README_EN.md`
  已切到 gait 项目定位
- `skills/rs-paper-pipeline/README.md`
  已更新为 gait 流水线说明
## 5. 还值得继续改的地方
这版还是 MVP，还没完全收口。下一步建议优先处理：
- 单篇论文标签提取，更贴近 gait 子方向
- 日报按“身份识别 / 临床康复 / 边缘关注”分区输出
- 仓库中残留的遥感文案继续替换
- 如有必要，未来再补 arXiv 之外的数据源
## 6. 配置方式
项目根目录支持 `.env` 配置文件，示例见 `.env.example`。
最少需要提供：
- `GITHUB_TOKEN`
- `BAILIAN_API_KEY`
常用可选项：
- `RS_GITHUB_REPO`
- `RS_PROXY_URL`
- `RS_WORKSPACE`
- `OPENCLAW_BIN`
- `FEISHU_TARGET`
- `DINGTALK_WEBHOOK`
- `RS_FILTER_KEYWORDS_FILE`
- `RS_FILTER_PROMPT_FILE`
## 7. 当前定位
这套东西现在更像一套**可迁移的 gait 论文自动化流水线**，而不是已经打磨完成的独立产品。
当前最优先目标是：
**先让 gait 相关候选抓取与筛选质量稳定下来，再逐步优化报告风格和展示层。**
