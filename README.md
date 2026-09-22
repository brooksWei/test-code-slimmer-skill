# Test Slimmer

Reduce test code while preserving meaningful behavioral coverage.

一个仅显式调用的 Codex skill，用于分析和精简已有测试：提取真正重复的准备逻辑、参数化等价用例、压缩快照，并验证测试点是否仍有明确的承载位置。

## 使用

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
使用 skill-installer 从 https://github.com/ycp424c/test-slimmer-skill 安装 test-slimmer 子目录到全局 skills 目录，保留显式调用配置。
```

也可以下载仓库，将 `test-slimmer/` 目录完整复制到 Codex 的用户级 skills 目录，例如 `~/.agents/skills/test-slimmer/`。保留其中的 `agents/openai.yaml`。

调用策略为：

```yaml
policy:
  allow_implicit_invocation: false
```

这是 Codex 的显式调用配置。其他读取 `SKILL.md` 的客户端是否遵守它，取决于对应客户端。此包不提供 hooks、后台任务或常驻指令。

## 方法与案例

- [SKILL.md](test-slimmer/SKILL.md)：基线、覆盖迁移、精简、验证和交付流程。
- [匿名复盘案例](test-slimmer/references/case-study.md)：一次表格组件测试精简，22 个文件净减少 **5,680 行**；包含独立审查发现并补回 `rowspan/colspan` 的过程。

适用于组件、单元、集成和浏览器测试。按项目现有工具链工作，不要求迁移测试框架。代码行数是维护成本指标之一，不能代替覆盖判断。

## 仓库结构

```text
test-slimmer/
├── SKILL.md
├── agents/openai.yaml
└── references/case-study.md
```

入口包含通用方法；需要具体取舍和失败案例时再读复盘。没有运行时依赖或自动改写脚本。

## License

[MIT](LICENSE).
