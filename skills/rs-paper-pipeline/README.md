# Gait Paper Pipeline
This directory contains the operational pipeline behind `Gait-PaperClaw`.
It is currently being adapted from `RS-PaperClaw` into a **gait-paper tracking MVP**.
Current scope:
1. fetch gait-related arXiv candidates
2. filter gait papers with keyword + LLM cross screening
3. preserve both identity-oriented gait papers and clinical / rehab / biomechanics gait papers
4. generate or update per-paper GitHub issues
5. generate daily digest issues
6. sync digest markdown back to the repository
7. optionally send notifications
## Current MVP framing
The current gait adaptation uses a layered scope:
- **Core identity-related gait papers**
  - gait recognition
  - gait identification
  - gait authentication
  - gait retrieval
  - gait re-identification
  - cross-view / cross-domain gait recognition
- **Extended gait papers**
  - gait analysis
  - gait assessment
  - clinical gait
  - rehabilitation gait
  - biomechanics gait
  - abnormal gait
  - gait disorder
  - walking pattern analysis
- **Exclusions / downranking**
  - generic action recognition without gait as the main problem
  - generic pose estimation / video understanding papers that only mention gait in passing
  - broad movement analysis where gait is not the paper's core target
## Structure
```text
skills/rs-paper-pipeline/
├── .env.example
├── .gitignore
├── AGENT_GUIDE_RS_PIPELINE.md
├── PROJECT_OVERVIEW.md
├── RUNBOOK_RS_PIPELINE.md
├── bootstrap.sh
├── requirements.txt
├── scripts/
│   ├── cli.py
│   ├── pipeline_config.py
│   ├── clients/
│   ├── services/
│   ├── config/filter_keywords.json
│   └── prompts/
└── SKILL.md
```
## Setup
```bash
cd skills/rs-paper-pipeline
./bootstrap.sh
```
Fill `.env` with at least:
- `GITHUB_TOKEN`
- `BAILIAN_API_KEY`
Optional:
- `DINGTALK_WEBHOOK`
- `FEISHU_TARGET`
- `RS_GITHUB_REPO`
- `BAILIAN_MODEL`
## Main commands
```bash
cd skills/rs-paper-pipeline
python3 scripts/cli.py doctor
python3 scripts/cli.py run
python3 scripts/cli.py run --date 20260317 --no-notify
python3 scripts/cli.py filter --dry-run --date 20260317
python3 scripts/cli.py reconcile --date 20260317 --dry-run
```
## Customization
If you want to keep adapting this project for gait research tracking, start with:
- `scripts/config/filter_keywords.json`
- `scripts/prompts/filter_cross_prompt.md`
- `.env.example`
These files control:
- candidate retrieval terms
- hard evidence regex patterns
- LLM cross-filter rules
- target repo / model / notification settings
## Filter configuration
The article-list filtering rules are file-based, not hardcoded in Python:
- keywords and regex: `scripts/config/filter_keywords.json`
- LLM filter prompt: `scripts/prompts/filter_cross_prompt.md`
Both local runs and GitHub Actions read the same files.
## Automation
Repository-level workflows live at:
- `.github/workflows/rs-pipeline-schedule.yml`
- `.github/workflows/rs-pipeline-manual.yml`
Both workflows execute this skill project via the unified CLI.
