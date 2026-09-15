# App Interaction Motion

一个适用于 **Codex、Claude Code、Cursor 与 Gemini CLI 的 App / Web 交互动效 Skill**，将 8 类动效的设计逻辑整理为可执行的实现建议和验收标准。

核心理念：让距离、速度、空间层级和用户输入产生合理的视觉反馈，让用户感到自己在控制界面。

## 包含什么

| 动效 | 设计重点 |
| --- | --- |
| 磁吸交互 | 先阻力、后贴合；越靠近目标，吸附感越强 |
| 液态形变 | 形变跟随运动速度，停止后自然恢复 |
| 3D 视差 | 不同层级采用不同位移幅度，建立空间感 |
| 滚动聚焦 | 越靠近视觉焦点的内容越突出 |
| 流体 Tab | 前沿先走、后沿追赶，产生拉伸和收缩 |
| 图片展开 | 保持同一张图片的连续性，改变容器和裁切范围 |
| 手势驱动转场 | 手指位移直接控制动画进度，支持反向与取消 |
| 物理碰撞 | 多元素遵循一致的质量、阻尼与碰撞规则 |

每种模式附带输入映射、实现建议和检查要点。技能还覆盖动画打断、状态连续性、不同刷新率、减少动态效果偏好和基础可访问性。

## 适用场景

- 为卡片、导航、图片预览和页面切换增加交互反馈。
- 优化拖拽跟手感、共享元素转场与滚动焦点。
- 评审现有动效，检查它是否准确表达状态和空间关系。
- 将动效概念转成符合现有项目技术栈的实现方案。

这是指导 AI 工作的技能说明，不是可直接导入应用的动画组件库，也不附带可运行的演示 App。它不绑定具体框架；实现时沿用项目已有动画系统。

## Agent 兼容性

本仓库采用 `SKILL.md` 加 `references/` 的技能结构，核心内容为平台无关的 Markdown 指令，不依赖专用脚本或某个 agent 的工具 API。支持 Agent Skills 格式的 agent 原则上可以加载；实际安装目录、触发机制和可用工具由各客户端决定，不能保证所有 agent 都能自动发现或直接执行。

不支持技能发现的 agent，也可以在能够读取仓库文件的前提下，将 `SKILL.md` 及相关参考文件作为任务上下文使用。`agents/openai.yaml` 仅提供 Codex 展示元数据。

以下新增安装方式已对照官方文档核对，未在每个客户端中逐一执行安装测试。

## 安装

选择你使用的客户端和安装范围即可。下方 shell 命令适用于 macOS、Linux 或兼容的 Bash 环境；请先安装相应客户端和 Git。

### Codex

将本仓库克隆到 Codex 的个人 skills 目录：

```bash
git clone https://github.com/kahomesl/app-interaction-motion.git "${CODEX_HOME:-$HOME/.codex}/skills/app-interaction-motion"
```

如果目标目录已存在，请先检查已有版本，避免覆盖自己的修改。安装后在新会话中使用技能名称调用。

也可以下载仓库 ZIP，解压后将文件夹命名为 `app-interaction-motion`，放入同一 skills 目录。确保 `SKILL.md` 直接位于该文件夹下。

### Claude Code

