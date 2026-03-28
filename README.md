# Awesome AutoResearch [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A curated list of AutoResearch use cases **with optimization traces** and open source implementations. Every entry includes a link to the actual optimization trajectory so you can see what the agent tried, not just the final result.

## What is AutoResearch?

AutoResearch is, at its core, a prompt. Karpathy released it as a single markdown file - `program.md`, that instructs a coding agent (Claude Code, Codex, or similar) to follow an optimization workflow. The agent edits one file (`train.py`, that trains a language model), runs for a fixed 5 minutes on a GPU, checks whether the metric improved, and either commits the change or reverts it. Then it loops forever.

<p align="center">
<img src="autoresearch-loop.png" width="400">
</p>

The specific `program.md` that ships with AutoResearch is written for one task: training a GPT model. But the structure - iteratively optimizing a file against an evaluation metric, with a discard/keep loop - turns out to be portable. In the weeks since release, the community has adapted it to GPU kernel optimization, template engine optimization, tabular ML engineering, and more. The `program.md` for each of these looks different, but the loop is the same.

## Use Cases

| Use Case | Description | Author | Links | Traces |
|----------|-------------|--------|-------|--------|
| LLM training optimization | The original - optimize nanoGPT training code. 20 improvements found overnight on hand-tuned code | Andrej Karpathy | [GitHub](https://github.com/karpathy/autoresearch) · [Tweet](https://x.com/karpathy/status/2031088031709675676) | [progress chart](https://github.com/karpathy/autoresearch/blob/master/progress.png) |
| Speed up Shopify's template engine | 53% faster parse+render, 61% fewer allocations from 93 automated commits on Shopify's Liquid engine | Tobi Lutke (Shopify CEO) | [GitHub](https://github.com/davebcn87/pi-autoresearch) · [Tweet](https://x.com/tobi/status/2032212531846971413) | [PR](https://github.com/Shopify/liquid/pull/2056) |
| GPU kernel optimization | Autoresearch applied to CUDA kernel optimization (18 → 187 TFLOPS) | RightNow AI | [GitHub](https://github.com/RightNow-AI/autokernel) · [Tweet](https://x.com/Akashi203/status/2031533857082646769) | [progress chart](https://github.com/RightNow-AI/autokernel/blob/main/progress.png) |
| Voice agent prompt engineering | Optimize voice agent prompts with automated evaluation (score 0.728 → 0.969) | Archie Sengupta | [GitHub](https://github.com/ArchishmanSengupta/autovoiceevals) · [Tweet](https://x.com/archiexzzz/status/2033258540312510702) | [progress chart](https://pbs.twimg.com/media/HDeR_UJbkAAj1P_?format=jpg&name=4096x4096) |
| Predict baseball pitch speed | Build predictive model for pitch velocity from biomechanics data (R² 0.44 → 0.78) | Kyle Boddy (Driveline Baseball) | [Tweet](https://x.com/drivelinekyle/status/2032242254035992610) | [progress chart](https://pbs.twimg.com/media/HDP5FzEbEAAIIB0?format=jpg&name=4096x4096) |
| XGBoost for tennis match prediction | Predict ATP/WTA match outcomes - encountered and documented reward hacking | Nick Oak | [Blog](https://nickoak.com/posts/tennis-xgboost-autoresearch/) · [GitHub](https://github.com/buildoak/tennis-xgboost-autoresearch) | [blog](https://nickoak.com/posts/tennis-xgboost-autoresearch/) |
| RL post-training optimization | Autoresearch for RL hyperparameters on Qwen 0.5B + GSM8K (eval 0.475 → 0.550 in fewer steps) | Vivek Kashyap | [GitHub](https://github.com/vivekvkashyap/autoresearch-rl) · [Tweet](https://x.com/vivek_2332/status/2034137143870402935) | [progress chart](https://pbs.twimg.com/media/HDY-RmoaAAAySpk?format=jpg&name=medium) |
| Ancient scroll ink detection | Vesuvius Challenge autoresearch agent swarm for ink detection models. 4 agents 24/7, cross-scroll generalization nearly doubled | Vesuvius Challenge | [Blog](https://scrollprize.substack.com/p/we-are-cooking) | [blog](https://scrollprize.substack.com/p/we-are-cooking) |
| Earth system model optimization | Hybrid: LLM proposes formula structures, TPE optimizes parameters. Fire correlation 0.09→0.65 | Dev Paragiri (UMD CS) | [Tweet](https://x.com/devparagiri/status/2035075626273739068) · [Blog](https://paragiri.com/blog/2026/autoresearch-earth-system-models/) | [blog](https://paragiri.com/blog/2026/autoresearch-earth-system-models/) |
| Bitcoin price formula discovery | Autonomous search for best time-based formula predicting Bitcoin price. 328 experiments, 50.5% RMSE improvement over power law. Walk-forward OOS evaluation with bootstrap significance testing | Carlos Baquero | [GitHub](https://github.com/CBaquero/BTCautoresearch) | [progress chart](https://github.com/CBaquero/BTCautoresearch/blob/main/fig_experiments.png) |

## Implementations & Forks

| Project | Description | Links |
|---------|-------------|-------|
| **autoresearch** | The original - single GPU, 630 lines of Python | [GitHub](https://github.com/karpathy/autoresearch) |
| **pi-autoresearch** | Generalized as a Pi extension. Works for any optimization target - test speed, bundle size, build times, Lighthouse scores | [GitHub](https://github.com/davebcn87/pi-autoresearch) |
| **autoresearch-mlx** | Apple Silicon (MLX) port. No PyTorch required, uses unified memory | [GitHub](https://github.com/trevin-creator/autoresearch-mlx) |
| **autoresearch-win-rtx** | Windows + consumer RTX GPU port (RTX 2060 through 4090) | [GitHub](https://github.com/jsegov/autoresearch-win-rtx) |
| **autoresearch-at-home** | Distributed autoresearch - SETI@home style. Multi-agent swarm coordination | [GitHub](https://github.com/mutable-state-inc/autoresearch-at-home) |
| **autoresearch (Claude Skill)** | Generalized as a Claude Code skill for any domain | [GitHub](https://github.com/uditgoenka/autoresearch) |
| **agent-digivolve-harness** | A control layer for long-running CLI agent work. Generalizes the autoresearch keep/revert loop with persistent run state, explicit eval packages, baseline and holdout cases, and one bounded mutation per iteration | [GitHub](https://github.com/MatthewZMD/agent-digivolve-harness) |

## Contributing

Want to add a use case? Open a PR or [file an issue](https://github.com/WecoAI/awesome-autoresearch/issues).

To make our work easier, please make submissions as verifiable as possible:

- **Minimum**: a progress chart showing each experiment's score and breakthrough annotations (e.g. [Karpathy's progress chart](https://github.com/karpathy/autoresearch/blob/master/progress.png))
- **Ideal**: a public repo with per-solution code and scores (the full exploration trace), or a [Weco Observe](https://docs.weco.ai/observe) dashboard link

## License

[CC0 1.0](LICENSE)
