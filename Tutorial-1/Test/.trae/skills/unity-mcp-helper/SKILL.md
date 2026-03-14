---
name: unity-mcp-helper
description: Use when user mentions Unity MCP, CoplayDev/unity-mcp, or wants to control Unity Editor.
---

# Unity MCP Helper Skill

## Quick Tool Selection Guide

### Scene Operations → `manage_scene`
- Create new scene
- Load/save scene
- Get scene hierarchy
- Switch between scenes

### GameObject Operations → `manage_gameobject`
- Create/destroy GameObjects
- Modify components (add/remove/update)
- Set parent/child relationships
- Change tags, layers, names
- Find objects by name/type

### Asset Management → `manage_asset`
- Import assets
- Create/modify/delete assets
- Organize project folders

### Script Management → `manage_script`
- Create new C# scripts
- Read existing scripts
- Update/modify scripts
- Delete scripts
- Get compiler diagnostics (errors with line numbers)

### Shader Management → `manage_shader`
- Create shaders
- Modify shader properties
- Apply shaders to materials

### Animation → `manage_animation` (v9.4.6+)
- Create animation clips
- Manage animation states
- Set keyframes

### Camera → `manage_camera` (v9.5.2+)
- Create cameras
- Configure Cinemachine (presets, priority, noise, blending)
- Set camera properties

### Graphics → `manage_graphics` (v9.5.3+)
- Volume/post-processing settings
- Light baking
- Rendering stats
- Pipeline settings
- URP renderer features

### ProBuilder → `manage_probuilder` (v9.4.8+)
- Mesh editing
- ProBuilder operations

### Editor Control → `manage_editor`
- Query editor state
- Change editor settings
- Get editor information

### Console → `read_console`
- Get console messages
- Clear console
- View logs/errors/warnings

### Menu Items → `execute_menu_item`
- Execute any menu item by path (e.g., "File/Save Project")
- Trigger editor commands

### Batch Operations → `batch_execute`
- Execute multiple operations in one call
- Configure batch limits
- Atomic operations (fail on any error)

### Tool Management → `manage_tools`
- Enable/disable tools
- Real-time tool toggling

### Screenshot → Built-in to manage_editor/tools
- Multi-view screenshot (v9.4.8+)
- Game view captures
- Scene view captures

## Version Features

### v9.5.3 (Latest Beta)
- `manage_graphics`: 33 actions for volume/post-processing, light baking, rendering stats
- New resources: `volumes`, `rendering_stats`, `renderer_features`

### v9.5.2
- `manage_camera`: Full Cinemachine support
- Camera presets, priority, noise, blending, extensions

### v9.4.8
- Multi-view screenshot
- `manage_probuilder`: ProBuilder mesh editing
- `manage_tools`: Real-time tool toggling
- Skill sync window
- One-click Roslyn installer

### v9.4.7
- Per-call Unity instance routing
- macOS pyenv PATH fix
- Domain reload resilience

### v9.4.6
- `manage_animation`: Animation tool
- Cline client support
- Stale connection detection
- Tool state persistence across reloads

## Common Workflows

### Create a Player Character
```
1. manage_gameobject: Create capsule
2. manage_script: Create PlayerController.cs
3. manage_gameobject: Add component to capsule
4. manage_asset: Save as prefab
```

### Set Up a Scene
```
1. manage_scene: Create new scene
2. manage_gameobject: Create directional light
3. manage_gameobject: Create ground plane
4. manage_camera: Set up main camera with Cinemachine
5. manage_graphics: Configure post-processing
```

### Debug and Fix
```
1. read_console: Get error messages
2. manage_script: Read the problematic script
3. manage_script: Update with fixes
```

### Batch Import
```
1. batch_execute: Multiple asset imports in one call
```

## Tool Parameters Reference

### manage_gameobject
- `action`: create|modify|delete|find|get_components|add_component|remove_component
- `name`: GameObject name
- `parent`: Parent GameObject path
- `components`: Array of components to add
- `properties`: Component properties to modify

### manage_script
- `action`: create|read|update|delete
- `path`: Script path
- `content`: Script content (for create/update)
- `namespace`: Optional namespace

### manage_scene
- `action`: create|load|save|get_hierarchy|unload
- `path`: Scene path

### manage_asset
- `action`: import|create|modify|delete|move
- `path`: Asset path
- `type`: Asset type

### manage_editor
- `action`: get_state|set_setting|execute_command
- `key`: Setting key
- `value`: Setting value

### read_console
- `mode`: get_messages|clear
- `filter`: error|warning|log|all

### execute_menu_item
- `path`: Menu item path (e.g., "File/Save Project")

### batch_execute
- `operations`: Array of operations to execute
- `atomic`: true|false (rollback on error)

## Best Practices

1. **Always check console first**: If something fails, use `read_console` to see errors
2. **Use batch_execute**: Group multiple operations to reduce HTTP calls
3. **Check version**: Newer versions have more tools (graphics, camera, animation)
4. **Screenshot for verification**: Use multi-view screenshot to verify visual changes
5. **Save frequently**: Use `execute_menu_item` with "File/Save Project"

## Resources (v9.5.3+)

The MCP exposes these resources for context:
- `volumes`: Volume profiles and settings
- `rendering_stats`: Rendering pipeline statistics
- `renderer_features`: URP renderer features
- `cameras`: Available cameras in scene

## Troubleshooting

### Connection Issues
- Check Unity MCP window: Window > Unity MCP
- Verify server is running on localhost:8080
- Try restarting the MCP server

### Script Compilation Errors
- Use `manage_script` with action "read" to get full diagnostics
- Check `read_console` for detailed error messages with line numbers

### Tool Not Available
- Check version: newer tools require v9.4.6+
- Use `manage_tools` to enable disabled tools

## Installation

```bash
# Unity Package (Package Manager)
https://github.com/CoplayDev/unity-mcp.git?path=/MCPForUnity#main

# Python Server
pip install unity-mcp
# or with uv
uv pip install unity-mcp
```

## MCP Configuration

```json
{
  "mcpServers": {
    "unity-mcp": {
      "command": "python",
      "args": ["-m", "unity_mcp.server"],
      "env": {
        "UNITY_MCP_PORT": "8080"
      }
    }
  }
}
```
