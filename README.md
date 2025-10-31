# Github Actions

A collection of reusable GitHub Actions for automating common development workflows.

## Actions

### Update Scoop Bucket

**File:** `.github/workflows/update-scoop.yml`  
**Documentation:** [update-scoop.yml.md](update-scoop.yml.md)

A reusable GitHub Action that automatically updates Scoop bucket manifests when new releases are published. This action creates both latest and version-specific manifests, allowing users to install specific versions of your application.

#### Key Features

- ✅ **Direct Push to Main** - No PR creation, commits directly to main branch
- ✅ **Keeps All Versions** - Creates both latest and version-specific manifests
- ✅ **Handles New Apps** - Perfect for first-time releases
- ✅ **All Settings Parameterized** - Everything configurable for easy reuse

#### Quick Usage

```yaml
name: Update MyHosts in Scoop Bucket

on:
  workflow_run:
    workflows: ['Build and Release']
    types:
      - completed

jobs:
  update-scoop:
    uses: your-username/my-github-actions/.github/workflows/update-scoop.yml@main
    with:
      tag: ${{ github.event.release.tag_name }}
      app_name: 'myhosts'
      exe_name: 'MyHosts.exe'
      scoop_bucket_repo: 'ropean/scoop-ropean'
      source_repo: 'username/repo-name'
      description: 'A simple hosts file editor'
      homepage: 'https://github.com/username/repo-name'
      license_identifier: 'MIT'
      license_url: 'https://github.com/username/repo-name/blob/main/LICENSE'
    secrets:
      SCOOP_ROPEAN_DEPLOY_KEY: ${{ secrets.SCOOP_ROPEAN_DEPLOY_KEY }}
```

For detailed documentation, see [update-scoop.yml.md](update-scoop.yml.md).

## License

This project is provided as-is. Please ensure your applications and repositories comply with their respective licenses.
