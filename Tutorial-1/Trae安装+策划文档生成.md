# Trae 安装 + 策划文档生成

Trae 的基本使用参考官方文档即可：https://docs.trae.ai/

本教程主要演示如何复制项目配置文件，然后进行游戏策划设计。

---

## 1. 安装 Trae

官网下载安装国内版：https://www.trae.cn/

![alt text](image.png)

> 有 VS Code 的话，打开时建议同步 VS Code 配置。

---

## 2. 修改配置

### 2.1 基础配置

**步骤 1：创建项目文件夹**

IDE 以文件夹为单位运行，需要先创建工作目录。

![alt text](image-1.png)

**步骤 2：打开 Trae，选择项目文件夹**

![alt text](image-2.png)

![alt text](image-5.png)

**步骤 3：安装扩展插件**

![alt text](image-17.png)

![alt text](image-6.png)

### 2.2 项目配置

**步骤 1：复制配置文件**

将本项目 `TraeConfigs/ProjectConfigs` 中的内容复制到 Trae 项目文件夹下：

![alt text](image-8.png)

**步骤 2：检查模型可用性**

![alt text](image-10.png)

**模型推荐：**

| 模型 | 特点 |
|------|------|
| GLM-5 | 推理最强 |
| GLM-4.7 | GLM-5 弱化版，不易排队 |
| Minimax-M2.5/2.7 | 速度最快 |
| Doubao-Seed-2.0-Code | 支持视觉理解 |
| Kimi-K2.5 | 支持视觉理解，不易排队 |

标注了beta的模型，处于测试阶段，可能出现不稳定的情况。

**步骤 3：进入 Solo 模式**

![alt text](image-11.png)

---

## 3. 游戏设计示例

**步骤 1：输入提示词**

![alt text](image-15.png)

**步骤 2：等待加载，进入头脑风暴**

![alt text](image-16.png)

最终会生成一个 Markdown 格式的游戏设计文档。（对话太长时，可以直接说"生成文档"）

> 生成完设计文档后，如果 AI 继续写实现方案，可以停止对话。