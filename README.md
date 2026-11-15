# iOS build tools

Tools for reviewed iOS builds with private inputs and results.
Keep app source, game data, signing files, and build outputs out of this repository.

## Local checks

Run `python -m unittest discover -s tests -v`.
Integration builds need an ARM64 Mac with Xcode 27.2 and Python 3.

## Build

Use [build-private.yml](.github/workflows/build-private.yml).

1. Review the workflow and the private build recipe.
2. Include `private-build.sh` and `collect-private.py` in the private source archive.
3. Set `PRIVATE_BUILD_SPEC` with private Release asset references and SHA-256 hashes.
4. Set `PRIVATE_TRANSFER_TOKEN` with a short-lived token scoped to the private repository.
5. Enable the workflow for the approved run.
6. Start the workflow manually as the repository owner.
7. Download outputs and detailed logs from the private result Release.
8. Stop the temporary transfer job, if used.
9. Remove both secrets.
10. Disable the workflow.

## Fixture check

Use [probe.yml](.github/workflows/probe.yml) for the synthetic UIKit and Metal fixture.
Set `FIXTURE_REPOSITORY`, `FIXTURE_REVISION`, and `FIXTURE_DEPLOY_KEY`.
Use a deploy key scoped to the fixture repository.
Revoke the key after the test.
Remove the three secrets after the test.
Disable the workflow after the test.

## Limits

Run trusted code only. These tools are not a sandbox for untrusted recipes.
Public logs expose run metadata. GitHub processes the private inputs.
Do not use public artifacts or caches for private data.
These tools do not install apps on a device.
Use the workflows within GitHub's service terms.
Keep detailed run records in private storage or local reports.
