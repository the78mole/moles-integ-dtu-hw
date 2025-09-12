# Production File Generation Workflow

This repository includes an automated GitHub Actions workflow that generates production files for the KiCad hardware project using the [KiCadDev Docker image](https://github.com/the78mole/docker-images).

## Automatic Generation

The workflow automatically triggers when:
- A new release is created or published
- The workflow is manually triggered via GitHub Actions

## Generated Files

When the workflow runs, it creates a complete set of production files:

### Manufacturing Files
- **Gerber files** - All PCB layers for manufacturing
- **Drill files** - Excellon format for drilling operations  
- **Manufacturing ZIP** - Combined package ready for PCB manufacturers

### Documentation
- **Schematic PDF** - Circuit diagram documentation
- **PCB Layout PDF** - Physical board layout
- **3D STEP Model** - For mechanical integration
- **Interactive HTML BOM** - Assembly reference with part placement

### Release Assets

All files are automatically uploaded to the GitHub release:
- `Moles-Integ-DTU-HW-production-{version}.zip` - Complete production package
- `Moles-Integ-DTU-HW_manufacturing.zip` - Manufacturing files only
- Individual PDFs, HTML BOM, and STEP files

## Manual Generation

### Via GitHub Actions
1. Go to the **Actions** tab in the repository
2. Select "Generate Production Files" workflow
3. Click "Run workflow"
4. Download artifacts from the completed run

### Via Docker (Local)
```bash
# Pull the KiCadDev image
docker pull ghcr.io/the78mole/kicaddev:latest

# Generate production files
docker run --rm -v $(pwd):/workspace \
  ghcr.io/the78mole/kicaddev:latest \
  kicad_export ./Moles-Integ-DTU-HW.kicad_pro
```

## File Structure

```
production/
├── gerbers/              # All Gerber layer files
│   ├── *.gtl, *.gbl     # Copper layers
│   ├── *.gto, *.gbo     # Silkscreen layers  
│   ├── *.gts, *.gbs     # Soldermask layers
│   ├── *.gtp, *.gbp     # Paste layers
│   └── *.gm1            # Mechanical layers
├── drill/
│   └── *.drl            # Excellon drill files
├── pdf/
│   ├── *_schematic.pdf  # Circuit schematic
│   └── *_pcb.pdf        # PCB layout
├── 3d/
│   └── *.step           # 3D model
├── bom/
│   └── *_ibom.html      # Interactive bill of materials
└── *_manufacturing.zip  # Ready-to-send manufacturing package
```

## Troubleshooting

### Workflow Fails
- Check that the KiCad project files are valid
- Ensure the Docker image is accessible
- Review the action logs for specific error messages

### Missing 3D Models
The workflow may show warnings about missing 3D models - this is normal and doesn't affect Gerber generation.

### Permissions
The workflow requires:
- `contents: write` - To upload release assets
- `packages: read` - To pull the Docker image from GitHub Container Registry

## Docker Image Details

The workflow uses `ghcr.io/the78mole/kicaddev:latest` which includes:
- KiCad 9.0 CLI tools
- Python automation libraries (KiKit, PCBDraw)
- Interactive HTML BOM generator
- PDF generation tools
- All necessary dependencies for headless operation