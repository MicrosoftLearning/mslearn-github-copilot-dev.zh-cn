---
lab:
  title: 练习 - 使用 GitHub Copilot 解决 GitHub 问题
  description: 了解如何在 Visual Studio Code 中使用 GitHub Copilot 识别并解决代码安全漏洞。
---

# 使用 GitHub Copilot 解决 GitHub 问题

GitHub 问题是跟踪项目中的 Bug、改进和任务的有效方式。

在本练习中，你将使用 GitHub Copilot 来帮助分析和解决与电子商务应用程序中的安全漏洞相关的 GitHub 问题。

完成此练习大约需要 40 分钟。

> **重要说明**：要完成本练习，需要提供自己的 GitHub 帐户和 GitHub Copilot 订阅。 如果没有 GitHub 帐户，可以<a href="https://github.com/" target="_blank">注册</a>免费的个人帐户，并使用 GitHub Copilot 免费版计划来完成练习。 如果可以从实验室环境中访问 GitHub Copilot Pro、GitHub Copilot Pro+、GitHub Copilot Business 或 GitHub Copilot Enterprise 订阅，则可以使用你现有的 GitHub Copilot 订阅来完成本练习。

## 开始之前

实验室环境必须包括以下资源：Git 2.48 或更高版本、.NET SDK 9.0 或更高版本、具有 C# 开发工具包扩展的 Visual Studio Code，以及访问启用了 GitHub Copilot 的 GitHub 帐户。

如果你将本地电脑用作本练习的实验室环境：

- 有关将本地电脑配置为实验室环境的帮助，请在浏览器中打开以下链接：<a href="https://go.microsoft.com/fwlink/?linkid=2320147" target="_blank">配置实验室环境资源</a>。

- 有关在 Visual Studio Code 中启用 GitHub Copilot 订阅的帮助，请在浏览器中打开以下链接：<a href="https://go.microsoft.com/fwlink/?linkid=2320158" target="_blank">在 Visual Studio Code 中启用 GitHub Copilot</a>。

如果你将在本练习中使用托管实验室环境：

- 有关在 Visual Studio Code 中启用 GitHub Copilot 订阅的帮助，请将以下 URL 粘贴到浏览器的网站导航栏中：<a href="https://go.microsoft.com/fwlink/?linkid=2320158" target="_blank">在 Visual Studio Code 中启用 GitHub Copilot</a>。

- 为了确保 .NET SDK 配置为使用官方 NuGet.org 存储库作为下载和还原包的源：

    打开命令终端，然后运行以下命令：

    ```bash

    dotnet nuget add source https://api.nuget.org/v3/index.json -n nuget.org

    ```

- 为了确保 Git 配置为使用名称和电子邮件地址：

    使用你的信息更新以下命令，然后运行命令：

    ```bash

    git config --global user.name "Julie Miller"

    ```

    ```bash

    git config --global user.email julie.miller@example.com

    ```

## 练习场景

你是一家咨询公司的软件开发人员。 你的客户需要帮助来解决他们 GitHub 存储库中的问题。 你需要确保所有问题都得到解决并关闭。 你将使用 Visual Studio Code 作为开发环境，并使用 GitHub Copilot 来协助完成开发任务。 公司给你分配了以下应用：

- ContosoShopEasy：ContosoShopEasy 是一个电子商务应用程序，包含多个安全漏洞。 这些漏洞代表了实际应用程序中常见的安全问题。

本练习包括以下任务：

1. 导入 ContosoShopEasy 存储库。
1. 查看 GitHub 中的问题。
1. 在本地克隆存储库并查看代码库。
1. 使用 GitHub Copilot 的询问模式分析问题。
1. 使用 GitHub Copilot 的智能体模式解决问题。
1. 测试并验证重构的代码。
1. 提交更改并关闭问题。

### 导入 ContosoShopEasy 存储库

使用 GitHub 导入工具，你可以在自己的 GitHub 帐户中创建现有存储库的副本，从而完全控制导入的副本。 尽管 GitHub 导入工具不会迁移问题、拉取请求或讨论，但它会导入 GitHub Actions 工作流。 你导入的存储库包含一个 GitHub Actions 工作流，用于创建与代码库关联的问题。

在此任务中，你将导入 ContosoShopEasy 存储库，并运行一个工作流，为代码库中包含的安全漏洞创建 GitHub 问题。

使用以下步骤完成此任务：

1. 打开浏览器窗口并导航到 GitHub.com。

1. 登录到 GitHub 帐户，然后打开存储库选项卡。

    通过单击右上角的配置文件图标，并选择“存储库”****，可以打开存储库选项卡。

1. 在“存储库”选项卡上，选择“新建”按钮。****

1. 在“新建存储库”部分下，选择“导入存储库”********。

    此时会显示“将项目导入到 GitHub”页。****

1. 在“将项目导入到 GitHub”页上的“源存储库详细信息”下，输入源存储库的以下 URL：****

    ```plaintext
    https://github.com/MicrosoftLearning/resolve-github-issues-lab-project
    ```

1. 在“新存储库详细信息”部分的“所有者”下拉列表中，选择你的 GitHub 用户名。********

1. 在“存储库名称”字段中，输入 ResolveGitHubIssues，然后选择“开始导入”。************

    GitHub 会使用 ContosoShopEasy 项目文件在你的帐户中创建一个新存储库。

1. 等待导入过程完成，然后打开新存储库。

    > 注意****：导入存储库可能需要一两分钟时间。

1. 打开存储库中的“操作”选项卡。

1. 在页面左侧的“所有工作流”下，选择“创建 ContosoShopEasy 训练问题”工作流，然后选择“运行工作流”。************

