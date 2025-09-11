YouTube Video Notifier
项目简介
YouTube Video Notifier 是一个自动化工具，用于监控指定 YouTube 频道的最新视频，并在新视频发布时通过电子邮件发送通知。该项目旨在帮助用户及时获取感兴趣的 YouTube 频道更新，无需手动检查。
功能特性

实时监控：通过 GitHub Actions 定期检查指定 YouTube 频道的最新视频。
电子邮件通知：新视频发布时，通过电子邮件发送视频标题、链接和发布时间。
自动化部署：利用 GitHub Actions 实现自动运行和计划任务。
可配置化：通过 GitHub Secrets 配置 API 密钥、频道 ID 和邮件设置。

技术栈

编程语言：Python
主要库：
google-api-python-client：用于访问 YouTube Data API。
smtplib：用于发送电子邮件通知。


自动化工具：GitHub Actions
依赖管理：pip

部署步骤

Fork 此仓库：

访问 https://github.com/laozhiy/youtube-video-notifier。
点击右上角的 Fork 按钮，将仓库复制到你的 GitHub 账户。


设置 GitHub Secrets：

在你的仓库页面，点击 Settings。
找到 Secrets and variables，选择 Actions。
点击 New repository secret，添加以下 Secrets：
YOUTUBE_API_KEY：你的 YouTube Data API 密钥（从 Google Cloud Console 获取）。
CHANNEL_ID：你要监控的 YouTube 频道 ID（例如 UCXXXXXXXXXXXXXXXXXXXXXX）。
EMAIL_ADDRESS：用于发送通知的电子邮件地址（例如 Gmail）。
EMAIL_PASSWORD：该邮箱的密码（对于 Gmail，建议使用应用专用密码）。




安装依赖：

确保项目包含 requirements.txt，列出所需库：google-api-python-client


GitHub Actions 会自动根据 requirements.txt 安装依赖。


配置 GitHub Actions：

项目根目录下的 .github/workflows/main.yml 已配置好自动化工作流。
默认设置将定期（例如每天）检查指定频道的视频更新。
可根据需要修改 main.yml 中的 schedule 字段调整运行频率，例如：on:
  schedule:
    - cron: '0 0 * * *' # 每天 UTC 时间 00:00 运行




运行项目：

推送代码到你的仓库后，GitHub Actions 将自动运行。
工作流会根据配置的计划检查 YouTube 频道更新，并在检测到新视频时发送邮件通知。



使用方法

确保 GitHub Secrets 已正确配置。
GitHub Actions 会自动运行 main.py，监控指定的 YouTube 频道。
当检测到新视频时，程序会通过配置的邮箱发送通知，包含视频标题、链接和发布时间。

项目结构
youtube-video-notifier/
├── .github/
│   └── workflows/
│       └── main.yml     # GitHub Actions 工作流配置文件
├── main.py              # 主程序入口
├── youtube_api.py       # YouTube API 交互模块
├── email_notifier.py    # 邮件通知逻辑实现
├── requirements.txt     # 依赖列表
└── README.md            # 项目说明

贡献
欢迎任何形式的贡献！请按照以下步骤：

Fork 本项目。
创建你的功能分支（git checkout -b feature/YourFeature）。
提交你的更改（git commit -m 'Add YourFeature'）。
推送到远程分支（git push origin feature/YourFeature）。
提交 Pull Request。

许可证
本项目采用 MIT 许可证。
联系方式
如有问题或建议，请通过 GitHub Issues 联系，或发送邮件至 [your-email@example.com]。
