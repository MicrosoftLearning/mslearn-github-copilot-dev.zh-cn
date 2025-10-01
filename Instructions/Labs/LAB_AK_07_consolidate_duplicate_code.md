---
lab:
  title: 练习 - 使用 GitHub Copilot 合并重复代码
  description: 了解如何使用 GitHub Copilot 工具来分析复杂的代码库并合并重复的代码逻辑。
---

# 使用 GitHub Copilot 合并重复代码

开发/扩展包含相似或相关功能的代码库时，通常会引入重复的代码逻辑。 这可能并非有意为之，可能只是重用代码来构建新功能的原型。 如果重复的逻辑随着时间的推移变得与周围的代码匹配，则问题可能会变得更复杂。 更改一个位置（不更改另一个位置）中的变量名、函数名和代码结构可掩盖重复的逻辑。 紧张的日程、糟糕的文档以及缺乏适当的代码评审都可能导致问题加剧。 最终，重复的逻辑使代码难以读取、维护、调试和测试。

在本练习中，你将查看包含重复代码逻辑的现有项目，分析合并选项，合并重复的代码逻辑，并测试重构后的代码以确保其按预期工作。 你使用 GitHub Copilot 的“询问”模式来了解现有代码项目并探索合并逻辑的选项。 使用 GitHub Copilot 的“智能体”模式，通过将重复逻辑合并到共享帮助程序方法来重构代码。 并测试原始代码和重构后的代码，确保合并后的逻辑按预期运行。

完成此练习大约需要 30 分钟****。

> **重要说明**：若要完成本练习，必须提供自己的 GitHub 帐户和 GitHub Copilot 订阅。 如果没有 GitHub 帐户，可以<a href="https://github.com/" target="_blank">注册</a>免费的个人帐户，并使用 GitHub Copilot 免费版计划来完成练习。 如果可以从实验室环境中访问 GitHub Copilot Pro、GitHub Copilot Pro+、GitHub Copilot Business 或 GitHub Copilot Enterprise 订阅，则可以使用你现有的 GitHub Copilot 订阅来完成本练习。

## 开始之前

实验室环境必须包括以下内容：Git 2.48 或更高版本、.NET SDK 9.0 或更高版本、具有 C# 开发工具包扩展的 Visual Studio Code，以及访问启用了 GitHub Copilot 的 GitHub 帐户。

### 配置实验室环境

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

### 下载示例代码项目

使用以下步骤下载示例项目并在 Visual Studio Code 中打开它：

1. 在实验室环境中打开浏览器窗口。

