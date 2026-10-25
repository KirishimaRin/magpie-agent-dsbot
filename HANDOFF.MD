# 项目交接与已知注意事项

## 上游命名与兼容性

- 2026-10-26 核对上游已改名为 DS-Bot，仓库为 `Fei2411/DS-Bot`。产品说明、Agent 名称、供应商显示名和链接统一使用 DS-Bot。
- 上游 `app/package.json` 的 `productName` 目前仍为 `DeepSeek Bot`，应用设置目录及可执行文件名称仍沿用该名称。`lib/paths.js` 中对应路径用于实际读取 `locations.json`，不要直接批量替换。
- 保留插件包名和仓库名 `magpie-agent-deepseek_bot`、Agent ID `deepseekbot`、桥接目录 `magpie-deepseekbot`、供应商 ID `magpie-dsbot*` 和凭据引用 `MAGPIE_DSBOT_GATEWAY_KEY`，避免改名导致重复安装、原模型选择失效或遗留凭据。
- 产品改名不等于已完成新版本兼容验收。README 的已测试版本以真实验证结果为准；原图标来源仍固定到已记录的 v0.2.0 提交。
- 上游版权和 NOTICE 原文按来源保留；说明文字采用当前产品名称。

## 工作区与发布

- `magpie-agent-deepseek_bot` 是原工作区 `magpie-agent-deepseekbot` 的目录映射，二者对应同一份文件与 Git 历史。原目录被进程占用时无法直接重命名，勿复制成两个独立工作区。
- README 面向使用者；许可和图标声明放在许可部分。参考其他项目的结构时，只写本插件实际涉及的内容。
- 图标源文件 `icon.svg` 与 `package.json` 的 `magpie.icon`、`agent.js` 的 `agent.icon` 必须字节一致；Git 中原图标保留原始换行。
- `magpie plugin list --json` 会输出很大的供应商目录，还可能加载其他插件。读取时限制输出并扩大缓冲，避免默认缓冲溢出后把整份响应打印到日志。
- 用户配置、桥接文件、密钥、`.smoke/` 和 `release/` 留在本机。检查提交内容，不要把诊断或验收数据带入源码仓库。

## 验证

- `npm test`：模型协议、配置同步、升级兼容及恢复测试。
- `npm run check:package`：包名、图标、文档链接和许可文件。
- `node scripts/smoke-magpie.mjs`：隔离环境中的安装、接入、断开及移除。
- 推送后核对对应提交的 GitHub Actions 状态，避免把上一个提交的通过结果当成本次结果。
