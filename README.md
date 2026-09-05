[README.md](https://github.com/user-attachments/files/31861019/README.md)
# Comsol-Automatic-wwj

**作者：抖音 萌猪过河**

用自然语言通过 MCP 驱动真实 COMSOL：建模、材料、物理场、耦合、网格、研究、求解与结果可视化。面向豆包自定义连接器及支持 MCP / SKILL.md 的其他 Agent。

**一定不要使用屏幕识别，只使用 MCP 操作 COMSOL。** 禁止截图定位、OCR 和鼠标键盘自动化；MPh / Java API 也必须经 MCP 工具调用。计算完成后，最后由用户手动点击 **文件 → COMSOL Multiphysics Server → 从服务器导入 App**，选择本次模型查看结果。

对外名称为 **Comsol-Automatic-wwj**；技能标识、目录及 Python 包名使用小写 `comsol-automatic-wwj`。

**整个仓库就是一个技能目录。** 根目录 `SKILL.md` 是 Agent 工作指南，`server/` 是 MCP 运行入口，`vendor/installed-mcp/` 包含原安装版 MCP 的 MIT 源码快照。必须下载完整目录；单独复制 SKILL.md 不会安装服务。

## 可以做什么

- 通过与 COMSOL Desktop 共享的服务器会话运行模型，前台查看模型树和图形。
- 提供 99 个 MCP 工具：保留安装版的建模能力，并补充会话保护、串行执行、案例任务及状态查询、HTTP 认证和可选的 Java API 代码通道。
- 提供原创三维散热器：10 W 热源、铝基座和翅片、对流换热，包含稳态、瞬态、网格细化和能量校验。
- 按物理场分类提供经典案例配方：AC/DC、声学、化学物质传递、电化学、流动、传热、光学、等离子体、射频、半导体、结构力学、数学，以及常见多物理场耦合和扩展模块。详见 [案例目录](docs/case-catalog.md)。

通用 API 入口可扩展到本机有许可的 COMSOL 接口；“覆盖类别”不等于穷尽或验证全部模型组合。官方案例配方调用用户安装的 Application Library，不随包复制商业模型。每个条目都标注验证状态。COMSOL、模块许可证、Python 运行环境均需用户自行具备。

<img width="1600" height="1000" alt="image" src="https://github.com/user-attachments/assets/dc027437-7fd7-40d2-9746-befd33b9aa62" />


## 快速开始

1. 将完整目录解压，安装 Python 3.11 或 3.12，以及 COMSOL。推荐先使用与本包实测一致的 COMSOL 6.2；已有可用 MCP 服务的用户继续使用原服务。
2. 按 [安装与豆包连接器配置](docs/connectors.md) 运行 `scripts/install.ps1`，再启动 MCP。第一次在豆包创建 HTTP 自定义连接器，填入服务 URL 与 Bearer 请求头。新版启动脚本将 Token 保存在本机 `.mcp-token`，以后重启复用，**无需反复改 Header**；指南包含旧版迁移和 PowerShell 脚本执行策略报错的解决方法。
3. 按 [豆包技能列表安装](docs/skill-discovery.md) 在技能管理页选择“新建／创建 → 上传技能”，上传完整发布 ZIP；确认上传、安全检测通过及 `comsol-automatic-wwj` 条目可见。复制到本地文件夹不能代替这一步，也不假设存在“一键 GitHub 导入”功能。
4. 开启连接器后先说：

   > 使用 comsol-automatic-wwj。一定不要使用屏幕识别，只使用 MCP。先检查连接，然后运行 heat_sink_3d 三维散热器案例，报告能量平衡与网格检查。最后告诉我本次模型名称，让我在 COMSOL 中点击“文件 → COMSOL Multiphysics Server → 从服务器导入 App”，查看模型树、温度云图和升温曲线。

5. 测试其他模块，例如：

   > 列出声学经典案例。先说明本机是否有对应模块，然后加载一个入门案例，检查研究设置，再求解、显示声场并报告验证结果。

最终查看步骤：在连接同一服务器的 COMSOL Desktop 中，手动点击 **文件 → COMSOL Multiphysics Server → 从服务器导入 App**，选择 MCP 返回的新模型。Agent 的全部仿真操作只走 MCP，菜单点击由用户完成。详细说明见 [连接器指南](docs/connectors.md)。

## 目录

| 路径 | 用途 |
| --- | --- |
| `SKILL.md` | 面向 Agent 的触发条件、首次配置、仿真工作流 |
| `server/` | 标准 MCP、HTTP/SSE、串行 COMSOL 执行及案例任务 |
| `vendor/installed-mcp/` | 已安装 MCP 源码、原 MIT 许可证、逐文件哈希 |
| `examples/original/` | 由 MCP 案例工具调用的原创源码 |
| `examples/catalog.json` | 经典官方库案例的路径、模块和学习目标 |
| `examples/validation/` | 发布时实际完成的验证记录 |
| `docs/` | 豆包配置、API 经验、物理验证、安全与发布说明 |
| `scripts/`、`tests/` | 安装、启动、发布审计及自动化测试 |

## 兼容性与验证边界

已将本地 COMSOL 6.2 的命令驱动经验整理成可重跑脚本；精确测试情况见 [验证记录](examples/validation/README.md)。用户已报告在其豆包环境通过本机 HTTP + 自定义 Header 完成连接并运行三维散热器，七项检查通过。本次 Token 持久化更新经本机脚本测试，尚未由用户在豆包重新验收；其他版本、技能导入方式及前台导入仍需按安装页验证，不能据此宣传“全版本一键适配”。

MCP 服务默认只绑定本机。代码执行工具需显式设置 `COMSOL_ALLOW_CODE=1`，能力等同受信任代码执行，并非安全沙盒。详见 [安全边界](docs/security.md)。

## 许可与来源

新增代码和技能采用 [MIT](LICENSE)。捆绑的安装版 MCP 源于 [wjc9011/COMSOL_Multiphysics_MCP](https://github.com/wjc9011/COMSOL_Multiphysics_MCP)，保留其许可证和引用资料。由于安装目录没有 Git 元数据，使用快照日期与 SHA-256 追溯，不声称对应某个上游 commit。见 [NOTICE](NOTICE.md)。

本包不含用户研究模型、个人配置、连接凭据、COMSOL 软件或官方案例文件。发布前运行 [发布审计](docs/publishing.md)。
