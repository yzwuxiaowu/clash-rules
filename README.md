# OpenClash 代理分组与分流规则

基于 [OpenClash](https://github.com/vernesong/OpenClash) 官方配置格式生成的代理分组和分流规则，**将国外 AI 网站单独分为一组**，可独立选择出口节点。

## 文件说明

| 文件 | 说明 |
|------|------|
| `config-template.yaml` | 完整配置模板，包含 proxies（占位）、proxy-groups、rules |
| `ai-rules.yaml` | 纯 AI 网站规则集（Rule-Provider 格式，behavior: classical） |

## 代理分组（8 组）

| 分组名 | 类型 | 用途 |
|--------|------|------|
| `Proxy` | select | 总出口，手动选择节点 |
| `auto` | url-test | 自动选延迟最低节点 |
| `fallback-auto` | fallback | 故障转移，按顺序切换 |
| `load-balance` | load-balance | 负载均衡 |
| **`AI`** | **select** | **国外 AI 网站专用组** |
| `Streaming` | select | 流媒体（Netflix/YouTube 等） |
| `Telegram` | select | 电报/即时通讯 |
| `REJECT` | select | 广告拦截 |

## 订阅链接

### AI 规则集（Rule-Provider）

```
https://raw.githubusercontent.com/yzwuxiaowu/clash-rules/main/ai-rules.yaml
```

### 完整配置模板

```
https://raw.githubusercontent.com/yzwuxiaowu/clash-rules/main/config-template.yaml
```

## 使用方法

### 方式一：使用 AI 规则集（推荐）

在你的 OpenClash/Mihomo 配置中添加：

```yaml
rule-providers:
  ai-rules:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/yzwuxiaowu/clash-rules/main/ai-rules.yaml"
    interval: 86400
    path: ./ai-rules.yaml

proxy-groups:
  - name: "AI"
    type: select
    proxies:
      - Proxy
      - 你的节点1
      - 你的节点2

rules:
  - RULE-SET,ai-rules,AI
  # ... 其他规则
  - MATCH,Proxy
```

### 方式二：直接使用完整配置模板

1. 下载 `config-template.yaml`
2. 将 `proxies` 中的占位节点替换为你自己的节点
3. 在 proxy-groups 中引用你的节点名称
4. 导入 OpenClash 使用

## AI 网站覆盖范围（67 条规则）

覆盖 70+ 国外 AI 平台，包括：

- **对话/聊天**：OpenAI/ChatGPT、Anthropic/Claude、Google Gemini、Microsoft Copilot、Perplexity、Poe、xAI/Grok、Meta AI、You.com
- **代码/开发**：GitHub Copilot、Cursor、Replicate、Hugging Face、Cohere、Mistral、Groq、Together AI、Fireworks AI
- **图像生成**：Midjourney、Stability AI、Pika、Luma AI
- **视频生成**：Runway、Pika、Kling（可灵海外版）
- **语音/音乐**：ElevenLabs、Suno、Udio
- **写作/办公**：Jasper、Writesonic、Notion AI
- **其他**：Character.AI、Octane AI 等

## 更新规则

如需添加或修改 AI 网站规则，直接编辑 `ai-rules.yaml` 并提交，订阅链接会自动更新（OpenClash 会按 interval 自动刷新）。
