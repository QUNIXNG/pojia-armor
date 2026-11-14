# pojia-armor


- 当前：`v0.1.0`
- 最新：`v0.1.0`（[Releases](https://github.com/QUNIXNG/pojia-armor/releases)）
Pi / PiDeck 扩展：按模型 ID，在请求前把指定提示词前置到 system prompt，减少模型答非所问、拒答或转到其他话题的情况。

## 工作方式

- 通过 `before_agent_start` 注入提示词。
- 模型映射由 `config.json` 配置。
- 提示词按文件修改时间缓存，修改后下一轮自动生效。
- 已包含同一提示词时不重复注入。
- 未匹配的模型不受影响。

当前映射：

- `deepseek-v4-pro` → `prompts/dsv4-pro.md`
- `deepseek-v4.1-flash` → `prompts/dsv41-flash.md`

## 安装

把整个目录放到：

```text
~/.pi/agent/extensions/pojia-armor/
```

重新加载扩展后生效。

## 命令

```text
/pojia on
/pojia off
/pojia status
/pojia list
/pojia reload
/pojia dump
/pojia dump all
/pojia check <文本>
/pojia check @<文件路径>
```

`dump` 用于核对实际提示词文件、正文字符数和指纹；`check` 只是对给定文本做拒答/回退标记检查，不会改写回复。

开关状态写入本目录的 `state.json`，该文件不上传。
