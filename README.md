# rutken_ci_test

A test bed for Rutken's CI/CD integration. `.github/workflows/rutken-ci.yml`
runs two Rutken gates on every push/PR:

1. **APK gate** - `rutken app.apk rules --baseline ci/rutken-baseline.json
   --fail-on medium`. Fails the build only on findings that are **new** or
   whose severity **escalated** relative to the committed baseline. Findings
   the baseline already accepts do not fail it.
2. **Pipeline scan** - `rutken cicd scan .github/workflows --fail-on high`.
   Scans this repo's own workflow files for insecure CI/CD patterns.

## One-time setup

Rutken is a private tool, so CI builds it from source. Add a repository secret
so the workflow can check it out:

- **Name:** `RUTKEN_REPO_TOKEN`
- **Value:** a PAT / fine-grained token with **read** access to
  `WRenchJr42/rutken_static_analyzer_private`.

Add it under *Settings > Secrets and variables > Actions*, or:

```sh
gh secret set RUTKEN_REPO_TOKEN -R WRenchJr42/rutken_ci_test
```

## Re-baselining

The baseline is recorded deliberately, never on every run:

```sh
rutken app.apk rules --baseline-out ci/rutken-baseline.json
```
