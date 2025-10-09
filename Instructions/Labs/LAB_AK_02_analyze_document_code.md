---
lab:
  title: 练习 - 使用 GitHub Copilot 分析和记录代码
  description: 了解如何在 Visual Studio Code 中使用 GitHub Copilot 分析新的或不熟悉的代码，以及如何生成文档。
---

# 使用 GitHub Copilot 分析和记录代码

GitHub Copilot 可通过生成解释和文档来帮助你了解和记录代码库。 在本练习中，你将使用 GitHub Copilot 分析代码库，并为项目生成文档。

完成此练习大约需要 **20** 分钟。

> **重要说明**：若要完成本练习，必须提供自己的 GitHub 帐户和 GitHub Copilot 订阅。 如果没有 GitHub 帐户，可以<a href="https://github.com/" target="_blank">注册</a>免费的个人帐户，并使用 GitHub Copilot 免费版计划来完成练习。 如果可以从实验室环境中访问 GitHub Copilot Pro、GitHub Copilot Pro+、GitHub Copilot Business 或 GitHub Copilot Enterprise 订阅，则可以使用你现有的 GitHub Copilot 订阅来完成本练习。

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

## 练习场景

你是一名在当地社区 IT 部门工作的开发人员。 支持公共图书馆的后端系统在火灾中丢失。 你的团队需要开发临时解决方案，以帮助图书馆员工管理其操作，直到可以更换系统。 你的团队已选择使用 GitHub Copilot 来加速开发流程。

你的同事开发了图书馆应用程序的初始版本，但由于时间有限，他们没有机会记录代码。 你需要分析代码库并为项目创建文档。

本练习包括以下任务：

- 在 Visual Studio Code 中设置图书馆应用程序。
- 使用 GitHub Copilot 解释图书馆应用程序代码库。
- 使用 GitHub Copilot 为图书馆应用程序创建 README.md 文件。

## 在 Visual Studio Code 中设置图书馆应用程序

你的同事开发了图书馆应用程序的初始版本，并以 .zip 文件的形式提供。 你需要下载 zip 文件、提取代码文件，然后在 Visual Studio Code 中打开解决方案。

使用以下步骤设置图书馆应用程序：

1. 在实验室环境中打开浏览器窗口。

