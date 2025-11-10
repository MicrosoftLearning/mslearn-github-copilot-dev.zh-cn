<!-- ---
lab:
    title: 'Exercise - Resolve GitHub issues using GitHub Copilot'
    description: 'Learn how to identify and address performance bottlenecks and code inefficiencies using GitHub Copilot tools.'
--- -->

# 使用 GitHub Copilot 解决 GitHub 问题

GitHub 问题是跟踪项目中的 Bug、改进和任务的有效方式。

在本练习中，你将使用 GitHub Copilot 来帮助分析和解决与电子商务应用程序中的安全漏洞相关的 GitHub 问题。

完成此练习大约需要 40 分钟。

> **重要说明**：要完成本练习，需要提供自己的 GitHub 帐户和 GitHub Copilot 订阅。 如果没有 GitHub 帐户，可以<a href="https://github.com/" target="_blank">注册</a>免费的个人帐户，并使用 GitHub Copilot 免费版计划来完成练习。 如果可以从实验室环境中访问 GitHub Copilot Pro、GitHub Copilot Pro+、GitHub Copilot Business 或 GitHub Copilot Enterprise 订阅，则可以使用你现有的 GitHub Copilot 订阅来完成本练习。

## 开始之前

实验室环境必须包括以下内容：Git 2.48 或更高版本、.NET SDK 9.0 或更高版本、具有 C# 开发工具包扩展的 Visual Studio Code，以及访问启用了 GitHub Copilot 的 GitHub 帐户。

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

> 注意****：为了节省时间，在本次培训练习中，你将解决一组问题并在一次提交中推送更新。 批量处理问题并不是推荐的最佳做法。 Microsoft 和 GitHub 建议使用独立的提交分别解决每个问题。 分别解决问题可以提供更好的可追溯性、更简便的代码评审流程，以及在出现问题时更安全的回滚选项。

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

1. 在左侧的“所有工作流”下，选择“创建 ContosoShopEasy 训练问题”工作流，然后选择“运行工作流”。************

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

    应会看到有 10 个问题列出。 请注意，这些问题被定义为 bug，并且已分配有优先级。

1. 若要仅显示严重问题，请选择“标签”下拉列表，然后选择“严重”标签。********

    问题列表会筛选，仅显示严重问题。

    - 🔐 删除硬编码的管理员凭据****  

    - 🔐 修复信用卡数据存储冲突****  

1. 若要仅显示高优先级问题，请选择“标签”下拉列表，取消选择“严重”，然后选择“高优先级”标签。************

    问题列表会筛选，仅显示高优先级问题。

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

1. 选择所有问题，然后使用“分配”下拉列表将问题分配给你自己。****

    将问题分配给自己有助于在修正过程中跟踪进度。

### 在本地克隆存储库并查看代码库

ContosoShopEasy 应用程序采用了企业应用程序的典型分层体系结构，在模型、服务、数据访问和安全组件之间实现了清晰的分离。

在尝试解决安全问题之前，花时间了解现有代码库的基本结构、行为和功能至关重要。