1. 在显示的工作流对话框中，键入 CREATE，然后选择“运行工作流”。********

1. 监视屏幕上工作流的进度。

    片刻之后，页面将刷新并显示进度栏。 工作流应在不到一分钟的时间内成功完成。

1. 在继续操作之前，请确保工作流成功完成。

    绿色圆圈内的勾号指示工作流已成功运行（应显示在工作流名称的左侧）。

    如果红色圆圈内的 X 出现在工作流名称的左侧，则表示工作流失败。 如果工作流无法成功运行，请确保在导入存储库时选择了你的帐户，并且帐户具有读取和写入权限。 可以使用 GitHub 的“使用 Copilot 聊天”功能来帮助诊断问题。****

### 查看 GitHub 中的问题

GitHub 问题是一个集中式跟踪系统，可用于跟踪 bug、安全漏洞和改进请求。 每个问题都会提供关于问题的背景信息、严重程度，以及它对应用程序可能产生的影响。 在深入研究代码之前了解这些问题，有助于确定优先事项，并确保进行全面的修正。

在此任务中，你需要审查 GitHub 问题并检查需要解决的安全漏洞。

使用以下步骤完成此任务：

1. 选择存储库的“问题”选项卡，然后花一分钟时间查看“问题”页。****

    你应该会看到列出的 10 个未解决问题。 注意有关这些问题的以下详细信息：

    - 所有问题都标记为 bug。
    - 所有问题都具有优先级。
    - 这些问题都没有分配给任何人。

1. 若要仅显示严重问题，请选择“标签”下拉列表，然后选择“严重”标签。********

    问题列表应更新为仅显示严重问题。

    - 🔐 删除硬编码的管理员凭据****  

    - 🔐 修复信用卡数据存储冲突****  

1. 若要仅显示高优先级问题，请选择“标签”下拉列表，取消选择“严重”，然后选择“高优先级”标签。************

    问题列表应更新为仅显示高优先级问题。

    - 🔐 修复输入验证安全性绕过****  

    - 🔐 从调试日志记录中删除敏感数据****  

    - 🔐 将 MD5 密码哈希替换为安全替代项****  

    - 🔐 修复产品搜索中的 SQL 注入漏洞****  

1. 选择“修复产品搜索中的 SQL 注入漏洞”问题。****

1. 花点时间查看问题详细信息。

    问题详细信息应描述问题和预期的修复方案。

    > 注意****：用于记录问题的过程（包括手动过程与 AI 自动化过程）可能会影响问题描述的整体质量和准确性。 本次培训中包含的问题是在智能体查看代码库后，使用 GitHub Copilot 的智能体模式编写的。 GitHub Copilot 可用于生成有关漏洞的详细描述、代码位置、易受攻击的代码示例、安全风险以及修复的验收标准。

1. 请注意，没有人被分配到此问题。

1. 导航回“问题”选项卡并清除筛选器。

1. 选择以下严重问题和高优先级问题，然后使用“分配”下拉列表将其分配给你自己。****

    - 🔐 修复信用卡数据存储冲突****  

    - 🔐 修复产品搜索中的 SQL 注入漏洞****  

    通常最好先处理优先级较高的问题。 将问题分配给你自己有助于在完成修正过程时跟踪进度。

### 在本地克隆存储库并查看代码库

ContosoShopEasy 应用程序采用了企业应用程序的典型分层体系结构，在模型、服务、数据访问和安全组件之间实现了清晰的分离。

花时间了解现有代码库的基本结构、行为和功能是解决安全问题的重要第一步。

在此任务中，你将创建存储库的本地克隆，检查 Visual Studio Code 中的项目结构，查看应用程序的控制台输出，并识别代码库中的安全漏洞。

使用以下步骤完成此任务：

1. 导航回存储库的根页（“代码”选项卡）。

1. 将 ResolveGitHubIssues 存储库克隆到本地开发环境。

    例如，可以使用以下步骤通过 Git CLI 克隆存储库：

    1. 通过选择“代码”按钮，然后复制 HTTPS URL，来复制存储库 URL。****

    1. 打开终端窗口，导航到要克隆存储库的目录，然后运行使用存储库 URL 的“git clone”命令。

        例如，打开 Windows PowerShell，导航到 C:\TrainingProjects，然后运行以下命令（将 your-username 替换为 GitHub 用户名）：****

        ```bash
        git clone https://github.com/your-username/ResolveGitHubIssues.git
        ```

1. 在 Visual Studio Code 中打开克隆的存储库。

    确保使用的是最新版本的 Visual Studio Code，并且已安装并启用了 GitHub Copilot 和 GitHub Copilot 对话助手扩展。

