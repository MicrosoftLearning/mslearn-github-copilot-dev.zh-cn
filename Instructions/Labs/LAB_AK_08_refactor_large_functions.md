---
lab:
  title: 练习 - 使用 GitHub Copilot 重构大型函数
  description: 了解如何使用 GitHub Copilot 工具分析复杂代码，并将大型函数重构为更小、更集中的方法。
---

# 使用 GitHub Copilot 重构大型函数

大型函数可能难以阅读、维护和测试。 它们通常包含多个职责，并且可能难以一目了然地理解。 将大型函数重构为更小、更集中的函数可以提高代码的可读性和可维护性。

在本练习中，你将查看一个包含大型函数的现有项目，分析将其拆分为更小的单一职责函数的选项，将大型函数重构为更小的函数，并测试重构后的代码以确保其按预期工作。 你将在“提问”模式下使用 GitHub Copilot 来了解现有代码项目并探索重构逻辑的选项。 你将在“智能体”模式下使用 GitHub Copilot 通过从大型函数中提取代码部分来创建较小的函数，从而重构代码。 你将测试原始代码和重构后的代码以确保重构后的代码按预期工作。

完成此练习大约需要 30 分钟****。

> **重要说明**：要完成本练习，必须提供自己的 GitHub 帐户和 GitHub Copilot 订阅。 如果没有 GitHub 帐户，可以<a href="https://github.com/" target="_blank">注册</a>免费的个人帐户，并使用 GitHub Copilot 免费版计划来完成练习。 如果可从实验室环境中访问 GitHub Copilot Pro、GitHub Copilot Pro+、GitHub Copilot Business 或 GitHub Copilot Enterprise 订阅，则可以使用现有的 GitHub Copilot 订阅完成本练习。

## 开始之前

实验室环境必须包括以下内容：Git 2.48 或更高版本、.NET SDK 9.0 或更高版本、带有 C# Dev Kit 扩展的 Visual Studio Code，以及启用了 GitHub Copilot 的 GitHub 帐户访问权限。

### 配置实验室环境

如果你将本地电脑用作本练习的实验室环境：

- 有关将本地电脑配置为实验室环境的帮助，请在浏览器中打开以下链接：<a href="https://go.microsoft.com/fwlink/?linkid=2320147" target="_blank">配置实验室环境资源</a>。

- 有关在 Visual Studio Code 中启用 GitHub Copilot 订阅的帮助，请在浏览器中打开以下链接：<a href="https://go.microsoft.com/fwlink/?linkid=2320158" target="_blank">在 Visual Studio Code 中启用 GitHub Copilot</a>。

如果在本练习中使用托管实验室环境：

- 有关在 Visual Studio Code 中启用 GitHub Copilot 订阅的帮助，请将以下 URL 粘贴到浏览器的网站导航栏中：<a href="https://go.microsoft.com/fwlink/?linkid=2320158" target="_blank">在 Visual Studio Code 中启用 GitHub Copilot</a>。

- 打开命令终端，并运行以下命令：

    若要确保将 Visual Studio Code 配置为使用正确的 .NET 版本，请运行以下命令：

    ```bash

    dotnet nuget add source https://api.nuget.org/v3/index.json -n nuget.org

    ```

    要确保 Git 配置为使用你的姓名和电子邮件地址，请使用你的信息更新以下命令，然后运行这些命令：

    ```bash

    git config --global user.name "John Doe"

    ```

    ```bash

    git config --global user.email johndoe@example.com

    ```

### 下载示例代码项目

使用以下步骤下载示例项目并在 Visual Studio Code 中打开它：

1. 在实验室环境中打开浏览器窗口。

