# 贡献指南

欢迎你为 "Breaking GFW" 项目做出贡献！以下是你参与本项目所需的指南：

## 环境准备

项目主要以 Docsify 制作。因此，在开始贡献前，你需要安装 Node.js 和 Docsify。在你的本地环境上，请按照以下步骤操作：

1. 安装 [Node.js](https://nodejs.org/)，你可以在其官方网站找到安装指南。

2. 安装 Docsify CLI。在你的命令行中执行以下命令：

```bash
npm i docsify-cli -g
```

## 如何开始

首先，你需要将这个项目 fork 到你的 GitHub 账户中。

然后，克隆你刚才 fork 的项目到你的本地环境中：

```bash
git clone https://github.com/<你的用户名>/breaking-gfw-book.git
cd breaking-gfw-book
```

现在，你可以在本地浏览和修改这个项目了。

## 文件结构

所有的文档都存放在 `docs` 文件夹中。如果你希望对现有的内容进行修改，或者增加新的内容，你可以在这个文件夹中找到对应的 `.md` 文件。

## 运行项目

在你的本地环境中运行以下命令：

```bash
docsify serve ./docs
```

然后，打开你的浏览器，输入 `http://localhost:3000` 就可以看到你的修改效果了。

## 提交更改

当你对内容进行了修改后，你需要提交这些更改。你可以执行以下命令：

```bash
git add .
git commit -m "你的提交信息"
git push origin main
```

请确保你的提交信息清晰，简洁地描述了你所做的更改。

## 创建 Pull Request

在你的 GitHub 项目页面，点击 "New pull request"。然后选择你所做更改的分支，并点击 "Create pull request"。

请在 Pull Request 中提供对你所做更改的详细描述。这将有助于项目维护者理解你的更改。

## 贡献内容

我们欢迎对所有主题进行贡献，特别是关于 Shadowsocks 以及与 Shadowsocks 相关的各种插件，ShadowsocksR, ShadowsocksRR, V2ray，以及 Telegram 通讯软件的 Proxy 的研究和使用经验。

同时，如果你发现任何错误或者有任何建议，也请不要犹豫，直接提出。

## 许可证

参与本项目，意味着你同意该项目的 CC BY-NC-SA 许可证。

感谢你对 "Breaking GFW" 项目的贡献！
