<div align="center">
  <img src="./docs/logo-220.png" alt="Gait-PaperClaw Logo" width="120" />
# Gait-PaperClaw🚶
### Automated Gait Paper Tracking & Analysis Pipeline
[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)](#)
[![Status](https://img.shields.io/badge/Status-MVP-2EA043)](#)
[![Workflow](https://img.shields.io/badge/Workflow-arXiv%20%E2%86%92%20Issue%20%E2%86%92%20Digest-8A2BE2)](#)
**arXiv → Per-paper Issue Report → Daily Digest → Local-first Gait Reading Workflow**
中文版本：**[README.md](./README.md)**
</div>
---
## What this project does
Gait-PaperClaw is a local-first adaptation of RS-PaperClaw for **gait-related paper tracking**.
Current MVP scope:
- fetch gait-related arXiv candidates
- filter papers with keyword + LLM cross screening
- keep both:
  - identity-oriented gait papers
  - clinical / rehabilitation / biomechanics gait papers
- generate or update per-paper GitHub Issues
- generate daily digest issues
- sync digest markdown to `daily_reports/YYYYMM/YYYYMMDD.md`
---
## Collection strategy (MVP)
This MVP uses a **layered gait scope**:
### 1. Core identity-related gait papers
- gait recognition
- gait identification
- gait authentication
- gait retrieval
- gait re-identification
- cross-view / cross-domain gait recognition
### 2. Extended gait papers
- gait analysis
- gait assessment
- abnormal gait
- gait disorder
- clinical gait
- rehabilitation gait
- biomechanics gait
- walking pattern analysis
### 3. Exclusions / downranking
- generic action recognition without gait as the core problem
- generic pose estimation or video understanding that only mentions gait in passing
- broad sports / movement analysis where gait is not the main research target
---
## Why this split matters
The goal is to avoid mixing very different gait subareas into one noisy stream.
For later iterations, the digest should ideally separate:
1. **Identity-oriented gait papers**
2. **Clinical / rehab / biomechanics gait papers**
3. **Borderline but worth watching**
This repository is currently at the **MVP adaptation stage**: filtering and project framing are already being switched to gait, while deeper reporting style and UI wording may still need refinement.
---
## Main files to customize
If you want to continue adapting this project, start with:
- `skills/rs-paper-pipeline/scripts/config/filter_keywords.json`
- `skills/rs-paper-pipeline/scripts/prompts/filter_cross_prompt.md`
- `skills/rs-paper-pipeline/.env.example`
These control:
- candidate retrieval terms
- hard evidence regex patterns
- LLM cross-filter logic
- target repo / model / notification settings
---
## Quick start
```bash
cd skills/rs-paper-pipeline
./bootstrap.sh
python3 scripts/cli.py doctor
python3 scripts/cli.py filter --dry-run --date 20260317
```
Then, after validating the filter quality:
```bash
python3 scripts/cli.py run --date 20260317 --no-notify
```
---
## Current status
This is a **local-first MVP fork**.
What has already been adapted:
- gait-focused keywords
- gait-focused LLM filter prompt
- project framing for gait literature tracking
What is still worth improving next:
- per-paper tags for gait subdomains
- digest sectioning by gait category
- README / Pages / issue wording cleanup across the repo
- optional future sources beyond arXiv
---
## Notes
- The pipeline structure is still inherited from `RS-PaperClaw`
- Internal path names may still contain `rs-paper-pipeline` for now
- The first goal is **working gait filtering**, not a full rename everywhere
