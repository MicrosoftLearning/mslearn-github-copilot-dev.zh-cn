---
lab:
  title: 准备 - 为 GitHub Copilot 练习配置实验室环境 (Python)
  description: 在开始 GitHub Copilot 练习前，请查看实验室要求并配置资源。
---

# 为 GitHub Copilot 练习配置实验室环境

必须使用 Visual Studio Code 和 GitHub Copilot 为 Python 开发配置实验室环境。 需要访问已启用 GitHub Copilot 的 GitHub 帐户。

完成以下步骤，验证实验室环境是否已正确配置：

1. 验证是否已在实验室环境中安装 Git 版本 2.48 或更高版本。

    在终端窗口中运行以下命令，检查已安装的 Git 版本：

    ```bash
    git --version
    ```

    如果运行 Windows 并想要更新 Git，可以使用以下命令：

    ```bash
    git update-git-for-windows
    ```

    如有必要，可以使用以下 URL 下载 Git：<a href="https://git-scm.com/downloads" target="_blank">下载 Git</a>。

1. 验证是否已在实验室环境中安装最新版本的 Python。

    在终端窗口中运行以下命令，检查已安装的 Python 版本：

    ```bash
    python3 --version
    ```

    如有必要，请按照以下 URL 中的步骤在 Visual Studio Code 中配置 Python ：<a href="https://code.visualstudio.com/docs/python/python-tutorial" target="_blank">VS Code 中的 Python 入门</a>。

1. 验证 Visual Studio Code 和 Python 扩展是否已安装在实验室环境中。

    如有必要，可以使用以下 URL 下载 Visual Studio Code：<a href="https://code.visualstudio.com/download" target="_blank">下载 Visual Studio Code</a>

    可以使用 Visual Studio Code 中的“扩展”视图安装 Python 扩展。

1. 验证你是否有权访问 GitHub 帐户和 GitHub Copilot 订阅。

    可以使用以下 URL 登录到 GitHub 帐户：<a href="https://github.com/login" target="_blank">GitHub 登录</a>。

    如果没有 GitHub 帐户，可以从 GitHub 登录页创建单个帐户。 在登录页上，选择“创建帐户”****。

    打开 GitHub 帐户的设置/配置文件页，并验证你是否有权访问 GitHub Copilot 订阅。 如果你有可用于训练的 GitHub Copilot Pro、GitHub Copilot Pro+、GitHub Copilot Business 或 GitHub Copilot Enterprise 的活动订阅，则可以使用现有的 GitHub Copilot 订阅来完成 GitHub Copilot 练习。

    如果你有单个 GitHub 帐户，但没有 GitHub Copilot 订阅，可以在训练练习期间从 Visual Studio Code 中设置 GitHub Copilot Free 计划。

    > **重要说明**：GitHub Copilot 免费计划是 GitHub Copilot 的有限版本，每月最多允许 2,000 个代码补全和 50 次聊天或高级请求。 如果在训练练习之外使用 GitHub Copilot 免费计划，则在完成训练前，可能会超出计划的资源限制。 GitHub Copilot 免费计划不适用于 GitHub Copilot Pro、GitHub Copilot Pro+、GitHub Copilot Business 或 GitHub Copilot Enterprise 订阅。
