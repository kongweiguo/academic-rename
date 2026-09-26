# academic-rename

为本地文件和可编辑在线学术文献提供规范命名、预览、安全改名和日志恢复。`academic-rename` 是 academic 系列技能唯一的书目命名规则维护者；完整规则及示例见 [命名规范](references/naming.md)。

[`academic-translate`](https://github.com/kongweiguo/academic-translate) 专注完整翻译，[`academic-read`](https://github.com/kongweiguo/academic-read) 专注文章精读，[`academic-suite`](https://github.com/kongweiguo/academic-suite) 编排整套工作。普通翻译和精读保留源名称；用户明确要求规范命名时，才读取本技能并交接核验后的名称和实际源状态。其他技能不复制书目模板、日期、CCF、缩写或冲突规则。

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

本仓库以全新 `master` 历史发布。使用历史重建前的旧克隆时，先将本地修改、私人文件和任务记录备份到技能目录外，再将旧目录移开，按上面步骤重新克隆；只将需要保留的文件改动对照迁回，不合并旧 Git 历史。

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
- 先读取 `~/.agents/skills/academic-rename/SKILL.md`，按该技能规范这篇论文的名称，然后使用已安装的 academic-read 精读。

`$academic-rename` 是部分宿主支持的可选调用语法，不是安装或使用前提。

## 能力与记录

这是指令型技能，不包含独立重命名程序、脚本或连接器。执行依赖宿主已授权的文献读取、公开书目核验、本地持久记录及原生名称更新能力；扫描文献可能需要 OCR。在线平台可以使用等价工具，不要求固定平台插件。

改名保留原资源的身份、位置、正文与批注，交付实际名称、证据、逐项结果和日志位置。恢复使用可验证日志，不根据新文件名猜原名。旧 `academic-translate` 留下的改名日志继续兼容，已有译文不因技能拆分而移动或重建。

## 维护与验收

- [SKILL.md](SKILL.md)：职责、入口、路由及场景验收。
- [references/naming.md](references/naming.md)：唯一书目命名规则、执行、恢复和专业技能交接。
- [agents/openai.yaml](agents/openai.yaml)：可选 Codex 界面元数据。

规则修改只在唯一规范中维护，入口和说明文件引用该规范。通过读取 SKILL、检查引用和对照场景审查改动，不新增或运行外部校验程序，不把文档审查称为真实平台验证。仓库不保存私人论文、访问令牌或用户的执行日志。

仓库：[kongweiguo/academic-rename](https://github.com/kongweiguo/academic-rename)。问题与改进建议请提交到 [Issues](https://github.com/kongweiguo/academic-rename/issues)，避免附带私人文献、访问令牌或未脱敏日志。
