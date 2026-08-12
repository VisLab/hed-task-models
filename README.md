# hed-task-models

Experimental development of graph models for tasks: directed-graph descriptions of the task structure of neuroscience and behavioral experiments, extracted from the events of BIDS datasets. Part of the HED (Hierarchical Event Descriptors) family of repositories.

The specification is [`model_rules.md`](model_rules.md): numbered rules for what a model is and how one is built. Worked examples live under `json_examples/`, one directory per task:

```
json_examples/
|-- sternberg/                    - modified Sternberg working memory task
    |-- data/                     - dataset README, events sidecar, one sample
    |                               run, and dataset_source.json (where the
    |                               full source dataset lives)
    |-- models/
        |-- draft/                - an early draft model, kept for comparison
        |-- event_type_task_role/ - the current model, its keymap, and reports
```

Source datasets are not stored here. They live in the `hed-examples` repository, and each model records its provenance against its source dataset; `dataset_source.json` in an example's `data/` directory gives the dataset root, resolved relative to that file.

This repository holds no code. The tools that build and read these models belong to the `hed-vis` project.
