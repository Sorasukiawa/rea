# 本 fork 的上游同步

`Sync upstream` 每天或手动把 `morluto/rea` 的 `main` 合入本 fork 的 `main`，保留本 fork 的提交。合并冲突、权限不足或同步后仍缺少目标上游提交时，工作流保持失败，不强制覆盖或改写历史。

## 凭据与权限

普通代码变化可使用工作流自带的 `GITHUB_TOKEN`。上游创建或修改 `.github/workflows/` 文件时，GitHub 会拒绝该令牌完成合并，即使已设置 `contents: write`；给这个默认令牌增加普通工作流权限也无法解决。

该情况使用本仓库的 Actions secret `UPSTREAM_SYNC_TOKEN`。建议由仓库所有者创建仅限 `Sorasukiawa/rea` 的 fine-grained token，授予 Repository permissions 中的 **Contents: Read and write** 和 **Workflows: Read and write**，设置合适的有效期，再通过 GitHub Settings → Secrets and variables → Actions 保存。令牌值不写入仓库、日志、聊天或示例配置。

工作流优先使用该 secret；未设置时继续使用默认令牌。权限错误会明确提示所需 secret 和权限，而不会把未同步状态记为成功。创建令牌、修改访问授权和添加 secret 需由仓库所有者明确决定，本次代码修复不自动更改账号授权。

## 验证与恢复

凭据就绪后手动运行 `Sync upstream`，核对工作流成功且默认分支包含该次运行开始时读取的上游 SHA。fork 自有提交继续可达；新增上游提交发生在取样之后时，下次再同步。

删除 secret 即恢复默认令牌路径；这不会回退已经合入的提交。不要为解决权限问题强推上游分支、删除 fork 自有提交或跳过 ancestry 检查。
