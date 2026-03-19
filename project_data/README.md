# Project Data

This folder is for project-specific data management outside the official CoTracker benchmark layout.

Use it for:

- metadata about custom videos
- notes about sources and licenses
- small tracked text files that describe experiments

Do not commit large raw videos here. Raw clips should go in:

- `project_data/raw_videos/`

That path is intentionally ignored by Git.

Recommended source priority:

1. repository assets for quick smoke tests
2. TAP-Vid DAVIS for benchmark comparisons
3. CoTracker3_Kubric for controlled synthetic stress tests
4. custom videos for failure analysis and project-specific insights

See:

- [docs/data_sources.md](/d:/Purdue/Courses/02.%20Spring%202026%20CS%2059300-CVD/Project/co-tracker/docs/data_sources.md)
- [project_data/manifests/custom_video_catalog_template.csv](/d:/Purdue/Courses/02.%20Spring%202026%20CS%2059300-CVD/Project/co-tracker/project_data/manifests/custom_video_catalog_template.csv)
