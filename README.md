# Auto LOD Generator

Personal fork for custom Unity workflow and mesh-specific LOD behavior.

[![Unity](https://img.shields.io/badge/Unity-2021.3%2B-black.svg)](https://unity.com/)
[![License](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](LICENSE)

This repository is a cleaned-up fork of the original Auto LOD Generator project. The goal is to keep the Unity package lean, remove upstream CI/release automation noise, and add custom workflow tweaks for personal use.

## Why this fork exists

- keep the package focused on the Unity editor workflow
- remove unrelated GitHub automation from the repository
- support custom per-mesh LOD configuration for multi-mesh models
- keep the project compatible with the original GPL v3 license terms

## Installation

### Unity Package Manager

1. Open Window > Package Manager
2. Click + > Add package from disk
3. Select the repository root folder containing package.json

### Required dependency

This package expects the UnityMeshSimplifier dependency to be available in the project. If it is not installed automatically, add it manually from the upstream dependency source.

## Basic usage

1. Open Tools > Auto LOD Generator
2. Select a mesh-bearing GameObject in the scene or hierarchy
3. Choose a preset or adjust the config
4. Generate the LOD group

## Notes for this fork

This fork intentionally keeps a minimal repository layout for personal use:

- no automated release workflow
- no upstream CI noise
- no contributor automation boilerplate
- only the Unity package code and fork-specific documentation remain

## License

This project remains under the GNU General Public License v3.0.

See [LICENSE](LICENSE) for the full license text.

The plugin automatically detects and handles both static and skinned meshes:

| Feature | Static Mesh | Skinned Mesh |
|---------|-------------|--------------|
| Component | MeshFilter + MeshRenderer | SkinnedMeshRenderer |
| Bone Preservation | N/A | ✅ Full support |
| Blend Shapes | N/A | ✅ Preserved |
| Animation | N/A | ✅ Works seamlessly |

### Custom LOD Settings

In the main window, expand **Advanced Settings** to customize:

- **Quality Factors** - Mesh quality for each LOD level (0.0 - 1.0)
- **Screen Transition Heights** - When each LOD level activates based on screen coverage (0.0 - 1.0)
- **Culled Level** - Optional level that completely hides the object at distance

### Batch Processing

1. Open the main window and go to the **Batch Process** tab
2. Drag multiple objects or click **Add from Selection**
3. Configure settings
4. Enable **Save Meshes to Assets** if needed
5. Click **Process X Objects**

---

## API Usage

For programmatic access, use the `LODGeneratorCore` class:

```csharp
using Plugins.AutoLODGenerator.Editor;

// Generate LOD group with default settings
var settings = new LODGeneratorSettings();
settings.ApplyPreset(LODPreset.Balanced);

var result = LODGeneratorCore.GenerateLODGroup(myGameObject, settings);

if (result.Success)
{
    Debug.Log($"Generated LOD group: {result.GeneratedLODGroup.name}");
    Debug.Log($"Original vertices: {result.OriginalVertexCount}");
}

// Generate with mesh saving
var resultWithSave = LODGeneratorCore.GenerateLODGroup(
    myGameObject,
    settings,
    saveMeshesToAssets: true,
    meshSavePath: "Assets/MyLODs");

// Simplify a single mesh
var simplifyResult = LODGeneratorCore.GenerateSimplifiedMesh(
    myGameObject,
    quality: 0.5f,
    saveMeshToAssets: true);

// Batch processing
var batchResult = LODGeneratorCore.ProcessBatch(
    gameObjectArray,
    settings,
    progressCallback: (progress, status) => Debug.Log($"{progress:P0} - {status}"),
    saveMeshesToAssets: true
);

// Save/Load custom presets
settings.SaveAsPreset("MyCustomPreset");
settings.LoadPreset("MyCustomPreset");

// Get available custom presets
string[] presets = LODGeneratorSettings.GetCustomPresetNames();

// Check mesh renderer type
var type = LODGeneratorCore.GetMeshRendererType(gameObject);
if (type == MeshRendererType.SkinnedMeshRenderer)
{
    Debug.Log("This is a skinned mesh!");
}
```

---

## Requirements

| Requirement | Version |
|-------------|---------|
| Unity | 2021.3 LTS or newer |
| UnityMeshSimplifier | Latest from [GitHub](https://github.com/Whinarn/UnityMeshSimplifier/) |
| Platform | Editor only (runtime not required) |

---

## Troubleshooting

### "Object does not have a valid mesh"
**Cause**: The selected object is missing required components.

**Solution**: Ensure the object has either:
- `MeshFilter` + `MeshRenderer` components, OR
- `SkinnedMeshRenderer` component

### LOD transitions are too aggressive/subtle
**Cause**: Default transition heights don't match your use case.

**Solution**: 
- Adjust Screen Transition Heights in Advanced Settings
- Try a different preset (Quality = gradual, Performance = aggressive)

### Simplified mesh looks bad
**Cause**: Some mesh topologies don't simplify well at low quality.

**Solution**:
- Increase the quality factor
- Try Quality preset instead of Performance
- Some meshes (with complex topology) may need manual LOD creation

### Skinned mesh animations break after LOD generation
**Cause**: Skeleton hierarchy or bone references were not preserved.

**Solution**:
- Ensure the original skeleton hierarchy is preserved
- Check that bone references are correctly assigned
- Verify the original `SkinnedMeshRenderer` has valid bone weights

---

## FAQ

**Q: Can I use this at runtime in my game?**

A: No, this is an Editor-only tool. The generated LOD groups work at runtime, but the generation process must happen in the Unity Editor.

**Q: Where are my custom presets saved?**

A: By default, presets are saved to `Assets/Editor/AutoLODGenerator/Presets/` in your project. This location survives package updates. You can change this in the Presets tab.

**Q: Will my old presets work after updating the package?**

A: Yes! The plugin automatically detects and migrates presets from the old location to the new location.

**Q: Does this work with procedural/mesh-animated objects?**

A: Yes, as long as the mesh has valid geometry. The simplified mesh will maintain the same structure as the original.

**Q: Can I edit the generated meshes?**

A: Yes, if you enable "Save Meshes to Assets", the meshes are saved as `.asset` files and can be modified like any other mesh asset.

**Q: What mesh formats are supported?**

A: Any mesh that Unity can import (FBX, OBJ, DAE, etc.) as long as it's a valid mesh with triangles. Imported prefab roots are supported when their child objects contain valid MeshRenderer or SkinnedMeshRenderer components, which covers common GLB/glTF and USDZ workflows after an appropriate Unity importer has converted the file into GameObjects.

---

## Contributing

Contributions are welcome! Please read our [Contributing Guidelines](CONTRIBUTING.md) before submitting issues or pull requests.

### Development Setup
1. Clone this repository
2. Create a new Unity 2021.3+ project (or use an existing one)
3. In Package Manager, click **+ > Add package from disk** and select the `package.json` at the repo root
4. Install the [UnityMeshSimplifier](https://github.com/Whinarn/UnityMeshSimplifier/) dependency
5. Import the demo scene from **Package Manager > Auto LOD Generator > Samples**
6. Make your changes and submit a pull request

---

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history and release notes.

---

## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.

---

## Credits

- Built on [UnityMeshSimplifier](https://github.com/Whinarn/UnityMeshSimplifier/) by Mattias Edlund
