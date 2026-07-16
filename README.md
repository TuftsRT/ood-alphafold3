# AlphaFold 3 Open OnDemand App

This repository contains an Open OnDemand Batch Connect app for launching
[AlphaFold 3](https://github.com/google-deepmind/alphafold3) on a Slurm-based
HPC cluster.

The app is structured so this directory can stand on its own as a GitHub
repository for Appverse-style sharing.

## Features

- Targets AlphaFold 3 `v3.0.2` parameters in the launch form.
- Supports either `--json_path` or `--input_dir`.
- Exposes useful upstream flags including `run_data_pipeline`,
  `run_inference`, `num_diffusion_samples`, `num_recycles`, `num_seeds`,
  `conformer_max_iterations`, `flash_attention_implementation`, `gpu_device`,
  `save_embeddings`, `save_distogram`, `force_output_dir`,
  `compress_large_output_files`, `jax_compilation_cache_dir`, and
  `resolve_msa_overlaps`.
- Dynamically discovers Slurm partitions, GPU types, CPU limits, memory limits,
  and wall-time limits at form-render time.
- Derives Slurm GPU submission flags from the selected partition and GPU type.
- Supports data-pipeline-only, inference-only, or combined runs.

## Repository Layout

- `manifest.yml`: Open OnDemand app metadata.
- `form.yml.erb`: Main launch form.
- `form.js`: Dynamic browser logic for partition and resource fields.
- `submit.yml.erb`: Slurm submission template.
- `/var/www/ood/apps/sys/ood-resource-discovery/resource_discovery.erb`:
  shared Slurm resource discovery helper used by this app.
- `template/before.sh.erb`: Session metadata and working-path setup.
- `template/script.sh.erb`: Main AlphaFold 3 launcher.
- `view.html.erb`: Session card view.
- `docs/DEPLOYMENT.md`: Site deployment checklist.

## Requirements

- Open OnDemand with Batch Connect support.
- Slurm scheduler.
- AlphaFold 3 installed and runnable on compute nodes.
- AlphaFold 3 model parameters obtained from Google DeepMind.
- Accessible AlphaFold 3 databases on the compute nodes.
- Either:
  - a site module named like `alphafold3/<version>`, or
  - an environment override for `ALPHAFOLD3_MODULE_NAME`

## Site-Specific Configuration

This app is meant to be reusable, but each site should review a few defaults.

### In `form.yml.erb`

Review these hardcoded defaults:

- `cluster: "pax"`: Open OnDemand cluster name.
- `/cluster/tufts`: default path-selector browser root (used by the directory pickers).
- the empty Model directory `value`: set a site default here if desired.

This app currently expects the shared site-level resource discovery partial at:

`/var/www/ood/apps/sys/ood-resource-discovery/resource_discovery.erb`

### In `template/script.sh.erb`

Review these runtime environment variables:

- `ALPHAFOLD3_MODULE_NAME`: module loaded before launch.
- `ALPHAFOLD3_DB_DIR`: AlphaFold 3 database directory.

If your site does not use modules, replace the module-loading section with your
preferred environment setup.

## Installation

1. Copy this app directory into your Open OnDemand system app location.
2. Review `form.yml.erb` and update site defaults if needed.
3. Review `template/script.sh.erb` and set the database/module strategy for
   your cluster.
4. Confirm compute nodes can execute `run_alphafold.py`.
5. Confirm the selected partitions expose the expected GPU and CPU resources to
   Slurm.
6. Launch a small test job from Open OnDemand.

## Testing

Useful syntax checks:

```bash
erb -x -T '-' form.yml.erb | ruby -c
erb -x -T '-' submit.yml.erb | ruby -c
node --check form.js
perl -0pe 's/<%= context\.version %>/3.0.2/g' template/script.sh.erb | bash -n
```

Then test three launch modes in Open OnDemand:

1. Data pipeline only
2. Inference only
3. Combined pipeline + inference

## Troubleshooting

- If inference is enabled on a CPU-only partition, the form and submit logic
  will block the job.
- If jobs fail immediately, verify the AlphaFold 3 module and database path on
  the compute node.
- If `json_path` and `input_dir` are both set, the launcher exits with a clear
  validation error.
- If custom paired MSAs are used, review `resolve_msa_overlaps` against the
  official AlphaFold 3 input documentation.

## Known Limitations

- This app assumes Slurm.
- Resource discovery depends on Slurm commands such as `scontrol` and `sinfo`
  being available where the OOD form renders.
- This app currently depends on a shared site-level resource-discovery partial
  rather than bundling that helper inside the app directory.
- The app does not bundle AlphaFold 3 itself or model parameters.
- Sites may need to adjust GPU naming conventions if their Slurm `Gres` and
  feature labels differ.

## Upstream References

- [AlphaFold 3 repository](https://github.com/google-deepmind/alphafold3)
- [AlphaFold 3 input documentation](https://github.com/google-deepmind/alphafold3/blob/main/docs/input.md)
