# AlphaFold 3 Input-Directory Smoke Test

This directory contains two small AlphaFold 3 JSON inputs for testing the
`input_dir` mode of the Open OnDemand app.

Use this directory as the `input_dir` form field:

`bio_alphafold3/test_inputs/input_dir_smoke`

## Files

- `01_monomer.json`: simple single-chain protein test
- `02_homodimer.json`: simple 2-copy homomer test

## Recommended Form Settings

- `json_path`: leave blank
- `input_dir`: this directory
- `work_dir`: your project directory
- `output_dir`: `af3_input_dir_smoke_output`
- `database_dir`: `/cluster/tufts/biocontainers/datasets/alphafold3/20241219/public_databases`
- `model_dir`: your AlphaFold 3 model directory
- `max_template_date`: `2021-09-30`
- `run_data_pipeline`: checked
- `run_inference`: checked
- `num_diffusion_samples`: `1`
- `num_recycles`: `3`
- `num_seeds`: leave blank
- `flash_attention_implementation`: `triton`
- `gpu_device`: `0`
- `save_embeddings`: unchecked
- `save_distogram`: unchecked
- `force_output_dir`: unchecked
- `compress_large_output_files`: unchecked
- `jax_compilation_cache_dir`: keep the default
- `resolve_msa_overlaps`: checked
- `extra_parameters`: leave blank

## Recommended Resource Settings

- `partition`: a GPU partition
- `gpu_type`: choose one concrete GPU type that is currently available
- `num_cores`: `8`
- `num_memory`: `32`
- `bc_num_hours`: `2`

## Expected Result

AlphaFold 3 should create one output subdirectory per input JSON under the
chosen output directory:

- `af3_test_monomer`
- `af3_test_homodimer`

If this is only a quick app validation test, the monomer file alone is usually
enough. The second file is there to verify that `input_dir` mode handles
multiple JSON files correctly.
