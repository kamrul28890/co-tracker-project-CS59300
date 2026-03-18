# Workflow Log

## Purpose

This document keeps a continuous chronology of the reproduction workflow, environment checks, execution results, and code changes made while adapting and validating the CoTracker3 repository for this course project.

## Chronology

### 2026-03-18 18:42:09 -04:00

- Created this log to track work continuously rather than reconstructing it later.
- Current goal for this phase:
  - download the CoTracker3 checkpoints
  - run explicit online and offline demos with checkpoint arguments
  - set up the TAP-Vid DAVIS evaluation path
  - run the first evaluation command if the dataset becomes available
- Current repository context:
  - `origin` points to the course project GitHub repo
  - `upstream` points to the official Meta CoTracker repository
  - previous Windows compatibility patches for training signal handling have already been applied and pushed

### 2026-03-18 18:45:36 -04:00

- Created the local `checkpoints/` directory.
- Downloaded the explicit CoTracker3 checkpoint files referenced by the repository:
  - `checkpoints/scaled_online.pth`
  - `checkpoints/scaled_offline.pth`
- Downloaded file sizes:
  - `scaled_online.pth`: `101,695,610` bytes
  - `scaled_offline.pth`: `101,890,938` bytes
- This step makes the demos and evaluation commands reproducible without relying on implicit `torch.hub` checkpoint download behavior.

### 2026-03-18 18:46:28 -04:00

- Ran explicit checkpoint demos from the local repository instead of relying on `torch.hub` model selection:
  - `python demo.py --grid_size 10 --checkpoint .\checkpoints\scaled_online.pth`
  - `python demo.py --grid_size 10 --checkpoint .\checkpoints\scaled_offline.pth --offline`
- Both commands completed successfully and printed `computed`.
- Preserved the outputs as:
  - `saved_videos/demo_online_explicit.mp4`
  - `saved_videos/demo_offline_explicit.mp4`
- Observed one non-blocking compatibility note during checkpoint loading:
  - PyTorch emitted a `FutureWarning` about `torch.load(..., weights_only=False)` in `cotracker/models/build_cotracker.py`
  - This is not a current execution failure, but it is worth noting for future maintenance

### 2026-03-18 19:13:12 -04:00

- Began TAP-Vid DAVIS setup for the first benchmark run.
- Verified from the CoTracker evaluation code that the expected DAVIS path is:
  - `dataset_root/tapvid_davis/tapvid_davis.pkl`
- First dataset download attempt used `Invoke-WebRequest` and produced a corrupted partial archive:
  - file name: `datasets/tapvid_davis.zip`
  - size observed before retry: `203,578,094` bytes
  - extraction and decompression checks failed
- Retried the dataset download with `curl.exe`, which completed successfully.
- Verified the extracted DAVIS payload:
  - `datasets/tapvid_davis/tapvid_davis.pkl`
  - size: `2,481,403,560` bytes
- Kept the auxiliary files that shipped with the archive:
  - `datasets/tapvid_davis/README.md`
  - `datasets/tapvid_davis/SOURCES.md`
- Ran the first full DAVIS evaluation with the explicit online checkpoint:
  - `python .\cotracker\evaluation\evaluate.py --config-name eval_tapvid_davis_first exp_dir=.\eval_outputs\tapvid_davis_first_online dataset_root=.\datasets checkpoint=.\checkpoints\scaled_online.pth`
- Evaluation completed successfully.
- Runtime recorded in the result JSON:
  - `62.29804277420044` seconds
- Result artifact written to:
  - `eval_outputs/tapvid_davis_first_online/result_eval_.json`
- Baseline metrics from that first run:
  - `occlusion_accuracy`: `0.9088604772803294`
  - `average_jaccard`: `0.6444301091643813`
  - `average_pts_within_thresh`: `0.771162818724843`
- Interpretation note:
  - this is a valid first benchmark run and a useful project baseline
  - it should not yet be treated as a final paper-comparison claim without checking whether every evaluation setting matches the paper protocol exactly

### 2026-03-18 19:13:56 -04:00

- Updated `.gitignore` to keep the repository clean after the new execution artifacts were created.
- Added ignore rules for:
  - `datasets/`
  - `*.log`
- Reason for this change:
  - the downloaded DAVIS benchmark data are large and should remain local
  - generated console logs are workflow artifacts, not source files
