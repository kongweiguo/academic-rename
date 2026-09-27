# academic-rename

为本地文件和可编辑在线学术文献提供共享编号、规范命名、预览、安全改名和日志恢复。`academic-rename` 统一维护编号、书目名称及原文、翻译、精读的类型标记；完整规则及示例见 [命名规范](references/naming.md)。

[`academic-translate`](https://github.com/kongweiguo/academic-translate) 专注完整翻译，[`academic-read`](https://github.com/kongweiguo/academic-read) 专注文章精读，[`academic-suite`](https://github.com/kongweiguo/academic-suite) 编排整套工作及论文索引。普通翻译和精读保留源名称，只为新成果取号或复用同篇编号、核实类型与基本名，不启动书目核验或改源名。用户明确要求规范书目名称时，才进入书目核验流程。其他技能不复制命名规则。

按 [目录取号](references/naming.md#共享编号与目录取号) 和 [类型标记规范](references/naming.md#类型前缀与基本名)，同组文档可以显示为：

```text
[P001][原文]B.pdf
[P001][翻译]B
[P001][精读]B
```

`P001` 为示例，`B` 是不含已确认编号、类型和扩展名的基本名。编号在前便于按名称聚合同篇文档，类型紧随便于辨认；原有六段书目字段不变。新号直接取当前目录最大编号加一，没有则从 `P001` 开始；同篇已有编号先核实复用，不重排。无需编号登记表、计数器或占号文件。翻译和精读维护开头的关联导航，suite 维护包含完整题名及三个实际链接的论文索引；索引只供阅读，不参与发号，只读原文通过真实链接定位。

例如，当前目录已有 `P003` 和 `P010`，下一篇新论文使用 `P011`；为已有 `P003` 论文补充译文或精读时继续使用 `P003`。预览中的编号只是候选，首次实际写入前会依据最新目录重新核对。跨目录和冲突处理仍以唯一命名规范为准。

## 安装

这是与 agent、LLM 厂商无关的指令型技能。推荐保存在通用目录 `~/.agents/skills/academic-rename/`，也可以选择其他目录；无需放入 Codex 或 Claude 的私有目录。通用目录只是存放约定，是否自动发现由宿主决定。保留完整仓库结构，入口是 [SKILL.md](SKILL.md)，相对引用按技能文件所在位置解析。`agents/openai.yaml` 仅是可选界面元数据，其他宿主可忽略。

### Git 安装

以下命令适用于 macOS/Linux，需已有 Git：

```bash
mkdir -p "$HOME/.agents/skills"
git clone --branch master https://github.com/kongweiguo/academic-rename.git "$HOME/.agents/skills/academic-rename"
```

Windows PowerShell：

```powershell
New-Item -ItemType Directory -Force "$HOME/.agents/skills"
git clone --branch master https://github.com/kongweiguo/academic-rename.git "$HOME/.agents/skills/academic-rename"
```

自选目录时替换命令中的目标路径。目标已存在时先检查已有内容，不覆盖安装。

### ZIP 安装

下载 [master 分支 ZIP](https://github.com/kongweiguo/academic-rename/archive/refs/heads/master.zip)，解压后将 `academic-rename-master` 文件夹改名为 `academic-rename`，放到 `~/.agents/skills/` 或自选目录。最终入口应是 `~/.agents/skills/academic-rename/SKILL.md`，不要多套一层 `academic-rename-master/`。ZIP 安装不需要 Git。

### 更新与历史重建后的迁移

本仓库此前已重建为 `master` 历史，后续更新以普通提交发布。本次功能更新无需再次重建或重新克隆。只有仍使用历史重建前旧克隆时，才先将本地修改、私人文件和任务记录备份到技能目录外，再将旧目录移开并重新克隆；只将需要保留的文件改动对照迁回，不合并旧 Git 历史。

新克隆的常规更新：

```bash
git -C "$HOME/.agents/skills/academic-rename" pull --ff-only origin master
```

该 Git 命令也可用于 PowerShell；自选目录时替换路径。存在本地修改时先保存并处理冲突；快进失败时检查本地状态，不强制覆盖。ZIP 安装则下载最新 ZIP，对照保留个人改动后替换技能文件。

## 使用

支持技能自动发现的宿主可直接接受自然语言请求；未发现技能时，明确要求读取安装目录里的 `SKILL.md` 及任务所需引用文件。只有能够读取这些文件的宿主才能使用本技能；若宿主使用独立注册机制，按其方式注册实际路径。文件安装本身不提供执行工具或平台授权。

示例请求：

- 使用 academic-rename，预览这个论文目录的重命名方案，不执行改名。
- 使用 academic-rename，规范这些文献的文件名并保存可恢复日志。
- 使用 academic-rename，按指定的改名日志恢复源文件名称。
- 使用 academic-rename，预览为这个目录中对应的原文、译文和精读添加共享编号，保留现有书目基本名。
- 使用 academic-rename，根据当前目录为这篇论文的新成果规划共享编号，不改原文名称，不查询 CCF。
- 先读取 `~/.agents/skills/academic-rename/SKILL.md`，按该技能规范这篇论文的名称，然后使用已安装的 academic-read 精读。

`$academic-rename` 是部分宿主支持的可选调用语法，不是安装或使用前提。

## 能力与记录

这是指令型技能，不包含独立重命名程序、脚本或连接器。执行依赖宿主已授权的文献读取、公开书目核验、本地持久记录及原生名称更新能力；扫描文献可能需要 OCR。在线平台可以使用等价工具，不要求固定平台插件。

改名保留原资源的身份、父级位置、正文与批注，交付实际目录与共享编号、实际名称、证据、逐项结果和恢复日志位置。仅保留原有改名恢复日志及专业进度，不把它们用作发号台账。写入前重新检查目录占用，重试先查实际资源；恢复使用可验证日志，不根据新文件名猜原名。当前目录最大号全部移出或删除后，未来可能再次取到该号，因此互联和索引按实际来源身份核对。

旧 `academic-translate` 留下的改名日志继续兼容，已有 `_翻译`、`_精读`、仅类型前缀或已编号成果仍原位续作；只有明确要求调整已有名称时才规划迁移，由专业技能核实并修复导航和进度，suite 更新实际索引。升级技能不会自动改名真实文献。

## 维护与验收

- [SKILL.md](SKILL.md)：职责、入口、路由及场景验收。
- [references/naming.md](references/naming.md)：唯一书目命名规则、执行、恢复和专业技能交接。
- [agents/openai.yaml](agents/openai.yaml)：可选 Codex 界面元数据。

规则修改只在唯一规范中维护，入口和说明文件引用该规范。通过读取 SKILL、检查引用和对照场景审查改动，不新增或运行外部校验程序，不把文档审查称为真实平台验证。仓库不保存私人论文、访问令牌或用户的执行日志。

仓库：[kongweiguo/academic-rename](https://github.com/kongweiguo/academic-rename)。问题与改进建议请提交到 [Issues](https://github.com/kongweiguo/academic-rename/issues)，避免附带私人文献、访问令牌或未脱敏日志。
