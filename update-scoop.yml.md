# Update Scoop Bucket GitHub Action

A reusable GitHub Action that automatically updates Scoop bucket manifests when new releases are published. This action creates both latest and version-specific manifests, allowing users to install specific versions of your application.

## Features

### ✅ Direct Push to Main

- No PR creation, commits directly to main branch
- Streamlined workflow for immediate availability

### ✅ Keeps All Versions

- Creates `myhosts.json` (always latest)
- Creates `myhosts@1.2.3.json` (version-specific)
- Users can install specific versions: `scoop install myhosts@1.2.3`

### ✅ Handles New Apps

- If `myhosts.json` doesn't exist, it creates it
- Perfect for first-time releases

### ✅ All Settings Parameterized

- Everything from your bucket file is now configurable
- Easy to reuse for different apps

## Usage

### Basic Usage

```yaml
name: Release
on:
  release:
    types: [published]

jobs:
  update-scoop:
    uses: your-username/my-github-actions/.github/workflows/update-scoop.yml@main
    with:
      app_name: 'myhosts'
      exe_name: 'MyHosts.exe'
      scoop_bucket_repo: 'ropean/scoop-ropean'
      source_repo: 'username/repo-name'
      description: 'A simple hosts file editor'
      homepage: 'https://github.com/username/repo-name'
      license_identifier: 'MIT'
      license_url: 'https://github.com/username/repo-name/blob/main/LICENSE'
```

### Advanced Usage with Optional Parameters

```yaml
name: Release
on:
  release:
    types: [published]

jobs:
  update-scoop:
    uses: your-username/my-github-actions/.github/workflows/update-scoop.yml@main
    with:
      app_name: 'myhosts'
      exe_name: 'MyHosts.exe'
      scoop_bucket_repo: 'ropean/scoop-ropean'
      source_repo: 'username/repo-name'
      description: 'A simple hosts file editor'
      homepage: 'https://github.com/username/repo-name'
      license_identifier: 'MIT'
      license_url: 'https://github.com/username/repo-name/blob/main/LICENSE'
      bin_name: 'myhosts' # Optional: different binary name
      shortcut_name: 'MyHosts' # Optional: shortcut display name
      shortcut_description: 'Edit hosts file' # Optional: shortcut description
      notes: 'Run as administrator for system hosts file editing' # Optional: additional notes
```

## Inputs

### Required Inputs

| Input                | Description                 | Example                                                   |
| -------------------- | --------------------------- | --------------------------------------------------------- |
| `app_name`           | Name of the application     | `myhosts`                                                 |
| `exe_name`           | Name of the executable file | `MyHosts.exe`                                             |
| `scoop_bucket_repo`  | Scoop bucket repository     | `ropean/scoop-ropean`                                     |
| `source_repo`        | Source repository           | `username/repo-name`                                      |
| `description`        | App description             | `A simple hosts file editor`                              |
| `homepage`           | Homepage URL                | `https://github.com/username/repo-name`                   |
| `license_identifier` | License identifier          | `MIT`                                                     |
| `license_url`        | License URL                 | `https://github.com/username/repo-name/blob/main/LICENSE` |

### Optional Inputs

| Input                  | Description                            | Default    | Example                                              |
| ---------------------- | -------------------------------------- | ---------- | ---------------------------------------------------- |
| `bin_name`             | Binary name (usually same as exe_name) | `exe_name` | `myhosts`                                            |
| `shortcut_name`        | Shortcut display name                  | `app_name` | `MyHosts`                                            |
| `shortcut_description` | Shortcut description                   | Empty      | `Edit hosts file`                                    |
| `notes`                | Additional notes for the package       | Empty      | `Run as administrator for system hosts file editing` |

## Authentication

This action uses the default `GITHUB_TOKEN` provided by GitHub Actions, so no additional secrets are required.

## How It Works

1. **Version Extraction**: Extracts version from the release tag name
2. **Asset Download**: Downloads the executable from the GitHub release
3. **Hash Calculation**: Calculates SHA256 hash of the downloaded file
4. **Bucket Checkout**: Checks out the target Scoop bucket repository
5. **Manifest Creation**: Creates/updates both latest and version-specific manifests
6. **Commit & Push**: Commits changes directly to the main branch

## Generated Files

For each release, the action creates two files in the bucket:

- `bucket/myhosts.json` - Latest version (always points to the most recent release)
- `bucket/myhosts@1.2.3.json` - Version-specific manifest (for the specific release)

## Example Generated Manifest

```json
{
  "version": "1.2.3",
  "description": "A simple hosts file editor",
  "homepage": "https://github.com/username/repo-name",
  "license": {
    "identifier": "MIT",
    "url": "https://github.com/username/repo-name/blob/main/LICENSE"
  },
  "url": "https://github.com/username/repo-name/releases/download/v1.2.3/MyHosts.exe",
  "hash": "a1b2c3d4e5f6...",
  "bin": "MyHosts.exe",
  "checkver": {
    "github": "username/repo-name"
  },
  "autoupdate": {
    "url": "https://github.com/username/repo-name/releases/download/v$version/MyHosts.exe"
  },
  "shortcuts": [["MyHosts.exe", "MyHosts", "Edit hosts file"]],
  "notes": "Run as administrator for system hosts file editing"
}
```

## Installation Commands

Users can install your application using:

```bash
# Install latest version
scoop install myhosts

# Install specific version
scoop install myhosts@1.2.3
```

## Setup Requirements

1. **Scoop Bucket Repository**: You need a Scoop bucket repository (e.g., `ropean/scoop-ropean`)
2. **Repository Permissions**: Ensure the `SCOOP_BUCKET_TOKEN` has write access to your bucket repository
3. **Release Assets**: Ensure your releases include the executable file with the exact name specified in `exe_name`

## Troubleshooting

### Common Issues

1. **Asset Not Found**: Ensure the executable file name in your release matches the `exe_name` parameter exactly
2. **Permission Denied**: Verify the `SCOOP_BUCKET_TOKEN` has write access to the bucket repository
3. **No Changes**: The action will skip if the manifest hasn't changed (same version/hash)

### Debug Information

The action provides detailed logging including:

- Version extraction
- Asset download URL
- SHA256 hash calculation
- Created/updated files
- Commit messages

## License

This GitHub Action is provided as-is. Please ensure your application and bucket repository comply with their respective licenses.
