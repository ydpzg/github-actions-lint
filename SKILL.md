---
name: github-actions-lint
description: 当用户要求检查或修复 GitHub Actions 工作流时使用；仅针对 .github/workflows 下的工作流，调用 actionlint 校验语法、表达式、action 输入输出和可复用工作流，不用于通用 YAML 校验或完整安全审计。
---

# GitHub Actions 工作流检查

用 `actionlint` 对仓库中的 GitHub Actions 工作流做有边界的静态检查。

## 操作

1. 确认仓库根目录和用户指定的检查范围。默认只检查 `.github/workflows/` 中的工作流；如果用户指定文件，就只针对这些文件。
2. 检查 `actionlint` 是否已安装。若可用，从仓库根目录运行 `actionlint`；它会自动发现工作流文件。用户指定了文件时，可将文件路径传给命令。
3. 按诊断中的文件、行号和规则解释问题。只有用户要求修复时才修改文件，并尽量做最小修复。
4. 修复后重新检查受影响的工作流；改动范围较小时，顺手检查整个工作流目录，确认没有引入相邻问题。

## 边界

- 找不到 `actionlint` 时，不要自行安装，也不要用宽泛的 YAML 检查冒充等价验证。说明本次未能运行检查，并给出 [actionlint 官方安装说明](https://github.com/rhysd/actionlint)。
- 不要为了让检查变绿而批量关闭规则、忽略整个文件或改动无关工作流。若诊断可能与项目自定义 runner 标签或本地约定有关，先核对仓库配置再判断。
- `actionlint` 是静态检查：通过不代表工作流在 GitHub runner 上一定能成功运行，也不代表完成了全面的 Actions 安全审计。
- 汇报实际执行的命令、检查范围、诊断和未能验证的部分；不要把“未安装”或网络受阻写成检查通过。
