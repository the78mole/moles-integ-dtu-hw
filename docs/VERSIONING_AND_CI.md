# Semantic Versioning and CI/CD Workflows

This repository uses automated workflows for version management and production file generation.

## Semantic Versioning

### How It Works
The `semantic-versioning.yml` workflow automatically creates version tags and releases when code is pushed to the `main` branch.

### Commit Message Conventions
Use these prefixes in your commit messages to control version bumping:

- **Patch version bump (default)**: Any commit without special prefixes
  ```
  Update documentation
  Fix typo in README
  ```

- **Minor version bump**: Start commit message with `fix:` or `feat:`
  ```
  fix: Correct trace routing on power supply
  feat: Add new connector for USB-C
  ```

- **Major version bump**: Add `!` after the prefix for breaking changes
  ```
  fix!: Change pin assignment (breaking change)
  feat!: Redesign PCB layout (breaking change)
  ```

### Version Format
Tags follow the format `vX.Y.Z` (e.g., `v1.2.3`)

### Automatic Actions
When you push to main:
1. Analyzes commit messages since last tag
2. Determines version bump type
3. Creates new version tag
4. Creates GitHub release with auto-generated release notes
5. Triggers production file generation for the release

## PR Production File Checks

### Purpose
Every pull request automatically generates production files to ensure:
- KiCad project builds successfully
- No manufacturing file generation errors
- Easy preview of changes before merging

### What's Generated
For each PR, the system creates:
- Complete Gerber file set
- Excellon drill files
- Manufacturing ZIP package
- PDF documentation (schematic + PCB)
- 3D STEP model
- Interactive HTML BOM

### Accessing Files
1. Open any PR
2. Look for the automated comment with 📦 "Production Files Generated"
3. Click the download link to access workflow artifacts
4. Files are retained for 30 days

## Workflow Integration

```
Push to main → Semantic Versioning → Create Release → Generate Production Files
     ↓                                                        ↓
Pull Request → PR Production Check                    Attach to Release
```

1. **Development**: Work in feature branches
2. **PR Review**: Automatic production file generation validates changes
3. **Merge**: Push to main triggers semantic versioning
4. **Release**: Automatic release creation with production files attached

## Manual Overrides

### Manual Release
You can still create releases manually through GitHub UI - the production workflow will generate files automatically.

### Manual Production Files
Use the "Generate Production Files" workflow dispatch to create production files for any commit without creating a release.

## Benefits

- **Zero manual effort**: Everything is automated
- **Consistent versioning**: Follows semantic versioning standards
- **Quality assurance**: Every PR validates production file generation
- **Professional output**: Industry-standard manufacturing files
- **Easy access**: Production files always attached to releases