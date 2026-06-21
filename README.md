# Astrbot_Plugin_QQGroup_Notice

一个灵活的 AstrBot 插件，用于在 QQ 群中自动通知成员**入群**和**退群**事件。  
支持通过 **AstrBot WebUI 可视化配置**，无需手动编辑文件，轻松自定义消息模板和用户昵称显示。

## ✨ 功能特性

- 🎉 **入群通知**：成员加入时自动发送欢迎消息（模板可自定义）
- 👋 **退群通知**：成员退出或被踢时自动通知（模板可自定义）
- 🧩 **灵活配置**：所有设置（模板、开关、NapCat 地址）均在 WebUI 中完成
- 👤 **昵称显示**：可选择显示 QQ 号或真实昵称（需启用 NapCat HTTP 服务）
- ⚡ **即时生效**：修改配置后，在 WebUI 重载插件即可，无需重启机器人

## 📦 安装

### 方法一：通过 Git 克隆（推荐）

进入 AstrBot 插件目录（默认 `AstrBot/data/plugins/`），执行：

```bash
git clone https://github.com/New-Weilai/astrbot_plugin_QQgroup_notice.git
```

方法二：手动创建

1. 在 AstrBot/data/plugins/ 下新建文件夹 astrbot_plugin_qqgroup_notice
2. 将 main.py、metadata.yaml、_conf_schema.json 放入该文件夹
3. （可选）放入 README.md

方法三：从插件市场安装（如果已上架）

在 AstrBot WebUI 的 “插件” → “插件市场” 中搜索并一键安装。

⚙️ 配置（通过 WebUI）

本插件所有配置项均通过 AstrBot WebUI 图形界面 进行管理，无需编辑任何配置文件。

1. 安装插件后，在 WebUI 左侧菜单进入 “插件” 页面
2. 找到 “群成员入群/退群通知插件”，点击其右侧的 “配置” 按钮
3. 你会看到以下配置表单：

配置项 类型 默认值 说明
显示用户昵称 开关 关闭 开启后，通知中会显示用户昵称（需 NapCat HTTP 服务正常运行）
入群欢迎模板 文本框 🎉 欢迎新成员 {nickname}！({way}) 自定义入群消息，支持变量
退群通知模板 文本框 👋 成员 {nickname} 已{reason} 自定义退群消息，支持变量
NapCat HTTP 服务地址 文本框 127.0.0.1 用于获取用户昵称（一般为 127.0.0.1 或容器内服务名）
NapCat HTTP 服务端口 数字 3000 NapCat HTTP 服务端口（默认 3000）

1. 修改后点击 “保存”，然后在插件页面点击 “重载插件” 使配置生效。

📝 消息模板支持的变量

入群模板可用变量：

· {user_id} – 入群者的 QQ 号
· {nickname} – 入群者的昵称（若开启昵称功能）
· {group_id} – 群号
· {way} – 入群方式（如“通过邀请或申请入群”、“被 xxx 邀请入群”）
· {inviter_id} – 邀请人的 QQ 号（仅当为邀请入群时有效）

退群模板可用变量：

· {user_id} – 退群者的 QQ 号
· {nickname} – 退群者的昵称（若开启昵称功能）
· {group_id} – 群号
· {reason} – 退群原因（如“主动退群”、“被管理员 xxx 踢出”）
· {operator_id} – 操作人（踢人者）的 QQ 号

💬 效果示例

默认配置（仅显示 QQ 号）

· 入群：🎉 欢迎新成员 123456！(通过邀请或申请入群)
· 退群：👋 成员 123456 已主动退群

开启昵称并自定义模板

假设配置为：

· 显示用户昵称：开启
· 入群模板：🎉 欢迎 {nickname}（{user_id}）！{way}
· 退群模板：👋 {nickname}（{user_id}）{reason}

则效果：

· 入群：🎉 欢迎 小明（123456）！通过邀请或申请入群
· 退群：👋 小明（123456）主动退群

🔧 前置依赖

· Python 3.8+
· AstrBot >= v4.0.0
· NapCat / OneBot v11 协议（用于接收群事件）
· 可选：若开启昵称显示，需在 NapCat 中启用 HTTP 服务（端口默认 3000）

如何开启 NapCat HTTP 服务？

1. 登录 NapCat WebUI（http://你的服务器IP:6099/webui/）
2. 进入 “网络配置” → 添加新配置
3. 类型选择 “HTTP 服务器”，Host 填 0.0.0.0，Port 填 3000（与插件配置一致）
4. 保存并重启 NapCat

📁 插件文件结构

```
astrbot_plugin_qqgroup_notice/
├── metadata.yaml          # 插件元数据（名称、作者、版本等）
├── _conf_schema.json      # 配置表单定义（供 WebUI 渲染）
├── main.py                # 插件主逻辑
└── README.md              # 本文档
```

📋 事件支持详情

事件类型 notice_type sub_type 触发条件
入群 group_increase approve 通过申请或邀请入群
入群 group_increase invite 被邀请入群（含邀请人信息）
退群 group_decrease leave 主动退群
退群 group_decrease kick 被管理员踢出（含踢人者信息）

❓ 常见问题

Q：为什么发送的通知里只显示 QQ 号，没有昵称？
A：请检查两项：① 在插件配置中是否开启了“显示用户昵称”；② NapCat 的 HTTP 服务是否正常运行，且地址/端口与插件配置一致。

Q：修改配置后不生效怎么办？
A：保存配置后，请务必在 WebUI 的插件页面点击 “重载插件”。如果仍不生效，尝试重启 AstrBot 服务。

Q：可以在一个群中禁用通知吗？
A：目前不支持按群开关，如需此功能可提交 Issue 请求添加。

🤝 贡献

欢迎提交 Issue 和 Pull Request。请确保代码符合 AGPL-3.0 许可证要求。

📄 许可证

本项目使用 AGPL-3.0 许可证。详情请参阅 LICENSE 文件。

🙏 致谢

· AstrBot – 强大的机器人框架
· NapCat – 优雅的 QQ机器人框架 实现
