<!-- ---
lab:
    title: 'Exercise - Implement performance profiling using GitHub Copilot'
    description: 'Learn how to identify and address performance bottlenecks and code inefficiencies using GitHub Copilot tools.'
--- -->

# 使用 GitHub Copilot 实现性能分析

性能分析是软件开发的重要方面，可帮助识别和解决性能瓶颈和代码效率低下的问题。

在本练习中，你将针对代码性能不佳且低效的现有项目，分析用于提高代码性能的选项，重构代码以解决已识别的问题，并测试重构的代码，以确保代码在保留原有功能和可读性的同时性能得到改进。 你将在“询问”模式下使用 GitHub Copilot 来了解现有代码项目并深入了解有关已识别问题的选项。 在“代理”模式下使用 GitHub Copilot 重构代码并提高性能。 测试原来的代码和重构代码，以度量更改的影响。

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

1. 要下载包含示例应用项目的 zip 文件，请在浏览器中打开以下 URL：[GitHub Copilot 实验室 - 实现性能分析](https://github.com/MicrosoftLearning/mslearn-github-copilot-dev/raw/refs/heads/main/DownloadableCodeProjects/Downloads/GHCopilotEx10LabApps.zip)

    zip 文件名为 GHCopilotEx10LabApps.zip。****

1. 从 GHCopilotEx10LabApps.zip 文件中提取文件。****

    例如：

    1. 导航到实验室环境中的下载文件夹。

    1. 右键单击“GHCopilotEx10LabApps.zip”，然后选择“全部提取”。******

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

## 练习场景

你是一家咨询公司的软件开发人员。 你的客户需要相关帮助，在遗留应用程序中实现性能分析。 你的目标是提高代码性能，同时保留可读性和现有功能。 公司给你分配了以下应用：

- ContosoOnlineStore：这是一个电子商务应用程序，可处理具有现实中业务复杂性的客户订单。 该应用程序包含带搜索功能的产品目录管理、带库存预留的库存跟踪、带验证与收据的订单处理、电子邮件通知服务以及安全验证功能。 应用程序使用新式 .NET 体系结构模式，包括依赖项注入、结构化日志记录和配置管理，但包含反映真实场景情况的性能瓶颈。

本练习包括以下任务：

1. 手动查看 ContosoOnlineStore 代码库。
1. 使用 GitHub Copilot 对话助手（“询问”模式）确定性能瓶颈。
1. 使用 GitHub Copilot 对话助手重构性能关键型代码（“代理”模式）。
1. 测试和验证重构的 ContosoOnlineStore 代码。

### 手动查看 ContosoOnlineStore 代码库

所有代码重构任务的第一步都是了解现有代码库，包括项目结构和业务逻辑。 处理性能改进时，同样重要的一步是建立基线性能指标。

在此任务中，要检查 ContosoOnlineStore 项目的主要组件，运行应用程序以建立基线性能指标，并确定潜在的可优化区域。

使用以下步骤完成此任务：

1. 花几分钟时间查看 ContosoOnlineStore 项目结构。

    代码库遵循新式 .NET 体系结构模式，具有清晰的关注点分离设计。 主要体系结构组件包括：

    - 配置****：含验证功能的强类型配置
    - **服务**：具有实现可测试性的接口的业务服务  
    - **异常**：自定义的特定于域的异常
    - **基准**：使用 BenchmarkDotNet 进行专业性能测试
    - **测试**：使用模拟框架进行单元测试

1. 检查主要业务逻辑类。

    ************ 打开 ProductCatalog.cs、OrderProcessor.cs 和 InventoryManager.cs。 这些类包含核心业务逻辑，很可能适合进行性能优化。

    请注意产品数据列表（包含类别和说明的 20 个产品）、复杂的订单处理工作流，以及含库存预留功能的库存管理。

    > 注意****：代码库包含有助于识别性能问题的注释。 查找标记为“性能瓶颈”或“性能问题”的注释，这些注释突显了刻意设置的低效之处。

1. 查看服务层和配置。

    ************ 导航到“服务”文件夹并检查 EmailService.cs 和 SecurityValidationService.cs。 **** 同时查看 Configuration/AppSettings.cs 文件。

    你会注意到，这些服务实现了贴近实际的业务逻辑，包含可配置的超时机制、安全验证规则以及电子邮件通知工作流。 这些服务使用依赖项注入和日志记录，遵循企业开发模式。

1. 运行应用程序并观察基线性能。

    可以通过导航到项目文件夹并运行以下 .NET CLI 命令，从 Visual Studio Code 集成终端运行应用程序：

    ```bash
    dotnet run
    ```

    应用程序将执行全面的性能测试，其中包括：

    - 带有计时测量功能的订单处理
    - 产品目录操作（搜索、查找、类别筛选）
    - 库存管理操作
    - 并发操作测试
    - 电子邮件通知模拟

1. 将基线性能指标存储在名为 baseline_metrics.txt 的文件中。****

    创建名为 baseline_metrics.txt 的文本文件。 将控制台输出复制到 baseline_metrics.txt 文件中。

    请注意控制台输出中显示的计时信息，例如：

    - 订单处理时间（通常一个订单为 2000-3000 毫秒）
    - 产品查找性能（单个操作计时）
    - 搜索操作持续时间
    - 库存检查计时
    - 并发操作性能

    该应用程序还运行全面的性能分析套件，用于测试各种操作和报告计时详细信息。

1. 花一分钟时间检查 OrderProcessingBenchmarks.cs 文件提供的性能基准功能。

    该应用程序包括使用 BenchmarkDotNet 进行专业基准测试。 可通过执行以下项运行详细的性能基准：

    ```bash
    dotnet run -c Release -- benchmark
    ```

    这会生成详细的性能报告，包括内存分配模式和统计分析。

了解现有体系结构和基线性能指标是识别优化机会的基础。 该应用程序展示了贴近实际的电子商务性能挑战，你将在后续任务中解决这些挑战。

### 使用 GitHub Copilot 对话助手确定性能瓶颈（“询问”模式）

GitHub Copilot 对话助手的“询问”模式是分析复杂代码库和识别性能瓶颈的绝佳工具。 在“询问”模式下，Copilot 可以分析代码模式、识别低效的算法，并根据最佳做法建议优化策略。

在此任务中，你将使用 GitHub Copilot 系统地分析 ContosoOnlineStore 应用程序，并确定具体的性能改进机会。

使用以下步骤完成此任务：

1. 打开 GitHub Copilot 对话助手视图，然后配置“询问”模式和 GPT-4o 模型。

    **** 如果“对话助手”视图尚未打开，选择 Visual Studio Code 窗口顶部的“对话助手”图标。 ******** 确保对话助手模式设置为“询问”，且使用的是 GPT-4o 模型。

    > 注意****：GPT-4o 模型提供出色的代码分析功能，建议在此性能分析任务中使用。

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

    查看 GitHub Copilot 的分析，该分析应识别出类似于以下的问题：

    - 特定条件下 GetProductById 中的线性搜索性能问题。
    - SearchProducts 中的缓存密钥生成效率低下。
    - GetProductsByCategory 中缺少用于类别筛选的优化数据结构。
    - 多个方法中存在带有人工延迟的顺序处理。

1. 要求 GitHub Copilot 确定 OrderProcessor 类中的性能问题并提供优化建议。

    例如，提交以下提示：

    ```text
    Examine the OrderProcessor class, particularly the CalculateOrderTotal and FinalizeOrderAsync methods. What performance problems do you see and what optimization strategies would you recommend?
    ```

    GitHub Copilot 应当识别出以下问题：

    - 循环中的单个产品查询（N+1 查询模式）。
    - 冗余的税费和运费计算。
    - 订单项的顺序处理。
    - 可改为异步的阻塞操作。

1. 要求 GitHub Copilot 确定 InventoryManager 类中的性能问题并提供优化建议。

    例如，使用以下提示检查库存操作：

    ```text
    Review the InventoryManager class, especially the GetLowStockProducts and UpdateStockLevels methods. What are the performance concerns and how could the inventory operations be improved?
    ```

    该分析应揭示出：

    - 循环中的单个数据库查询模拟。
    - 存在阻塞操作的低效日志记录实现。
    - 缺少批处理作支持。
    - 库存级别检查中不必要的线程延迟。

1. 要求 GitHub Copilot 识别 EmailService 类中的性能问题并提供优化建议。

    例如，提交此提示以分析电子邮件服务：

    ```text
    Analyze the EmailService class for performance issues. How does the email sending process impact overall application performance and what improvements could be made?
    ```

    GitHub Copilot 应识别出：

    - 带有阻塞操作的电子邮件内容顺序生成。
    - 电子邮件模板中的单个产品查找。
    - 同步验证操作。
    - 针对多个接收者，存在并行化机会的缺失。

通过使用 GitHub Copilot 的分析功能，你已确定 ContosoOnlineStore 应用程序中的主要性能瓶颈。 该分析提供了优化工作的路线图，该路线图侧重于算法改进、缓存策略和异步处理模式。

### 使用 GitHub Copilot 对话助手（“代理”模式）重构性能关键型代码

GitHub Copilot 的代理模式提供一个自治代理，可帮助完成编程任务。 开发人员分配高级任务，然后启动代理代码编辑会话来完成该任务。 在代理模式下，Copilot 自主计划所需的工作，并确定相关的文件和上下文。 代理可以更改代码、运行测试，甚至部署应用程序。

在“代理”模式下，GitHub Copilot 可以生成优化的代码实现、建议体系结构改进并帮助实现性能增强功能。

在此任务中，你将使用 GitHub Copilot“代理”模式系统地解决上一任务中确定的性能瓶颈。

使用以下步骤完成此任务：

1. 配置 GitHub Copilot 对话助手的“代理”模式。

    ******** 在“对话助手”视图中，将模式从“请求”更改为“代理”。 “代理”模式提供更有针对性的代码生成和修改功能。

1. 将一个任务分配给可优化 ProductCatalog 类中 GetProductById 方法的代理。

    ******** 打开 ProductCatalog.cs 并选择 GetProductById 方法。 在对话助手中使用以下提示：

    ```text
    Optimize this GetProductById method to improve performance. Consider using a dictionary lookup instead of linear search and implement proper caching mechanisms. Retain any existing artificial/simulated delays for "before and after" performance comparisons.
    ```

    查看 GitHub Copilot 建议的改进并实现更改。 优化的版本应包括：

    - 基于字典的产品查询，可实现 O (1) 级别的性能。
    - 正确的缓存初始化和管理。
    - 冗余操作减少。

1. 将一个任务分配可提高 SearchProducts 方法效率的代理。

    ******** 在 ProductCatalog.cs 中选择 SearchProducts 方法，并使用此提示：

    ```text
    Refactor the SearchProducts method to eliminate performance bottlenecks. Optimize the search algorithm and remove unnecessary delays while maintaining search functionality. Retain any existing artificial/simulated delays for "before and after" performance comparisons.
    ```

    应用 GitHub Copilot 的建议来实现：

    - 高效的字符串匹配算法。
    - 多个搜索条件的并行处理。
    - 优化的缓存密钥生成。

1. 将一个任务分配给可提高 OrderProcessor 类中 CalculateOrderTotal 方法性能的代理。

    ******** 打开 OrderProcessor.cs 并选择 CalculateOrderTotal 方法。 提交此提示：

    ```text
    Optimize the CalculateOrderTotal method to reduce redundant product lookups and improve calculation performance. Consider batch operations and caching strategies. Retain any existing artificial/simulated delays for "before and after" performance comparisons.
    ```

    实现建议的改进，其中应包括：

    - 批处理产品检索，可消除 N+1 查询模式。
    - 订单处理过程中缓存的产品信息。
    - 优化的税费和运费计算。

1. 优化 FinalizeOrderAsync 方法。

    ******** 在 OrderProcessor.cs 中选择 FinalizeOrderAsync 方法，并使用此提示：

    ```text
    Refactor the FinalizeOrderAsync method to improve async performance. Focus on parallel processing where possible and optimizing await patterns. Retain any existing artificial/simulated delays for "before and after" performance comparisons.
    ```

    应用建议的更改来实现：

    - 独立操作的并行处理
    - 优化的 async/await 使用方式
    - 在异步上下文中更好地处理异常

1. 增强 InventoryManager 批处理操作。

    ******** 打开 InventoryManager.cs 并选择 UpdateStockLevels 方法。 使用此提示：

    ```text
    Optimize the UpdateStockLevels method to support batch operations and reduce individual update overhead. Implement efficient logging, but retain any existing artificial delays for performance comparison.
    ```

    实现改进，包括：

    - 批量库存级别更新
    - 高效日志记录策略
    - 阻塞操作减少

1. 改进 EmailService 异步处理。

    **** 打开 Services/EmailService.cs 并选择电子邮件发送方法。 提交此提示：

    ```text
    Optimize the email service to support parallel email processing and improve async performance. Consider implementing email queuing and batch operations. Retain any existing artificial/simulated delays for "before and after" performance comparisons.
    ```

    应用建议的优化以实现：

    - 并行的电子邮件内容生成
    - 异步电子邮件发送操作
    - 改进的错误处理和重试逻辑

在整个重构过程中，GitHub Copilot 代理模式充当你的协作伙伴，提供具体的代码改进和优化策略。 关键是仔细阅读每个建议，并进行所需调整，以满足特定要求和编码标准。

### 测试和验证重构的 ContosoOnlineStore 代码

实现性能优化后，很重要的一步是验证更改是否提高了性能同时维持了功能的正确性。 此任务侧重于全面的测试和性能测量。

使用以下步骤完成此任务：

1. 生成并测试重构的应用程序。

    在 Visual Studio Code 终端运行以下命令，以确保应用程序成功生成：

    ```bash
    dotnet build
    ```

    解决在重构过程中可能引入的任何编译错误。

1. 运行性能测试套件。

    执行应用程序以衡量性能改进：

    ```bash
    dotnet run
    ```

    将新的性能指标与第一个任务的基线测量进行比较。 应会观察到：

    - 订单处理时间大幅缩短（从 2000-3000 毫秒缩短到 500 毫秒）
    - 产品查找操作速度提升
    - 搜索性能提升
    - 库存管理响应时间缩短

1. 执行综合基准套件。

    运行详细的性能基准以获取精确的度量：

    ```bash
    dotnet run -c Release -- benchmark
    ```

    查看 BenchmarkDotNet 报告，其中包含了详细的统计信息，包括：

    - 带有置信区间的平均执行时间
    - 内存分配模式
    - 吞吐量度量值
    - 改进的统计学意义

1. 验证功能正确性。

    确保优化未引入功能回归：

    - 验证订单总计数据是否计算正确
    - 确认库存水平正确更新
    - 测试电子邮件通知是否成功发送
    - 验证安全验证是否仍然正常运行

1. 运行单元测试套件。

    执行现有单元测试以确保代码正确性：

    ```bash
    dotnet test
    ```

    所有测试均应通过，由此确认重构的代码维持了预期行为。

1. 记录性能改进。

    使用 GitHub Copilot 帮助记录更改。 在对话助手中提交此提示：

    ```text
    Help me create a summary of the performance optimizations implemented in this ContosoOnlineStore project. Include before/after metrics and the main optimization strategies used.
    ```

    创建性能改进报告，其中包括：

    - 基线指标对比优化的性能指标
    - 应用的关键优化技术
    - 改进最显著的领域
    - 有关未来增强功能的建议

测试和验证过程确认了已成功完成性能优化工作。 ContosoOnlineStore 应用程序当前体现了显著提高的性能，同时保持了其功能要求和体系结构的完整性。

通过本练习，你已了解如何将 GitHub Copilot 用作性能分析与优化的强大工具，这体现了 AI 辅助开发在应对复杂性能挑战方面的价值。

## 总结

在本练习中，你已成功使用 GitHub Copilot 识别和解决复杂电子商务应用程序中的性能瓶颈。 主要成果包括：

- **** 性能分析：使用了 GitHub Copilot“询问”模式系统地分析代码并确定瓶颈
- **** 战略优化：实施了针对性优化，解决了 N+1 查询模式、低效算法和阻塞操作等问题
- **** 协作重构：利用了 GitHub Copilot“代理”模式实现性能改进
- **验证**：通过全面测试确认了性能改进和功能正确性

优化的 ContosoOnlineStore 体现了显著的性能改进，同时维持了代码质量和体系结构最佳做法。 此方法展示了 AI 驱动的开发工具可以如何加速完成性能优化工作，并帮助开发人员对复杂的应用程序进行数据驱动的改进。