1. 在“资源管理器”中，检查项目结构。

    ContosoShopEasy 应用程序采用了具有以下组件的分层体系结构：

    - Data/****：包含 OrderRepository.cs、ProductRepository.cs 和 UserRepository.cs 的数据存储库。************

    - Models/****：包含用于 Category.cs、Order.cs、Product.cs 和 User.cs 的数据模型。****************

    - Security/****：包含 SecurityValidator.cs 的安全验证逻辑****

    - Services/****：包含 OrderService.cs、PaymentService.cs、ProductService.cs 和 UserService.cs 的业务逻辑。****************

    - Program.cs****：包含依赖项注入设置的主应用程序入口点

    - README.md****：说明应用程序的用途和漏洞的文档

1. 若要观察应用程序的当前行为，请生成并运行应用程序。

    例如，可以打开 Visual Studio Code 的集成终端窗口并运行以下命令：

    ```bash
    cd ContosoShopEasy
    dotnet build
    dotnet run
    ```

    该应用程序运行一个电子商务工作流模拟，通过详细的控制台日志记录公开安全漏洞。

1. 花点时间查看控制台输出。

    ContosoShopEasy 应用程序使用刻意过度的日志记录，以此作为教学工具。 除了暴露代码库中的安全问题之外，某些日志实际上也会导致问题。 包括会导致安全问题的日志可演示在某些生产系统中发现的实际过度日志记录问题。 ContosoShopEasy 应用程序中的日志记录用于帮助开发人员区分两种类型的问题：

    - 由日志记录导致的问题：ContosoShopEasy 应用程序中大约 40% 的漏洞是由过度日志记录引起的。 例如，密码泄露、信用卡号泄露、会话令牌暴露和配置信息泄露。

    - 与日志记录无关的问题：ContosoShopEasy 应用程序中大约 60% 的漏洞与日志记录无关。 例如，SQL 注入、弱密码哈希、硬编码凭据、可预测令牌、输入验证绕过、信用卡存储和弱电子邮件验证。 尽管日志记录不会导致这些漏洞，但日志记录确实有助于在训练环境中暴露这些问题。

1. 若要开始审查代码库中的安全漏洞，请展开 Models 文件夹，然后打开 Order.cs 文件。********

1. 向下滚动，找到 PaymentInfo 类。****

    注意有关 CardNumber 和信用卡验证码 (CVV) 属性的注释。 此代码与分配给你自己的“修复信用卡数据存储违规”问题相关。****

1. 展开 Security 文件夹，然后打开 SecurityValidator.cs 文件。********

    请注意，ContosoShopEasy 应用程序使用代码注释、逻辑和日志记录来暴露安全问题。 尽管实施是人为设计的，但此方法有助于突出显示实际应用程序中常见的漏洞。

    > 注意****：SecurityValidator.cs 类旨在集中 ContosoShopEasy 应用程序与安全相关的逻辑，以便更轻松地查找、管理和解决安全问题。 在实际应用程序中，可以使用 SecurityValidator 等类来强制实施安全最佳做法和输入验证。 但是，ContosoShopEasy 中的具体实施故意采用不安全的设计，并人为暴露漏洞。

1. 花点时间查找以下安全问题：

    - 在文件顶部附近，注意与管理员凭据常量相关的注释（第 7-9 行）。 这段代码与“删除硬编码的管理员凭据”问题相关。

    - 定位到 ValidateInput 方法并查看描述安全漏洞的注释。 这段代码与“修复输入验证安全绕过”问题相关。

    - 定位到 ValidateEmail 方法并查看描述安全漏洞的注释。 这段代码与“改进电子邮件验证安全性”问题相关。

    - 定位到 ValidatePasswordStrength 方法并查看描述安全漏洞的注释。 这段代码与“加强密码安全要求”问题相关。

    - 找到 ValidateCreditCard 方法并查看描述安全漏洞的注释。 此代码与分配给你自己的“修复信用卡数据存储违规”问题相关。****

    - 定位到 GenerateSessionToken 方法并查看描述安全漏洞的注释。 这段代码与“修复可预测会话令牌生成”问题相关。

    - 定位到 RunSecurityAudit 方法并查看描述安全漏洞的注释。 此代码与“减少错误消息中的信息泄漏（控制台输出）”问题相关。

    SecurityValidator.cs 文件中的一些方法还与“从调试日志记录中移除敏感数据”问题相关。

    SecurityValidator 类暴露的问题通常分布在实际应用程序的类中，尤其是旧代码库或维护不佳的代码库。

1. 展开 Services 文件夹，然后打开 UserService.cs 文件。********

1. 花点时间查找以下安全问题：

    - 定位到 RegisterUser、LoginUser 和 ValidateUserInput 方法，并查看描述安全漏洞的注释。 这段代码与“从调试日志记录中删除敏感数据”问题相关。

    - 定位到 GetMd5Hash 方法并查看描述安全漏洞的注释。 这段代码与“将 MD5 密码哈希替换为安全替代项”问题相关。

1. 打开 PaymentService.cs 文件。****

1. 花点时间查看付款和验证方法中的注释。

    此代码中的安全漏洞与分配给你自己的“修复信用卡数据存储违规”问题相关。****

    PaymentService 类还与其他问题相关。 例如，“从调试日志记录中移除敏感数据”和“减少错误消息中的信息泄漏（控制台输出）”问题。

    请注意，PaymentService 类使用 OrderRepository 来保留与付款相关的订单数据。 如果 OrderRepository 类无法正确处理敏感数据，则可能会导致 OrderRepository 类中的数据泄露漏洞。

1. 打开 ProductService.cs 文件。****

1. 花点时间查看 SearchProducts 方法。

    此代码中的安全漏洞与分配给你自己的“修复产品搜索中的 SQL 注入漏洞”问题相关。****

    请注意，ProductService 中的 SearchProducts 方法调用 ProductRepository 中的 SearchProducts 方法。 你可能需要分析存储库方法，以确定它是否也需要安全改进。

1. 列出与分配给你的问题相关的代码文件。

    分配给你自己的问题包括：

    - 🔐 修复信用卡数据存储冲突****
    - 🔐 修复产品搜索中的 SQL 注入漏洞****

    与“修复信用卡数据存储违规”问题相关的代码文件包括：

    - Models/Orders.cs/PaymentInfo 类
    - Security/SecurityValidator.cs/ValidateCreditCard 方法
    - Data/OrderRepository.cs

    与“修复产品搜索中的 SQL 注入漏洞”问题相关的代码文件包括：

    - Services/ProductService.cs/SearchProducts 方法
    - Data/ProductRepository.cs/SearchProducts 方法

### 使用 GitHub Copilot 的询问模式分析问题

GitHub 问题通常包含复杂的问题，这些问题需要在实施修复之前进行仔细分析。 了解根本原因、潜在影响和最佳修正策略对于有效解决问题至关重要。

Visual Studio Code 的以下 GitHub 扩展可帮助你分析 GitHub 问题：

- GitHub Copilot 对话助手：**** GitHub Copilot 的询问模式提供智能代码分析功能，有助于识别安全漏洞、了解其潜在影响并提出修正策略。

- GitHub 拉取请求：**** GitHub 拉取请求扩展将 GitHub 问题直接集成到 Visual Studio Code 中，让你无需离开开发环境即可管理问题并与之交互。

通过系统地分析安全问题，可以在实现修补程序之前全面了解这些问题。 此方法可确保解决方案解决根本原因，而非仅仅是表面症状。

在此任务中，你将使用 GitHub Copilot 的“询问”模式分析分配给你的 GitHub 问题。

使用以下步骤完成此任务：

1. 确保 Visual Studio Code 中安装了 GitHub Copilot 对话助手和 GitHub 拉取请求扩展。

    在 Visual Studio Code 中打开“扩展”视图并查看已安装的扩展。 如果缺少任一扩展，请先安装再继续操作。

    例如，可以使用以下步骤安装 GitHub 拉取请求扩展：

    1. 在 Visual Studio Code 中打开“扩展”视图。

    1. 在“扩展”视图中，搜索“GitHub 拉取请求”。****

    1. 从搜索结果中选择“GitHub 拉取请求”，然后安装该扩展。****

        安装完成后，可能需要重新加载 Visual Studio Code 才能使更改生效。 GitHub 图标应该会添加到 Visual Studio Code 的活动栏中。****

1. 若要打开“GitHub 拉取请求”视图，请从活动栏中选择“GitHub”图标。****

    如果系统提示，请登录到 GitHub 帐户，将 Visual Studio Code 连接到 GitHub 存储库。

1. 请注意，GitHub 视图包括两个部分：“拉取请求”和“问题”。********

    通过“问题”部分，可直接在 Visual Studio Code 中查看和管理 GitHub 存储库中的问题。**** 通过“拉取请求”部分，可以管理拉取请求。****

1. 折叠“拉取请求”部分。****

1. 花点时间查看“问题”部分。****

    请注意，分配给你自己的问题列在“我的问题”部分下（未定义里程碑）。 如果展开“最近的问题”部分，可以看到已添加到存储库的所有问题。****

1. 在“我的问题”部分下，选择“修复产品搜索中的 SQL 注入漏洞”。****

    “GitHub 拉取请求”扩展将在新的编辑器选项卡中打开问题详细信息。可以在此选项卡中查看问题描述、注释及任何相关信息。可以使用问题详细信息来帮助构建在“聊天”视图中提交到 GitHub Copilot 的提示。

1. 打开 GitHub Copilot 对话助手视图，并确保选中“询问”模式。****

    **** 如果“聊天”视图尚未打开，选择 Visual Studio Code 窗口顶部的“聊天”图标。 确定聊天模式设置为“询问”，并且使用的是 GPT-4.1 模型。********

    > 注意****：GPT-4.1 模型可提供出色的代码分析功能，并包含在 GitHub Copilot Free 计划中。 选择不同的模型可能会产生不同的结果。

1. 确保从干净的聊天会话开始。

    聊天会话有助于整理你与 GitHub Copilot 的交互。 每个会话都维护自己的上下文，使你能够专注于特定任务或问题。 会话中的对话历史记录提供连续性，使 GitHub Copilot 能够基于以前的交互构建，以获取更准确且相关的响应。 此聊天对话将侧重于分析和解决 ContosoShopEasy 应用程序中分配给你的两个安全漏洞。 使用 GitHub Copilot 的“询问”模式完成对 GitHub 问题的分析后，可以在同一对话中使用 GitHub Copilot 的“智能体”模式来协助实施代码更改。 GitHub Copilot 可以使用“询问”模式下的详细分析来指导“智能体”模式下的代码生成，从而确保修复与已识别的漏洞和建议的修正策略保持一致。

    如果需要，可以通过选择“新建聊天”按钮（“聊天”面板顶部的 + 图标）来启动新的聊天会话。********

#### 分析 SQL 注入漏洞

SQL 注入漏洞存在于 ProductService.cs 文件中，并可能存在于 ProductRepository.cs 文件中。 你将分析这两个文件，了解该漏洞的完整范围。

使用以下步骤分析 SQL 注入漏洞：

1. 打开 ProductService.cs 文件，然后定位到 SearchProducts 方法。********

1. 在代码编辑器中，选择整个 SearchProducts 方法。****

    在编辑器中选择代码有助于聚焦于聊天上下文。 GitHub Copilot 会使用所选的代码提供相关分析和建议。

1. 要求 GitHub Copilot 分析代码是否存在 SQL 注入漏洞。

    例如，你可以提交以下提示：

    ```text
    Analyze the SearchProducts method for SQL injection vulnerabilities. Consider the following issue description: "The product search functionality is vulnerable to SQL injection attacks. User input is directly concatenated into SQL queries without proper parameterization or sanitization." Explain the impact of directly concatenating user input into SQL queries without proper parameterization or sanitization. What are the potential consequences if an attacker exploits this vulnerability?
    ```

1. 查看 GitHub Copilot 的分析。

    GitHub Copilot 应该会识别出该方法直接使用用户输入来构造 SQL 查询，而没有进行适当的清理。 模拟的 SQL 查询演示了如何将用户输入直接连接到查询字符串中，从而允许攻击者操控数据库查询。

1. 请求具体的修正指导。

    例如，在查看初始分析后，你可以提交以下提示：

    ```text
    How can I modify this method to prevent SQL injection attacks? What secure coding practices should I implement to safely handle user input in database queries? Where should user input be validated and sanitized? What techniques can I use to construct SQL queries safely?
    ```

1. 花点时间查看 GitHub Copilot 的修正建议。

    应该会看到有关使用参数化查询或对象关系映射 (ORM) 方法的建议，这些方法有助于管理 SQL 注入风险。 还可能会看到有关输入验证和清理技术的建议。 GitHub Copilot 通常会提供演示如何实施建议的代码片段。

1. 打开 Data 文件夹中的 ProductRepository.cs 文件，然后找到 SearchProducts 方法。************

    在代码评审期间，你注意到 ProductRepository 中的 SearchProducts 方法是由 ProductService 中的 SearchProducts 方法调用的。 你可以分析存储库方法，以确定它是否也需要安全改进。

1. 在代码编辑器中，选择整个 SearchProducts 方法，然后要求 GitHub Copilot 分析 SQL 注入漏洞的代码。****

    例如，你可以提交以下提示：

    ```text
    Analyze the SearchProducts method in ProductRepository. Does this method properly handle the search term to prevent SQL injection, or are there vulnerabilities here as well? How does this method relate to the vulnerability in ProductService?
    ```

1. 查看 GitHub Copilot 对存储库方法的分析。

    GitHub Copilot 应注意，虽然存储库方法使用安全字符串操作（ToLower 和 Contains），但主要漏洞位于 ProductService 层中，其中模拟的 SQL 查询是使用用户输入构造的。 存储库实施本身相对安全，但服务层通过不正确的 SQL 查询构造暴露了漏洞。

1. 关闭 ProductRepository.cs 文件。

1. 要求 GitHub Copilot 为 SQL 注入漏洞提出全面的修正策略，其中包括输入验证和清理技术。

    例如，你可以提交以下提示：

    ```text
    #codebase I need to resolve SQL injection vulnerabilities associated with the SearchProducts method in the ProductService.cs file. Notice that user input is directly concatenated into SQL queries without proper parameterization or sanitization. The updated codebase should use parameterized queries or prepared statements, implement proper input validation and sanitization, remove debug logging of SQL queries, and add input length restrictions. My acceptance criteria includes: User input is properly parameterized; No raw SQL construction with user input; Input validation prevents malicious characters; Debug logging removed or sanitized. Review the codebase and identify the code files that must be updated to address the SQL injection vulnerability. Based on your code review and the current Chat conversation, suggest a phased approach to required file updates.
    ```

1. 在修正阶段记录分析结果以供参考。

    记录 GitHub Copilot 针对这两个漏洞类别的建议。 本文档将指导你在下一个任务中实现安全修补程序。

#### 分析信用卡数据存储违规

信用卡数据存储漏洞存在于多个文件中：Order.cs 模型、PaymentService.cs 服务、SecurityValidator.cs 验证程序以及 OrderRepository.cs 数据层。 你将分析这些文件，了解该漏洞的完整范围。

使用以下步骤分析信用卡数据存储违规：

1. 在 Models 文件夹下，打开 Order.cs 文件，然后找到 PaymentInfo 类。************

1. 在代码编辑器中，选择 PaymentInfo 类中的 CardNumber 和 CVV 属性。************

    请注意，注释指示这些属性是安全漏洞。 存储完整的卡号和 CVV 代码违反了支付卡行业数据安全标准 (PCI DSS) 合规性要求。

1. 要求 GitHub Copilot 分析信用卡数据存储冲突。

    例如，你可以提交以下提示：

    ```text
    Why is storing full credit card numbers and CVV codes in the PaymentInfo class a PCI DSS compliance violation? What are the proper ways to handle payment card data securely?
    ```

1. 查看 GitHub Copilot 的分析。

    GitHub Copilot 应说明 PCI DSS 要求禁止在授权后存储敏感的身份验证数据，包括 CVV 码。 它还应说明应对完整卡号进行令牌化或掩码处理。

1. 请求具体的修正指导。

    例如，你可以提交以下提示：

    ```text
    How should I modify the PaymentInfo class to comply with PCI DSS requirements? What properties should I add or change to store payment information securely?
    ```

1. 花点时间查看 GitHub Copilot 的修正建议。

    应该会看到有关完全移除 CVV 属性的建议，将 CardNumber 替换为已掩码的版本或令牌，仅存储最后 4 位数字，并添加卡片类型属性以供显示。

1. 打开 PaymentService.cs 文件，然后定位到 ProcessPayment 方法。********

1. 在代码编辑器中，选择整个 ProcessPayment 方法。****

    请注意，该方法会创建 PaymentInfo 对象并存储完整卡号和 CVV。 此方法还会记录敏感的付款信息。

1. 要求 GitHub Copilot 分析 ProcessPayment 方法是否存在信用卡数据存储问题。

    例如，你可以提交以下提示：

    ```text
    What security vulnerabilities exist in the ProcessPayment method related to credit card data storage and logging? How does this method contribute to the PCI DSS violations?
    ```

1. 查看 GitHub Copilot 的分析。

    GitHub Copilot 应该会识别多个问题：记录完整卡号和 CVV 码、将这些值存储在 PaymentInfo 对象中，以及在整个处理流中暴露敏感数据。

1. 请求 ProcessPayment 方法的具体修正指导。

    例如，你可以提交以下提示：

    ```text
    How should I modify the ProcessPayment method to handle credit card data securely? What changes are needed to prevent storing and logging sensitive card information?
    ```

1. 打开 SecurityValidator.cs 文件，然后找到 ValidateCreditCard 方法。********

1. 在代码编辑器中，选择整个 ValidateCreditCard 方法。****

    请注意，此方法会记录完整的信用卡号，这是一个安全漏洞。

1. 要求 GitHub Copilot 分析 ValidateCreditCard 方法。

    例如，你可以提交以下提示：

    ```text
    What security issues exist in the ValidateCreditCard method? How should credit card validation be performed without logging sensitive data?
    ```

1. 查看 GitHub Copilot 的分析和修正建议。

    GitHub Copilot 应该会生成安全问题列表以及针对安全编码做法的一些建议。 这些建议可能包括移除或掩蔽日志语句中的信用卡号，使用算法验证卡号，以及改进卡号长度和格式验证。

1. 打开 Data 文件夹中的 OrderRepository.cs 文件。********

1. 查看该文件以确定它是否处理 PaymentInfo 对象。

    请注意，OrderRepository 类存储 Order 对象，其中包括 PaymentInfo。 如果 PaymentInfo 类存储完整卡号和 CVV 码，存储库将保留这些敏感数据。

1. 要求 GitHub Copilot 分析 OrderRepository 对信用卡数据存储的影响。

    例如，你可以提交以下提示：

    ```text
    How does the OrderRepository contribute to credit card data storage violations? What happens when Order objects containing PaymentInfo with full card numbers and CVV codes are stored?
    ```

1. 查看 GitHub Copilot 的分析。

    GitHub Copilot 应该会说明存储库保留了 Order 和 PaymentInfo 对象中的所有数据。 如果已修复 PaymentInfo 模型以仅存储安全数据（令牌，最后 4 位数字），而存储库将自动存储安全数据。

1. 关闭 OrderRepository.cs 文件。

1. 要求 GitHub Copilot 为“修复信用卡数据存储违规”问题（包括输入验证和清理技术）提出全面的修正策略。

    例如，你可以提交以下提示：

    ```text
    #codebase I need to resolve credit card data storage violations associated with the PaymentInfo model in the OrderRepository.cs file. Notice that the model currently stores full card numbers and CVV codes. The updated codebase should never store CVV codes (remove CVV storage completely), tokenize card numbers and store tokens instead of actual card numbers, mask the display of credit card numbers to show only last 4 digits, and implement proper encryption if card data must be stored temporarily. My acceptance criteria includes: CVV storage completely removed; Full card numbers replaced with tokens; Only the last 4 digits of a credit card are stored for display; Card type detection implemented. Review the codebase and identify the code files that must be updated to address the credit card data storage violations. Based on your code review and the current Chat conversation, suggest a phased approach to required file updates.
    ```

1. 在修正阶段记录分析结果以供参考。

    记录 GitHub Copilot 针对这两个漏洞类别的建议。 本文档将指导你在下一个任务中实现安全修补程序。

### 使用 GitHub Copilot 的智能体模式解决问题

GitHub Copilot 的智能体模式支持跨多个文件和方法自动实现复杂的安全修补程序。 与提供分析和建议的询问模式不同，智能体模式可以直接修改代码来实现安全改进。 此方法适用于系统安全修正，在这些场景，需要一致地解决多个相关漏洞。

在此任务中，你将使用 GitHub Copilot 的“智能体”模式修正分配给你的 GitHub 问题。

使用以下步骤完成此任务：

1. 将 GitHub Copilot 对话助手切换为智能体模式。

    智能体模式允许 GitHub Copilot 根据说明直接修改代码。 代理模式通过查看代码库中的相关文件来建立适当的上下文。 可以手动将文件和文件夹添加到上下文，确保智能体具有执行复杂任务所需的信息。

1. 花点时间考虑你的修正策略。

    创建一个修正策略，该策略采用使用 GitHub Copilot 的“询问”模式完成的分析。 请考虑解决已分配问题的顺序、解决问题的方法以及如何验证代码漏洞是否已成功修正。

    分配给你的两个 GitHub 问题包括：

    1. 🔐 修复产品搜索中的 SQL 注入漏洞（高优先级）
    1. 🔐 修复信用卡数据存储违规（严重优先级）

    尽管信用卡存储问题的严重性较高，但 SQL 注入问题更为简单，可以先解决。 使用此方法，可以在处理更复杂的信用卡存储违规之前，使用较简单的修补程序来验证工作流。

    这些问题与代码库中的特定文件和方法相关联：

    - SQL 注入问题****：ProductService.cs（SearchProducts 方法）
    - 信用卡存储问题****：Models/Order.cs（PaymentInfo 类）、PaymentService.cs（ProcessPayment 方法）、SecurityValidator.cs（ValidateCreditCard 方法）和 OrderRepository.cs（数据持久性）

    > 注意****：“GitHub 拉取请求”扩展支持在单独的分支中分别处理问题。 分别解决问题可以提供更好的可追溯性、更简便的代码评审流程，以及在出现问题时更安全的回滚选项。 在生产环境中，应使用单独的提交和拉取请求分别解决每个问题。

#### 解决 SQL 注入漏洞

使用以下步骤解决 SQL 注入漏洞：

1. 在代码编辑器中关闭所有打开的文件。

    关闭文件有助于智能体专注于添加到上下文中的文件。 在编辑器中保持打开状态的文件会无意中分散智能体对手头任务的注意力。

1. 将 ProductService.cs 文件添加到聊天上下文。****

    SQL 注入问题主要位于 ProductService.cs 文件的 SearchProducts 方法中。

1. 要求 GitHub Copilot 解决 SQL 注入漏洞。

    使用 GitHub Copilot 的“询问”模式完成的分析显示，该方法在构造 SQL 查询时直接使用了用户输入，而没有进行适当的清理。 使用分析来构造明确的任务说明，供智能体用来修正漏洞。

    例如，可以将以下任务分配给智能体：

    ```text
    #codebase I need you to fix the SQL injection vulnerability in the SearchProducts method. Review the current Chat conversation related to SQL injection vulnerabilities to identify my expected code fixes and acceptance criteria. Remove the simulated SQL query logging that demonstrates the vulnerability, and implement proper input sanitization to safely handle search terms. Ensure that the method still functions correctly for legitimate searches while preventing malicious input. Update the DisplayKnownVulnerabilities method in SecurityValidator.cs to reflect that SQL injection protection is enabled.
    ```

1. 监视智能体的进度。

    智能体将修改代码以删除易受攻击的日志记录并实现更安全的输入处理。

1. 花一分钟时间查看建议的更改，然后在“聊天”视图中选择“保存”****。

    始终在代码编辑器中查看 GitHub Copilot 建议的编辑内容。 确保在解决安全问题的同时保持功能正常。

    这些更改应包括：
    - 移除模拟 SQL 查询日志记录
    - 移除或清理暴露搜索词的调试日志记录
    - 添加输入验证或清理逻辑

    在生产环境中，团队在继续处理下一个问题之前，应完成以下清单：

    - 代码中已不存在该漏洞。
    - 应用程序仍能正常运行。
    - 已实施最佳安全做法，且未引入新的安全问题。
    - 已成功通过自动测试（如有）。
    - 代码更新已清晰记录。
    - 在合并及关闭问题之前，已使用具有描述性的消息提交更改，并已通过同行评审。

#### 解决信用卡数据存储违规

信用卡数据存储违规跨越多个文件，需要协调更改。 需要修改数据模型、更新处理付款数据的服务，以及从日志中移除敏感数据。

使用以下步骤解决信用卡数据存储违规：

1. 关闭编辑器中任何打开的文件，然后将 Order.cs 文件（位于 Models 文件夹中）添加到聊天上下文中。****

    此文件中的 PaymentInfo 类存储完整的卡号和 CVV 码，这违反了 PCI DSS 合规性要求。

1. 要求 GitHub Copilot 修复 PaymentInfo 类。

    例如，可以将以下任务分配给智能体：

    ```text
    Fix PCI DSS compliance violations in the PaymentInfo class in Order.cs. Remove the CVV property entirely as CVV codes should never be stored. Replace the CardNumber property with a CardLastFourDigits property that stores only the last 4 digits. Add a CardType property to identify the card brand (Visa, Mastercard, etc.). Update the constructor and any initializations accordingly.
    ```

1. 监视智能体的进度并查看建议的更改。

    智能体应修改 PaymentInfo 类以移除敏感的数据存储。 查看更改并选择“保留”（如果已正确解决问题）。****

1. 关闭 Order.cs 文件，然后将 PaymentService.cs 文件添加到聊天上下文中。****

    此文件中的 ProcessPayment 方法记录敏感的付款数据，并创建包含完整卡号和 CVV 代码的 PaymentInfo 对象。

1. 要求 GitHub Copilot 修复 ProcessPayment 方法。

    例如，可以将以下任务分配给智能体：

    ```text
    Fix the credit card data handling in the ProcessPayment method in PaymentService.cs. Remove all logging of full card numbers, CVV codes, and other sensitive payment data. Update the PaymentInfo object creation to store only the last 4 digits of the card number and the card type, without storing CVV. Implement card number masking in any remaining log statements (show only last 4 digits). Ensure the payment processing logic still works correctly.
    ```

1. 监视智能体的进度。

    这些更改应包括：
    - 移除或屏蔽日志语句中的敏感数据
    - 更新 PaymentInfo 对象创建，以仅使用最后 4 位数字
    - 移除 CVV 存储
    - 根据需要添加卡片类型检测逻辑

1. 花点时间查看代码编辑器中建议的更改，然后在“聊天”视图中选择“保留”。****

    始终在代码编辑器中查看 GitHub Copilot 建议的编辑内容。 确保在解决安全问题的同时保持功能正常。

1. 关闭 PaymentService.cs 文件，然后将 SecurityValidator.cs 文件添加到聊天上下文中。****

    ValidateCreditCard 方法记录完整的信用卡号。

1. 要求 GitHub Copilot 修复 ValidateCreditCard 方法。

    例如，可以将以下任务分配给智能体：

    ```text
    Fix the credit card validation logging in the ValidateCreditCard method in SecurityValidator.cs. Remove or mask the full credit card number in log statements, showing only the last 4 digits if logging is necessary. Ensure the validation logic continues to work correctly. Update the DisplayKnownVulnerabilities method to reflect that credit card data storage is now secure.
    ```

1. 监视智能体的进度。

    智能体应更新日志记录以掩蔽敏感数据，同时维护验证功能。

1. 花点时间查看代码编辑器中建议的更改，然后在“聊天”视图中选择“保留”。****

    始终在代码编辑器中查看 GitHub Copilot 建议的编辑内容。 确保在解决安全问题的同时保持功能正常。

1. 考虑对 OrderRepository 的影响。

    OrderRepository.cs 文件存储 Order 对象，其中包括 PaymentInfo。 你已更新 PaymentInfo 类以仅存储安全数据（最后 4 位数字、卡类型）。 因此，存储库会自动保存安全数据，而不是完整的卡号和 CVV 代码。 无需对存储库进行直接更改，但应在测试期间验证数据安全性。

1. 生成应用程序以确保所有更改均已成功编译。

    在终端中运行以下命令：

    ```bash
    dotnet build
    ```

    如果出现编译错误，请使用 GitHub Copilot 来帮助识别和解决在安全修复过程中引入的任何问题。 常见问题可能包括：
    - 引用已移除的属性（CVV、完整卡号）
    - 构造函数参数不匹配
    - 工作分配中的类型不匹配

### 测试并验证重构的代码

安全修正后的全面测试可确保漏洞修补程序不会引入功能回归，同时可验证安全改进措施的有效性。 此验证流程需同时测试应用程序的安全层面与业务功能。 恰当的测试能够验证应用程序在提升安全性的同时，仍保持其预期行为。

在此任务中，你将系统地测试更新后的 ContosoShopEasy 应用程序，以验证是否已解决两个安全问题，并且核心功能保持不变。

使用以下步骤完成此任务：

1. 运行完整的应用程序以观察整体行为。

    执行应用程序并查看控制台输出。

    ```bash
    dotnet run
    ```

    将输出结果与原始应用程序运行中的笔记进行比较。 你应该会看到记录的敏感信息减少。

1. 测试 SQL 注入修复。

    验证 SearchProducts 方法是否不在记录直接将用户输入拼接到查询字符串中的模拟 SQL 查询。 应用程序应：

    - 仍正确执行产品搜索
    - 不显示易受攻击的 SQL 查询日志记录
    - 在不暴露 SQL 注入漏洞的情况下安全地处理搜索词
    - 不过度记录原始搜索词

1. 测试信用卡数据存储修复。

    验证 PaymentInfo 类和相关代码是否不再存储或记录完整的信用卡号和 CVV 码。 应用程序应：

    - 不记录完整的信用卡号（检查是否已掩码，例如 ****1234）
    - 根本不记录 CVV 码
    - 不在 PaymentInfo 对象中存储 CVV 码
    - 仅存储卡号的最后 4 位
    - 继续正确处理付款（模拟）

1. 验证整体安全性改进。

    将控制台输出与初始观测结果进行比较。 关键改进应包括：

    - SQL 注入****：没有显示用户输入拼接的模拟 SQL 查询
    - 信用卡数据：**** 日志或存储的数据中没有完整卡号或 CVV 码
    - 应用程序核心功能（产品搜索，付款处理）仍能正常运行

1. 记录任何剩余的问题或改进方面。

    记录任何可能需要更多关注的安全问题，以及任何需要解决的功能问题。

### 提交更改并关闭问题

正确的版本控制做法可确保安全改进措施得到妥善记录与跟踪。 提交消息应清楚地描述已实现的安全修补程序，以便团队成员轻松了解所做的更改及其原因。 用描述性的提交信息关闭 GitHub 问题，能够为安全修正工作创建清晰的审核跟踪记录。

在此任务中，你需将安全改进内容提交到存储库，并关闭对应的 GitHub 问题。

使用以下步骤完成此任务：

1. 打开 Visual Studio Code 的“源代码管理”视图，然后查看对每个更新后的文件所做的更改。

    查找修复过程中引入的任何意外更改。 确保所有更改都符合修正策略，并且未产生任何新漏洞。

1. 要求 GitHub Copilot 创建全面的提交消息。

    例如，可以在“聊天”视图中使用以下提示：

    ```text
    I need to create a commit message that summarizes the security fixes I implemented for two GitHub issues: "Fix SQL Injection Vulnerability in Product Search" and "Fix Credit Card Data Storage Violations." The commit message should clearly describe the changes made to address each issue, including specific code modifications and the overall impact on application security. Draft a detailed commit message that captures all relevant information.
    ```

1. 花点时间查看建议的提交消息。

    确保它准确反映了所做的安全性改进，并提供足够的详细信息供将来参考。

    例如，提交消息可能类似于以下示例：

    ```text
    Fix SQL injection and credit card data storage vulnerabilities

    Security improvements implemented:
    - Fix SQL injection in ProductService SearchProducts method
      - Remove vulnerable SQL query logging with user input
      - Implement proper input handling and sanitization
    
    - Fix PCI DSS violations for credit card data storage
      - Remove CVV property from PaymentInfo class
      - Replace CardNumber with CardLastFourDigits
      - Add CardType property for card brand identification
      - Update PaymentService to not log or store sensitive card data
      - Mask credit card numbers in SecurityValidator logs
    
    Fixes #[SQL_INJECTION_ISSUE_NUMBER] #[CREDIT_CARD_ISSUE_NUMBER]
    ```

1. 将 `[SQL_INJECTION_ISSUE_NUMBER]` 和 `[CREDIT_CARD_ISSUE_NUMBER]` 替换为 GitHub 存储库中的实际问题编号。

    可以在 Visual Studio Code 的“GitHub 拉取请求”视图中或通过在 GitHub 上查看问题来查找这些编号。

    > 注意****：在生产环境中，每个问题通常应通过单独的提交进行解决，并配合独立测试和代码评审。 此处将这两个修补程序组合在单个提交中来简化训练练习工作流。

1. 暂存并提交更改，然后将更改推送到 GitHub 存储库（或同步）。

1. 打开 GitHub 并验证 GitHub 问题是否已自动关闭。

    导航到 GitHub 上的存储库，检查提交消息中引用的两个问题是否标记为已关闭。 当提交消息包括“修复 #[issue_number]”语法时，GitHub 会自动关闭问题。

1. 查看提交历史记录，确保已正确记录安全修补程序。

    验证提交消息是否清楚地描述了安全改进，并为将来的参考提供了良好的审核线索。

## 清理

现在，你已经完成了本练习，请花点时间确保你没有对 GitHub 帐户或 GitHub Copilot 订阅作出任何你不希望保留的更改。 例如，你可能想要删除 ResolveGitHubIssues 存储库。 如果你是使用本地电脑作为实验室环境，可以存档或删除为本次练习创建的存储库的本地克隆。
