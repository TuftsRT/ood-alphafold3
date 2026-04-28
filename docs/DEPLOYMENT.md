# Deployment Notes

## Checklist

- Set the correct OOD cluster name.
- Verify Slurm commands are available on the OOD host.
- Verify compute nodes can load the AlphaFold 3 runtime.
- Set `ALPHAFOLD3_DB_DIR` for your site.
- Set `ALPHAFOLD3_MODULE_NAME` if your module name differs.
- Review default browser paths in `form.yml.erb`.
- Test CPU-only data pipeline mode.
- Test GPU inference mode.
- Test combined mode.

## Recommended Repository Metadata

For the public GitHub repository:

- repository name: `bc_alphafold3`
- topics:
  - `open-ondemand`
  - `appverse`
  - `batch-connect`
  - `alphafold3`
  - `bioinformatics`
  - `slurm`

## After Publishing

- enable GitHub Issues
- add a repository description
- create a first release tag such as `v0.1.0`
- submit the repository to the Open OnDemand Appverse
