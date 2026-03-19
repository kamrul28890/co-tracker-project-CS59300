# Project Scripts

This folder is reserved for project-specific wrappers and analysis scripts.

Recommended additions later:

- a batch runner for custom videos
- a result parser that turns JSON outputs into CSV rows
- a small plotting script for online-vs-offline comparisons

For now, the core commands are:

```powershell
python .\cotracker\evaluation\evaluate.py --config-name eval_tapvid_davis_first exp_dir=.\eval_outputs\tapvid_davis_first_online dataset_root=.\datasets checkpoint=.\checkpoints\scaled_online.pth
python .\cotracker\evaluation\evaluate.py --config-name eval_tapvid_davis_first exp_dir=.\eval_outputs\tapvid_davis_first_offline dataset_root=.\datasets offline_model=True window_len=60 checkpoint=.\checkpoints\scaled_offline.pth
```