在此任务中，你将创建存储库的本地克隆，检查 Visual Studio Code 中的项目结构，查看应用程序的控制台输出，并查找安全漏洞。

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

    - Models/****：包含用于 Category.cs、Order.cs、Product.cs 和 User.cs 的数据模型。****************

    - Services/****：包含 OrderService.cs、PaymentService.cs、ProductService.cs 和 UserService.cs 的业务逻辑。****************

    - Data/****：包含 OrderRepository.cs、ProductRepository.cs 和 UserRepository.cs 的数据存储库。************

    - Security/****：包含 SecurityValidator.cs 的安全验证逻辑****

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

    请注意，应用程序会记录敏感信息，例如密码、信用卡卡号、管理员凭据和内部系统详细信息。 此输出为需要解决的安全问题提供了明确的证据。

    > 注意****：此应用程序的代码注释、逻辑和日志记录旨在帮助暴露安全漏洞。 尽管实施是人为设计的，但注释和输出日志突出显示了实际应用程序中常见的安全问题。

1. 要开始审查过程并识别代码库中的安全漏洞，请展开 Models 文件夹，然后打开 Order.cs 文件。********

1. 向下滚动，找到 PaymentInfo 类。****

    注意关于 CardNumber 和 CVV 属性的注释。 这段代码与“修复信用卡数据存储冲突”问题相关。

1. 展开 Security 文件夹，然后打开 SecurityValidator.cs 文件。********

    > 注意****：SecurityValidator.cs 类旨在集中 ContosoShopEasy 应用程序与安全相关的逻辑，以便更轻松地查找、管理和解决安全问题。 在实际应用程序中，可以使用 SecurityValidator 等类来强制实施安全最佳做法和输入验证。 但是，ContosoShopEasy 中的具体实施故意采用不安全的设计，并人为暴露漏洞。

1. 花点时间查找以下安全问题：

    - 在文件顶部附近，注意与管理员凭据常量相关的注释（第 7-9 行）。 这段代码与“删除硬编码的管理员凭据”问题相关。

    - 定位到 ValidateInput 方法并查看描述安全漏洞的注释。 这段代码与“修复输入验证安全绕过”问题相关。

    - 定位到 ValidateEmail 方法并查看描述安全漏洞的注释。 这段代码与“改进电子邮件验证安全性”问题相关。

    - 定位到 ValidatePasswordStrength 方法并查看描述安全漏洞的注释。 这段代码与“加强密码安全要求”问题相关。

    - 找到 ValidateCreditCard 方法并查看描述安全漏洞的注释。 这段代码与“修复信用卡数据存储冲突”问题相关。

    - 定位到 GenerateSessionToken 方法并查看描述安全漏洞的注释。 这段代码与“修复可预测会话令牌生成”问题相关。

    - 定位到 RunSecurityAudit 方法并查看描述安全漏洞的注释。 此代码与“减少错误消息中的信息泄漏（控制台输出）”问题相关。

    SecurityValidator.cs 文件中的一些方法还与“从调试日志记录中移除敏感数据”问题相关。

    SecurityValidator 类暴露的问题通常分布在实际应用程序的类中，尤其是旧代码库或维护不佳的代码库。

1. 展开 Services 文件夹，然后打开 UserService.cs 文件。********

1. 花点时间查找以下安全问题：

    - 定位到 RegisterUser、LoginUser 和 ValidateUserInput 方法，并查看描述安全漏洞的注释。 这段代码与“从调试日志记录中删除敏感数据”问题相关。

    - 定位到 GetMd5Hash 方法并查看描述安全漏洞的注释。 这段代码与“将 MD5 密码哈希替换为安全替代项”问题相关。

1. 打开 PaymentService.cs 文件。****

1. 花点时间查看描述安全漏洞的注释。

    此代码中的安全漏洞与“从调试日志记录中移除敏感数据”问题相关。

1. 打开 ProductService.cs 文件。****

1. 花点时间查看 SearchProducts 方法。

    此代码中的安全漏洞与“修复产品搜索中的 SQL 注入漏洞”问题相关。

### 使用 GitHub Copilot 的询问模式分析问题

GitHub 问题通常包含复杂的问题，这些问题需要在实施修复之前进行仔细分析。 了解根本原因、潜在影响和最佳修正策略对于有效解决问题至关重要。

可以使用以下 Visual Studio Code 扩展来协助分析和解决问题：

- GitHub Copilot 对话助手：**** GitHub Copilot 的询问模式提供智能代码分析功能，有助于识别安全漏洞、了解其潜在影响并提出修正策略。

- GitHub 拉取请求：**** GitHub 拉取请求扩展将 GitHub 问题直接集成到 Visual Studio Code 中，让你无需离开开发环境即可管理问题并与之交互。

通过系统地分析每个安全问题，你可以在实现修补程序之前全面了解这些这些问题。 此方法可确保解决方案解决根本原因，而非仅仅是表面症状。

在此任务中，你将使用 GitHub Copilot 的询问模式，对安全漏洞进行系统性分析。

使用以下步骤完成此任务：

1. 在 Visual Studio Code 中打开“扩展”视图。

    若要打开“扩展”视图，请从 Visual Studio Code 窗口左侧的活动栏中选择“扩展”图标。****

1. 在“扩展”视图中，搜索“GitHub 拉取请求”，然后安装扩展。

    此扩展允许查看和管理 Visual Studio Code 中的 GitHub 拉取请求和问题。

    安装完成后，可能需要重新加载 Visual Studio Code 才能使更改生效。 GitHub 图标应该会添加到 Visual Studio Code 的活动栏中。****

1. 若要打开“GitHub 拉取请求”视图，请从活动栏中选择“GitHub”图标。****

    如果系统提示，请登录到 GitHub 帐户，将 Visual Studio Code 连接到 GitHub 存储库。

1. 请注意“GitHub”视图中的“拉取请求”和“问题”部分。********

    通过“问题”部分，可直接在 Visual Studio Code 中查看和管理 GitHub 存储库中的问题。****

1. 花点时间查看“问题”部分下列出的问题。****

    应会看到之前在 GitHub Web 界面中查看过的相同问题。

1. 打开 GitHub Copilot 对话助手视图，并确保选中“询问”模式。****

    **** 如果“聊天”视图尚未打开，选择 Visual Studio Code 窗口顶部的“聊天”图标。 确定聊天模式设置为“询问”，并且使用的是 GPT-4.1 模型。********

    > 注意****：GPT-4.1 模型可提供出色的代码分析功能，并包含在 GitHub Copilot Free 计划中。 选择不同的模型可能会产生不同的结果。

1. 确保从干净的聊天会话开始。

    聊天会话有助于整理你与 GitHub Copilot 的交互。 每个会话都维护自己的上下文，使你能够专注于特定任务或问题。 会话中的对话历史记录提供连续性，使 GitHub Copilot 能够基于以前的交互构建，以获取更准确且相关的响应。 此聊天对话将侧重于分析和解决 ContosoShopEasy 应用程序中的安全漏洞。 使用 GitHub Copilot 的“询问”模式完成对 GitHub 问题的分析后，可以在同一对话中使用 GitHub Copilot 的“智能体”模式来协助实施代码更改。 GitHub Copilot 可以使用“询问”模式下的详细分析来指导“智能体”模式下的代码生成，从而确保修复与已识别的漏洞和建议的修正策略保持一致。

    如果需要，可以通过选择“新建聊天”按钮（“聊天”面板顶部的+ 图标）来启动新的聊天会话。********

1. 打开 ProductService.cs 文件，然后定位到 SearchProducts 方法。********

1. 在代码编辑器中，选择整个 SearchProducts 方法。****

    在编辑器中选择代码有助于聚焦于聊天上下文。 GitHub Copilot 会使用所选的代码提供相关分析和建议。

    SearchProducts 方法与“修复产品搜索中的 SQL 注入漏洞”问题相关联。****

1. 要求 GitHub Copilot 分析代码是否存在 SQL 注入漏洞。

    例如，你可以提交以下提示：

    ```text
    Analyze the SearchProducts method for security vulnerabilities. What makes this code susceptible to SQL injection attacks, and what are the potential consequences if an attacker exploits this vulnerability?
    ```

1. 查看 GitHub Copilot 的分析结果，然后请求获取具体的修正指导。

    例如，在查看初始分析后，你可以提交以下提示：

    ```text
    How can I modify this method to prevent SQL injection attacks? What secure coding practices should I implement to safely handle user input in database queries?
    ```

1. 花点时间查看 GitHub Copilot 的修正建议。

    应该会看到有关使用参数化查询或 ORM 方法的建议，这些方法有助于管理 SQL 注入风险。 还可能会看到有关输入验证和清理技术的建议。 GitHub Copilot 通常会提供演示如何实施建议的代码片段。

1. 打开 UserService.cs 文件。****

1. 要求 GitHub Copilot 查看 UserService.cs 文件，识别安全漏洞，然后列出相关的 GitHub 问题。

    例如，你可以提交以下提示：

    ```text
    Review the UserService.cs file and identify the security vulnerabilities that are present in the code. Create a list the corresponding GitHub issues. Indicate the methods associated with each issue.
    ```

1. 请花点时间查看 GitHub Copilot 的回复。

1. 在代码编辑器中，找到 GetMd5Hash 方法。****

1. 在代码编辑器中，选择整个 GetMd5Hash 方法。****

    GetMd5Hash 方法与“将 MD5 密码哈希替换为安全替代项”问题相关联。****

1. 要求 GitHub Copilot 分析弱密码哈希漏洞。

    例如，你可以提交以下提示：

    ```text
    Why is MD5 hashing unsuitable for password storage? What are the security risks of using MD5 for passwords, and what stronger alternatives should I use instead?
    ```

1. 查看 GitHub Copilot 的分析结果，然后请求获取具体的修正指导。

    例如，在查看初始分析后，你可以提交以下提示：

    ```text
    Show me how to implement secure password hashing using bcrypt or PBKDF2. What additional security measures should I implement for password handling?
    ```

1. 花点时间查看 GitHub Copilot 的修正建议。

    GitHub Copilot 应该会提供“PBKDF2”和“bcrypt”的比较。 还应该提供代码片段，演示如何使用这些算法实现安全密码哈希，以及用于密码处理的其他安全措施列表。

1. 在 UserService.cs 文件中，定位到 RegisterUser 和 LoginUser 方法。************

1. 在代码编辑器中，选择这两种方法。

    RegisterUser 和 LoginUser 方法与“从调试日志记录中移除敏感数据”问题相关联。********

1. 要求 GitHub Copilot 分析敏感数据日志记录漏洞。

    例如，你可以提交以下提示：

    ```text
    What sensitive information is being logged in the user registration and login methods? Why is logging passwords and user data a security risk?
    ```

1. 查看 GitHub Copilot 的分析结果，然后请求获取具体的修正指导。

    例如，在查看初始分析后，你可以提交以下提示：

    ```text
    How can I modify these methods to prevent sensitive data logging? What secure logging practices should I implement to protect user information?
    ```

1. 花点时间查看 GitHub Copilot 的修正建议。

1. 打开 PaymentService.cs 文件，然后定位到 ProcessPayment 方法。********

1. 在代码编辑器中，选择整个 ProcessPayment 方法。****

    ProcessPayment 方法与“从调试日志记录中移除敏感数据”问题相关联。****

1. 要求 GitHub Copilot 分析敏感付款数据的日志记录。

    例如，你可以提交以下提示：

    ```text
    What sensitive payment information is being logged in this method? Why is logging credit card numbers and CVV codes a security risk?
    ```

1. 打开 SecurityValidator.cs 文件，然后在文件顶部附近找到管理员凭据常量。****

1. 在代码编辑器中，选择硬编码的管理员凭据常量。

1. 要求 GitHub Copilot 分析硬编码的凭据漏洞。

    例如，你可以提交以下提示：

    ```text
    What security risks are created by hardcoding admin credentials in source code? How should application credentials be managed securely in production environments?
    ```

1. 查看 GitHub Copilot 的分析结果，然后请求获取具体的修正指导。

    例如，在查看初始分析后，你可以提交以下提示：

    ```text
    What are best practices for managing application credentials securely? How can I implement secure credential management in this application?
    ```

1. 花点时间查看 GitHub Copilot 的修正建议。

1. 在 SecurityValidator.cs 文件中，定位到 ValidateInput 方法。********

1. 在代码编辑器中，选择整个 ValidateInput 方法。****

1. 要求 GitHub Copilot 分析输入验证绕过漏洞。

    例如，你可以提交以下提示：

    ```text
    What makes this input validation method ineffective? Why does it detect dangerous input but still return true, and how should proper input validation work?
    ```

1. 查看 GitHub Copilot 的分析结果，然后请求获取具体的修正指导。

    例如，在查看初始分析后，你可以提交以下提示：

    ```text
    How can I modify this method to implement effective input validation? What secure coding practices should I follow to prevent input validation bypass vulnerabilities?
    ```

1. 花点时间查看 GitHub Copilot 的修正建议。

1. 在 SecurityValidator.cs 文件中，定位到 GenerateSessionToken 方法。********

1. 在代码编辑器中，选择整个 GenerateSessionToken 方法。****

1. 要求 GitHub Copilot 分析可预测的会话令牌生成漏洞。

    例如，你可以提交以下提示：

    ```text
    Why are predictable session tokens based on username and timestamp a security risk? How should secure, unpredictable session tokens be generated?
    ```

1. 查看 GitHub Copilot 的分析结果，然后请求获取具体的修正指导。

    例如，在查看初始分析后，你可以提交以下提示：

    ```text
    How can I modify this method to generate secure, unpredictable session tokens? What cryptographic techniques should I use to enhance session token security?
    ```

1. 花点时间查看 GitHub Copilot 的修正建议。

1. 在 SecurityValidator.cs 文件中，定位到 ValidateEmail 方法。********

1. 在代码编辑器中，选择整个 ValidateEmail 方法。****

1. 要求 GitHub Copilot 分析弱电子邮件验证漏洞。

    例如，你可以提交以下提示：

    ```text
    What makes this email validation insufficient? What are the security risks of weak email validation, and how should proper email validation be implemented?
    ```

1. 查看 GitHub Copilot 的分析结果，然后请求获取具体的修正指导。

    例如，在查看初始分析后，你可以提交以下提示：

    ```text
    How can I modify this method to implement robust email validation? What techniques should I use to ensure email addresses are properly validated?
    ```

1. 在 SecurityValidator.cs 文件中，定位到 ValidatePasswordStrength 方法。********

1. 在代码编辑器中，选择整个 ValidatePasswordStrength 方法。****

1. 要求 GitHub Copilot 分析密码要求不足漏洞。

    例如，你可以提交以下提示：

    ```text
    Why are these password requirements insufficient for security? What are proper password complexity requirements, and how should password strength be validated?
    ```

1. 查看 GitHub Copilot 的分析结果，然后请求获取具体的修正指导。

    例如，在查看初始分析后，你可以提交以下提示：

    ```text
    How can I modify this method to enforce strong password requirements? What best practices should I follow for password strength validation?
    ```

1. 花点时间查看 GitHub Copilot 的修正建议。

1. 在 Models 文件夹下，打开 Order.cs 文件，然后找到 PaymentInfo 类。************

1. 在代码编辑器中，选择 PaymentInfo 类中的 CardNumber 和 CVV 属性。************

1. 要求 GitHub Copilot 分析信用卡数据存储冲突。

    例如，你可以提交以下提示：

    ```text
    Why is storing full credit card numbers and CVV codes a PCI DSS compliance violation? What are the proper ways to handle payment card data securely?
    ```

1. 返回到 SecurityValidator.cs 文件，然后定位到 RunSecurityAudit 方法。********

1. 在代码编辑器中，选择整个 RunSecurityAudit 方法。****

1. 要求 GitHub Copilot 分析信息泄漏漏洞。

    例如，你可以提交以下提示：

    ```text
    How does the security audit method create information disclosure vulnerabilities? What information should never be exposed in logs or error messages?
    ```

1. 在修正阶段记录分析结果以供参考。

    记下 GitHub Copilot 针对每个漏洞类别提供的建议。 本文档将指导你在下一个任务中实现安全修补程序。

### 使用 GitHub Copilot 的智能体模式解决问题

GitHub Copilot 的智能体模式支持跨多个文件和方法自动实现复杂的安全修补程序。 与提供分析和建议的询问模式不同，智能体模式可以直接修改代码来实现安全改进。 此方法特别适用于系统安全修正，在这些场景，需要一致地解决多个相关漏洞。

在此任务中，你将使用 GitHub Copilot 的智能体模式为 ContosoShopEasy 应用程序中的所有已识别漏洞实现全面的安全修补程序。

使用以下步骤完成此任务：

1. 将 GitHub Copilot 对话助手切换为智能体模式。

    智能体模式允许 GitHub Copilot 根据说明直接修改代码。 代理模式通过查看代码库中的相关文件来建立适当的上下文。 可以手动将文件和文件夹添加到上下文，确保智能体具有执行复杂任务所需的信息。

1. 花点时间考虑你的修正策略。

    创建基于使用 GitHub Copilot 的“询问”模式完成的分析的计划。 请考虑解决问题的顺序、修复之间的依赖关系以及如何验证是否已成功修正每个漏洞。

    GitHub 问题（从严重问题开始，按优先级降序排列）如下所示：

    1. 🔐 修复产品搜索中的 SQL 注入漏洞
    1. 🔐 将 MD5 密码哈希替换为安全替代项
    1. 🔐 从调试日志记录中删除敏感数据
    1. 🔐 删除硬编码的管理员凭据
    1. 🔐 修复信用卡数据存储冲突
    1. 🔐 修复输入验证安全性绕过
    1. 🔐 修复可预测会话令牌生成
    1. 🔐 改进电子邮件验证安全性
    1. 🔐 加强密码安全要求
    1. 🔐 减少错误消息中的信息泄漏

    这些问题与代码库中的特定文件和方法相关联。 按文件关联进行组织时，问题如下所示：

    - ProductService.cs****：问题 #1
    - UserService.cs****:问题 #2 和 #3
    - PaymentService.cs****：问题 #3
    - SecurityValidator.cs****：问题 #4、#6、#7、#8、#9 和 #10
    - Models/Order.cs****：问题 #5

    修正策略应系统地解决每个问题，确保正确且一致地实施修补程序。

    若要在训练练习中节省时间，请解决所有问题，然后将所有代码更新一起提交。 基于文件的修正策略在这种情况下适用。 但是，大批量处理问题并不是推荐的最佳做法。

    在生产环境中，最好通过单独的提交单独解决每个问题。 此方法可以提供更好的可追溯性、更简便的代码评审流程，以及在出现问题时更安全的回滚选项。

1. 在代码编辑器中关闭所有打开的文件。

    关闭文件有助于智能体专注于添加到上下文中的文件。 在编辑器中保持打开状态的文件会无意中分散智能体对手头任务的注意力。

1. 将 ProductService.cs 文件添加到聊天上下文。****

    SQL 注入问题与 ProductService.cs 文件中的 SearchProducts 方法相关联。

1. 要求 GitHub Copilot 解决 SQL 注入漏洞。

    使用 GitHub Copilot 的“询问”模式完成的分析显示，该方法在构造 SQL 查询时直接使用了用户输入，而没有进行适当的清理。 使用分析来构造明确的任务说明，供智能体用来修正漏洞。

    例如，可以将以下任务分配给智能体：

    ```text
    Fix the SQL injection vulnerability in the SearchProducts method. Remove the simulated SQL query logging that demonstrates the vulnerability, and implement proper input sanitization to safely handle search terms. Ensure the method still functions correctly for legitimate searches while preventing malicious input.
    ```

1. 监视智能体的进度。

    智能体将修改代码以删除易受攻击的日志记录并实现更安全的输入处理。

1. 花一分钟时间查看建议的更改，然后在“聊天”视图中选择“保存”****。

    始终在代码编辑器中查看 GitHub Copilot 建议的编辑内容。 确保在解决安全问题的同时保持功能正常。

    在生产环境中，团队在继续处理下一个问题之前，应完成以下清单：

    - 代码中已不存在该漏洞。
    - 应用程序仍能正常运行。
    - 已实施最佳安全做法，且未引入新的安全问题。
    - 已成功通过自动测试（如有）。
    - 代码更新已清晰记录。
    - 在合并及关闭问题之前，已使用具有描述性的消息提交更改，并已通过同行评审。

1. 实现安全密码哈希。

    聚焦于`UserService.cs` 文件并使用以下提示：

    ```text
    Replace the MD5 password hashing with bcrypt or PBKDF2. Update the GetMd5Hash method to use a cryptographically secure hashing algorithm with proper salt generation. Ensure compatibility with existing user authentication while improving security.
    ```

1. 查看并测试密码哈希更改。

    智能体将实现更强的密码哈希。 通过运行应用程序来测试更改，确保用户注册和登录仍然正常运行。

1. 解决敏感数据日志记录问题（问题 #3）。

    关注`PaymentService.cs` 和`UserService.cs` 文件，并对智能体发出指令：

    ```text
    Fix sensitive data logging throughout the application. Remove logging of passwords, full credit card numbers, CVV codes, and other sensitive information. Implement secure logging that masks sensitive data while maintaining useful operational information.
    ```

1. 移除硬编码管理员凭据（问题 #4）。

    聚焦于`SecurityValidator.cs` 文件并使用以下提示：

    ```text
    Remove hardcoded admin credentials from the SecurityValidator class. Replace the hardcoded ADMIN_USERNAME and ADMIN_PASSWORD constants with a secure configuration approach using environment variables while maintaining the functionality for educational demonstration purposes.
    ```

1. 修复信用卡数据存储冲突（问题 #5）。

    关注`Models/Order.cs` 文件，并对智能体发出指令：

    ```text
    Fix PCI DSS compliance violations in the Order model. Remove or modify the CardNumber and CVV properties to avoid storing full credit card numbers and CVV codes. Implement secure payment data handling that stores only last 4 digits for display purposes.
    ```

1. 修复输入验证绕过问题（问题 #6）。

    指示智能体修复输入验证漏洞：

    ```text
    Fix the ValidateInput method in SecurityValidator that currently always returns true despite detecting threats. Implement proper input validation that actually rejects dangerous content when SQL injection, XSS, or other malicious patterns are detected.
    ```

1. 实现安全会话令牌生成（问题 #7）。

    聚焦于会话令牌漏洞：

    ```text
    Replace the predictable session token generation in GenerateSessionToken method with a cryptographically secure random token generator. Remove the username and timestamp-based pattern and implement unpredictable tokens with sufficient entropy.
    ```

1. 加强电子邮件验证（问题 #8）。

    解决弱电子邮件验证问题：

    ```text
    Fix the ValidateEmail method that only checks for '@' and '.' characters. Implement proper email format validation using regex or built-in validation methods. Remove email logging and add appropriate length restrictions.
    ```

1. 改进密码要求（问题 #9）。

    关注密码强度验证：

    ```text
    Strengthen the ValidatePasswordStrength method that currently only requires 4 characters. Implement proper password complexity requirements including minimum 8 characters, uppercase, lowercase, numbers, and special characters. Remove password logging.
    ```

1. 减少信息泄漏（问题 #10）。

    解决调试日志记录和安全审核问题：

    ```text
    Fix information disclosure vulnerabilities by removing or restricting the RunSecurityAudit method and reducing verbose error messages throughout the application. Remove sensitive system information from logs while maintaining useful debugging capabilities.
    ```

1. 测试应用程序。

    在智能体对每个漏洞类别实现修补程序后，运行应用程序以确保功能得以保留：

    ```bash
    dotnet build
    dotnet run
    ```

1. 验证安全改进措施不会破坏核心功能。

    确保产品搜索、用户注册、付款处理和其他核心功能在实现安全修补程序后继续正常工作。

### 测试并验证重构的代码

安全修正后的全面测试可确保漏洞修补程序不会引入功能回归，同时可验证安全改进措施的有效性。 此验证流程需同时测试应用程序的安全层面与业务功能。 恰当的测试能够验证应用程序在提升安全性的同时，仍保持其预期行为。

在此任务中，你将对更新后的 ContosoShopEasy 应用程序进行系统性测试，确保安全问题已得到解决，且核心功能保持完好。

使用以下步骤完成此任务：

1. 生成应用程序并解决任何编译错误。

    运行以下命令以确保代码编译成功：

    ```bash
    dotnet build
    ```

    如果出现编译错误，请使用 GitHub Copilot 来帮助识别和解决在安全修复过程中引入的任何问题。

1. 运行完整的应用程序以观察整体行为。

    执行应用程序并查看控制台输出。

    ```bash
    dotnet run
    ```

    将输出结果与原始应用程序运行中的笔记进行比较。 应会看到记录的敏感信息显著减少。

1. 测试 SQL 注入修复（问题 #1）。

    验证`SearchProducts` 方法是否不再记录易受攻击的 SQL 查询，且搜索功能对于合法搜索词是否仍能正常工作。

1. 验证密码安全改进（问题 #2）。

    检查用户注册和登录进程是否不再记录纯文本密码，以及是否实现了更强大的密码哈希。 应用程序仍应正确对用户进行身份验证。

1. 确认敏感数据日志记录修复（问题 #3）。

    确保在成功处理交易的同时，付款流程和用户操作不再记录密码、完整的信用卡号或 CVV 码。

1. 验证硬编码凭据删除（问题 #4）。

    验证硬编码管理员凭据是否不再显示在日志或安全审核中，而管理员功能仍可通过安全配置访问。

1. 测试信用卡存储合规性（问题 #5）。

    确认订单模型不再存储完整的信用卡号或 CVV 码，并且仅保留遮蔽付款信息用于显示目的。

1. 验证输入验证修复（问题 #6）。

    确认改进后的 ValidateInput 方法现在能够正确拒绝危险输入，而不是仅记录警告并返回 true。

1. 检查会话令牌安全性（问题 #7）。

    如果应用程序在运行过程中生成会话令牌，需确认这些令牌看起来是随机且不可预测的，而不是沿用之前的用户名-时间戳模式。

1. 测试电子邮件验证改进（问题 #8）。

    验证电子邮件验证现在是否正确地拒绝无效电子邮件格式，而不是接受包含“@" and ".”字符的任何字符串。

1. 验证密码要求增强功能（问题 #9）。

    测试密码验证现在是否强制实施适当的复杂性要求，而不是接受任意 4 个字符的字符串。

1. 审查信息泄漏修复（问题 #10）。

    检查是否删除或限制了安全审核方法，以及详细错误消息是否不再公开敏感的系统信息。

1. 将整体安全状况与原始版本进行比较。

    运行应用程序并将控制台输出与初始观测结果进行比较。 应用程序应在保持所有核心功能的同时，表现出显著提升的安全性。

1. 记录任何剩余的问题或改进方面。

    记录任何可能需要额外关注的安全问题，以及任何需要解决的功能问题。

### 提交更改并关闭问题

正确的版本控制做法可确保安全改进措施得到妥善记录与跟踪。 提交消息应清楚地描述已实现的安全修补程序，以便团队成员轻松了解所做的更改及其原因。 用描述性的提交信息关闭 GitHub 问题，能够为安全修正工作创建清晰的审核跟踪记录。

在此任务中，你需将安全改进内容提交到存储库，并关闭对应的 GitHub 问题。

使用以下步骤完成此任务：

1. 查看对代码库所做的所有更改。

    使用 Git 查看修改的文件：

    ```bash
    git status
    git diff
    ```

1. 暂存所有与安全相关的更改以便提交。

    将修改后的文件添加到暂存区域：

    ```bash
    git add .
    ```

1. 提交所有安全修补程序，其中包含引用所有 GitHub 问题的综合消息。

    创建一个提交，以解决培训练习中发现的所有安全漏洞：

    ```bash
    git commit -m "Fix all ContosoShopEasy security vulnerabilities

    Security improvements implemented:
    - Fix SQL injection in ProductService SearchProducts method
    - Replace MD5 with secure password hashing (bcrypt/PBKDF2)
    - Remove sensitive data from debug logging (passwords, card numbers, CVV)
    - Remove hardcoded admin credentials, use environment variables
    - Fix PCI DSS violations in Order model (remove full card storage)
    - Fix input validation bypass to properly reject dangerous input
    - Implement secure session token generation with crypto randomness
    - Strengthen email validation with proper format checking
    - Improve password requirements (8+ chars, complexity rules)
    - Reduce information disclosure from security audit and debug logs

    Fixes #1 #2 #3 #4 #5 #6 #7 #8 #9 #10"
    ```

    > 注意****：在生产环境中，每个问题通常应通过单独的提交进行解决，并配合独立测试和代码评审。 这里使用单一提交方法只是为了在培训练习中节省时间。

1. 将更改推送到 GitHub 存储库。

    ```bash
    git push origin main
    ```

1. 验证 GitHub 问题是否已自动关闭。

    导航到 GitHub 上的存储库，并检查那些被提交信息引用的问题是否已标记为“已关闭”。

1. 查看提交历史记录，确保正确记录所有安全修补程序。

    验证提交消息是否清楚地描述了安全改进措施，以及是否为将来的参考提供了完善的审核线索。

## 清理

现在，你已经完成了本练习，请花点时间确保你没有对 GitHub 帐户或 GitHub Copilot 订阅作出任何你不希望保留的更改。 例如，你可能想要删除 ResolveGitHubIssues 存储库。 如果你是使用本地电脑作为实验室环境，可以存档或删除为本次练习创建的存储库的本地克隆。
