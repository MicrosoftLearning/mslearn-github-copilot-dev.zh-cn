<!-- ---
lab:
    title: 'Exercise - Resolve GitHub issues using GitHub Copilot'
    description: 'Learn how to identify and address performance bottlenecks and code inefficiencies using GitHub Copilot tools.'
--- -->

# 使用 GitHub Copilot 解决 GitHub 问题

GitHub 问题是跟踪项目中的 Bug、改进和任务的有效方式。 在本练习中，你将了解如何使用 GitHub Copilot 来帮助分析和解决示例代码库中的问题。

在本练习中，你将使用名为 ContosoShopEasy 的示例电子商务应用程序。 该应用程序包含已记录为 GitHub 问题的安全漏洞。 你的目标是使用 GitHub Copilot 来帮助分析和解决这些问题。

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

    git config --global user.name "John Doe"

    ```

    ```bash

    git config --global user.email johndoe@example.com

    ```

## 练习场景

你是一家咨询公司的软件开发人员。 你的客户需要帮助来解决针对 GitHub 项目记录的问题。 你的目标是在 Visual Studio Code 中使用 GitHub Copilot 更新代码项目时，以这些问题作为指导。 你需要确保所有问题都得到处理并关闭。 公司给你分配了以下应用：

- ContosoShopEasy：一个真实的电子商务应用程序，存在多个已记录为 GitHub 问题的安全漏洞。 该应用程序展示了在实际应用程序中发现的常见安全问题，同时保持了正常运作的电子商务工作流。

本练习包括以下任务：

1. 导入 ContosoShopEasy 存储库。
1. 查看 GitHub 中的问题。
1. 克隆存储库并查看代码库。
1. 使用 GitHub Copilot 的询问模式分析问题。
1. 使用 GitHub Copilot 的智能体模式解决问题。
1. 测试并验证重构的代码。
1. 提交更改并关闭问题。

### 导入 ContosoShopEasy 存储库

通过 GitHub 存储库导入，你可以在自己的 GitHub 帐户中创建现有存储库的副本。 此过程会保留原始存储库的历史记录，同时让你对导入的副本拥有完全控制权。 在此任务中，你需要导入 ContosoShopEasy 项目，并创建与代码库中存在的安全漏洞相对应的 GitHub 问题。

使用以下步骤完成此任务：

1. 打开浏览器窗口并导航到 GitHub.com。

1. 登录到 GitHub 帐户。

1. 创建名为 ContosoShopEasy 的新存储库****。

    可通过选择 GitHub 右上角的+ 图标并选择“新建存储库”来创建新存储库********。

1. 将 ContosoShopEasy 项目文件从本地实验室环境复制到新存储库。

    可直接通过 GitHub Web 界面上传文件，也可克隆空存储库，并从本地计算机推送 ContosoShopEasy 文件。

1. 创建以下 GitHub 问题来跟踪安全漏洞：

    - SQL 注入漏洞****:产品搜索功能接受未经批准的输入，并且容易受到注入攻击。 `ProductService.cs` 中的`SearchProducts` 方法可直接将用户输入合并到模拟的 SQL 查询中，而无需进行适当的参数化。

    - 弱密码哈希****：应用程序使用 MD5 哈希来存储密码，而该算法为弱加密。 `UserService.cs` 中的`GetMd5Hash` 方法可实现这种易受攻击的哈希处理方式。

    - 敏感数据泄漏****：以纯文本形式记录完整的信用卡卡号、信用卡验证码和密码。 `PaymentService.cs` 中的`ProcessPayment` 方法和`UserService.cs` 中的注册/登录方法会泄漏敏感信息。

    - 硬编码的管理员凭据****：管理员的用户名和密码被硬编码在`SecurityValidator.cs` 类中，这使得任何能访问源代码的人都可以获取到这些信息。

    - 输入验证问题****：应用程序接受可能具有危险性的字符，却未进行适当的清理。 `SecurityValidator.cs` 中的`ValidateInput` 方法会记录警告，但仍接受恶意输入。

    - 信息泄漏****：调试日志记录会公开敏感的系统信息和配置详细信息。 应用程序中的多个类出于调试目的会记录敏感数据。

    - 可预测会话令牌****：会话令牌遵循基于用户名和时间戳的可预测模式。 `SecurityValidator.cs` 中的`GenerateSessionToken` 方法生成的令牌容易被猜测到。

    - 文件上传安全问题****：应用程序接受危险的文件类型，却未进行适当的验证。 `SecurityValidator.cs` 中的`ValidateFileUpload` 方法对恶意文件上传的防护措施不足。

### 查看 GitHub 中的问题

GitHub 问题是一个集中式跟踪系统，可用于跟踪 bug、安全漏洞和改进请求。 每个问题都会提供关于问题的背景信息、严重程度，以及它对应用程序可能产生的影响。 在深入研究代码之前了解这些问题，有助于确定优先事项，并确保进行全面的修正。

在此任务中，你需要查看 ContosoShopEasy 项目的待解决问题，并了解需要处理的安全漏洞。

使用以下步骤完成此任务：

1. 在 GitHub 中导航到 ContosoShopEasy 存储库。

1. 选择“问题”选项卡以查看所有待解决的问题****。

1. 查看每个问题说明并记下以下安全漏洞类别：

    SQL 注入漏洞****：产品搜索功能将用户输入直接整合到数据库查询中，这为恶意 SQL 注入攻击创造了可乘之机。

    弱加密做法****：使用 MD5 哈希存储密码，该哈希已被证实存在加密缺陷，不适用于密码安全保护。

    敏感数据泄漏****：记录完整的信用卡卡号、信用卡验证码、密码以及其他敏感信息时绝不应以明文形式进行存储或记录。

    硬编码凭据****：管理员的用户名和密码被直接嵌入源代码中，这使得任何拥有存储库访问权限的人都能获取到这些信息。

    输入验证失败****：对用户输入的验证和清理不足，这使得潜在恶意内容得以被应用程序处理。

    信息泄漏****：调试日志记录，可公开敏感系统配置、内部进程和用户数据。

    弱会话管理****：可预测会话令牌生成，使攻击者能够劫持用户会话。

    文件上传漏洞****：对上传的文件的验证不充分，可能会允许执行恶意代码。

1. 请注意，这些问题是在实际应用程序中发现的常见安全漏洞，并与 OWASP 安全准则保持一致。

### 克隆存储库并查看代码库

在实现安全修补程序之前，了解现有代码库的结构和功能至关重要。 ContosoShopEasy 应用程序采用了企业应用程序的典型分层体系结构，在模型、服务、数据访问和安全组件之间实现了清晰的分离。 审查代码结构并运行应用程序有助于在实施安全改进后建立测试基线。

在此任务中，克隆 ContosoShopEasy 存储库，检查项目结构，并观察应用程序的当前行为。

使用以下步骤完成此任务：

1. 将 ContosoShopEasy 存储库克隆到本地开发环境。

    打开终端窗口并运行以下命令，将`your-username` 替换为你的 GitHub 用户名：

    ```bash
    git clone https://github.com/your-username/ContosoShopEasy.git
    ```

1. 在 Visual Studio Code 中打开克隆的存储库。

    在 Visual Studio Code 中，导航到存储库文件夹并将其打开。 确保已安装并启用 GitHub Copilot 和 GitHub Copilot 对话助手扩展。

1. 在“解决方案资源管理器”中，检查项目结构。

    ContosoShopEasy 应用程序采用了具有以下组件的分层体系结构：

    - Models/****：包含`Product.cs`、`User.cs`、`Order.cs` 和`Category.cs` 的数据模型
    - Services/****：包含`ProductService.cs`、`UserService.cs`、`PaymentService.cs` 和`OrderService.cs` 的业务逻辑
    - Data/****：包含`ProductRepository.cs`、`UserRepository.cs` 和`OrderRepository.cs` 中的数据存储库
    - Security/****：包含`SecurityValidator.cs` 中的安全验证逻辑
    - Program.cs****：包含依赖项注入设置的主应用程序入口点
    - README.md****：说明应用程序的用途和漏洞的文档

1. 生成并运行应用程序以观察其当前行为。

    在终端运行以下命令：

    ```bash
    cd ContosoShopEasy
    dotnet build
    dotnet run
    ```

    该应用程序将全面演示电子商务功能，其中包括一项安全审核，以揭示那些刻意设置的漏洞。

1. 查看控制台输出以确定与安全相关的日志记录。

    请注意，应用程序会记录敏感信息，例如密码、信用卡卡号、管理员凭据和内部系统详细信息。 此输出为需要解决的安全问题提供了明确的证据。

1. 识别与每个安全漏洞相关的特定文件和方法：

    - SQL 注入****：`ProductService.cs` -`SearchProducts` 方法
    - 弱密码哈希****：`UserService.cs` -`GetMd5Hash` 方法
    - 敏感数据泄漏****：`PaymentService.cs` -`ProcessPayment` 方法和`UserService.cs` - 注册/登录方法
    - 硬编码凭据****：`SecurityValidator.cs` - 管理员凭据常量
    - 输入验证****：`SecurityValidator.cs` -`ValidateInput` 方法
    - 信息泄漏****：具有调试日志记录的多个类
    - 可预测令牌****：`SecurityValidator.cs` -`GenerateSessionToken` 方法
    - 文件上传问题****：`SecurityValidator.cs` -`ValidateFileUpload` 方法

### 使用 GitHub Copilot 的询问模式分析问题

GitHub Copilot 的询问模式提供智能代码分析功能，有助于识别安全漏洞、了解其潜在影响并提出修正策略。 通过系统地分析每个安全问题，你可以在实现修补程序之前全面了解这些这些问题。 此方法可确保解决方案解决根本原因，而非仅仅是表面症状。

在这项任务中，你将使用 GitHub Copilot 的询问模式，对 ContosoShopEasy 应用程序中的每个安全漏洞进行系统性分析。

使用以下步骤完成此任务：

1. 打开 GitHub Copilot 对话助手视图，并确保已选中“询问模式”。

    **** 如果“聊天”视图尚未打开，选择 Visual Studio Code 窗口顶部的“聊天”图标。 确认聊天模式已设置为“询问”，且你在使用 GPT-4.1 模型进行复杂的安全分析********。

1. 从 SQL 注入漏洞分析开始。

    打开`ProductService.cs` 文件并找到`SearchProducts` 方法。 选择整个方法，然后使用拖放或右键单击并选择“添加到聊天”，将其添加到聊天上下文****。

1. 让 GitHub Copilot 分析 SQL 注入漏洞。

    提交以下提示以分析此安全问题：

    ```text
    Analyze the SearchProducts method for security vulnerabilities. What makes this code susceptible to SQL injection attacks, and what are the potential consequences if an attacker exploits this vulnerability?
    ```

1. 查看 GitHub Copilot 的分析结果并请求具体的修正指导。

    查看初始分析结果后，请求具体的修补程序：

    ```text
    How can I modify this method to prevent SQL injection attacks? What secure coding practices should I implement to safely handle user input in database queries?
    ```

1. 分析弱密码哈希漏洞。

    打开`UserService.cs` 文件并找到`GetMd5Hash` 方法。 将此方法添加到聊天上下文并提交以下提示：

    ```text
    Why is MD5 hashing unsuitable for password storage? What are the security risks of using MD5 for passwords, and what stronger alternatives should I use instead?
    ```

1. 请求有关实现安全密码哈希的具体指导。

    ```text
    Show me how to implement secure password hashing using bcrypt or PBKDF2. What additional security measures should I implement for password handling?
    ```

1. 分析敏感数据泄漏问题。

    打开`PaymentService.cs` 文件并找到`ProcessPayment` 方法。 将其添加到聊天上下文并询问：

    ```text
    What sensitive information is being logged in this payment processing method? Why is logging full credit card numbers and CVV codes a security risk, and how should payment data be handled securely?
    ```

1. 检查硬编码凭据漏洞。

    打开`SecurityValidator.cs` 文件并找到管理员凭据常量。 将相关代码添加到聊天上下文并询问：

    ```text
    What security risks are created by hardcoding admin credentials in source code? How should application credentials be managed securely in production environments?
    ```

1. 分析输入验证弱点。

    聚焦于`SecurityValidator.cs` 中的`ValidateInput` 方法并询问：

    ```text
    What makes this input validation method ineffective against malicious input? How can I implement proper input sanitization to prevent XSS and other injection attacks?
    ```

1. 查看信息泄露问题。

    选择多个包含调试日志的方法（这些方法分布在不同文件中），然后询问：

    ```text
    How does excessive debug logging create security vulnerabilities? What information should never be logged, and how can I implement secure logging practices?
    ```

1. 检查可预测令牌生成。

    聚焦于`GenerateSessionToken` 方法并询问：

    ```text
    Why are predictable session tokens a security risk? How should secure, unpredictable session tokens be generated to prevent session hijacking attacks?
    ```

1. 分析文件上传验证问题。

    查看`ValidateFileUpload` 方法并询问：

    ```text
    What makes this file upload validation insufficient for security? How can I implement comprehensive file upload security to prevent malicious file execution?
    ```

1. 在修正阶段记录分析结果以供参考。

    记下 GitHub Copilot 针对每个漏洞类别提供的建议。 本文档将指导你在下一个任务中实现安全修补程序。

### 使用 GitHub Copilot 的智能体模式解决问题

GitHub Copilot 的智能体模式支持跨多个文件和方法自动实现复杂的安全修补程序。 与提供分析和建议的询问模式不同，智能体模式可以直接修改代码来实现安全改进。 此方法特别适用于系统安全修正，在这些场景，需要一致地解决多个相关漏洞。

在此任务中，你将使用 GitHub Copilot 的智能体模式为 ContosoShopEasy 应用程序中的所有已识别漏洞实现全面的安全修补程序。

使用以下步骤完成此任务：

1. 将 GitHub Copilot 对话助手切换为智能体模式。

    在“对话助手”视图中，找到模式选选择器，将模式从“询问”更改为“智能体”********。 智能体模式允许 GitHub Copilot 根据说明直接修改代码。

1. 首先解决 SQL 注入漏洞。

    打开`ProductService.cs` 文件并找到`SearchProducts` 方法。 使用以下提示指示智能体：

    ```text
    Fix the SQL injection vulnerability in the SearchProducts method. Remove the simulated SQL query logging that demonstrates the vulnerability, and implement proper input sanitization to safely handle search terms. Ensure the method still functions correctly for legitimate searches while preventing malicious input.
    ```

1. 监视智能体的进度并查看建议的更改。

    智能体将修改代码以删除易受攻击的日志记录并实现更安全的输入处理。 查看这些更改，确保在解决安全问题的同时保持功能正常。

1. 实现安全密码哈希。

    聚焦于`UserService.cs` 文件并使用以下提示：

    ```text
    Replace the MD5 password hashing with bcrypt or PBKDF2. Update the GetMd5Hash method to use a cryptographically secure hashing algorithm with proper salt generation. Ensure compatibility with existing user authentication while improving security.
    ```

1. 查看并测试密码哈希更改。

    智能体将实现更强的密码哈希。 通过运行应用程序来测试更改，确保用户注册和登录仍然正常运行。

1. 解决付款处理中的敏感数据泄露问题。

    打开`PaymentService.cs` 文件并指示智能体：

    ```text
    Fix sensitive data logging in the ProcessPayment method. Remove logging of full credit card numbers, CVV codes, and other sensitive payment information. Implement secure logging that masks sensitive data while maintaining useful operational information.
    ```

1. 删除硬编码的管理员凭据。

    聚焦于`SecurityValidator.cs` 文件并使用以下提示：

    ```text
    Remove hardcoded admin credentials and replace them with a secure configuration approach. Implement environment variable or configuration file-based credential management while maintaining the functionality for educational demonstration purposes.
    ```

1. 实现正确的输入验证。

    指示智能体修复输入验证漏洞：

    ```text
    Strengthen the ValidateInput method to properly sanitize user input and prevent XSS attacks. Implement comprehensive input validation that rejects malicious content while allowing legitimate user input. Ensure the method returns false for truly dangerous input.
    ```

1. 通过日志记录减少信息泄露。

    解决跨多个文件的调试日志记录问题：

    ```text
    Review all console logging throughout the application and remove or mask sensitive information. Implement secure logging practices that provide useful debugging information without exposing passwords, credit card data, or system internals.
    ```

1. 实现安全会话令牌生成。

    聚焦于会话令牌漏洞：

    ```text
    Replace the predictable session token generation with a cryptographically secure random token generator. Ensure tokens are unpredictable and sufficiently long to prevent brute force attacks.
    ```

1. 加强文件上传验证。

    解决文件上传安全问题：

    ```text
    Implement comprehensive file upload validation that checks file types, sizes, and content. Reject dangerous file types and implement proper security controls to prevent malicious file uploads.
    ```

1. 在每次重大更改后测试应用程序。

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

1. 通过检查产品搜索功能来测试 SQL 注入修复。

    验证`SearchProducts` 方法是否不再记录易受攻击的 SQL 查询，且搜索功能对于合法搜索词是否仍能正常工作。

1. 验证密码安全改进措施。

    检查用户注册和登录进程是否不再记录纯文本密码，以及是否实现了更强大的密码哈希。 应用程序仍应正确对用户进行身份验证。

1. 确认付款处理安全性改进。

    确保付款处理不再记录完整的信用卡卡号或信用卡验证码，同时保持成功处理付款的能力。

1. 验证管理员凭据安全性。

    验证硬编码管理员凭据是否不再显示在日志或安全审核中，而管理员功能仍可通过安全方式访问。

1. 测试输入验证改进措施。

    确认改进的输入验证可正确处理合法和潜在的恶意输入，而不会破坏正常的用户交互。

1. 检查信息泄露修补程序。

    检查控制台输出，确保敏感的系统信息、配置详细信息和用户数据不再通过调试日志公开。

1. 验证会话令牌安全性。

    如果应用程序在运行过程中生成会话令牌，需确认这些令牌显示为随机性和不可预测性，而非采用以前基于模式的生成方式。

1. 验证文件上传安全性改进。

    测试文件上传验证改进措施，确保危险文件类型被正确拦截，同时合法文件能被正常接收。

1. 将安全审核输出与原始版本进行比较。

    运行安全审核功能，并将结果与初始观察结果进行比较。 审核应显示改进后的安全状况，同时保持教育价值。

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

1. 使用引用 GitHub 问题的描述性消息提交更改。

    使用清晰描述安全修补程序并引用问题编号的提交消息：

    ```bash
    git commit -m "Fix SQL injection vulnerability in ProductService SearchProducts method

    - Remove vulnerable SQL query logging
    - Implement proper input sanitization
    - Maintain search functionality while preventing injection attacks

    Fixes #1"
    ```

1. 针对每个主要的安全修补程序类别分别进行额外提交。

    提交密码哈希改进措施：

    ```bash
    git commit -m "Replace MD5 with secure password hashing

    - Implement bcrypt/PBKDF2 for password storage
    - Add proper salt generation
    - Maintain authentication compatibility

    Fixes #2"
    ```

1. 继续针对剩余的安全修补程序进行提交：

    ```bash
    git commit -m "Remove sensitive data from payment processing logs

    - Mask credit card numbers in logging
    - Remove CVV code exposure
    - Maintain operational logging capabilities

    Fixes #3"

    git commit -m "Remove hardcoded admin credentials

    - Implement secure credential management
    - Use environment variables for sensitive config
    - Maintain admin functionality for demo purposes

    Fixes #4"

    git commit -m "Strengthen input validation and prevent XSS

    - Implement comprehensive input sanitization
    - Reject malicious content properly
    - Maintain user experience for legitimate input

    Fixes #5"

    git commit -m "Reduce information disclosure in debug logging

    - Remove sensitive data from console output
    - Implement secure logging practices
    - Maintain useful debugging information

    Fixes #6"

    git commit -m "Implement secure session token generation

    - Replace predictable tokens with cryptographically secure random generation
    - Increase token entropy to prevent brute force attacks

    Fixes #7"

    git commit -m "Enhance file upload security validation

    - Implement comprehensive file type checking
    - Add file size and content validation
    - Reject dangerous file types effectively

    Fixes #8"
    ```

1. 将更改推送到 GitHub 存储库。

    ```bash
    git push origin main
    ```

1. 验证 GitHub 问题是否已自动关闭。

    导航到 GitHub 上的存储库，并检查那些被提交信息引用的问题是否已标记为“已关闭”。

1. 查看提交历史记录，确保正确记录所有安全修补程序。

    验证提交消息是否清楚地描述了安全改进措施，以及是否为将来的参考提供了完善的审核线索。

