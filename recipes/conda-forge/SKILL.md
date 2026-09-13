---
name: conda-forge
description: Create or update conda-forge recipes, diagnose feedstock CI, and enable build platforms.
license: BSD-3-Clause
---

# Conda-Forge Skill

Use GitHub/repository tooling such as gh and git, and pixi for recipe tools as needed. Target-platform builds may require a remote runner.

Use the user's request and repository state to identify the workflow. A staged-recipes checkout usually means a new package; a feedstock usually means an existing package. Confirm the intended repository when working elsewhere.

Complete the requested change and relevant validation within the authorized scope. Status checks are read-only. Present local work for review when requested; push, open or update PRs, and dispatch CI only when authorized. Existing approval covers the reviewed action without another confirmation. Stop at the requested finish line or a concrete blocker.

## Choose the relevant guidance

- New Python package: [Python recipe template](references/example-recipe-python.md).
- New Go or Rust package: [Go template](references/example-recipe-go.md) or [Rust template](references/example-recipe-rust.md).
- ARM enablement or cross-compilation: [ARM builds](references/arm-builds-and-cross-compilation.md).
- Requested or necessary recipe-format conversion: [v0 → v1 migration](references/rattler-build-migration.md).
- Adding CLI completions: [shell completions](references/shell-completions.md).

Read only references relevant to the current decision.

## New packages

Check for an existing feedstock or overlapping submission before adding a recipe under `staged-recipes/recipes/<PACKAGE_NAME>/`. Work from a personal fork.

For Python, `pixi exec grayskull pypi --use-v1-format --strict-conda-forge <PACKAGE_NAME>` can provide a starting point; run it from `recipes/` and review the generated recipe. Use the language references for other packages.

Use verified source checksums, appropriate SPDX identifiers and packaged license files, accurate dependency placement, and meaningful package tests. Check dependency availability for the enabled platforms; related recipes submitted together may satisfy dependencies within staged-recipes. Remove unused template material.

Use current repository pinning, including `python_min` for noarch Python recipes. Supply a variant override if local validation requires one; do not assume a fixed default Python version.

Complete the repository PR template accurately. When authorized to submit, open a draft and leave readiness to the human. Do not disable platforms merely to obtain passing CI.

## Existing feedstocks

Read the current recipe, relevant review requests, and the parts of the build configuration affected by the change. Preserve the recipe format unless conversion is in scope or required for the change.

Develop on a personal fork branch based on the intended upstream revision. Inspect existing branches and local changes before synchronizing; a new branch from upstream usually avoids resetting or force-pushing fork `main`.

Keep edits focused. Recipe changes belong in `recipe/`; platform changes may also require maintained configuration such as `conda-forge.yml`. Regenerate derived CI files with conda-smithy rather than hand-editing them.

For a rebuild at the same version, check the base and published builds and bump the build number once. Keep an existing bump throughout follow-up fixes. For a new version, reset as appropriate to the recipe's build-number scheme.

## CI diagnosis and validation

For a reported CI failure, inspect the current job and causal log before choosing a local reproduction. `pixi exec cf-job-logs list-jobs --all --json <PR_URL>` and `download-log <PR_URL> <JOB_ID>` can help. Check the tool's help for supported options.

Pending jobs need not prevent investigation of completed failures. Wait when completion is needed for the next decision; avoid rerunning unchanged deterministic failures.

Treat missing candidates, imports, compilation errors, and missing commands as symptoms. Check names, variants, dependency availability, activation, and upstream requirements before changing dependencies.

Run relevant recipe lint and targeted tests. Use a matching variant for local builds where supported; local testing on a different architecture does not establish target-platform acceptance. Preserve existing meaningful tests and add coverage for demonstrated gaps.

Rerender when maintained configuration or recipe changes affect generated files, or when required by the repository. Review all generated changes before committing. Keep recipe and rerender commits separate when useful or requested.

## Final review

Compare the final diff and description with all outstanding maintainer requests and the current PR template, including disclosure requirements. Describe the problem, change, and material validation briefly. Report pending checks honestly; distinguish successful builds from publication and native runtime acceptance.

Keep existing maintainer work intact. Never push development branches to the upstream feedstock: those branches can publish packages. Do not post routine status commentary merely because a check changed.
