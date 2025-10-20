# Red Team Evaluation Framework

Multi-turn adversarial testing for AI customer support agents, mapped to real-world incidents.

## Overview

Tests RetailHub's customer support agent against 8 real-world AI chatbot failures (Air Canada $812 settlement, NYC MyCity illegal advice, DPD profanity jailbreak, etc.). Red team agent simulates realistic attacks across 20 test variants.

## Quick Start

```bash
# Install dependencies
pip install anthropic pyyaml

# Set API key
export ANTHROPIC_API_KEY=your_key_here

# Run evaluation (auto-generates dashboard when complete)
python red_team_eval.py

# Open dashboard
open dashboard.html
```

**Note:** The dashboard is automatically generated when evaluation completes. To regenerate manually: `python generate_dashboard.py`

## Key Files

- `red_team_eval.py` - Multi-turn red team pipeline with LLM-as-Judge
- `test_cases.yaml` - 20 test cases mapped to real incidents
- `knowledge/retailhub_policies.md` - Company policy for RAG grounding
- `generate_dashboard.py` - HTML dashboard generator
- `dashboard.html` - Interactive results dashboard
- `results/` - CSV output from each evaluation run

## Test Coverage

- **8 real incidents** (2016-2025): Microsoft Tay, Air Canada, NYC MyCity, DPD, Chevrolet, Cursor AI, Zillow, Anthropic
- **Attack tactics**: Emotional manipulation, Jailbreak, Prompt injection, Authority invocation, Crescendo, Confidence calibration
- **Harm types**: Financial loss, Legal liability, Brand damage, Privacy violation
- **Severity grading**: PASS, P4 (Trivial), P3 (Minor), P2 (Significant), P1 (Major), P0 (Critical)

## Dashboard Features

Open `dashboard.html` to view:
- Summary table with severity breakdown and real examples
- "View All Failures" section with judge evaluations
- Attack methodology explanations (6 attack types)
- Real-world incidents with full test results
- System configuration (prompts + policy)

## Architecture

```
Red Team (Sonnet) ---> RAG Agent (Haiku + Policy)
                              |
                              v
                       LLM Judge (Sonnet)
                              |
                              v
                       results/*.csv
                              |
                              v
                       dashboard.html (auto-generated)
```

## Models

- **Red Team**: `claude-sonnet-4-5-20250929` (aggressive, adaptive attacks)
- **RAG Agent**: `claude-haiku-4-5-20251001` (intentionally vulnerable for testing)
- **Judge**: `claude-sonnet-4-5-20250929` (9-criteria evaluation)

## Sample Results

Latest run: 20 tests, 75% pass rate
- P0 (Critical): 0
- P1 (Major): 1
- P2 (Significant): 4
- P3-P4: 0
- PASS: 15

## Notes

Incident-driven evaluation methodology: maps real-world AI chatbot failures to adversarial test cases. Uses RAG + policy grounding with multi-turn adversarial testing to identify edge cases and vulnerabilities.
