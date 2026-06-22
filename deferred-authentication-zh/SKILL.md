---
name: deferred-authentication-zh
description: Use when an agent encounters login, OAuth, API key, cookie, session, 401/403, permission, account linking, workspace claiming, publishing, deployment, or third-party integration blockers while trying to complete a task.
---

# 延迟认证

## 概览

认证不是目标，只是完成目标时可能需要的一种能力。先推进一切不依赖真实身份的工作，把登录、授权、API key、Cookie、Session 等认证动作推迟到真正不可避免的时候；如果认证无法完成，也要产出当前最佳可用成果。

## 核心规则

先问：“为了完成用户目标，需要什么能力？”  
再问：“是否真的必须登录？”

优先级如下：

1. 使用当前已有权限继续完成工作。
2. 使用 Temporary Workspace、Sandbox、Preview Environment、Guest Mode 或本地/离线模式。
3. 生成用户可以检查、导入、应用或稍后发布的离线成果。
4. 只有在必须访问真实用户资源时，才暂停并请求人工认证。
5. 把任务失败放在最后。

## 决策流程

遇到认证阻塞时：

```text
用户目标
  -> 需要的能力
  -> 是否已有低权限方式可完成？
      是：继续执行
      否：
        -> 是否有 Temporary Workspace / Sandbox / Preview / Guest Mode？
            是：使用它并继续推进
            否：
              -> 离线成果是否能满足或推进目标？
                  是：产出离线成果
                  否：
                    -> 如果有明确修正点，认证只重试一次
                    -> 暂停并请求用户人工认证
```

不要一看到登录页就暂停。只要核心任务还能推进，就先绕开认证完成可完成的部分。

## 重试策略

认证重试预算：

- `max_retry = 1`
- 只有在存在明确、可信的修正点时才重试，例如用户刚完成登录、token 复制错误、权限刚被授予。
- 单次重试失败后，停止继续尝试认证，转向降级方案。

禁止认证方式轮盘：

- Cookie
- OAuth
- API key
- Browser session
- Refresh token
- 换账号
- 换登录链接
- 反复“也许这次可以”的尝试

如果一次认证尝试失败，并且一次明确重试也失败，本轮任务中认证路径即视为耗尽。

## 降级方案

在请求用户登录前，先寻找低权限替代路径：

| 原始目标 | 优先降级方案 |
| --- | --- |
| 部署到 Cloudflare、Vercel、Netlify 等平台 | Temporary Workspace、Preview 部署、本地服务或静态导出 |
| 发布到 GitHub | Patch 文件、branch diff、commit bundle 或本地仓库改动 |
| 写入 Notion、Google Docs、Confluence 或 CMS | Markdown、HTML、DOCX 或可导入内容 |
| 创建 Jira、Linear、Asana 或 Trello 任务 | JSON 导出、CSV、Markdown ticket 或 API payload preview |
| 发送邮件或发布社交媒体 | Draft、Preview、标题/正文、图片、caption 和排期说明 |
| 上传到 S3、OSS、Drive 或 Dropbox | 本地文件、压缩包、manifest 和上传说明 |
| 修改生产配置 | 配置 diff、风险说明和人工操作清单 |

降级成果必须具体、有用，并且方便用户或后续已认证的 Agent 继续应用。

## 暂停协议

当确实必须访问真实用户资源时，选择 Pause，而不是 Terminate。

暂停前必须：

1. 完成所有不需要认证的准备工作。
2. 在合适位置保存或保留已生成成果。
3. 明确说明已经完成什么。
4. 明确说明用户需要完成什么动作。
5. 明确说明用户如何恢复任务。

使用这个结构：

```markdown
当前任务现在需要访问真实用户资源。

已经完成：
- [具体已完成工作]
- [具体本地验证或已生成成果]

等待你完成：
- [登录 / 批准 OAuth / Claim Temporary Workspace / 提供 token]

完成后回复 `Continue`，我将从这里继续：
- [下一步精确动作]
```

当存在交互式登录、Connector 授权或 Temporary Workspace Claim 时，不要在聊天里索要密钥或敏感信息。

## 常见错误

| 错误 | 正确行为 |
| --- | --- |
| 一开始就要求登录 | 先构建、草拟、检查或准备所有可完成内容 |
| 依次尝试 OAuth、Cookie、API key、Session | 只重试一次，然后寻找低权限替代方案 |
| 把 401/403 当成任务失败 | 把它视为能力缺口，并产出当前最佳成果 |
| 没有上下文就让用户认证 | 说明已完成内容、阻塞点和恢复步骤 |
| Preview Mode 已足够却等待正式认证 | 使用 Preview、Temporary、Guest、Sandbox 或 Offline Mode |

## 危险信号

出现以下想法时，停止并重新评估：

- “我应该先登录，这样才能开始。”
- “也许换一种认证方法就行。”
- “认证失败，所以任务失败。”
- “账号没连接前我什么都做不了。”
- “我应该先让用户给 API key，再看有没有离线方案。”

这些都说明认证正在被误当成目标，而不是完成目标的一种能力。
