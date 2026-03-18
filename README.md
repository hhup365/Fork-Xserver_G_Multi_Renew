# XServer 游戏服务器自动续签（多账号随机乱序版）

自动延长 XServer Game VPS 实例期限，支持多账号、代理和 Telegram 通知。

---

## 前期准备：关闭邮件验证功能

> ⚠️ **重要**：请严格按照以下步骤操作，否则脚本可能因邮件验证而失败。

### 1. 点击「账户」

<img width="1818" height="501" alt="image" src="https://github.com/user-attachments/assets/f774ea20-ba10-4913-b01f-e0107bce9e94" />

### 2. 点击「查看和编辑注册信息」

<img width="874" height="546" alt="image" src="https://github.com/user-attachments/assets/c8c332d3-1e81-457b-9328-5e6971c31c42" />

### 3. 修改「可疑登录期间的身份验证」为「无效的」

<img width="1694" height="367" alt="image" src="https://github.com/user-attachments/assets/d331a64b-e94f-4663-8530-67e438264302" />

---

## 使用方法

### 第一步：Fork 本仓库

点击右上角 **Fork** 按钮，将项目复制到你的 GitHub 账户。

### 第二步：配置 Secrets

进入你 Fork 的仓库：**Settings → Secrets and variables → Actions**

点击 **New repository secret**，添加以下配置：

| Secret 名称 | 必填 | 说明 |
|:---|:---:|:---|
| `EML_1` | ✅ | 第1个账号的登录邮箱或ID |
| `PWD_1` | ✅ | 第1个账号的密码 |
| `EML_2` / `PWD_2` | ❌ | 第2个账号（以此类推） |
| `TG_TOKEN` | ❌ | Telegram Bot Token（从 @BotFather 获取） |
| `TG_ID` | ❌ | 接收通知的用户/群组 ID |
| `PROXY_URL` | ❌ | 代理链接（支持 socks5/http/vless/vmess/tuic/hy2） |

#### 多账号配置示例

```
EML_1 = account1@example.com
PWD_1 = password1
EML_2 = account2@example.com
PWD_2 = password2
EML_3 = account3@example.com
PWD_3 = password3
```

#### 代理格式示例

支持 Base64 编码和明文两种格式：

```
tuic://uuid:password@server:port?sni=domain.com
vless://uuid@server:port?security=tls&sni=domain.com
vmess://eyJhZGQiOiIxMjcuMC4wLjEiLCJpZCI6InV1aWQifQ==
hy2://password@server:port?sni=domain.com
socks5://user:pass@server:port
```

### 第三步：运行

#### 自动运行

脚本每天北京时间 **06:00** 自动执行。

#### 手动运行

1. 进入 **Actions** 页面
2. 选择 **XServer Extend (Node.js Matrix)**
3. 点击 **Run workflow**

---

## 运行逻辑

| 场景 | 行为 |
|:---|:---|
| 首次签到 | 随机延迟 0-6 小时后尝试签到 |
| 首次签到失败 | 发送通知「等待中」，明天自动重试 |
| 签到成功后 | 48小时后才能再次签到 |
| 后续签到 | 满48小时后，随机延迟 0-6 小时签到 |

---

## Telegram 通知说明

| 图标 | 状态 | 说明 |
|:---:|:---|:---|
| ✅ | 续签成功 | 实例已延期 |
| 🕐 | 首次签到 - 等待中 | 签到按钮尚未出现，明天重试 |
| ⚠️ | 自动跳过 | 未发现延期按钮 |
| ❌ | 续签失败 | 出现错误 |

---

## 常见问题

### 签到失败怎么办？

1. 检查是否已关闭邮件验证功能（见「前期准备」）
2. 检查账号密码是否正确
3. 检查代理是否可用（如配置了代理）
4. 查看 Actions 日志和截图

### 如何查看截图？

每次运行后，进入 **Actions** → 点击对应记录 → 下载 **Artifacts** 中的截图压缩包。

### 支持多少个账号？

无限制。按顺序添加 `EML_1`/`PWD_1`、`EML_2`/`PWD_2` ... 即可。

---

## 注意事项

- 签到按钮在实例开通/续签后 **48小时** 才会出现
- 首次使用建议手动触发一次，确认配置正确
- GitHub Actions 不支持 IPv6，如需访问 IPv6 节点请使用代理

## 🌟 特别鸣谢

在此感谢 [XCQ0607/Xserver_script](https://github.com/XCQ0607/Xserver_script) 项目提供的签到思路。
