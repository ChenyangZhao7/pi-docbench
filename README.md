# pi-docbench: A Chinese Document Retrieval Benchmark for CLI Agents

**English** | [中文](README.zh-CN.md)

**pi-docbench** is a Chinese document-retrieval benchmark for CLI agents. It uses S/M/L corpora of 10/50/100 public annual-report summaries and 163 Chinese questions to compare full-context, `grep`/`read`, directory-index, `bash`, vector-retrieval, and hybrid strategies. The repository provides questions, corpora, runner code, per-run metrics, aggregate results, and reproduction materials. The main experiments use `DeepSeek-V4.1-Flash-1017`.

## Key results

| Scale | G0 | G1 | G1+ | G2 | G3 | G4 |
|---|---:|---:|---:|---:|---:|---:|
| S | 0.975 | **0.975** | 0.950 | 0.967 | 0.958 | 0.958 |
| M | 0.950 | **0.958** | 0.933 | 0.967 | 0.900 | 0.942 |
| L | — | **0.975** | 0.950 | 0.975 | 0.950 | 0.950 |

- `grep`/`read` (G1) offers the best balance between accuracy and cost;
- G3/G4 do not consistently outperform G1;
- full-context G0 consumes about 18× more tokens per run than G1;
- S/M main groups use 3 repeats; L and hybrid use 1 repeat; all comparisons use core20.

## Repository layout

```text
code/                 Runner, grading, aggregation code and configs
corpus/               S / M / L public annual-report summaries and index copies
docs/                 Task book, grading and question-design notes
questions/            163 questions, core20, scale expectations
report/               Final report, talk, slides, trajectory analysis
results/              Aggregate metrics, scores, audits, human review
reproduction/         Second-person reproduction kit (S / G1)
scripts/              Reproduction scripts
tools/                CNINFO annual-report summary crawler
```

## Quick start

```powershell
# 1. Install Python dependencies as needed
pip install -r tools/cninfo-summary-crawler/requirements.txt

# 2. Set the model key
$env:DEEPSEEK_API_KEY = "sk-..."

# 3. Run core20 on S / G1 (from reproduction/)
cd reproduction
powershell -ExecutionPolicy Bypass -File scripts\reproduce_core20.ps1
```

Manual run:

```powershell
python code/runner/batch.py `
  --questions questions_core20.jsonl `
  --corpus corpus/S `
  --groups G1 `
  --scale S `
  --repeats 3 `
  --metrics results/metrics_G1_S_core20.jsonl
```

> Reproduction is intended to be performed independently by a second person. This repository provides scripts, protocol, and baselines; it does not claim that independent reproduction has been completed.

## Data and scope

- Corpora are public 2025 annual-report summaries from CNINFO;
- No internal company documents, personal ID numbers, bank accounts, or private contact details are included;
- Shareholder/executive names and company contact details in annual reports are public disclosures;
- Copyright in the underlying reports belongs to the respective companies/platforms; check their terms before reuse.

## Reproduction status

The reproduction kit is under `reproduction/` and includes the S corpus, core20 questions, runner/grading code, baseline, and record sheet. Independent reproduction by a second person is still pending; the repository does not include the author's local run traces.

## License

MIT