**个人安装（所有项目可用）**，在终端执行：

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/kahomesl/app-interaction-motion.git ~/.claude/skills/app-interaction-motion
```

**项目安装（仅当前项目可用）**，在项目根目录执行：

```bash
mkdir -p .claude/skills
git clone https://github.com/kahomesl/app-interaction-motion.git .claude/skills/app-interaction-motion
```

两种方式选一种即可。目标目录已存在时，先检查已有内容。也可以下载 ZIP，将解压后的技能文件夹放入上述目录；确认路径为 `app-interaction-motion/SKILL.md`，不要多嵌套一层文件夹。

在 Claude Code 中输入以下命令调用：

```text
/app-interaction-motion 为当前页面的 Tab 切换实现流体指示器，支持快速连续点击。
```

Claude Code 读取 `SKILL.md` 和关联参考文件；`agents/openai.yaml` 是 Codex 的展示元数据，安装到 Claude Code 时可以保留。安装目录与调用方式参见 [Claude Code 官方 Skills 文档](https://code.claude.com/docs/en/skills)。

### Cursor

**个人安装（本机所有项目可用）**：

```bash
mkdir -p ~/.cursor/skills
git clone https://github.com/kahomesl/app-interaction-motion.git ~/.cursor/skills/app-interaction-motion
```

**项目安装**，在项目根目录执行：

```bash
mkdir -p .cursor/skills
git clone https://github.com/kahomesl/app-interaction-motion.git .cursor/skills/app-interaction-motion
```

两种方式选一种。重新打开 Cursor 后，在 Agent 聊天中输入 `/`，搜索并选择 `app-interaction-motion`，然后输入任务，例如：

```text
/app-interaction-motion 优化卡片拖拽的磁吸与回弹，保持快速反向拖动时的状态连续。
```

可在 Customize → Skills 查看是否已发现技能。本机个人安装不等同于远程或 Cloud Agent 已安装；远程场景需按 Cursor 文档配置。参见 [Cursor 官方 Skills 文档](https://cursor.com/docs/skills)。

### Gemini CLI

**个人安装（默认范围）**，在终端执行：

```bash
gemini skills install https://github.com/kahomesl/app-interaction-motion
```

**项目安装**，在项目根目录执行：

```bash
gemini skills install https://github.com/kahomesl/app-interaction-motion --scope workspace
```

两种方式选一种，按客户端提示确认安装来源。已打开会话时，在 Gemini CLI 中依次输入：

```text
/skills reload
/skills list
```

确认列表出现 `app-interaction-motion` 后，用自然语言请求使用该技能：

```text
请使用 app-interaction-motion 技能，为图片预览实现从缩略图到全屏的连续转场。
```

Gemini CLI 的 `/skills` 用于技能管理；不要将其他客户端的 `/app-interaction-motion` 调用语法直接套用到这里。参见 [Gemini CLI 官方 Skills 管理文档](https://geminicli.com/docs/cli/using-agent-skills/)。

## 使用示例

以下使用 Codex 的 `$app-interaction-motion` 调用语法；Claude Code 和 Cursor 使用上面的斜杠调用方式，Gemini CLI 使用自然语言指定技能。

```text
使用 $app-interaction-motion，为当前页面的 Tab 切换实现流体指示器，支持不同标签宽度和快速连续点击。
```

```text
使用 $app-interaction-motion，优化图片从缩略图展开到全屏并返回的交互，保持裁切连续且没有闪烁。
```

```text
使用 $app-interaction-motion，评审当前卡片拖拽动效，重点检查跟手、松手回弹和中途取消。
```

技能会根据任务选择适合的动效，不要求在一个界面中同时使用全部八种。示例数值用于调参起点，不是固定规范。

## 文件结构

```text
app-interaction-motion/
├── SKILL.md                       # 技能入口、选择规则与验收标准
├── agents/
│   └── openai.yaml                # Codex 展示信息与默认提示词
├── references/
│   ├── motion-patterns.md         # 八类动效的实现参考
│   └── video-source.md            # 视频来源、时间索引与提炼边界
└── README.md
```

## 来源与说明

原作者：**叨叨AI**（抖音）。感谢原作者对交互动效设计思路的分享。

原视频：[《8种交互动效，让App质感直接拉满》](https://v.douyin.com/NqiOm5-jJ-Q/)

作者名称与原视频链接依据用户提供的抖音分享信息补充。视频画面标题为《8 种有质感的 App 交互动效》，系列字样为 `MOTION CRAFT`，结尾署名为 `DaoDao`。本技能的设计原则基于视频画面和可见字幕整理，未进行独立音轨逐字转写。

时间步处理、状态机、无障碍和验收清单属于为实际实现补充的工程建议，不代表视频作者原话或原始源码。仓库不包含原始视频及其中的视觉资产，也不宣称与原作者存在官方关联。详见 [来源摘要](references/video-source.md)。
