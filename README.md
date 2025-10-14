# workflow-m365-audit-logs

English | [中文简体](./README.zh-CN.md)

This GitHub Actions workflow fetches Microsoft 365 audit logs and uploads them to OneDrive, so you can use them with Power Automate or Power Apps. It helps keep E5 development active and track user activity.

## ⚠️ Security Notice

By default, **forked repositories are PUBLIC**, so it is **STRONGLY RECOMMENDED** to set your forked repository to **PRIVATE** before running the workflow.

This workflow automatically retrieves Microsoft 365 audit logs and stores them as JSON files in your repository.

If your repository remains public, these logs may **EXPOSE** user activity details, timestamps, IP addresses, or other sensitive information.

To protect your **DATA PRIVACY** and **TENANT SECURITY**, always **REVIEW YOUR REPOSITORY VISIBILITY AND SECRETS CONFIGURATION** carefully before activating the workflow.

## Quick Start

**1. Fork This Repository**

Click Fork at the top-right of this repository to create a copy under your GitHub account.

**2. [Register Azure AD Apps](https://portal.azure.com/#view/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)**

You need two apps in your Azure AD tenant:

- M365 Audit Logs App
  - API permissions: AuditLog.Read.All (Application)
  - Grant admin consent.

- OneDrive Upload App
  - API permissions: Files.ReadWrite.All (Application)
  - Grant admin consent.

Both apps use client credentials (app-only) flow.

**3. Add GitHub Secrets**

Go to Settings → Secrets and Variables → Actions in your forked repository. Add these secrets:

| Secret Name | Value |
| --- | --- |
| `AZURE_CLIENT_ID` | M365 Audit Logs app client ID |
| `AZURE_CLIENT_SECRET` | M365 Audit Logs app client secret |
| `AZURE_TENANT_ID` | Your Azure AD tenant ID |
| `ONEDRIVE_CLIENT_ID` | OneDrive app client ID |
| `ONEDRIVE_CLIENT_SECRET` | OneDrive app client secret |
| `ONEDRIVE_TENANT_ID` | Your Azure AD tenant ID |
| `AUDIT_USER` | Optional: specific user UPN to filter logs. If not set, all users’ audit logs will be collected. |
| `ONEDRIVE_USER` | OneDrive account UPN where logs will be uploaded |

**4. Enable Automatic Run (Optional)**

By default, this workflow runs manually. To enable daily automatic runs:

- Edit `.github/workflows/daily-audit.yml`.

- Uncomment these lines:

```yml
  # schedule:
  #   - cron: '0 20 * * *'  # UTC 20:00 (Beijing Time 04:00)
```

**5. Test Workflow**

- Go to Actions → Daily M365 Audit Logs → Run workflow.

- Select branch main and click Run workflow.

- Verify:
  - JSON logs appear in the repository logs folder.
  - Logs are uploaded to your configured OneDrive folder.

**6. Notes**

- Ensure **admin consent** is granted for all API permissions.
- **ALWAYS USE GitHub Secrets — NEVER COMMIT CREDENTIALS.**
- The workflow commits JSON logs to the repository by default; you may ignore or remove them if unnecessary, or **make this repository PRIVATE.**
- Point Power Apps / Power Automate to the OneDrive folder for dashboard visualization.

## License

[MIT License](LICENSE) — Free to use, modify, and share.