1. 要下载包含示例应用项目的 zip 文件，请在浏览器中打开以下 URL：[GitHub Copilot 实验室 - 重构大型函数](https://github.com/MicrosoftLearning/mslearn-github-copilot-dev/raw/refs/heads/main/DownloadableCodeProjects/Downloads/GHCopilotEx8LabApps.zip)

    zip 文件名为 GHCopilotEx8LabApps.zip。****

1. 从 GHCopilotEx8LabApps.zip 文件中提取文件。****

    例如：

    1. 导航到实验室环境中的下载文件夹。

    1. 右键单击“GHCopilotEx8LabApps.zip”，然后选择“全部提取”。******

    1. 选择“完成时显示解压缩的文件”，然后选择“解压缩”。

1. 将 GHCopilotEx8LabApps 文件夹复制到易于访问的位置，例如 Windows 桌面文件夹。****

1. 在 Visual Studio Code 中打开 GHCopilotEx8LabApps 文件夹。****

    例如：

    1. 在实验室环境中打开 Visual Studio Code。

    1. 在 Visual Studio Code 中的“文件”菜单上，选择“打开文件夹” 。

    1. 导航到 Windows 桌面文件夹，选择“GHCopilotEx8LabApps”，然后选择“选择文件夹”********。

1. 在 Visual Studio Code 的“解决方案资源管理器”视图中，验证以下项目结构：

    - GHCopilotEx8LabApps\
        - ECommerceOrderProcessing\
            - src\
                - ECommerce.ApplicationCore\
                    - Entities\
                    - Exceptions\
                    - Interfaces\
                    - Services\
                        - OrderProcessor.cs
                - ECommerce.Console\
                    - order_audit_log.txt
                    - Program.cs
                - ECommerce.Infrastructure\
                    - Services\
        - ServerLogAnalysisUtility\

## 练习场景

你是一家咨询公司的软件开发人员。 你的客户需要帮助重构旧版应用程序中的大型函数。 你的目标是在保留现有功能的同时提高代码的可读性和可维护性。 你被分配到以下应用：

- E-CommerceOrderProcessing：此电子商务应用用于处理客户订单。 该流程包括订单验证、库存管理、付款处理、发货协调和客户通知。 该应用程序使用具有分层结构的“整洁体系结构”原则，但 OrderProcessor 类中包含一个处理多个职责的大型方法，需要将其重构为更小、更集中的方法。****

本练习包括以下任务：

1. 手动查看电子商务订单处理代码库。
1. 使用 GitHub Copilot Chat（“提问”模式）识别重构机会。
1. 使用 GitHub Copilot Chat（“智能体”模式）将大型函数重构为更小、更易于管理的函数。
1. 测试重构后的电子商务订单处理代码。

### 手动查看电子商务订单处理代码库

任何重构工作的第一步都是确保你了解现有的代码库。 了解代码结构、业务逻辑以及代码运行时生成的结果非常重要。

在此任务中，你将查看电子商务订单处理项目的主要组件，并运行该应用以观察其功能。

使用以下步骤完成此任务：

1. 确保在 Visual Studio Code 中打开了 GHCopilotEx8LabApps 文件夹。

    如果你尚未下载示例代码项目，请参阅“开始之前”**** 部分。

1. 花点时间查看 ECommerceOrderProcessing 项目结构。

    代码库遵循分层体系结构模式，包含三个主要项目：

    - ECommerce.ApplicationCore：**** 包含域实体、业务逻辑接口和主 OrderProcessor 服务。
    - ECommerce.Console：**** 包含控制台应用程序入口点和依赖项注入设置。
    - ECommerce.Infrastructure：**** 包含外部集成（付款、发货、库存等）的服务实现。

    此结构表示使用“整洁体系结构”原则的真实 .NET 应用程序，其中业务逻辑与基础结构问题分离。

1. 打开 GitHub Copilot Chat 视图。

    如果“聊天”视图尚未打开，你可以通过选择 Visual Studio Code 窗口顶部的“聊天”**** 图标来打开它。

1. 在“聊天”视图中，确保聊天模式设置为“提问”，模型设置为“GPT-4o”********。

    这些设置可在“聊天”视图的左下角找到。 GitHub Copilot 的“提问”模式用于询问一般编码问题并生成与代码相关的说明****。 GPT-4o 模型包含在 GitHub Copilot 免费计划中，是代码分析和重构指导的不错选择。****

    在本练习的后面部分，你将使用 GitHub Copilot 的“智能体”模式，但现在，你应该使用“提问”模式进行代码分析和说明********。

    > 注意****：GitHub Copilot 的响应可能因所选模型而异。 我们建议你在执行本实验室练习时使用指定的模型。 如果你想查看差异，可使用其他模型重复本练习。

1. 使用“解决方案资源管理器”视图定位 OrderProcessor.cs 文件。****

    OrderProcessor.cs 文件位于 src/ECommerce.ApplicationCore/Services 文件夹中。********

1. 在代码编辑器中打开 OrderProcessor.cs 文件。****

    OrderProcessor 类负责处理客户订单。 它包含订单处理的主要业务逻辑，包括验证、付款处理和通知。

1. 花点时间查看 OrderProcessor 类。****

    注意 ProcessOrder 方法。 此方法表示处理客户订单的核心业务逻辑。 请注意，它处理多个不同的操作。 ProcessOrder 方法特意设计得庞大且复杂，以演示业务逻辑随时间累积的真实场景，这使得它难以阅读、测试和维护。

1. 右键单击“ProcessOrder”方法，然后选择“Copilot” > “说明”************。

    如果提示你“选择要说明的封闭范围”，请选择“ProcessOrder”********。

    GitHub Copilot 将分析 ProcessOrder 方法，并提供有关代码功能的详细说明，帮助你在研究重构选项之前了解业务逻辑。

1. 花几分钟时间查看 GitHub Copilot 的说明。

    说明中应突出显示主要处理步骤和业务规则，例如全面的验证过程、安全风险评估、多服务协调以及具有回退功能的错误处理。

1. 运行应用程序以了解其当前行为。

    你有几种运行应用程序的选项。 例如：

    在“解决方案资源管理器”视图中，右键单击“ECommerce.Console”项目，选择“调试”，然后选择“启动新实例”************。 或者，如果你在 Visual Studio Code 中打开了 Program.cs 文件，可以选择编辑器上方的运行按钮。****

    你也可以导航到终端中的 src/ECommerce.Console 文件夹，然后输入以下 .NET CLI 命令：****

    ```bash
    dotnet run
    ```

1. 查看应用程序运行时生成的控制台输出。

    应用程序会为四个测试用例生成输出。 每个测试用例演示不同的场景：

    - 测试 1：**** 包含多个项目的有效订单处理。
    - 测试 2：**** 无效电子邮件地址验证。
    - 测试 3：**** 已拒绝付款处理。
    - 测试 4：**** 可疑订单安全检查。

    每个测试用例的输出显示逐步处理情况，包括验证消息、库存检查、付款处理、发货安排和通知。 输出还显示如何使用适当的错误消息和清理过程处理不同的失败场景。

1. 花点时间考虑 ProcessOrder 方法的重构机会。

    ProcessOrder 方法有几个不同的职责。 每个对应的代码部分都可以提取到一个单独的方法中。

了解现有功能并识别重构机会将帮助你创建重构策略，在改进代码结构的同时保留业务逻辑。 分层体系结构已经在项目级别提供了良好的关注点分离，但大型 ProcessOrder 方法需要关注。

### 使用 GitHub Copilot Chat（“提问”模式）识别重构机会

GitHub Copilot Chat 的“提问”模式是分析复杂代码并识别重构大型方法的机会的绝佳工具。 在“提问”模式下，Copilot 可以分析你的代码结构，并建议将整体式方法分解为更小、更集中方法的方式。

在此任务中，你将使用 GitHub Copilot 评估 ProcessOrder 方法并识别重构机会，在保持业务逻辑的同时改进代码结构。

使用以下步骤完成此任务：

1. 确保 GitHub Copilot Chat 视图已打开，并处于“提问”模式，且已选择“GPT-4o”模型。********

1. 如果你打开了 OrderProcessor.cs 以外的任何文件，请立即关闭它们。

    GitHub Copilot 使用编辑器中打开的文件来建立上下文。 仅打开目标文件有助于将分析集中在你想要重构的代码上，并确保 GitHub Copilot 提供最相关的建议。

1. 将 OrderProcessor.cs 文件添加到聊天上下文中。

    使用拖放操作将 src/ECommerce.ApplicationCore/Services/OrderProcessor.cs 文件从“解决方案资源管理器”添加到聊天上下文中。**** 将文件添加到聊天上下文会告诉 GitHub Copilot 在分析你的提示时包含该文件，这可以提高其分析的准确性。

1. 让 GitHub Copilot 分析 ProcessOrder 方法以查找重构机会。

    提交一个提示，要求 GitHub Copilot 分析 ProcessOrder 方法并确定需要改进的具体领域。 考虑包含有关你希望通过重构实现的目标的详细信息。

    例如：

    ```text
    Analyze the ProcessOrder method in the OrderProcessor class. This method handles multiple responsibilities. Identify opportunities to break this large method into smaller, more focused methods. What specific functions could be extracted, and what would be the benefits of doing so?
    ```

1. 花几分钟时间查看 GitHub Copilot 的响应。

    GitHub Copilot 应识别 ProcessOrder 方法中的各种职责，并建议如何将它们提取到单独的方法中。 分析应识别可以成为单独方法的不同逻辑部分，例如验证逻辑、安全评估、库存操作、付款处理、发货协调、通知处理和订单完成。

    例如，GitHub Copilot 可能会识别：

    - 输入验证和安全检查作为提取的候选对象。
    - 可以组合在一起的库存管理操作。
    - 具有自己的错误处理的付款处理逻辑。
    - 作为单独关注的发货和通知逻辑。
    - 可以分离的订单完成步骤。

1. 让 GitHub Copilot 提供详细的重构计划。

    例如：

    ```text
    Create a detailed refactoring plan for the ProcessOrder method. Show me what the ProcessOrder method would look like after refactoring and provide a list of the methods that should be extracted. I'd like to keep the input validation and security checks together in a single method. Include suggestions for method signatures and return types that would maintain the current error handling behavior.
    ```

    可以使用此类后续提示来获取其他见解，但应避免偏离目标的提示。 在聊天过程中偏离主题会影响 GitHub Copilot 的响应。 清晰的聊天历史记录非常重要。

1. 花几分钟时间查看 GitHub Copilot 的重构计划。

    GitHub Copilot 应提供清晰的大纲，展示如何将 ProcessOrder 方法从一个庞大的整体式方法转变为一系列更小、更集中的方法调用。 此计划应在改进代码结构和可读性的同时保留现有业务逻辑。

    响应应包括：
    - 显示主要步骤作为单独方法调用的高层级流。
    - 提取方法的建议方法名称和签名。
    - 有关跨方法一致处理错误的指导。
    - 有关重构后的结构如何提高可维护性的说明。

1. 请求提供有关错误处理模式的额外指导。

    了解错误处理过程对于在重构 ProcessOrder 方法时保留现有行为至关重要。 你可以让 GitHub Copilot 分析当前的错误处理策略，并建议一种保留或改进现有行为的方法。

    例如：

    ```text
    In the current ProcessOrder method, there are multiple error scenarios with specific cleanup procedures (like releasing inventory on payment failure). In the refactored version, how should I handle errors consistently across the extracted methods? Should each method return an OrderResult object, throw exceptions, or use another pattern to maintain the existing error handling behavior?
    ```

1. 花几分钟时间查看错误处理建议。

    GitHub Copilot 应提供有关在重构后的方法中保持一致错误处理模式的指导。 这一点至关重要，因为当前方法具有复杂的错误处理，其中包含必须保留的回退程序。

    建议应涉及：
    - 如何保持当前回退行为（例如付款失败时释放库存）。
    - 是否使用返回值、异常或结果对象进行错误信号传递。
    - 如何在重构后的方法中保留审核日志。
    - 确保在错误场景中仍执行清理过程的方法。

GitHub Copilot 的“提问”模式擅长分析复杂代码结构并为重构提供策略指导。 此分析的见解将为你在下一部分中实现的具体重构方法提供依据，确保你在实现更好的代码组织的同时保持业务逻辑完整性。

### 使用 GitHub Copilot Chat（“智能体”模式）重构大型函数

“智能体”模式使你能够将复杂的代码重构任务分配给 GitHub Copilot。 分配的任务可以包括创建和/或更新多个文件。 GitHub Copilot 智能体自主处理任务，在处理过程中测试和调试更新，并通过在“聊天”视图中报告进度让你了解情况。

在此任务中，你将使用 GitHub Copilot 智能体通过提取更小、更集中的方法来系统地重构 ProcessOrder 方法，同时保留现有业务逻辑和错误处理行为。

使用以下步骤完成此任务：

1. 确保 GitHub Copilot Chat 视图已在 Visual Studio Code 中打开。

1. 在“聊天”视图中，选择“智能体”模式。****

    “设置模式”下拉菜单位于“聊天”视图的左下角。**** 在“智能体”模式下，GitHub Copilot 自主处理分配给它的任务（你的提示）****。

1. 花点时间考虑你的重构策略。

    使用上一个任务中的分析来制定重构 ProcessOrder 方法的策略。 你的方法应支持增量测试。

    例如，考虑以下分阶段重构策略：

    - **第 1 阶段**：在 OrderProcessor 类中创建存根方法。
    - **第 2 阶段**：提取输入验证和安全评估逻辑。
    - 阶段 3：**** 提取库存管理操作（检查和保留）。
    - 阶段 4：**** 提取付款处理（包括欺诈检测和错误处理）。
    - 阶段 5：**** 提取发货协调和跟踪管理。
    - 阶段 6：**** 提取通知和通信逻辑。
    - 阶段 7：**** 提取订单最终确定和完成过程。

    此分阶段方法确保更改易于管理，并且可以增量测试更新。 重构后的代码应保留与原始方法相同的业务逻辑和错误处理。

1. 在代码编辑器中，滚动到 OrderProcessor 类的底部，然后在 ProcessOrder 方法的右大括号后创建以下代码注释：

    ```csharp

        }
    
        // Add stub methods here
        
    }

    ```

    代码注释应位于 ProcessOrder 方法的最后一个右大括号之后，且在 OrderProcessor 类的右大括号之前。

    你将指示 GitHub Copilot 在 OrderProcessor 类中创建新的单一用途方法时使用此位置。

1. 让 GitHub Copilot 智能体创建存根方法以保存提取的代码。

    例如：

    ```text
    Review the current conversation. I want to start by creating stub code for the new single-purpose methods that will be used when refactoring the ProcessOrder method. Use the method declarations that you proposed in this conversation. Create the new stub methods below the "Add stub methods here" comment in the OrderProcessor class. Ensure that you're using appropriate method parameters and return types. Do not extract any code from the ProcessOrder method, just create the stub methods. After the stub methods are created, open the terminal, navigate to the "ECommerceOrderProcessing/src/ECommerce.Console" directory, then run a "dotnet build" command. Ensure that there are no build errors.
    ```

    首先构建存根方法有助于确保新方法正确定义并集成到现有代码结构中。 此方法允许你在从 ProcessOrder 中完全提取代码之前增量测试和验证每个方法的功能。

1. 监视智能体的进度并在需要时提供帮助。

    GitHub Copilot 智能体将在指定位置创建新方法，然后请求允许在终端中运行构建命令。 出现提示时选择“继续”按钮（以允许智能体继续操作）。****

    如果智能体遇到问题，它应通知你，然后尝试自动修复该问题。 继续在需要时提供帮助。

    执行构建任务后，智能体应通知你新方法已正确定义且没有语法错误。

1. 要接受编辑，请选择“保留”****。

    你可以在代码编辑器中单独接受（或拒绝）各项编辑，也可以在 GitHub Copilot Chat 界面中一次性全部接受。

1. 让 GitHub Copilot 智能体重构 ProcessOrder 方法。

    重构大型方法时，最好将任务分解为可管理的多个阶段。 在这种情况下，这些阶段与你已识别的单一进程方法一致。 在使用智能体重构代码时，也应采用相同的方法。 编写一个任务，指示智能体一次重构一个部分，然后在继续下一部分之前测试更新。

    但是，使用 GitHub Copilot 智能体就像有一个开发人员可以独立完成一系列任务。 在这种情况下，一系列任务是重构每个代码部分并在继续下一部分之前测试更新。 换句话说，你可以编写一个单独的任务（提示），让 GitHub Copilot 智能体重构每个代码部分，在继续重构下一部分之前测试每个单一用途方法。

    例如：

    ```text
    Review the current conversation. Examine the ProcessOrder method and identify the code sections that should be extracted into the single-purpose stub methods that you already created. Move the identified code sections into the associated single-purpose stub methods, constructing and testing the methods in the suggested order. Replace the extracted code sections with a call to the associated single-purpose method. Use local variables of the associated return value type to ensure that the ProcessOrder method maintains the same error handling behavior that's provided by the original code. As each method is updated, use a "dotnet run" command to ensure that the code features and error handling processes work correctly (including rollback features, like releasing inventory on payment failure). Also verify that the four test case scenarios generate the expected console output when the app is run (all test cases should pass). Continue extracting code into the new single-purpose methods (and testing the app) until all methods are complete and the application generates the expected console output for the four test case scenarios. Don't stop working on this task until all methods are constructed and tested. Display the step-by-step approach that you'll use to complete this task and then begin.
    ```

    > 注意****：在处理生产代码时，在进行重大重构操作后对代码进行彻底测试非常重要。 这涉及构建和测试应用程序以验证功能是否按预期工作、单元测试是否通过以及输出是否与原始行为保持一致。 为了在此培训练习中节省时间，我们依靠智能体执行增量测试。 代码重构任务完成后，你将完成额外的（手动验证）测试。

1. 监视智能体的进度并在需要时提供帮助。

    GitHub Copilot 智能体应首先描述其重构 ProcessOrder 方法每个部分的计划。 该计划应包括针对每个代码部分的分步方法，其中包括：将代码从 ProcessOrder 方法移动到对应的单一进程方法中，用方法调用替换 ProcessOrder 中的提取代码，以及测试应用以确保重构后的代码按预期工作。

    智能体还将在“聊天”视图中提供更新以描述其进度，包括它遇到的任何问题。 你可以与智能体交互以阐明指令或根据需要提供其他上下文。

    GitHub Copilot 智能体通常会在重构过程中请求允许构建或运行应用程序。 出现这种情况时，请在“聊天”视图中选择“继续”按钮以允许智能体继续操作。****

    > **重要说明**：如果 GitHub Copilot 智能体在重构 ProcessOrder 方法的所有部分之前停止处理分配的任务，请输入提示告诉智能体继续重构任务。

1. 重构过程完成后，接受更改。

    在“聊天”视图中选择“保留”按钮以接受智能体所做的所有更改。****

1. 花点时间查看更新后的 OrderProcessor 类。

    智能体完成重构任务后，OrderProcessor 类应包含几个更小、更集中的方法，这些方法处理订单处理的特定方面。 ProcessOrder 方法应显著缩短且更易于阅读，订单处理工作流的每个步骤都在自己的方法中明确定义。

    例如，更新后的 ProcessOrder 方法应类似于以下内容：

    ```csharp
    public OrderResult ProcessOrder(Order order)
    {
        // Log the start of order processing for audit trail
        _auditLogger.LogOrderProcessingStarted(order.Id, order.CustomerEmail);

        try
        {
            // Validate order and perform security checks
            if (!ValidateOrderAndSecurity(order, out string? validationFailure))
            {
                return OrderResult.Failure(validationFailure ?? "Validation failed for unknown reasons");
            }

            Console.WriteLine($"Processing Order {order.Id} for {_securityValidator.MaskEmail(order.CustomerEmail)}...");
            Console.WriteLine($"Order contains {order.Items.Count} items, Total: ${order.TotalAmount:F2}");

            // Check inventory and reserve stock
            if (!CheckAndReserveInventory(order, out string? inventoryFailure))
            {
                return OrderResult.Failure(inventoryFailure ?? "Inventory check failed for unknown reasons");
            }
            Console.WriteLine("Inventory reserved successfully.");
            _auditLogger.LogInventoryReserved(order.Id, order.Items.Count);

            // Payment Processing with Enhanced Security
            Console.WriteLine("Processing payment...");
            // Process payment
            if (!ProcessPayment(order, out string? paymentFailure))
            {
                return OrderResult.Failure(paymentFailure ?? "Payment processing failed for unknown reasons");
            }

            // Shipping and Logistics Management
            Console.WriteLine("Scheduling shipping...");
            // Schedule shipping
            if (!ScheduleShipping(order, out string? shippingFailure))
            {
                return OrderResult.Failure(shippingFailure ?? "Shipping scheduling failed for unknown reasons");
            }

            // Customer Communication and Notifications
            Console.WriteLine("Sending notifications...");
            // Send notifications
            SendNotifications(order);

            // Order Finalization and Data Recording
            Console.WriteLine("Finalizing order...");
            order.Status = OrderStatus.Completed;
            order.CompletionDate = DateTime.UtcNow;
            order.ProcessingDuration = DateTime.UtcNow - order.OrderDate;

            // In a real app, this would update the order record in a database
            // _orderRepository.UpdateOrder(order);

            Console.WriteLine($"Order {order.Id} completed successfully in {order.ProcessingDuration.TotalSeconds:F1} seconds.");
            _auditLogger.LogOrderCompleted(order.Id, order.TotalAmount);

            // Finalize the order
            FinalizeOrder(order);

            return OrderResult.Success(order.Id, order.TrackingNumber ?? "");
        }
        catch (Exception ex)
        {
            HandleUnexpectedError(order, ex);
            return OrderResult.Failure("An unexpected error occurred during order processing");
        }
    }

    ```

GitHub Copilot 智能体擅长执行需要理解代码流、业务逻辑和错误处理模式的系统重构任务。 通过将重构分解为逻辑阶段，可以确保每个更改易于管理、可测试，并在显著改进代码组织和可维护性的同时保留原始系统行为。

### 测试重构后的电子商务订单处理代码

手动测试和验证可确保重构代码保留预期的业务逻辑和功能。 成功的重构过程应改进代码结构，同时产生与原始实现完全相同的行为。

在此任务中，你将测试重构后的代码以验证是否保留了所有业务逻辑，并且重构是否实现了改进可维护性和可读性的目标。

使用以下步骤完成此任务：

1. 运行重构后的应用程序并验证预期行为。

    将输出与重构之前观察到的行为进行比较。 控制台输出应完全相同，包括：

    - 测试 1（有效订单）****：应成功完成付款处理、发货安排和通知
    - 测试 2（无效电子邮件）：**** 应使用相同的错误消息使验证失败
    - 测试 3（已拒绝付款）：**** 应使付款处理失败并触发库存回退
    - 测试 4（可疑订单）：**** 应被安全评估标记并拒绝

    重构后的代码应产生完全相同的结果，表明业务逻辑在整个重构过程中得以保留。

1. 创建并测试其他边缘案例场景以确保可靠性。

    创建其他测试场景以验证错误处理在各种边缘案例中仍能正常工作。 你可以临时修改 Program.cs 中的测试用例以测试其他场景。****

    例如，你可以在显示测试摘要的代码之前添加以下代码片段：

    ```csharp
    // Test with empty items list (should fail validation)
    System.Console.WriteLine("\n--- Test Case 5: Empty Order ---");
    var emptyOrder = CreateSampleOrder("ORD-EMPTY", "test@example.com", "123 Test St",
        new List<OrderItem>(),
        new PaymentInfo { CardNumber = "4111111111111111", CardCVV = "123", CardHolderName = "Test User", ExpiryMonth = "12", ExpiryYear = "2025", BillingAddress = "123 Test St" });

    var result5 = processor.ProcessOrder(emptyOrder);
    testResults.Add($"Test 5: {(result5.IsSuccess ? "FAILED" : "PASSED")} - Should reject empty order");

    // Test with invalid shipping address (should fail validation)
    System.Console.WriteLine("\n--- Test Case 6: Invalid Shipping Address ---");
    var invalidAddressOrder = CreateSampleOrder("ORD-ADDR", "user@example.com", "",
        new List<OrderItem> { new() { ProductId = "BOOK-001", Quantity = 1, Price = 15.99m } },
        new PaymentInfo { CardNumber = "4111111111111111", CardCVV = "123", CardHolderName = "Test User", ExpiryMonth = "12", ExpiryYear = "2025", BillingAddress = "123 Test St" });

    var result6 = processor.ProcessOrder(invalidAddressOrder);
    testResults.Add($"Test 6: {(result6.IsSuccess ? "FAILED" : "PASSED")} - Should reject invalid shipping address");
    ```

    这些附加测试有助于验证重构后的验证逻辑是否正确处理边缘案例，并且错误消息是否与原始实现保持一致。

1. 运行应用程序并验证测试结果。

    运行应用程序时，你应该会在控制台中看到显示的测试结果，指示每个测试用例是通过还是失败。 请注意测试运行期间生成的任何错误消息或日志。

    例如，如果你添加了上面列出的测试 5 和测试 6 场景，新的测试输出和更新的摘要应类似于以下内容：

    ```text

    --- Test Case 5: Empty Order ---
    [AUDIT] 2025-08-20 18:28:11.099 UTC | ORDER_PROCESSING_STARTED | Order: ORD-EMPTY | Started processing order for t***@example.com
    [AUDIT] 2025-08-20 18:28:11.100 UTC | VALIDATION_FAILURE | Order: ORD-EMPTY | Empty order items
    
    --- Test Case 6: Invalid Shipping Address ---
    [AUDIT] 2025-08-20 18:28:11.101 UTC | ORDER_PROCESSING_STARTED | Order: ORD-ADDR | Started processing order for u***@example.com
    [AUDIT] 2025-08-20 18:28:11.101 UTC | VALIDATION_FAILURE | Order: ORD-ADDR | Invalid shipping address
    
    === TEST SUMMARY ===
    Test 1: PASSED - ORD-001
    Test 2: PASSED - Should reject invalid email
    Test 3: PASSED - Should reject declined payment
    Test 4: PASSED - Should flag suspicious order
    Test 5: PASSED - Should reject empty order
    Test 6: PASSED - Should reject invalid shipping address

    ```

1. 再次运行应用程序以验证审核日志是否仍能正常工作。

    检查 order_audit_log.txt 文件以确保审核日志在整个重构方法中仍能正常工作。**** 最新事件位于文件底部。

    审核线索应完整，并表明所有提取的方法中都保留了记录。

    > **提示**：order_audit_log.txt 文件是在应用程序的当前工作目录中创建/更新的。 根据你选择运行 ECommerce.Console 项目的方式，工作目录可能是“src/ECommerce.Console/bin/Debug/net9.0”目录，而不是“src/ECommerce.Console”目录。 若要在“src/ECommerce.Console”目录中生成审核文件，请使用 .NET CLI 命令从终端运行应用程序。

    以下事件类型可以存储在订单审核日志文件中：

    - 订单处理事件：
        - LogOrderProcessingStarted(string orderId, string email)
        - LogOrderCompleted(string orderId, decimal amount)
    - 安全事件：
        - LogSecurityEvent(string eventType, string details)
    - 验证事件：
        - LogValidationFailure(string orderId, string reason)
    - 库存事件：
        - LogInventoryIssue(string orderId, string productId, string issue)
        - LogInventoryReserved(string orderId, int itemCount)
    - 付款事件：
        - LogPaymentProcessed(string orderId, decimal amount, string reference)
        - LogPaymentFailure(string orderId, string reason)
    - 发货事件：
        - LogShippingScheduled(string orderId, string trackingNumber)
        - LogShippingFailure(string orderId, string reason)
    - 通知事件：
        - LogNotificationSent(string orderId, string type)
        - LogNotificationFailure(string orderId, string reason)
    - 意外错误：
        - LogUnexpectedError(string orderId, string error)

手动测试可验证你的重构工作是否成功实现了改善代码结构的目标，同时保持了系统功能的正常。 重构后的代码现在提供了一个更易于维护的基础，其中每个方法都有明确、集中的职责，从而显著简化了未来的增强和 bug 修复。

## 总结

在本练习中，你了解了如何使用 GitHub Copilot 重构应用程序中的大型函数。 你探索了电子商务订单处理系统，识别了需要重构的大型函数，并使用 GitHub Copilot 将整体式方法分解为更小、更集中的功能，以提高可维护性和可读性。

## 清理

现在，你已经完成了本练习，请花点时间确保你没有对 GitHub 帐户或 GitHub Copilot 订阅作出任何你不希望保留的更改。 如果你进行了任何更改，请根据需要还原。 如果你将本地电脑用作实验室环境，则可以存档或删除为此练习创建的示例项目文件夹。
