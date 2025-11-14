---
lab:
  title: 练习 - 使用 GitHub Copilot 实现性能分析
  description: 了解如何使用 GitHub Copilot 工具识别和解决性能瓶颈和代码效率低下的问题。
---

# 使用 GitHub Copilot 实现性能分析

性能分析是软件开发的重要方面，可帮助识别和解决性能瓶颈和代码效率低下的问题。

在本练习中，你将针对代码性能不佳且低效的现有项目，分析用于提高代码性能的选项，重构代码以解决已识别的问题，并测试重构的代码，以确保代码在保留原有功能和可读性的同时性能得到改进。 你将在“询问”模式下使用 GitHub Copilot 来了解现有代码项目并深入了解有关已识别问题的选项。 在“代理”模式下使用 GitHub Copilot 重构代码并提高性能。 测试原来的代码和重构代码，以度量更改的影响。

完成此练习大约需要 30 分钟****。

> **重要说明**：要完成本练习，需要提供自己的 GitHub 帐户和 GitHub Copilot 订阅。 如果没有 GitHub 帐户，可以<a href="https://github.com/" target="_blank">注册</a>免费的个人帐户，并使用 GitHub Copilot 免费版计划来完成练习。 如果可以从实验室环境中访问 GitHub Copilot Pro、GitHub Copilot Pro+、GitHub Copilot Business 或 GitHub Copilot Enterprise 订阅，则可以使用你现有的 GitHub Copilot 订阅来完成本练习。

## 开始之前

实验室环境必须包括以下资源：Git 2.48 或更高版本、.NET SDK 9.0 或更高版本、具有 C# 开发工具包扩展的 Visual Studio Code，以及访问启用了 GitHub Copilot 的 GitHub 帐户。

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