1. 要下载包含图书馆应用程序的 zip 文件，请将以下 URL 粘贴到浏览器的地址栏中：[GitHub Copilot 实验室 - 分析和记录代码](https://github.com/MicrosoftLearning/mslearn-github-copilot-dev/raw/refs/heads/main/DownloadableCodeProjects/Downloads/AZ2007LabAppM2.zip)

    名为 AZ2007LabAppM2.zip 的 zip 文件将下载到你的实验室环境中。

1. 从 AZ2007LabAppM2.zip 文件中解压缩文件。****

    例如：

    1. 导航到实验室环境中的下载文件夹。

    1. 右键单击“AZ2007LabAppM2.zip”，然后选择“全部解压”********。

    1. 选择“完成时显示解压缩的文件”，然后选择“解压缩”。

1. 打开已解压缩的文件所在的文件夹，然后将 AccelerateDevGHCopilot 文件夹复制到易于访问的位置，例如 Windows 桌面文件夹。****

1. 在 Visual Studio Code 中打开 AccelerateDevGHCopilot 文件夹****。

    例如：

    1. 在实验室环境中打开 Visual Studio Code。

    1. 在 Visual Studio Code 中的“文件”菜单上，选择“打开文件夹” 。

    1. 导航到 Windows 桌面文件夹，选择“AccelerateDevGHCopilot”，然后选择“选择文件夹”********。

1. 在 Visual Studio Code 的“解决方案资源管理器”视图中，展开解决方案以显示以下解决方案结构：

    - AccelerateDevGHCopilot\
        - src\
            - Library.ApplicationCore\
            - Library.Console\
            - Library.Infrastructure\
        - tests\
            - UnitTests\

1. 确保解决方案成功生成。

    例如，在“解决方案资源管理器”视图中，右键单击“AccelerateDevGHCopilot”，然后选择“生成”********。

    你会看到一些警告，但不应该有任何错误。

## 使用 GitHub Copilot 解释图书馆应用程序代码库

GitHub Copilot 可以通过在解决方案、文件和代码行级别生成解释来帮助你理解不熟悉的代码库。

### 在“聊天”视图中使用提示分析代码

GitHub Copilot 的“聊天”视图包含一个基于聊天的界面，使你能使用自然语言提示与 GitHub Copilot 交互。 首次评估现有代码库时，你可以创建提示，以在工作区或项目级别或在代码块或代码行级别生成解释。 为了帮助你指定提示的上下文，GitHub Copilot 提供了聊天参与者、聊天变量和 / 命令。

- 聊天参与者用于将提示限定到特定域。
- 聊天变量用于在提示中包含特定上下文。
- 而 / 命令用于避免为常见场景编写复杂的提示。

请使用以下步骤完成本练习的这一部分：

1. 确保在 Visual Studio Code 中打开了 AccelerateDevGHCopilot 解决方案。

1. 打开 GitHub Copilot 的“聊天”视图。

    要打开“聊天”视图，请选择 Visual Studio Code 窗口顶部的“切换聊天”按钮。****

    ![显示 GitHub Copilot 状态菜单的屏幕截图。](./Media/m02-github-copilot-toggle-chat.png)

    你还可以使用 Ctrl+Alt+I 键盘快捷方式打开“聊天”视图。****

1. 在“聊天”视图中，输入一个使用 GitHub Copilot 的 @workspace 聊天参与者的提示，以生成项目描述****。

    例如，在“聊天”视图中输入以下提示：

    ```plaintext
    @workspace describe this project
    ```

    使用聊天参与者（例如 @workspace）来改进 GitHub Copilot 生成的响应****。 聊天参与者就像领域专家，可以在其专业领域为你提供帮助。 当你希望 GitHub Copilot 考虑项目结构、代码的不同部分如何交互或项目中的设计模式时，请使用 @workspace****。

    要查看所有可用聊天参与者的列表，请在聊天提示框中键入 @****。

1. 花点时间将 GitHub Copilot 的响应与实际项目文件进行比较。

    你应该会看到一个描述解决方案中每个项目的响应：

    - Library.ApplicationCore****
    - Library.Console****
    - Library.Infrastructure****
    - **UnitTests**

1. 使用“解决方案资源管理器”视图展开项目文件夹。

1. 找到并打开 ConsoleApp.cs**** 文件。

    ConsoleApp.cs 文件位于 src/Library.Console**** 文件夹中。

1. 花点时间查看该代码文件。

1. 在“聊天”视图中输入一个提示，生成 ConsoleApp**** 类的说明。

    例如，在“聊天”视图中输入以下提示：

    ```plaintext
    @workspace #usages How is the ConsoleApp class used?
    ```

    使用聊天变量（例如 #usages）在提示中包含特定上下文。**** 要查看聊天变量列表，请在聊天提示框中键入 #****。

    > 注意****：GitHub Copilot 在为提示构建上下文并生成响应时会考虑你的聊天历史记录和你在 Visual Studio Code 中打开的代码文件。

1. 花点时间验证 GitHub Copilot 响应的准确性。

    你应该会看到一个响应，描述 ConsoleApp 类的定义位置及其在代码库中的使用方式。**** 响应中引用了 ConsoleApp.cs 和 Program.cs 文件以及行号

1. 打开 Program.cs**** 文件并检查代码。

1. 在“聊天”视图中输入一个提示，生成对 Program.cs 文件的解释。

    例如，在“聊天”视图中输入以下提示：

    ```plaintext
    @workspace /explain Explain the Program.cs file
    ```

    使用 / 命令（例如 /explain****）避免为常见场景编写复杂的提示。 要查看所有可用 / 命令的列表，请在聊天提示框中键入 **/**。 可用的斜杠命令可能会有所不同，具体取决于你的环境和聊天上下文。

1. 花点时间查看 GitHub Copilot 生成的详细响应。

    你应该会看到一个响应，其中包含概述和明细，解释了文件在应用程序中的使用方式。

1. 关闭 Program.cs 文件。

### 添加上下文以改进聊天响应

GitHub Copilot 使用上下文来生成更相关的响应。

在代码编辑器中打开文件是建立上下文的一种方式，但你也可以使用拖放操作或使用“聊天”视图中的“附加上下文”按钮将文件添加到聊天上下文中****。

请使用以下步骤完成本练习的这一部分：

1. 展开 Library.Infrastructure 项目，然后展开“数据”文件夹********。

1. 使用拖放操作将解决方案资源管理器视图中的以下文件添加到“聊天”上下文：JsonData.cs****、JsonLoanRepository.cs**** 和 JsonPatronRepository.cs****。

    GitHub Copilot 使用聊天上下文来了解与你的提示相关的代码文件。 你可以使用拖放操作将文件添加到聊天上下文中，也可以使用“聊天”视图中的“附加上下文”**** 按钮。

    你可以让 Copilot 自动从代码库中查找正确的文件，而不必手动添加单个文件。 当你不知道哪些文件与你的问题相关时，此方法非常有用。

    要让 Copilot 自动查找正确的文件，请在提示中添加 #codebase 或从上下文类型列表中选择“代码库”。

1. 在“聊天”视图中输入一个提示，以生成数据访问类的说明。

    例如，在“聊天”视图中输入以下提示：

    ```plaintext
    @workspace /explain Explain how the data access classes work
    ```

1. 花几分钟时间阅读响应。

    你应该会看到一个响应，描述每个数据访问类（JsonData、JsonLoanRepository 和JsonPatronRepository）以及它们如何协同工作以管理应用程序中的数据访问。************ 响应中应提及关键方法，例如 LoadData、SaveLoans 和 SavePatrons。************

1. 花点时间检查用于模拟图书馆记录的 JSON 数据文件。

    JSON 数据文件位于 src/Library.Console/Json**** 文件夹中。

    数据文件使用 ID 属性来链接实体。 例如，Loan 对象有一个 PatronId 属性，该属性链接到具有相同 ID 的 Patron 对象。************ JSON 文件包含作者、书籍、书籍项、借阅者和借阅记录的数据。

    > 注意****：请注意，出于本培训的目的，作者姓名、书名和读者姓名都已匿名化。

### 生成并运行应用程序

运行应用程序有助于你了解用户界面、应用程序的关键功能以及应用组件的交互方式。

请使用以下步骤完成本练习的这一部分：

1. 确保已打开“解决方案资源管理器”视图****。

    “解决方案资源管理器”视图与“资源管理器”视图不同。 “解决方案资源管理器”视图使用项目和解决方案文件作为“目录”节点来显示解决方案的结构。

1. 若要运行该应用程序，请右键单击“Library.Console”，选择“调试”，然后选择“启动新实例”************。

    如果未显示“调试”和“启动新实例”选项，请确保使用的是“解决方案资源管理器”视图，而不是“资源管理器”视图********。

    以下步骤将引导你完成一个简单的用例。

1. 当系统提示你输入借阅者姓名时，请键入 1，然后按 Enter****。

    你应会看到与搜索查询匹配的借阅者列表。

    > 注意****：该应用程序使用区分大小写的搜索过程。

1. 在“输入选项”提示处键入 2，然后按 Enter****。

    输入 2 会选择列表中的第二位借阅者****。

    你应会看到借阅者的姓名和会员状态，以及书籍的借阅详细信息。

1. 在“输入选项”提示处键入 1，然后按 Enter****。

    输入 1 会选择列表中的第一本书****。

    你应会看到列出的书籍详细信息，包括到期日期和归还状态。

1. 在“输入选项”提示处键入 r，然后按 Enter****。

    输入 r 将归还书籍****。

1. 确认是否显示“书籍已成功归还。” 的消息。

    “书籍已成功归还。”的消息 后面应该是书籍详细信息。 归还的书籍标记为 **Returned: True**。

1. 若要开始新的搜索，请键入 s，然后按 Enter****。

1. 当系统提示你输入借阅者姓名时，请键入 1，然后按 Enter****。

1. 在“输入选项”提示处键入 2，然后按 Enter****。

1. 确认第一本书籍的借阅记录是否标记为 **Returned: True**。

1. 在“输入选项”提示处键入 q，然后按 Enter****。

1. 停止调试会话。

## 为 README 文件创建项目文档

自述文件为项目参与者和利益相关者提供有关代码存储库的重要信息。 它们帮助用户了解项目的目的、使用方法以及参与方式。 结构良好的 README 文件可以显著提高项目的可用性和可维护性。

你需要一个 README 文件，其中包含以下部分：

- **项目标题**：简明扼要的项目标题。
- **说明**：项目的详细介绍及用途说明。
- **项目结构**：对项目结构的分析，包括关键文件夹和文件。
- **关键类和接口**：项目中关键类和接口的列表。
- **用法**：有关如何使用项目的说明，通常包括代码示例。
- **许可证**：项目所使用的许可证。

在本练习的这一部分中，你将使用 GitHub Copilot 创建项目文档，并将其添加到 README.md 文件中。****

请使用以下步骤完成本练习的这一部分：

1. 向 AccelerateDevGHCopilot 解决方案的根文件夹中添加一个名为 README.md 的新文件。********

1. 打开“聊天”视图。

1. 若要为 README 文件生成项目文档，请输入以下提示：

    ```plaintext

    @workspace Generate the contents of a README.md file for a code repository. Use "Library App" as the project title. The README file should include the following sections: Description, Project Structure, Key Classes and Interfaces, Usage, License. Format all sections as raw markdown. Use a bullet list with indents to represent the project structure. Do not include ".gitignore" or the ".github", "bin", and "obj" folders.

    ```

    > 注意****：使用多个提示，README 文件的每个部分分别对应一个提示，可以生成更详细的结果。 为了简化过程，本练习只使用一个提示。

1. 查看答复，确保每个部分的格式都设置为 Markdown。

    可以单独更新各个部分，以提供更详细的信息，或者在格式不正确时进行更正。 也可以将 GitHub Copilot 的答复复制到 README 文件中，然后直接在 Markdown 文件中进行更正。

1. 复制建议的文档，然后将其粘贴到 README.md 文件中。

    要复制整个响应，请滚动到响应底部，在“点赞”图标右侧的空白处右键单击，然后选择“复制”****

1. 根据需要设置 README.md 文件的格式。

    你可以更新 GitHub Copilot 的设置以启用 Markdown 格式设置，然后使用 GitHub Copilot 帮助你更新 README.md 文件的各个部分。

    完成时，你应该有一个 README.md 文件，其中包含每个指定部分，并且具有适当的细节层次。

## 总结

在本练习中，你了解了如何使用 GitHub Copilot 分析和记录代码库。 你使用 GitHub Copilot 生成项目结构、关键类和数据访问类的说明。 你还使用 GitHub Copilot 为项目创建了 README.md 文件。

## 清理

现在，你已经完成了本练习，请花点时间确保你没有对 GitHub 帐户或 GitHub Copilot 订阅作出任何你不希望保留的更改。 如果你进行了任何更改，请立即还原。
