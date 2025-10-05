# workflow-m365-audit-logs

[English](./README.md) | 简体中文

这个 GitHub Actions 工作流会获取 Microsoft 365 审计日志并上传到 OneDrive，方便你在 Power Automate 或 Power Apps 中使用。

（主要用于帮助保持E5开发者帐户的开发活动活跃度）

## 快速开始

**1. Fork 此仓库**

点击本仓库右上角的 **Fork** 按钮，将其复制到你的 GitHub 账号下。

**2. [注册 Azure AD 应用](https://portal.azure.com/#view/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)**

你需要在 Azure AD 租户中创建两个应用并添加 API 权限：

- **M365 Audit Logs App**
  - API 权限: `AuditLog.Read.All`（应用程序）
  - 授予管理员同意。

- **OneDrive Upload App**  
  - API 权限: `Files.ReadWrite.All`（应用程序）
  - 授予管理员同意。

这两个应用都使用 **客户端凭据（仅应用）流**。

**3. 添加 GitHub Secrets**

进入你 fork 的仓库 **Settings → Secrets and Variables → Actions**，添加以下 Secrets：

| Secret 名称 | 值 |
|-------------------------- |----|
| `AZURE_CLIENT_ID`         | M365 Audit Logs 应用的客户端 ID |
| `AZURE_CLIENT_SECRET`     | M365 Audit Logs 应用的客户端密钥 |
| `AZURE_TENANT_ID`         | 你的 Azure AD 租户 ID |
| `ONEDRIVE_CLIENT_ID`      | OneDrive 应用的客户端 ID |
| `ONEDRIVE_CLIENT_SECRET`  | OneDrive 应用的客户端密钥 |
| `ONEDRIVE_TENANT_ID`      | 你的 Azure AD 租户 ID |
| `AUDIT_USER`              | *(可选)* 指定用户的 UPN，用于筛选日志。不设置则会收集所有用户的日志。 |
| `ONEDRIVE_USER`           | 日志将上传到的 OneDrive 用户 UPN（就是前面注册API的帐户，例：example@your-domain.onmicrosoft.com）|

**4. 启用自动运行（可选）**

默认情况下，此工作流为手动运行。如需启用每天自动运行：

- 编辑 `.github/workflows/daily-audit.yml`。

- 去掉以下行开头的 `#` 注释符号：
```yml
  # schedule:
  #   - cron: '0 20 * * *'  # 每天 UTC 时间 20:00（北京时间 04:00）
```

**5. 测试工作流**

- 打开 Actions → Daily M365 Audit Logs → Run workflow。

- 选择 main 分支，点击 Run workflow。

- 验证结果：
  - JSON 日志会出现在仓库的 logs 文件夹中。
  - 日志会上传到你配置的 OneDrive 文件夹中。

**6. 注意事项**

- 必须为所有 API 权限 授予管理员同意。

- 为保安全，请**务必使用 GitHub Secrets**，切勿将凭据提交到代码仓库。

- 该工作流会默认把 JSON 日志提交到仓库中；建议**将仓库设为私有**，以避免日志公开。

- 你可以在 Power Apps / Power Automate 中连接 OneDrive 文件夹，实现可视化的仪表盘展示。

## 许可证

[MIT License](LICENSE) — 自由使用、修改和分享。