1. 要下载包含示例应用项目的 zip 文件，请在浏览器中打开以下 URL：[GitHub Copilot 实验室 - 合并重复代码](https://github.com/MicrosoftLearning/mslearn-github-copilot-dev/raw/refs/heads/main/DownloadableCodeProjects/Downloads/GHCopilotEx7LabApps.zip)

    zip 文件名为 GHCopilotEx7LabApps.zip****。

1. 从 GHCopilotEx7LabApps.zip 文件中解压缩文件****。

    例如：

    1. 导航到实验室环境中的下载文件夹。

    1. 右键单击 GHCopilotEx7LabApps.zip，然后选择“全部解压”******。

    1. 选择“完成时显示解压缩的文件”，然后选择“解压缩”。

1. 将 GHCopilotEx7LabApps 文件夹复制到易于访问的位置，例如 Windows 桌面文件夹****。

1. 在 Visual Studio Code 中打开 GHCopilotEx7LabApps 文件夹****。

    例如：

    1. 在实验室环境中打开 Visual Studio Code。

    1. 在 Visual Studio Code 中的“文件”菜单上，选择“打开文件夹” 。

    1. 导航到 Windows 桌面文件夹，选择 GHCopilotEx7LabApps，然后选择“选择文件夹”********。

1. 在 Visual Studio Code 的“解决方案资源管理器”视图中，验证以下项目结构：

    - GHCopilotEx7LabApps\
        - ECommerceOrderAndReturn\
            - Dependencies\
            - Configuration\
                - AppConfig.cs
            - Models\
                - Order.cs
                - Return.cs
            - Security\
                - SecurityValidator.cs
            - Services\
                - AuditService.cs
                - EmailService.cs
                - InventoryService.cs
            - EXPECTED_OUTPUT.md
            - OrderProcessor.cs
            - Program.cs
            - README.md
            - ReturnProcessor.cs

## 练习场景

你是一家咨询公司的软件开发人员。 客户需要你帮忙合并重复的代码逻辑。 你的目标是在保留现有功能的同时提高代码的可维护性。 公司给你分配了以下应用：

- E-CommerceOrdersAndReturns:这是一个电子商务应用，用于处理客户订单和产品退货。 它包含用于验证订单和退货、计算运费、发送电子邮件通知、记录审核活动以及管理库存水平的核心业务逻辑。

本练习包括以下任务：

1. 手动查看电子商务订单与退货代码库。
1. 使用 GitHub Copilot 对话助手（询问模式）识别重复代码。
1. 使用 GitHub Copilot 对话助手（智能体模式）合并重复的逻辑。
1. 测试重构后的电子商务订单与退货代码。

### 手动查看电子商务订单与退货代码库

任何重构工作的第一步都是确保你了解现有的代码库。 了解代码结构、业务逻辑以及代码运行时生成的结果非常重要。

在此任务中，你将查看电子商务订单与退货处理项目的主要组件，扫描代码中的重复代码模式，并测试代码。

使用以下步骤完成此任务：

1. 花点时间查看 ECommerceOrderAndReturn 项目结构。

    该代码库采用企业级应用程序典型的分层体系结构。 主要体系结构层包括：模型、配置、安全、服务和处理。

1. 检查主处理类。

    将 OrderProcessor.cs 和 ReturnProcessor.cs 并排打开********。 这两个类分别表示处理客户订单和产品退货的核心业务逻辑。

    请注意，这两个类的方法签名和处理模式相似。 这是最明显的一类重复，但整个代码库中还存在其他更不易察觉的重复。

1. 评审服务层。

    导航到“Services”文件夹，检查 EmailService.cs、AuditService.cs 和 InventoryService.cs****************。

    你可能会注意到，这些服务实现相似的模式来处理电子邮件通知、审核日志和库存管理。 每个服务都有采用相似的结构，但因不同业务流程（订单与退货）而重复的方法。

1. 运行应用程序并查看控制台输出。

    > 注意****：控制台输出的副本存储在项目目录中包含的 EXPECTED_OUTPUT.md 文件中。 你将使用此文件来验证合并重复代码后应用程序的行为是否未发生变化。

    右键单击“ECommerceOrdersAndReturns”，再依次选择“调试”和“启动新实例”，即可从解决方案资源管理器视图中运行应用程序************。

    控制台输出包括：

    - 初始库存水平
    - 订单处理（含验证）、运费计算、付款处理、库存预留、电子邮件通知以及审核日志
    - 退货流处理（步骤相似，但针对退货进行了调整）
    - 更新后的库存水平
    - 具有各种无效输入的安全验证测试

    应用程序运行 5 个测试方案来展示成功处理和安全验证失败两种情况。

1. 花点时间对观察到的任何重复代码模式进行分类。

    例如：

    **重复的方法**：OrderProcessor 和 ReturnProcessor 具有相同的 `Validate()` 方法以及相似的 `CalculateShipping()` 方法。

    **服务层中的相似模式**：每个服务都有采用相似的模式，但因不同业务流程（订单与退货）而重复的方法。

务必先了解现有功能，再进行更改。 通过运行代码并检查输出建立一个基线，用于验证重构不破坏现有功能。

### 使用 GitHub Copilot 对话助手（询问模式）识别重复代码

GitHub Copilot 对话助手的“询问”模式是一种强大的工具，可用于分析复杂代码库以及识别可能并不显而易见的重复模式。 在“询问”模式下，Copilot 充当智能代码审阅者，可同时分析多个文件并识别显性和隐性的（代码逻辑）重复。

在此任务中，你将使用 GitHub Copilot 系统地识别电子商务应用程序中的各类重复代码模式。

使用以下步骤完成此任务：

1. 打开 GitHub Copilot 的“聊天”视图，然后配置“询问”模式和 GPT-4.1 模型。

    **** 如果“聊天”视图尚未打开，选择 Visual Studio Code 窗口顶部的“聊天”图标。 ******** 确保聊天模式设置为“询问”，且使用的是 GPT-4.1 模型。

    GitHub Copilot 免费版计划提供了 GPT-4.1 模型。该模型专为处理复杂任务设计，提供智能代码分析/建议功能。

1. 关闭编辑器中已打开的所有文件。

    GitHub Copilot 使用编辑器中打开的文件来构建上下文。 关闭不需要的文件选项卡有助于减少分析中的干扰。

1. 将 OrderProcessor 和 ReturnProcessor 文件添加到聊天上下文。

    使用拖放作将解决方案资源管理器中的 OrderProcessor.cs**** 和 ReturnProcessor.cs**** 文件添加到聊天上下文。

    向聊天上下文中添加一个文件，这种做法就是在指示 GitHub Copilot 在分析提示时将该文件包含在上下文中。

    如果使用默认文件夹视图而不是解决方案资源管理器，可以右键单击文件，然后选择“将文件添加到聊天”。**** 还可以在代码编辑器中打开文件，以帮助建立上下文。

1. 要求 GitHub Copilot 识别所选文件中重复的代码模式。

    例如，提交以下提示来分析核心重复：

    ```text
    What duplicate code exists between OrderProcessor.cs and ReturnProcessor.cs? Identify specific methods and logic that are duplicated between these classes. Describe opportunities to consolidate duplicate code.
    ```

1. 请花点时间查看 GitHub Copilot 的回复。

    GitHub Copilot 应识别 `Validate()` 方法重复以及 `CalculateShipping()` 方法中的相似模式。 它可能还会注意到与审核日志、错误处理以及付款/退款处理相关的类似模式。

1. 更新聊天上下文以指定 EmailService.cs 文件****。

    在编辑器中打开 EmailService.cs****。 你也可以使用拖放操作将 EmailService.cs**** 文件添加到聊天上下文。 从上下文中移除所有其他文件。

1. 要求 GitHub Copilot 识别 EmailService 类中的重复。

    例如：

    ```text
    Analyze the EmailService class. What duplicate logic exists within this service for handling order confirmations versus return confirmations? Describe opportunities to consolidate duplicate code.
    ```

1. 请花点时间查看 GitHub Copilot 的回复。

    GitHub Copilot 应识别用于处理订单确认和退货确认的重复逻辑。 这包括准备及发送电子邮件的模板化方法。 GitHub Copilot 也可以识别与帮助程序方法和控制台输出相关的重复模式。

1. 更新聊天上下文以指定 AuditService.cs**** 和 InventoryService.cs**** 文件。

    在编辑器中打开 AuditService.cs**** 和 InventoryService.cs****。 你也可以使用拖放操作将这些文件添加到聊天上下文中。 从上下文中移除所有其他文件。

1. 要求 GitHub Copilot 识别审核和库存服务文件中的重复。

    例如：

    ```text
    Analyze the AuditService and InventoryService classes. Identify the methods that contain duplicate logic patterns that could be consolidated. Describe opportunities to consolidate duplicate code.
    ```

1. 请花点时间查看 GitHub Copilot 的回复。

    GitHub Copilot 应识别的模式包括 AuditService 中的审核条目创建/验证/存储，以及 InventoryService 中的库存验证/更新/记录等。 GitHub Copilot 也可以识别与帮助程序方法相关的重复模式。

1. 要求 GitHub Copilot 执行全面的重复分析。

    例如：

    ```text
    @workspace Analyze the entire ECommerceOrderAndReturn codebase and identify all forms of code duplication, including method-level, service-level, and architectural pattern duplications. Prioritize the duplications by impact and refactoring difficulty. Describe an approach for consolidating this code.
    ```

    分析特定文件后，可通过要求 GitHub Copilot 将整个代码库纳入分析范围来获得更全面的视角。 范围扩大可以揭露额外的重复模式，例如多个文件或层级中相似的错误处理、记录和验证逻辑。 拥有一个提供信息并优先处理重构策略的响应，通常很有帮助。

    > 注意****：`@workspace` 和 `#codebase` 关键字指示 GitHub Copilot 在分析上下文中包含整个代码库。 `@workspace` 关键字仅在使用 GitHub Copilot 的“询问”模式时可用。 `#codebase` 关键字可在任何模式（“询问”“编辑”或“智能体”）下使用。

1. 请花点时间查看 GitHub Copilot 的回复。

    该响应应当提供对所有重复模式的全面分析，并提出合并重复代码的方法建议。 在生产场景中，应分析响应的每个部分，并考虑使用跟进提示深入挖掘特定区域。

    在本培训练习中，务必了解 GitHub Copilot 可提供哪些类型的信息，但在考虑重构策略时，可以专注于摘要部分。

    例如：

    ```md
    
    **Summary**

    The codebase contains significant method-level and service-level duplication, especially in validation, shipping calculation, audit logging, email notification, and inventory management. The highest priority and easiest wins are consolidating validation and shipping logic. Service-level helper methods should be refactored next. The processor workflow pattern can be abstracted for further maintainability, though this is more complex.

    Start with shared services for validation and shipping, then refactor service helpers, and finally consider architectural abstraction for processors.

    ```

1. 使用手动代码评审验证 GitHub Copilot 的分析。

    对照 GitHub Copilot 的发现与你自己观察到的情况。 确保你不仅了解重复模式有哪些，还了解这些模式存在的原因，以及如何在维护业务逻辑完整性的同时合并这些模式。

GitHub Copilot 的“询问”模式特别强大，可用于识别除简单复制粘贴情况以外的不易察觉的重复。 它可以识别相似的逻辑模式、以不同方式实现的等效业务规则以及跨多个文件范围的体系结构重复。

> 注意****：在此任务期间生成的分析用于为你在下一部分中实现的重构策略提供依据。

### 使用 GitHub Copilot 对话助手（智能体模式）合并重复的逻辑

通过 GitHub Copilot 的“智能体”模式，你可以分配跨多个文件和体系结构层次的复杂多步骤重构任务。 智能体可以自主创建新文件、修改现有代码并实现全面的重构策略，同时让你知道其进度。

在此任务中，你将使用 GitHub Copilot 智能体系统地消除在上一个任务中识别出的重复代码模式，从消除最简单的重复开始，再推进到更复杂的服务层合并。

使用以下步骤完成此任务：

1. 将 GitHub Copilot 的“聊天”视图切换为“智能体”模式。

    使用“智能体”模式时，GitHub Copilot 可以对代码库进行自主更改。 “智能体”模式对于复杂的多步骤重构任务特别有用。

    若要更改模式，请找到模式选择器（通常位于聊天视图的左下角），然后选择“智能体”****。

1. 花点时间计划重构策略。

    将任务分配给 GitHub Copilot 智能体之前，请考虑重构的逻辑顺序。 使用上一个任务中的分析作为决策依据。

    例如：

    如果 GitHub Copilot 建议：

    “从共享服务（验证和运费计算）开始，再进行服务帮助程序的重构，最后考虑处理器的体系结构抽象化。”

    你可以使用以下分阶段方法：

    - **第 1 阶段**：核心业务逻辑重复（验证和运费计算）
    - **第 2 阶段**：服务层重复（电子邮件、审核、库存服务）
    - **第 3 阶段**：跨领域关注点和体系结构改进

    此分阶段方法确保更改易于管理，并且可以循序渐进地进行测试。

1. 要求 GitHub Copilot 智能体合并 OrderProcessor 和 ReturnProcessor 类中的验证逻辑。

    例如，将以下任务提交到 GitHub Copilot 智能体：

    ```text
    Create a shared ValidationService class that consolidates the duplicate Validate() method logic from OrderProcessor and ReturnProcessor. The service should handle ID validation for both orders and returns while maintaining all existing security checks, business rules, and logging. Update both processor classes to use the new shared service and remove the duplicate private methods. Build and test the code to ensure the functionality remains intact. Continue working until the validation service is fully integrated.
    ```

1. 在聊天视图中监视智能体的进度。

    智能体完成分配的任务时，其进度应显示在聊天中。

    若要帮助智能体处理任务，请根据需要授予继续操作或提供额外上下文的权限。 例如，如果智能体要求生成或运行应用程序的权限，请选择“继续”****

    在此任务期间，智能体将完成以下操作：

    - 在 Services 文件夹中创建一个新的 ValidationService.cs 文件****
    - 将验证逻辑提取到可重用的方法中
    - 更新这两个处理器类以使用新服务
    - 删除重复的专用验证方法
    - 通过成功的生成和测试运行来验证功能

1. 查看并接受验证服务的更改。

    使用代码编辑器选项卡检查智能体建议的更改。 你可以滚动浏览聊天编辑，查看具体的代码修改。 在编辑器选项卡上选择“保留”以接受当前编辑****。 可在编辑器选项卡上选择“撤消”**** 以拒绝编辑。

    在聊天视图中选择“保留”**** 或“撤消”**** 以接受或拒绝所有更改

    新的验证服务应保留所有现有验证逻辑，同时提供一个可重用的实现。 如果更改看起来正确，请接受它们。

    > 注意****：在处理生产代码时，在进行重大重构操作后对代码进行彻底测试非常重要。 这涉及构建和测试应用程序以验证功能是否按预期工作、单元测试是否通过以及输出是否与原始行为保持一致。 为了在此培训练习中节省时间，我们依靠智能体执行增量测试。 一项额外的（手动验证）测试将在所有重构任务完成后执行。

1. 要求 GitHub Copilot 智能体合并运费计算逻辑。

    例如，使用以下任务合并运费计算：

    ```text
    Create a shared ShippingCalculationService that consolidates the similar CalculateShipping() logic from OrderProcessor and ReturnProcessor. The service should handle both order shipping (with free shipping thresholds) and return shipping (with processing fees) while maintaining the different business rules for each type. Update both processor classes to use the new shared service. Build and test the code to ensure the functionality remains intact. Continue working until the shipping calculation service is fully integrated.
    ```

    智能体应创建一个运费服务，用于处理这两种情况，同时保留订单与退货的不同业务规则。

    > 注意****：如果智能体在代码重构过程中遇到任何问题，则应引用原始处理器类以获取指导，并应继续更新和测试代码，直到运费计算服务完全集成。 如果智能体未能完成分配的任务或者陷入无法解决的代码修改循环，请停止该智能体并撤消编辑。 聊天会话应包含足够的上下文，以便排查问题并更新任务（提示）。 你也可以要求 GitHub Copilot 帮你更新分配的任务，确保问题得到解决。

1. 查看并接受更改。

1. 要求 GitHub Copilot 智能体重构 EmailService 重复项。

    例如，使用以下任务合并电子邮件服务重复项：

    ```text
    Refactor the EmailService class to eliminate duplicate logic in the helper methods. Create a unified approach for template building, subject formatting, email sending, and activity logging that can handle both order and return confirmations. The public methods SendOrderConfirmation and SendReturnConfirmation should remain, but they should use shared private helper methods. Build and test the code to ensure the functionality remains intact. Continue working until the unified approach is fully integrated.
    ```

1. 查看并接受更改。

1. 要求 GitHub Copilot 智能体合并 AuditService 重复项。

    例如，使用以下任务合并审核服务重复项：

    ```text
    Refactor the AuditService class to consolidate the duplicate logic in LogOrderActivity and LogReturnActivity. Create shared helper methods for audit entry creation, validation, storage, and compliance checking. The public methods should remain but use common underlying logic. Build and test the code to ensure the functionality remains intact. Continue working until the shared helper methods are fully integrated.
    ```

1. 查看并接受更改。

1. 要求 GitHub Copilot 智能体解决 InventoryService 重复问题。

    例如，使用以下任务来处理库存服务的重复问题：

    ```text
    Refactor the InventoryService class to eliminate duplicate logic between ReserveOrderInventory and RestoreReturnInventory. Create shared helper methods for inventory validation, level updates, and transaction logging while maintaining the different business logic for reservations versus restorations. Build and test the code to ensure the functionality remains intact. Continue working until the shared helper methods are fully integrated.
    ```

1. 要求 GitHub Copilot 智能体合并代码库中的任何剩余重复项。

    例如，使用以下任务解决任何剩余的重复问题：

    ```text
    Analyze the entire ECommerceOrderAndReturn codebase and identify any remaining duplicate code patterns that should be consolidated. Focus on cross-cutting concerns like payment processing, status updates, and error handling. Create shared services or helper methods as needed to eliminate these duplications while maintaining existing functionality. Build and test the code to ensure the functionality remains intact. Continue working until all identified duplications are fully integrated.
    ```

    对于像“捕获任何剩余问题”这样的任务，应仅在重构流程结束时分配给 GitHub Copilot 智能体，且仅在必要时使用。 过早尝试这类全面分析可能会导致需要回滚的意外结果或任务失败。 最好使用分阶段流程来创建计划的方法并按优先级处理各项修订。

    > **注意：** 即使在 GitHub Copilot 合并剩余的重复代码模式”后，可能还有进一步合并的机会。 但是，你实现的计划方法应合并在代码库中所有主要的重复模式。 如有疑问，可以重复分析和重构过程。 请记住，不应将 GitHub Copilot 用作正式代码评审过程的替代方案。

GitHub Copilot 智能体擅长处理需要理解业务逻辑和体系结构模式的复杂多文件重构任务。 通过将重构拆分为逻辑阶段并循序渐进地进行测试，可确保合保持系统完整性，同时显著提高代码质量、可维护性、可扩展性和可重用性。

### 测试重构后的电子商务订单与退货代码

手动测试和验证对于确保重构后的代码保留预期的业务逻辑和功能而言非常重要。 成功的重构过程应实现预期目标（例如合并重复的代码逻辑），同时生成与原始实现相同的行为。

在此任务中，你将验证重构后的代码是否保留了所有原始功能，以及合并是否成功。

使用以下步骤完成此任务：

1. 生成重构后的项目以检查编译错误。

    如果有任何编译错误，请检查重构后的代码并解决问题。 你可以使用 GitHub Copilot 来帮助诊断和修复重构过程中产生的任何问题。

1. 运行重构后的应用程序并捕获输出。

    该应用程序应像以前一样运行全部 5 个测试方案：

    - 初始库存显示
    - 带有完整工作流的有效的订单处理
    - 带有完整工作流的有效的退货处理
    - 更新后的库存水平
    - 具有无效输入的安全验证测试

1. 让 GitHub Copilot 比较重构后的代码生成的输出与原始输出。

    例如：

    ```text
    Run the app and compare the generated output with the contents of the EXPECTED_OUTPUT.md file. Does the current output match the stored output? Explain any differences.
    ```

    原始输出 EXPECTED_OUTPUT.md**** 包含在 ECommerceOrderAndReturn 文件夹中。

    可以创建第二个输出文件，然后让 GitHub Copilot 明确这两个文件之间的任何差异。

    输出应完全相同，从而证明在合并重复的代码逻辑时，业务逻辑保持不变。

1. 执行最终代码评审。

    检查重构后的代码库，确保：

    - 代码质量****：方法命名规范，采用一致的模式
    - 可维护性****：现在业务规则更改要求只在一个位置更新
    - 可读性****：代码结构清晰且逻辑合理
    - 可重用性****：可以轻松地扩展共享服务，使其满足未来的需求
    - 扩展性****：添加新功能，并使其对现有代码的影响非常小

手动测试验证合并工作是否实现了预期目标：在保留系统功能的同时消除重复代码。 该体系结构在未来的开发中更易维护，其中业务规则更改可在一个位置实现，不需要跨多个重复实现进行更新。

## 总结

在本练习中，你学习了如何使用 GitHub Copilot 合并应用程序中的重复代码。 你了解了电子商务订单和退货处理系统、识别了重复的代码模式，并使用 GitHub Copilot 对代码库进行了重构以提升可维护性和可读性。

## 清理

现在，你已经完成了本练习，请花点时间确保你没有对 GitHub 帐户或 GitHub Copilot 订阅作出任何你不希望保留的更改。 如果你进行了任何更改，请根据需要还原。 如果你将本地电脑用作实验室环境，则可以存档或删除为此练习创建的示例项目文件夹。
