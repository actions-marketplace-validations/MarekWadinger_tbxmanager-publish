# tbxmanager-publish

GitHub Action to automatically publish MATLAB packages to the [tbxmanager registry](https://github.com/MarekWadinger/tbxmanager-registry).

## What it does

When you create a GitHub Release, this action:

1. Reads `tbxmanager.json` from your repo
2. Builds a zip archive of your MATLAB code (or uses pre-built archives for MEX packages)
3. Uploads the archive to the release
4. Computes SHA256 hashes
5. Opens a PR to the tbxmanager registry

## Quick start

### 1. Add `tbxmanager.json` to your repo root

```json
{
  "name": "my-package",
  "version": "1.0.0",
  "description": "What my package does",
  "authors": ["YourGitHubUsername"],
  "license": "MIT",
  "homepage": "https://github.com/you/your-repo",
  "matlab": ">=R2022a",
  "platforms": {
    "all": {}
  },
  "dependencies": {}
}
```

### 2. Add the workflow

Copy [`example-workflow.yml`](example-workflow.yml) to `.github/workflows/tbxmanager-publish.yml` in your repo.

### 3. Add the registry token

Go to **Settings > Secrets > Actions** and add `TBXMANAGER_REGISTRY_TOKEN` -- a GitHub PAT with write access to `MarekWadinger/tbxmanager-registry`.

### 4. Create a release

```bash
git tag v1.0.0 && git push --tags
```

Then create a GitHub Release from the tag. The action handles everything else.

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `registry-token` | Yes | -- | GitHub token with write access to the registry repo |
| `registry-repo` | No | `MarekWadinger/tbxmanager-registry` | Registry repository (owner/repo) |
| `package-file` | No | `tbxmanager.json` | Path to package metadata file |
| `archive-dir` | No | `dist` | Directory for pre-built platform archives |

## Platform-specific packages (MEX)

For packages with compiled MEX files, pre-build archives per platform and place them in `dist/`:

```
dist/
  my-package-win64.zip
  my-package-maci64.zip
  my-package-glnxa64.zip
```

And declare the platforms in `tbxmanager.json`:

```json
"platforms": {
  "win64": {},
  "maci64": {},
  "glnxa64": {}
}
```

## License

MIT
