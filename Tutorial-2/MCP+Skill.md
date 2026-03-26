# 什么是 Skill，什么是 MCP

视频链接：https://www.bilibili.com/video/BV1ojfDBSEPv/

> 官方课程：https://anthropic.skilljar.com/introduction-to-agent-skills

---

## 元技能

用于管理 Skill 本身的技能。

| 元技能 | 功能 |
|--------|------|
| **find-skills** | 发现和安装新 Skill |
| **skill-creator** | 创建、修改和优化 Skill |

### find-skills

当你需要某个功能但不确定是否有现成的 Skill 时，使用此技能发现和安装。

```
/find-skills
```

然后描述你需要的功能，AI 会搜索可用的 Skill。

官网在这里：https://skills.sh/    也可以在里面手动搜索

### skill-creator

当你需要创建自己的 Skill 或修改现有 Skill 时使用。

```
/skill-creator
```

---

## pdf2skill

> 官网：https://pdf2skills.memect.cn/quick-start

一个"文档转技能编译器"，可以自动把 PDF 书籍、手册、行业报告等转换成完整的 Skill 技能包。

### 使用方式

**官网直接上传**

1. 打开 https://pdf2skills.memect.cn/quick-start
2. 上传 PDF 文件
3. 等待编译完成（10-30 分钟，根据文件大小）
4. 下载 `skills.zip`

### 工作原理

1. **文档解析** - 将 PDF 转换为结构化文本
2. **语义拆解** - 按语义密度和逻辑边界智能拆分（不按章节机械切割）
3. **逻辑建模** - 建立知识点之间的依赖关系
4. **技能封装** - 输出标准 Skill 文件，包含触发条件、输入参数、执行逻辑

### 适用场景

- 将技术文档、专业书籍转为 Skill
- 将教程、规范文档转为可复用的技能
- 让 AI 能参考和运用专业知识

类似功能开源替代：https://github.com/yusufkaraaslan/Skill_Seekers

---

## 参考资源
**本项目配置的game-design技能来自：https://mp.weixin.qq.com/s/ci7e-uukIjnlozofqQLjrQ?scene=1&click_id=46**