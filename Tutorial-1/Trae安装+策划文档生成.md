其实Trae的基本使用，参考官方文档就好了,内容很少，上手还是比较快的。

https://docs.trae.ai/

这里主要是有一个复制项目配置文件，然后头脑风暴做游戏策划设计的过程。

# 1. 安装Trae
官网直接下载安装国内版：https://www.trae.cn/

![alt text](image.png)

有vscode的话，打开时候最好同步vscode配置。

# 2. 修改配置
## 基础配置
1. 创建新的文件夹
这些IDE都是以文件夹为单位运行的。需要先有用来工作的项目文件夹。
![alt text](image-1.png)

2. 打开Trae，打开刚创建的文件夹
![alt text](image-2.png)

![alt text](image-4.png)

![alt text](image-5.png)

3. 安装扩展插件
![alt text](image-6.png)

## 项目配置
1. 将本项目文件夹中的TraeConfigs/ProjectConfigs中的内容复制到Trae项目文件夹下
![alt text](image-8.png)

2. 检查一下模型是否可用
![alt text](image-10.png)

这里可以选择模型。简单来说：

GLM-5/4.7, Minimax-M2.7/2.5, Doubao-Seed-2.0-Code，Kimi-K2.5都好一些。

其中Minimax-2.7/2.5速度最快。GLM-5推理最强（GLM4.7为弱化版）。Doubao-Seed-2.0-Code和Kimi-K2.5带视觉理解。

以上。GLM4.7，Minimax-M2.5和Kimi-K2.5最不容易排队。

1. 进入Solo模式
![alt text](image-11.png)


# 3. 游戏设计例子
1. 输入提示词
![alt text](image-15.png)

2. 等一段加载时间，然后就是一连串的头脑风暴过程
![alt text](image-16.png)

最后会生成一个md的游戏设计文档。（对话太长，也可以直接说让生成文档）

生成完设计文档后，可能它还会继续写实现方案，这个时候其实就可以停止对话了。
