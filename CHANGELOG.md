# pojia-armor

## [1.2.0] - 2026-11-04

### Added
- `/pojia dump`：输出当前命中提示词的正文指纹 —— 绝对路径、正文（去 frontmatter）字符数、正文 sha256 前 16 位、mtime、首行。`/pojia dump all` 一次列出全部映射。用途：不用等行为异常才发现「装的是哪一版」。
- `/pojia check <文本>`：用实测判定口径给一段回复打分（拒答 / 回退 / 通过），输出命中标记与开头 80 字；`/pojia check @<文件路径>` 直接读文件打分。
- 判定标记表照搬 `_archive/pojia-opt-2026-10-26/pojia-opt-test.js`，并带 rescore 修正：软标记（`可改为` / `可以改为` / `改为授权`）要求同句共现 `合法|安全|防御|授权|合规` 才算回退，避免「…可改为：」这类正常技术表述被误判。

### Changed
- 命令参数解析改为「首个 token 当子命令、其余原样保留」，`check` 的被检文本不再被 `toLowerCase()` 改写。

## [1.1.0] - 2026-11-02

### Added
- `config.json`：模型 → 提示词映射改成外部配置，新增模型不用改代码；文件缺失或损坏时回退内置的两条映射。
- 模型 id 匹配改为三级：精确 → 去 provider 前缀（`provider/model`、`model@ver`）→ 尾部匹配，带前缀的 id 不再漏命中。
- 提示词按 mtime 缓存，改完 `.md` 下一轮直接生效；`config.json` 改动自动重读，也可 `/pojia reload` 强制重载。
- `/pojia list` 查看全部映射与文件状态；`/pojia status` 显示命中的文件与字数。

### Changed
- 判重从 `systemPrompt.startsWith("[MODE: SANDBOX")` 改为「正文前 80 字是否已出现」，别的扩展在前面加东西也不会重复叠加；切换模型时不会误判为已注入。
- `state.json` 改为临时文件 + rename 原子写。
- 类型从 `pi: any` 换成 `ExtensionAPI`，事件/命令参数改为推断类型。

### Removed
- `pi.appendEntry(STATE_TYPE, …)`：没有对应 renderer，且状态真实来源是 `state.json`，属于无效写入。

## [1.0.0] - 2026-11-02

### Added
- `package.json` 与 `pi.changelogPath`：更新后 pi 会在「Extension Updates」里显示本文件的版本条目。

### Fixed
- `pi.setLabel("破甲注入")` 参数数量不对（`setLabel(entryId, label)` 需要两个参数），实际一直在清空一个不存在的条目。改为 `pi.getSessionName()` 为空时调用 `pi.setSessionName("破甲注入")`，不再覆盖用户手动命名的会话。
- 开关状态反馈统一走 `ctx.ui.notify`：PiDeck 的 chat 界面不渲染 `ctx.ui.setStatus`，页脚方案无效。
