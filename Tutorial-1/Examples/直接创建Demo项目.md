# 直接创建 Demo 项目

## 新建 Unity 项目

Unity 新建 3D 项目（2D 也可以）。

---

## MCP 配置

详细安装说明参考：https://github.com/CoplayDev/unity-mcp

### 步骤 1：安装 Python

下载地址：https://www.python.org/downloads/

> 已安装 Python 的可跳过。

### 步骤 2：安装 uv

打开 PowerShell（开始菜单搜索 `powershell`）：

![alt text](image-4.png)

执行以下命令：

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

> PowerShell 粘贴使用 `Ctrl+Shift+V`，最后按 `Enter` 执行。

### 步骤 3：安装 MCP
安装方式有两种

1. 通过git URL安装
在 Unity 中：`Window > Package Manager > + > Add package from git URL...`

```
https://github.com/CoplayDev/unity-mcp.git?path=/MCPForUnity#main
```
2. unity asset store
![alt text](image-11.png)

使用Unity asset store版本会落后一些。

### 步骤 4：开启 MCP

![alt text](image-5.png)

![alt text](image-8.png)

> 之后会启动一个网络监听进程，不要关闭，最小化即可。

![alt text](image-7.png)

---

## 复制配置文件到项目

将本项目 `TraeConfigs/ProjectConfigs` 中的 `.trae` 文件夹和 `AGENTS.md` 复制到 Unity 项目根目录：

![alt text](image-10.png)

---

## 打开 Trae 并进入 Solo 模式

详细步骤参考 [游戏策划案例](Tutorial-1/Trae安装+策划文档生成.md)

---

## Trae 对话流程

### 步骤 1：输入提示词

```
用 superpowers 工作流，balabala
```

![alt text](image.png)

接下来进入 brainstorming（头脑风暴）阶段，回答 AI 的问题。

### 步骤 2：确认设计文档

![alt text](image-1.png)

![alt text](image-2.png)

可在此指出问题，或说"用 game-development 优化一下方案"。

### 步骤 3：选择开发方式

AI 会自动生成计划文档，然后让你选择开发方式：

![alt text](image-3.png)

两种方式都可以：
- 第一种：直接在当前窗口执行
- 第二种：新开窗口执行

### 步骤 4：等待完成

之后是较长时间的等待，AI 会自动生成内容、控制编辑器等。

> 中间可以打断干预，有时 AI 会犯错并绕圈子。

完成后自行测试，如有 Bug 参考 [修复Bug](Tutorial-1/Examples/修复Bug.md)。