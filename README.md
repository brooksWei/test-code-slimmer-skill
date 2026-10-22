# Test & Code Slimmer

两个可独立安装的 Codex skill，分别处理实现架构与测试维护。

| Skill | 用途 | 调用策略 |
| --- | --- | --- |
| [code-slimmer](code-slimmer/SKILL.md) | 基于《软件设计的哲学》与《架构整洁之道》，作为架构师生成目标代码的优化方案 | 默认自动发现，也可显式调用 |
| [test-slimmer](test-slimmer/SKILL.md) | 提取重复测试准备、参数化等价用例、压缩快照，核对测试点迁移 | 仅显式调用，保留原配置 |

## Code Slimmer

主要用途：基于 [《软件设计的哲学》第二版](https://github.com/yingang/aposd2e-zh) 和 [《架构整洁之道》](https://cactus-proj.github.io/Clean-Architecture-zh/)，作为架构师读取指定模块的源码与调用方，生成架构优化方案。这两本书是 skill 的默认设计依据。

精简的重点是减少调用方与维护者必须理解的细节，覆盖契约、状态归属、接口泄漏和依赖方向。目录与代码行数只作为辅助指标。

```text
使用 $code-slimmer，基于《软件设计的哲学》第二版与《架构整洁之道》，作为架构师生成 src/components/filter 的优化方案。
```

```text
使用 $code-slimmer 为这个模块生成优化方案，写入现有架构文档，包含代码证据、原则与章节依据、目标职责与依赖、兼容迁移、测试验收和回退策略。
```

默认只生成方案，保持实现不变。用户后续需要教程或实施时再展开，例如：

```text
使用 $code-slimmer 把现有优化方案改成实战教程，按里程碑指导我修改，每步给修改顺序、代码示例、验证命令和过关条件；先不要改实现。
```

```text
使用 $code-slimmer 执行教程的 M1，将改动和验证结果追加到原文档。
```

方案包含分阶段实施建议，但不自动扩展为教程或实际重构。里程碑按目标实际情况裁剪，不要求固定分层或执行固定的 M0–M7。

- [设计视角](code-slimmer/references/design-lenses.md)：将设计原则转成代码诊断问题。
- [交付结构](code-slimmer/references/deliverables.md)：优化方案的内容与检查，附后续教程和实施记录的写法。
- [Filter 案例](code-slimmer/references/filter-case-study.md)：两本书如何用于诊断契约冲突、状态分散和接口泄漏，形成优化方案。

## Test Slimmer

用于分析和精简已有测试：提取真正重复的准备逻辑、参数化等价用例、压缩快照，并验证测试点是否仍有明确的承载位置。

安装后明确点名调用：

```text
使用 $test-slimmer 看看这个模块的测试是否有明显冗余，先只分析。
```

```text
使用 $test-slimmer 精简这个模块的测试。优先保留原测试点；少量低价值检查若能换来明显的维护收益，可以说明取舍后删除。
```

分析请求保持只读；改动请求按已有授权执行。提交、推送和创建 PR/MR 沿用用户的明确指令，不由 skill 调用本身授权。

## 安装到 Codex

如果 Codex 已提供 `skill-installer`，可以直接请求：

```text
使用 skill-installer 从 https://github.com/brooksWei/test-code-slimmer-skill 安装 test-slimmer 子目录到全局 skills 目录，保留显式调用配置。
```

也可以将所需 skill 目录完整复制到 Codex 的用户级 skills 目录，例如 `~/.codex/skills/code-slimmer/` 或 `~/.codex/skills/test-slimmer/`，保留其中的 `agents/openai.yaml` 和引用文件。当前仓库中的新增内容需要先发布到远端，才能通过仓库链接安装。

`test-slimmer` 的调用策略为：

```yaml
policy:
  allow_implicit_invocation: false
```

`code-slimmer` 使用默认自动发现策略。其他读取 `SKILL.md` 的客户端是否遵守调用配置，取决于对应客户端。两个包均不提供 hooks、后台任务或常驻指令。

## Test Slimmer 方法与案例

- [SKILL.md](test-slimmer/SKILL.md)：基线、覆盖迁移、精简、验证和交付流程。
- [匿名复盘案例](test-slimmer/references/case-study.md)：一次表格组件测试精简，22 个文件净减少 **5,680 行**；包含独立审查发现并补回 `rowspan/colspan` 的过程。

适用于组件、单元、集成和浏览器测试。按项目现有工具链工作，不要求迁移测试框架。代码行数是维护成本指标之一，不能代替覆盖判断。

## 仓库结构

```text
code-slimmer/
├── SKILL.md
├── LICENSE
├── agents/openai.yaml
└── references/
    ├── design-lenses.md
    ├── deliverables.md
    └── filter-case-study.md
test-slimmer/
├── SKILL.md
├── LICENSE
├── agents/openai.yaml
└── references/case-study.md
```

入口包含通用方法；按交付需要读取参考文档。没有运行时依赖或自动改写脚本。

## License

[MIT](LICENSE).
