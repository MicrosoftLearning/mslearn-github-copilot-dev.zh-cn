---
lab:
  title: 准备 - 为 GitHub Copilot 练习配置实验室环境
  description: 在开始 GitHub Copilot 练习之前，查看实验室要求并配置资源。
---

# 为 GitHub Copilot 练习配置实验室环境

你的实验室环境必须配置为使用 Visual Studio Code 和 GitHub Copilot 进行 C# 开发。 需要访问启用了 GitHub Copilot 的 GitHub 帐户。

完成以下步骤以验证你的实验室环境是否配置正确：

1. 验证实验室环境中是否安装了 Git 2.48 或更高版本。

    在终端窗口中运行以下命令以检查已安装的 Git 版本：

    ```bash
    git --version
    ```

    如果你运行的是 Windows，并且想要更新 Git，可以使用以下命令：

    ```bash
    git update-git-for-windows
    ```

    如有必要，你可以使用以下 URL 下载 Git：<a href="https://git-scm.com/downloads" target="_blank">下载 Git</a>。

1. 验证你的实验室环境中是否安装了 .NET SDK 的最新 LTS 或 STS 版本。

    在终端窗口中运行以下命令以检查已安装的 .NET SDK 版本：

    ```dotnetcli
    dotnet --version
    ```

    如有必要，你可以使用以下 URL 下载 .NET SDK：<a href="https://dotnet.microsoft.com/download/dotnet" target="_blank">下载 .NET SDK</a>。

1. 验证实验室环境中是否安装了 Visual Studio Code 和 C# Dev Kit 扩展。

    如有必要，你可以使用以下 URL 下载 Visual Studio Code：<a href="https://code.visualstudio.com/download" target="_blank">下载 Visual Studio Code</a>

    你可以使用 Visual Studio Code 中的“扩展”视图安装 C# Dev Kit 扩展。

1. 验证你是否可以访问 GitHub 帐户和 GitHub Copilot 订阅。

    你可以使用以下 URL 登录到你的 GitHub 帐户：<a href="https://github.com/login" target="_blank">GitHub 登录</a>。

    如果你没有 GitHub 帐户，可以从 GitHub 登录页面创建个人帐户。 在登录页面上，选择“创建帐户”****。

    打开 GitHub 帐户的设置/个人资料页面，并验证你是否可以访问 GitHub Copilot 订阅。 如果你有可用于培训的 GitHub Copilot Pro、GitHub Copilot Pro+、GitHub Copilot Business 或 GitHub Copilot Enterprise 的有效订阅，则可以使用现有的 GitHub Copilot 订阅完成 GitHub Copilot 练习。

    如果你有一个个人 GitHub 帐户，但没有 GitHub Copilot 订阅，则可以在培训练习期间从 Visual Studio Code 设置 GitHub Copilot 免费计划。

    > **重要说明**：GitHub Copilot 免费计划是 GitHub Copilot 的有限版本，每月最多允许 2,000 个代码补全和 50 次聊天或高级请求。 如果你在培训练习之外使用 GitHub Copilot 免费计划，则可能会在完成培训之前超出计划的资源限制。 GitHub Copilot 免费计划不适用于 GitHub Copilot Pro、GitHub Copilot Pro+、GitHub Copilot Business 或 GitHub Copilot Enterprise 订阅。
