# OpenClaw 官方仓库学习笔记

## 来源: github.com/openclaw/openclaw

---

## 一、推荐模型组合（已验证）

| 用途 | 推荐模型 |
|------|----------|
| 编码/工具调用 | GLM |
| 写作/氛围感 | MiniMax M2.1 ✅ 当前使用 |

**官方推荐配置：**
- Anthropic Pro/Max + Opus 4.5（长上下文 + 提示注入防护）
- 但我们用 MiniMax + Kimi 备用更经济

---

## 二、模型配置技巧

### 1. 模型选择顺序
1. Primary 模型
2. Fallbacks（按顺序）
3. Provider auth 回退

### 2. 配置结构
```json5
{
  agents: {
    defaults: {
      model: {
        primary: "minimax/MiniMax-M2.1",
        fallbacks: ["kimi-code/kimi-for-coding"]
      },
      imageModel: {  // 仅在主模型不支持图片时使用
        primary: "...",
        fallbacks: [...]
      }
    }
  }
}
```

### 3. 模型别名
在 `agents.defaults.models` 中设置别名，方便 `/model` 命令使用

### 4. 模型允许列表
如果设置了 `agents.defaults.models`，只有列表中的模型可用
- 错误提示："Model is not allowed"
- 解决：添加到列表或清空列表

---

## 三、GitHub Skill 用法（已安装）

```bash
# PR 检查
gh pr checks 55 --repo owner/repo

# 工作流
gh run list --repo owner/repo --limit 10
gh run view <run-id> --log-failed

# API 查询
gh api repos/owner/repo/pulls/55 --jq '.title, .state'
gh issue list --json number,title --jq '.[] | "\(.number): \(.title)"'
```

---

## 四、推荐 Skills 清单

### 已配置 ✅
- github - GitHub CLI 集成
- bird - X/Twitter 操作

### 待考虑配置
| Skill | 用途 | 优先级 |
|-------|------|--------|
| notion | Notion 笔记集成 | 中 |
| obsidian | Obsidian 笔记 | 中 |
| sag | ElevenLabs TTS 语音 | 中 |
| spotify-player | Spotify 控制 | 低 |
| voice-call | 语音通话 | 低 |
| weather | 天气查询 | 低 |
| summarize | 文本摘要 | 低 |

---

## 五、当前 Issues 状态

| Issue | 状态 | 影响 |
|-------|------|------|
| #5919 `/think` 与 Gemini | 待修复 | 不影响我们（用 MiniMax） |
| #5870 Token 用量夸张 | 待修复 | 我们用 MiniMax 已避免 |
| #5868 Webhooks agentId 定向 | 已修复 | 多代理设置可用 |

---

## 六、Skill 创建原则（来自 skill-creator）

### 核心原则

1. **简洁是关键**
   - 上下文窗口是公共资源
   - 默认假设：模型已经很聪明
   - 只添加模型没有的知识
   - 用简洁示例替代冗长解释

2. **设置适当的自由度**
   - 提供必要的约束
   - 但不过度限制

### Skill 提供什么
1. 专业工作流 - 特定领域的多步骤程序
2. 工具集成 - 特定文件格式或 API 的操作指南
3. 领域专业知识 - 公司特定知识、模式、业务逻辑
4. 捆绑资源 - 脚本、参考和资产

---

## 七、有用的链接

- 官网：https://openclaw.ai
- 文档：https://docs.openclaw.ai
- 模型配置：https://docs.openclaw.ai/concepts/models
- 多代理设置：https://docs.openclaw.ai/concepts/multi-agent

---

## 八、即将合并的功能（关注）

- **#5924** 高级多轮攻击检测（安全增强）
- **#5921** memory-lancedb 增加 Gemini 支持
- **#5925** 媒体理解支持旧版 `{file}` 占位符

---

学习日期：2026-02-01
