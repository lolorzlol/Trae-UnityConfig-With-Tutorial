# 新建项目
unity新建3D项目，2D也行。

# mcp配置
具体安装内容https://github.com/CoplayDev/unity-mcp里面有配置指引。

1. 安装Python
https://www.python.org/downloads/ 下载安装。（已经有python的别下）

2. 安装uv
https://docs.astral.sh/uv/getting-started/installation/ （就是几个命令行的事情）

开始菜单搜powershell，然后打开powershell
![alt text](image-4.png)
然后再powershell里面复制这一行执行就行：
```
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```
（powershell粘贴用ctrl+shift+v, 最后按enter执行命令）

1. 安装mcp
In Unity: Window > Package Manager > + > Add package from git URL...

```
https://github.com/CoplayDev/unity-mcp.git?path=/MCPForUnity#main
```
4. 开启mcp


# Trae对话
1. 直接提问，“用superpowers工作流，balabala”
![alt text](image.png)
接下来会进入brain-storming（头脑风暴）阶段，回答他的问题。

3. 确认设计文档、架构文档
![alt text](image-1.png)
![alt text](image-2.png)
可以在这里指出有什么问题之类的。
同样也可以说“用game-development优化一下方案”。学过skill就知道这是什么意思

4. 选择开发方式
之后会自动生成计划文档。然后让选择开发方式，两个都行，第一个直接在当前窗口执行。第二个在Trae里面要自己新开一个窗口。
![alt text](image-3.png)

5. 等待
之后就是比较长时间的等待，他会自动生成内容，控制编辑器等等。中间可以打断干预，有时候会犯错而且一直绕圈子。

最后完成后可以自己测试，有bug的话,可以直接在下面让改,也可以新开窗口（清零上下文）。提示词，看[修复Bug](Tutorial-1/Examples/修复Bug.md)。
