# EXP-MOCK-01-run

MOCK orchestration smoke. Not a Story Evidence candidate.

## what was done

In-process Python (numpy `default_rng(42)`), no code-repo edit:

- Draw 100 samples from N(mean=0, std=1) and 100 from N(mean=3, std=1); n=200 total.
- Label class 0 / class 1 respectively.
- Classify with fixed midpoint threshold 1.5: predict 1 iff x >= 1.5.
- Metric: accuracy = mean(pred == y).

Command was a one-shot `python3` stdin script; no files written besides this report.

## git commit

no code change

## result location

`examples/mock-flow-detection/.research/work/EXP-MOCK-01-run.md` (this file; no separate metrics artifact)

## raw findings

- n = 200 (100 per class)
- seed = 42 (`numpy.random.default_rng`)
- threshold = 1.5
- accuracy = 0.980000 (196/200 correct)
- confusion: tn=99, fp=1, fn=3, tp=97
- class0 empirical mean/std (ddof=0) = -0.050270 / 0.772784
- class1 empirical mean/std (ddof=0) = 2.989368 / 0.974604
- class0 range = [-1.951035, 2.141648]; count x>=1.5 = 1
- class1 range = [0.867954, 5.913862]; count x<1.5 = 3

## issues

none
