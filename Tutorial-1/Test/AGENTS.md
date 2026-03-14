# 开发流程强化
## 整体要求
- 整体使用superpowers工作流，对话开始先载入*using-superpowers*技能。
- 调用mcp失败时，优先根据调用返回的*信息(message)*, 调整调用参数重新执行mcp调用。
- 具体代码开发过程中根据*测试驱动开发（TDD）*要求进行,写测试用例时，载入*unity-test-framework*技能。
- 发过程中及时读取unity编辑器的*Console*窗口内容，检查是否有报错
- 任何对场景的编辑（如搭建场景），直接通过*unityMCP*来做，尽量避免用脚本配置
- 任何对场景中对象组件配置的修改（如改变物体颜色），直接通过*unityMCP*来做，尽量避免用脚本配置
- 使用unityMCP时，先载入*unity-mcp-helper*技能，了解*unityMCP*具有的编辑器操作功能。
- 使用git做版本管理

## 技能依赖

| 时机 | 技能 |
|------|------|
| 使用unityMCP时| `unity-mcp-helper` |
| 方案设计时 | `unity-developer` |
| 测试驱动开发 | `test-driven-development`, `unity-test-framework` |

# 偏好
## 输入系统
- 使用旧的输入系统
## git
- 不使用*git worktree*。
- 及时更新git commit。

# 测试驱动开发（TDD）
- 使用*unity-test-framework*框架
- 按照*Red-Green-Refactor*方式迭代开发，先写失败测试，代码开发完成后通过测试，然后重构。
- 通过*unityMCP*在编辑器内启动测试

# 游戏编辑器内测试
### 基本流程
- 通过play模式进入游戏
- 模拟输入
- 截图game窗口
- 对截图内容进行图像理解，并修复存在的问题

# 场景搭建
- 需要充分设计不同物体的大小和材质

# Unity MCP
## Unity MCP 常用操作示例
### 材质应用
- 创建材质：`manage_asset` action: create, asset_type: Material, path: 材质路径
- 应用材质：`manage_components` action: set_property, component_type: MeshRenderer, property: material, value: 材质路径(字符串)
- 注意：`set_property` 时只需 property 和 value 参数，不需要 properties 参数

#### 从场景中已有对象创建预制体
- 使用 manage_gameobject 工具的 modify 动作，设置 save_as_prefab 参数为 true。
- 示例：
```
# 例子：将场景中名为 "Player" 的对象保存为预制体  
manage_gameobject(  
    action="modify",  
    target="Player",  
    save_as_prefab=True,  
    prefab_path="Assets/Prefabs/Player.prefab"  
)
```