1. 要下载包含示例应用项目的 zip 文件，请在浏览器中打开以下 URL：[GitHub Copilot 实验室 - 实现性能分析](https://github.com/MicrosoftLearning/mslearn-github-copilot-dev/raw/refs/heads/main/DownloadableCodeProjects/Downloads/GHCopilotEx10LabApps.zip)

    zip 文件名为 GHCopilotEx10LabApps.zip。****

1. 从 GHCopilotEx10LabApps.zip 文件中提取文件。****

    例如：

    1. 导航到实验室环境中的下载文件夹。

    1. 右键单击“GHCopilotEx10LabApps.zip”，然后选择“全部提取”。********

    1. 选择“完成时显示解压缩的文件”，然后选择“解压缩”。

1. 将 GHCopilotEx10LabApps 文件夹复制到易于访问的位置，例如 Windows 桌面文件夹。****

1. **** 在 Visual Studio Code 中打开 GHCopilotEx10LabApps 文件夹。

    例如：

    1. 在实验室环境中打开 Visual Studio Code。

    1. 在 Visual Studio Code 中的“文件”菜单上，选择“打开文件夹” 。

    1. 导航到 Windows 桌面文件夹，选择“GHCopilotEx10LabApps”，然后选择“选择文件夹”********。

1. 在 Visual Studio Code 的“解决方案资源管理器”视图中，验证以下项目结构：

    - GHCopilotEx10LabApps\
        - ContosoOnlineStore\
            - Benchmarks\
            - Configuration\
            - Exceptions\
            - Services\
            - appsettings.json
            - InventoryManager.cs
            - Orders.cs
            - OrderItem.cs
            - OrderProcessor.cs
            - PERFORMANCE_GUIDE.md
            - Product.cs
            - ProductCatalog.cs
            - Program.cs
            - README.md
        - ContosoOnlineStore.Tests\
            - ContosoOnlineStoreTests.cs
            - Usings.cs
        - DataAnalyzerReporter\
            - data.txt
            - DataAnalyzer.cs
            - FileLoader.cs
            - output.txt
            - Program.cs
            - README.md
            - ReportGenerator.cs

## 练习场景

你是一家咨询公司的软件开发人员。 你的客户需要在遗留应用程序中实现性能分析的相关帮助。 你的目标是提高代码性能，同时保留可读性和现有功能。 公司给你分配了以下应用：

- ContosoOnlineStore：ContosoOnlineStore 是一个可处理客户订单的电子商务应用程序。 该应用程序包含带搜索功能的产品目录管理、带库存预留的库存跟踪、带验证与收据的订单处理、电子邮件通知服务以及安全验证功能。 应用程序使用新式 .NET 体系结构模式，包括依赖项注入、结构化日志记录和配置管理，但包含反映真实场景情况的性能瓶颈。

> 注意****：代码瓶颈包括有意效率低下和性能问题，以及模拟延迟，这些延迟近似于外部依赖项的真实时间。 在重构代码以允许“之前和之后”性能比较时，应保留模拟延迟。

本练习包括以下任务：

1. 手动查看 ContosoOnlineStore 代码库。
1. 使用 GitHub Copilot 对话助手（“询问”模式）确定性能瓶颈。
1. 使用 GitHub Copilot 对话助手重构性能关键型代码（“代理”模式）。
1. 测试和验证重构的 ContosoOnlineStore 代码。

### 手动查看 ContosoOnlineStore 代码库

所有代码重构任务的第一步都是了解现有代码库，包括项目结构和业务逻辑。 处理性能改进时，同样重要的一步是建立基线性能指标。

在此任务中，要检查 ContosoOnlineStore 项目的主要组件，运行应用程序以建立基线性能指标，以及确定潜在的优化区域。

使用以下步骤完成此任务：

1. 花一分钟时间检查 ContosoOnlineStore 项目结构。

    代码库遵循新式 .NET 体系结构模式，具有清晰的关注点分离设计。 主要体系结构组件包括：

    - 配置****：含验证功能的强类型配置
    - **服务**：具有实现可测试性的接口的业务服务  
    - **异常**：自定义的特定于域的异常
    - **基准**：使用 BenchmarkDotNet 进行专业性能测试
    - **测试**：使用模拟框架进行单元测试

1. 花几分钟时间查看 ProductCatalog.cs、OrderProcessor.cs 和 InventoryManager.cs 类。************

    这些类包含核心业务逻辑，很可能适合进行性能优化。

    > 注意****：代码库包含有助于识别性能问题的注释。 查找标记为“性能瓶颈”或“性能问题”的注释，这些注释突显了刻意设置的低效之处。 还包含模拟延迟，以近似于外部依赖项（缓慢查询、网络调用等）的真实时间。 在重构代码库以允许“之前和之后”性能比较时，应保留这些延迟。

    - ProductCatalog.cs：**** ProductCatalog 类提供检索、搜索产品以及对产品进行分类，以及缓存搜索结果的方法。

    - OrderProcessor.cs：**** OrderProcessor 类处理订单验证、计算总额和订单确认，包括库存更新和电子邮件通知。

    - InventoryManager.cs：**** InventoryManager 类管理存货水平、预留和存货不足警报。

1. 展开“服务”和“配置”文件夹。********

    这些文件夹包含支持主要应用程序功能的额外业务逻辑和配置设置。

1. 花几分钟时间查看 Program.cs 和 AppSettings.cs 文件。********

    检查 Program.cs 和 AppSettings.cs 文件之间的关系。 请注意，Program.cs 文件初始化 AppSettings 配置并将其注入到应用程序的服务中，从而实现对应用程序行为的集中和灵活控制。 启动时会对应用程序配置进行强类型化和验证，以确保所有必需的设置都存在且已正确设置格式。

1. 花几分钟时间查看 EmailService.cs 和 SecurityValidationService.cs 文件。********

    检查这些服务的实施情况。 请注意，这些服务为业务逻辑提供了可配置的超时、安全验证规则以及电子邮件通知工作流。 这些服务使用依赖项注入和日志记录，遵循企业开发模式。

1. 运行应用程序并观察基线性能。

    可以通过导航到项目文件夹并运行以下 .NET CLI 命令，从 Visual Studio Code 集成终端运行应用程序：

    ```bash
    dotnet run
    ```

    应用程序将执行全面的性能测试，其中包括：

    - 带有计时测量功能的订单处理。
    - 产品目录操作（搜索、查找、类别筛选）。
    - 库存管理操作。
    - 并发操作测试。
    - 电子邮件通知模拟。

1. 将基线性能指标存储在名为 baseline_metrics.txt 的文件中。****

    使用 EXPLORER 视图在“基准”文件夹中创建名为 baseline_metrics.txt 的文本文件，然后将控制台输出复制到 baseline_metrics.txt 文件中。

1. 查看 baseline_metrics.txt 文件。

    注意“正在运行性能分析”部分下显示的计时信息。** 关键性能指标包括：

    - 产品查找性能。
    - 搜索性能。
    - 订单处理性能。
    - 库存操作性能。
    - 并发操作性能。

    该应用程序还运行全面的性能分析套件，用于测试各种操作和报告计时详细信息。

1. 花一分钟时间检查 OrderProcessingBenchmarks.cs 文件提供的性能基准功能。

    该应用程序包括使用 BenchmarkDotNet 进行专业基准测试。 可通过执行以下项运行详细的性能基准：

    ```bash
    dotnet run -c Release -- benchmark
    ```

    执行此命令将生成详细的性能报告，包括内存分配模式和统计分析。 如果需要，可以保存详细的性能基准报告以供以后比较。

了解现有体系结构和基线性能指标可为识别优化机会奠定基础。

### 使用 GitHub Copilot 对话助手确定性能瓶颈（“询问”模式）

GitHub Copilot 对话助手的“询问”模式是分析复杂代码库和识别性能瓶颈的绝佳工具。 在“询问”模式下，Copilot 可以分析代码模式、识别低效的算法，并根据最佳做法建议优化策略。

在此任务中，你将使用 GitHub Copilot 系统地分析 ContosoOnlineStore 应用程序，并确定具体的性能改进机会。

使用以下步骤完成此任务：

1. 打开“GitHub Copilot 对话助手”视图，然后配置“询问”模式和 GPT-4o 模型。********

    要打开“聊天”视图，请选择 Visual Studio Code 窗口顶部的“切换聊天”图标。****

    > 注意****：GPT-4o 模型提供出色的代码分析功能，并包含在 GitHub Copilot Free 计划中。 选择不同的模型可能会产生不同的结果。

1. 关闭已在编辑器中打开的所有文件。

    GitHub Copilot 使用编辑器中打开的文件来构建上下文。 仅打开目标文件有助于将分析重点放在要优化的代码上。

1. ************ 将 InventoryManager.cs、OrderProcessor.cs 和 ProductCatalog.cs 文件添加到“对话助手”上下文中。

    ************ 使用拖放操作将解决方案资源管理器中的 InventoryManager.cs、OrderProcessor.cs 和 ProductCatalog.cs 添加到“对话助手”上下文。

    将文件添加到对话助手上下文会告知 GitHub Copilot 在分析提示时包含这些文件，从而提高其分析的准确性和相关性。

1. 要求 GitHub Copilot 确定 ProductCatalog 类中的性能瓶颈并提供优化建议。

    例如，在“聊天”视图中输入以下提示：

    ```text
    Analyze the ProductCatalog class for performance bottlenecks. Focus on the GetProductById, SearchProducts, and GetProductsByCategory methods. What are the main inefficiencies and how could they be optimized?
    ```

1. 查看 GitHub Copilot 为 ProductCatalog 类生成的分析。

    分析应确定如下问题：

    - 特定条件下 GetProductById 中的线性搜索性能问题。
    - SearchProducts 中的缓存密钥生成效率低下。
    - GetProductsByCategory 中缺少用于类别筛选的优化数据结构。
    - 多个方法中存在带有人工延迟的顺序处理。

1. 要求 GitHub Copilot 评估建议的优化，以应对潜在风险。

    例如，在“聊天”视图中输入以下提示：

    ```text
    Do any of the suggested optimizations include security risks or introduce other adverse effects?
    ```

    > **重要说明**：盲目采用代码重构建议可能会带来安全风险和其他问题。 评估建议的优化并确定任何潜在问题非常重要。 例如，涉及缓存或并行处理的优化可能会引发线程安全性顾虑或数据一致性问题。 建议进行手动代码评审，以确保 AI 建议的优化不会危及安全性或功能。 定期安全评审和测试应是任何代码重构工作的一部分。

1. 查看 GitHub Copilot 生成的风险分析。

    风险分析应突出显示任何潜在的安全漏洞或与建议的优化相关的其他问题。 此信息可以帮助你在重构代码时做出明智决策，确定要实施的优化。

1. 要求 GitHub Copilot 确定 OrderProcessor 类中的性能问题并提供优化建议。

    例如，提交以下提示：

    ```text
    Examine the OrderProcessor class, particularly the CalculateOrderTotal and FinalizeOrderAsync methods. What performance problems do you see and what optimization strategies would you recommend?
    ```

1. 查看 GitHub Copilot 为 OrderProcessor 类生成的分析。

    分析应确定如下问题：

    - 循环中的单个产品查询（N+1 查询模式）。
    - 冗余的税费和运费计算。
    - 订单项的顺序处理。
    - 可改为异步的阻塞操作。

1. 要求 GitHub Copilot 评估建议的优化，以应对潜在风险。

    例如，在“聊天”视图中输入以下提示：

    ```text
    Do any of the suggested optimizations include security risks or introduce other adverse effects?
    ```

1. 查看 GitHub Copilot 生成的风险分析。

1. 要求 GitHub Copilot 确定 InventoryManager 类中的性能问题并提供优化建议。

    例如，使用以下提示检查库存操作：

    ```text
    Review the InventoryManager class, especially the GetLowStockProducts and UpdateStockLevels methods. What are the performance concerns and how could the inventory operations be improved?
    ```

1. 查看 GitHub Copilot 为 InventoryManager 类生成的分析。

    分析应确定如下问题：

    - 循环中的单个数据库查询模拟。
    - 存在阻塞操作的低效日志记录实现。
    - 缺少批处理作支持。
    - 库存级别检查中不必要的线程延迟。

1. 要求 GitHub Copilot 评估建议的优化，以应对潜在风险。

    例如，在“聊天”视图中输入以下提示：

    ```text
    Do any of the suggested optimizations include security risks or introduce other adverse effects?
    ```

1. 查看 GitHub Copilot 生成的风险分析。

1. 要求 GitHub Copilot 识别 EmailService 类中的性能问题并提供优化建议。

    例如，提交此提示以分析电子邮件服务：

    ```text
    Analyze the EmailService class for performance issues. How does the email sending process impact overall application performance and what improvements could be made?
    ```

1. 查看 GitHub Copilot 为 EmailService 类生成的分析。

    分析应确定如下问题：

    - 带有阻塞操作的电子邮件内容顺序生成。
    - 电子邮件模板中的单个产品查找。
    - 同步验证操作。
    - 针对多个接收者，存在并行化机会的缺失。

1. 要求 GitHub Copilot 评估建议的优化，以应对潜在风险。

    例如，在“聊天”视图中输入以下提示：

    ```text
    Do any of the suggested optimizations include security risks or introduce other adverse effects?
    ```

1. 查看 GitHub Copilot 生成的风险分析。

通过使用 GitHub Copilot 的分析功能，你已确定 ContosoOnlineStore 应用程序中的性能瓶颈。 该分析提供了优化工作的路线图，该路线图侧重于算法改进、缓存策略和异步处理模式。 分析 AI 建议的代码优化有助于识别与潜在性能改进相关的风险。 手动代码评审、安全评审和测试应是任何代码重构工作的一部分。

### 使用 GitHub Copilot 对话助手（“代理”模式）重构性能关键型代码

GitHub Copilot 的代理模式提供一个自治代理，可帮助完成编程任务。 开发人员将高级任务分配给智能体，然后启动智能体代码编辑会话来完成任务。 GitHub Copilot 智能体自主评估所需的工作，确定相关的文件和上下文，并计划如何完成任务。 代理可以更改代码、运行测试，甚至部署应用程序。

在“代理”模式下，GitHub Copilot 可以生成优化的代码实现、建议体系结构改进并帮助实现性能增强功能。

在此任务中，你将使用 GitHub Copilot“智能体”模式系统地解决上一任务中确定的性能瓶颈。

使用以下步骤完成此任务：

1. 配置 GitHub Copilot 对话助手的“代理”模式。

    ******** 在“对话助手”视图中，将模式从“请求”更改为“代理”。 “代理”模式提供更有针对性的代码生成和修改功能。

1. 打开 ProductCatalog.cs 文件，然后选择 GetProductById 方法。********

1. 将一个任务分配给可优化 GetProductById 方法的智能体。

    例如，在“聊天”视图中输入以下任务：

    ```text
    Review the current chat session. Optimize the GetProductById method to improve performance. Consider using a dictionary lookup instead of linear search and implement proper caching mechanisms. Retain any existing artificial/simulated delays for "before and after" performance comparisons. Ensure that the refactored code doesn't introduce security vulnerabilities or other issues.
    ```

1. 花一分钟时间查看 GitHub Copilot 建议的编辑，然后接受这些更改。

    优化的版本应包括：

    - 基于字典的产品查询，可实现 O (1) 级别的性能。
    - 正确的缓存初始化和管理。
    - 冗余操作减少。

    可以在代码编辑器中查看和接受（或拒绝）单个编辑，也可以通过在“聊天”视图中选择“保持”一次性接受所有更改。****

    > 注意****：完成本练习的这一部分时，请考虑在上一任务中识别的安全漏洞和其他问题。 开发人员应确保在优化过程中不会引入任何新的漏洞。 在生产环境中，手动代码评审、安全评审和测试应是这个过程的一部分。

1. 在代码编辑器中，选择 SearchProducts 方法。****

1. 将一个任务分配可提高 SearchProducts 方法效率的代理。

    例如，在“聊天”视图中输入以下任务：

    ```text
    Review the current chat session. Refactor the SearchProducts method to eliminate performance bottlenecks. Optimize the search algorithm while maintaining search functionality. Retain any existing artificial/simulated delays for "before and after" performance comparisons. Ensure that the refactored code doesn't introduce security vulnerabilities or other issues.
    ```

1. 花一分钟时间查看 GitHub Copilot 建议的编辑，然后接受这些更改。

    优化的版本应包括：

    - 高效的字符串匹配算法。
    - 多个搜索条件的并行处理。
    - 优化的缓存密钥生成。

1. 保存并关闭 ProductCatalog.cs 文件。****

1. 打开 OrderProcessor.cs 文件，然后选择 CalculateOrderTotal 方法。********

1. 将一个任务分配给可提高 CalculateOrderTotal 方法性能的智能体。

    例如，在“聊天”视图中输入以下任务：

    ```text
    Review the current chat session. Optimize the CalculateOrderTotal method to reduce redundant product lookups and improve calculation performance. Consider batch operations and caching strategies. Retain any existing artificial/simulated delays for "before and after" performance comparisons. Ensure that the refactored code doesn't introduce security vulnerabilities or other issues.
    ```

1. 花一分钟时间查看 GitHub Copilot 建议的编辑，然后接受这些更改。

    优化的版本应包括：

    - 批处理产品检索，可消除 N+1 查询模式。
    - 订单处理过程中缓存的产品信息。
    - 优化的税费和运费计算。

1. 在代码编辑器中，选择 FinalizeOrderAsync 方法。****

1. 将一个任务分配给可提高 FinalizeOrderAsync 方法性能的智能体。

    例如，在“聊天”视图中输入以下任务：

    ```text
    Review the current chat session. Refactor the FinalizeOrderAsync method to improve async performance. Focus on parallel processing where possible and optimizing await patterns. Retain any existing artificial/simulated delays for "before and after" performance comparisons. Ensure that the refactored code doesn't introduce security vulnerabilities or other issues.
    ```

1. 花一分钟时间查看 GitHub Copilot 建议的编辑，然后接受这些更改。

    优化的版本应包括：

    - 独立操作的并行处理
    - 优化的 async/await 使用方式
    - 在异步上下文中更好地处理异常

1. 保存并关闭 OrderProcessor.cs 文件。****

1. 打开 InventoryManager.cs 文件，然后选择 UpdateStockLevels 方法。********

1. 将一个任务分配给可提高 UpdateStockLevels 方法性能的智能体。

    例如，在“聊天”视图中输入以下任务：

    ```text
    Review the current chat session. Optimize the UpdateStockLevels method to support batch operations and reduce individual update overhead. Implement efficient logging, but retain any existing artificial delays for performance comparison. Ensure that the refactored code doesn't introduce security vulnerabilities or other issues.
    ```

1. 花一分钟时间查看 GitHub Copilot 建议的编辑，然后接受这些更改。

    优化的版本应包括：

    - 批量库存级别更新
    - 高效日志记录策略
    - 阻塞操作减少

1. 保存并关闭 OrderProcessor.cs 文件。****

1. 打开 EmailService.cs 文件。****

1. 将一个任务分配给可提高电子邮件发送方法性能的智能体。

1. 改进 EmailService 异步处理。

    例如，在“聊天”视图中输入以下任务：

    ```text
    Review the current chat session. Optimize the email service methods to support parallel email processing and improve async performance. Consider implementing email queuing and batch operations. Retain any existing artificial/simulated delays for "before and after" performance comparisons. Ensure that the refactored code doesn't introduce security vulnerabilities or other issues.
    ```

1. 花一分钟时间查看 GitHub Copilot 建议的编辑，然后接受这些更改。

    优化的版本应包括：

    - 并行的电子邮件内容生成
    - 异步电子邮件发送操作
    - 改进的错误处理和重试逻辑

在整个重构过程中，GitHub Copilot 代理模式充当你的协作伙伴，提供具体的代码改进和优化策略。 关键是仔细阅读每个建议，并进行所需调整，以满足特定要求和编码标准。

### 测试和验证重构的 ContosoOnlineStore 代码

实现性能优化后，很重要的一步是验证更改是否提高了性能同时维持了功能的正确性。 此任务侧重于全面的测试和性能测量。

使用以下步骤完成此任务：

1. 生成并运行重构的应用程序。

    在 Visual Studio Code 终端中运行以下命令，确保成功生成应用程序：

    ```bash
    dotnet build
    ```

    解决在重构过程中可能已引入的任何编译错误。

1. 运行性能测试套件。

    在 Visual Studio Code 终端中运行以下命令，为重构的代码生成性能指标：

    ```bash
    dotnet run
    ```

1. 将新的性能指标保存在名为 optimized_metrics.txt 的文件中。****

    使用 EXPLORER 视图在“基准”文件夹中创建名为 optimized_metrics.txt 的文本文件，然后将控制台输出复制到 optimized_metrics.txt 文件中。

1. 花一分钟时间手动比较优化的性能指标与第一个任务中的基线度量值。

    应会看到以下性能改进：

    - 显著加快产品查找操作的速度。
    - 提升了搜索性能。
    - 减少了订单处理时间。
    - 提高了并发操作的性能。

    验证重构代码的功能正确性。

    - 验证订单总额是否计算正确。
    - 确认库存水平正确更新。
    - 验证电子邮件通知是否成功发送。
    - 验证安全验证是否仍然正常运行。

1. 生成并运行单元测试项目。

    例如，在 Visual Studio Code 终端中，导航到 ContosoOnlineStore.Tests 文件夹并运行以下命令：****

    ```bash
    dotnet test
    ```

    所有测试均应通过，由此确认重构的代码维持了预期行为。

    输出应如下所示：

    ```plaintext
    Restore complete (0.6s)
      ContosoOnlineStore succeeded (0.6s) → C:\Users\cahowd\Desktop\GHCopilotEx10LabApps\ContosoOnlineStore\bin\Debug\net9.0\ContosoOnlineStore.dll
      ContosoOnlineStore.Tests succeeded (0.2s) → bin\Debug\net9.0\ContosoOnlineStore.Tests.dll
    [xUnit.net 00:00:00.00] xUnit.net VSTest Adapter v2.4.5+1caef2f33e (64-bit .NET 9.0.9)
    [xUnit.net 00:00:02.94]   Discovering: ContosoOnlineStore.Tests
    [xUnit.net 00:00:03.02]   Discovered:  ContosoOnlineStore.Tests
    [xUnit.net 00:00:03.02]   Starting:    ContosoOnlineStore.Tests
    [xUnit.net 00:00:03.18]   Finished:    ContosoOnlineStore.Tests
      ContosoOnlineStore.Tests test succeeded (4.7s)
    
    Test summary: total: 16, failed: 0, succeeded: 16, skipped: 0, duration: 4.7s
    Build succeeded in 7.0s
    ```

1. 请求 GitHub Copilot 帮助分析性能改进。

    例如，在“聊天”视图中输入以下提示：

    ```text
    Compare the baseline_metrics.txt and optimized_metrics.txt files. Summarize the performance improvements achieved through the optimizations. Review the codebase and calculate the time associated with simulated delays for each performance test. Subtract the time associated with simulated delays from the performance data and summarize the impact of code optimizations.
    ```

1. 花一分钟时间查看 GitHub Copilot 生成的（性能改进）分析。

    分析应突出通过优化实现的特定性能改进，不包括模拟延迟的影响。 这样可以更直观地展示代码更改的有效性。

1. 可选 - 如果在本练习开始时运行了详细的性能基准套件并保存了结果，则可以再次运行详细的性能基准，并让 GitHub Copilot 比较结果。

    要运行详细的性能基准，请在 Visual Studio Code 终端中执行以下命令：

    ```bash
    dotnet run -c Release -- benchmark
    ```

    请求 GitHub Copilot 帮助查看 BenchmarkDotNet 报告。

测试和验证过程确认了已成功完成性能优化工作。 ContosoOnlineStore 应用程序当前展示了改进的性能，同时保持了其功能要求和体系结构的完整性。

## 总结

在本练习中，你已成功使用 GitHub Copilot 识别和解决复杂电子商务应用程序中的性能瓶颈。 主要成果包括：

- **** 性能分析：使用了 GitHub Copilot“询问”模式系统地分析代码并确定瓶颈
- **** 战略优化：实施了针对性优化，解决了 N+1 查询模式、低效算法和阻塞操作等问题
- **** 协作重构：利用了 GitHub Copilot“代理”模式实现性能改进
- **验证**：通过全面测试确认了性能改进和功能正确性

优化的 ContosoOnlineStore 体现了显著的性能改进，同时维持了代码质量和体系结构最佳做法。 此方法展示了 AI 驱动的开发工具可以如何加速完成性能优化工作，并帮助开发人员对复杂的应用程序进行数据驱动的改进。

## 清理

现在，你已经完成了本练习，请花点时间确保你没有对 GitHub 帐户或 GitHub Copilot 订阅作出任何你不希望保留的更改。 如果你进行了任何更改，请根据需要还原。 如果你将本地电脑用作实验室环境，则可以存档或删除为此练习创建的示例项目文件夹。
