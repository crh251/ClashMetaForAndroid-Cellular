# ClashMetaForAndroid-Cellular

This repository is a **patch-only** overlay for the upstream **MetaCubeX/ClashMetaForAndroid** project.

## Maintenance model

- **Only the `main` branch is used.**
- **No version branches** should be created in this repository.
- **Upstream source code is never committed here.**
  - The GitHub Action downloads the upstream release tarball at runtime into the runner workspace.
  - The upstream tree exists only temporarily during the build.
- All local changes live as **versioned patch files** under `patches/`.

## Versioning rule

- Upstream versions are tracked by **Git tags**, not branches.
- Patch files must match the upstream release tag exactly.
  - Example: `patches/v2.11.33.patch`
- The workflow only accepts a **dropdown release tag input** named `release_tag`.
  - Current supported value: `v2.11.33`

## GitHub Actions workflow

The workflow at `.github/workflows/build-patched-arm64.yml` does the following:

1. Checks out this repository.
2. Downloads the upstream tarball for the selected `release_tag`.
3. Extracts it into a temporary `src/` directory.
4. Applies the matching patch file from `patches/<release_tag>.patch`.
5. Builds an arm64 APK.
6. Renames the APK so it ends with `-Cellular`.
7. Uploads the APK as a workflow artifact.

## What to update when upstream changes

When a new upstream release tag is needed:

1. Generate a new patch file named after the tag.
   - Example: `patches/v2.11.34.patch`
2. Update the workflow dropdown options if you want the new tag selectable.
3. Keep the repository itself on `main` only.

## Current supported release

- `v2.11.33`

## Notes for future maintainers

- If the patch no longer applies, it means the upstream tag changed or the patch must be regenerated for the selected release tag.
- Keep the repository small: only workflow files, patch files, and documentation belong here.
- Do not add the full upstream source tree to this repository.
