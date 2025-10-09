---
lab:
  title: 练习 - 使用 GitHub Copilot 重构现有代码 (Python)
  description: 了解如何在 Visual Studio Code 中使用 GitHub Copilot 重构和改进现有代码片段。
---

# 使用 GitHub Copilot 重构现有代码

GitHub Copilot 可用于评估整个代码库，并建议有助于重构和改进代码的更新。 在本练习中，将使用 GitHub Copilot 重构 Python 应用程序的指定部分，同时提升代码质量、可靠性、性能和安全性。

完成此练习大约需要 30 分钟****。

> **重要说明**：要完成本练习，需要提供自己的 GitHub 帐户和 GitHub Copilot 订阅。 如果没有 GitHub 帐户，可以<a href="https://go.microsoft.com/fwlink/?linkid=2320148" target="_blank">注册</a>免费的个人帐户，并使用 GitHub Copilot 免费版计划来完成练习。 如果可从实验室环境中访问 GitHub Copilot Pro、GitHub Copilot Pro+、GitHub Copilot Business 或 GitHub Copilot Enterprise 订阅，则可以使用现有的 GitHub Copilot 订阅完成本练习。

## 开始之前

实验室环境必须包括以下内容：Git 2.48 或更高版本、Python 3.10 或更高版本、带有 Microsoft Python 扩展的 Visual Studio Code，以及启用了 GitHub Copilot 的 GitHub 帐户访问权限。

如果你将本地电脑用作本练习的实验室环境：

- 有关将本地电脑配置为实验室环境的帮助，请在浏览器中打开以下链接：<a href="https://microsoftlearning.github.io/mslearn-github-copilot-dev/Instructions/Labs/LAB_AK_00_configure_lab_environment_py.html" target="_blank">配置实验室环境资源</a>。

- 有关在 Visual Studio Code 中启用 GitHub Copilot 订阅的帮助，请在浏览器中打开以下链接：<a href="https://go.microsoft.com/fwlink/?linkid=2320158" target="_blank">在 Visual Studio Code 中启用 GitHub Copilot</a>。

如果在本练习中使用托管实验室环境：

- 有关在 Visual Studio Code 中启用 GitHub Copilot 订阅的帮助，请将以下 URL 粘贴到浏览器的网站导航栏中：<a href="https://go.microsoft.com/fwlink/?linkid=2320158" target="_blank">在 Visual Studio Code 中启用 GitHub Copilot</a>。

- 打开命令终端，并运行以下命令：

    若要确保将 Visual Studio Code 配置为使用正确的 Python 版本，请验证 Python 安装是否为 3.10 或更高版本：

    ```bash
    python --version
    ```

    要确保 Git 配置为使用你的姓名和电子邮件地址，请使用你的信息更新以下命令，然后运行这些命令：

    ```bash

    git config --global user.name "John Doe"

    ```

    ```bash

    git config --global user.email johndoe@example.com

    ```

## 练习场景

你是一名在当地社区 IT 部门工作的开发人员。 支持公共图书馆的后端系统在一场火灾中被烧毁。 你的团队需要开发临时项目，以帮助图书馆员工管理其操作，直到可以更换系统。 你的团队已选择使用 GitHub Copilot 来加速开发流程。

你提交了图书馆应用程序的初始版本以待评审。 评审团队确定了可提升代码质量、性能、可读性、可维护性和安全性的方面。

本练习包括以下任务：

1. 在 Visual Studio Code 中设置图书馆应用程序。
1. 在“提问”和“编辑”模式下使用对话助手视图分析和重构代码。
1. 在“编辑”和“代理”模式下使用内联对话助手和对话助手视图来重构代码。

## 在 Visual Studio Code 中设置图书馆应用程序

你需要下载现有的应用程序、提取代码文件，然后在 Visual Studio Code 中打开项目。

使用以下步骤设置图书馆应用程序：

1. 在实验室环境中打开浏览器窗口。

1. 若要下载包含图书馆应用程序的 zip 文件，请将以下 URL 粘贴到浏览器的地址栏中：[GitHub Copilot 实验室 - 重构现有代码](https://github.com/MicrosoftLearning/mslearn-github-copilot-dev/raw/refs/heads/main/DownloadableCodeProjects/Downloads/AZ2007LabAppM5Python.zip)

    **** zip 文件命名 为 AZ2007LabAppM5Python.zip。

1. 从 AZ2007LabAppM5Python.zip 文件中提取文件。****

    例如：

    1. 导航到实验室环境中的下载文件夹。

    1. 右键单击“AZ2007LabAppM5Python.zip”，然后选择“全部提取”********。

    1. 选择“完成时显示解压缩的文件”，然后选择“解压缩”。

1. 打开提取的文件文件夹，然后将 AccelerateDevGHCopilot 文件夹复制到易于访问的位置，例如 Windows 桌面文件夹。****

1. 在 Visual Studio Code 中打开 AccelerateDevGHCopilot 文件夹。****

    例如：

    1. 在实验室环境中打开 Visual Studio Code。

    1. 在 Visual Studio Code 中的“文件”菜单上，选择“打开文件夹” 。

    1. 导航到 Windows 桌面文件夹，选择“AccelerateDevGHCopilot”，然后选择“选择文件夹”********。

1. 在 Visual Studio Code 的“资源管理器”视图中，验证以下项目结构：

    - AccelerateDevGHCopilot/library   ├── application_core   ├── console   ├── infrastructure   └── tests

1. 确保初始代码能在接下来的部分通过测试并成功运行。 ********（可选）如果熟悉往期实验内容，可在终端中使用 `python console\main.py` 从 \library 文件夹中运行该应用程序。

### 启用 Pytest

与 Unittest 相比，Pytest 具有一些优势，例如简洁的语法、固定例程和参数化等功能，以及更好的故障报告。 Pytest 使测试更易于编写和维护，Pytest 运行 Unittest 测试用例。

1. 通过 <a href="https://marketplace.visualstudio.com/items?itemName=ms-python.python" target="_blank">Visual Studio Microsoft Python 扩展</a>的安装启用 Pytest（如有需要请安装）。

1. 选择 flask 图标  ![显示测试 flask 图标的屏幕截图。](./Media/m04-pytest-flask-py.png) 发现测试后，就在工具栏上。 如果图标不存在，请查看之前的说明

1. 选择“配置 Python 测试”或之前已配置：

1. 如果测试尚未配置或正在测试正确的项目，请转到下一步。 如果需要更改测试项目，请继续：
    - 通过 `Ctrl+Shift+P` 打开命令面板。
    - 输入“Python:**** 配置测试”。
    - 选择“pytest”。
    - 选择 Python 代码的目录。
    - 选择播放图标以运行测试。

1. 从选项中选择 Pytest。

1. 选择包含测试代码的 (`library\`) 文件夹。

1. 选择播放图标以运行测试。
    ![显示 pytest 结果的屏幕截图。](./Media/m04-pytest-configure-results-py.png)

1. （可选）从终端中的 `library` 路径运行 ptytest 命令：

    ```plaintext
    pytest -v
    ```

## 在“提问”和“编辑”模式下使用对话助手视图分析和重构代码

反射是一项强大的编码功能，可用于在运行时检查和操作对象。 但是，反射可能很慢，并且存在与应考虑的反射相关的潜在安全风险。

你需要：

1. 分析工作区并调查如何解决已分配任务。
1. 将 Python Enum 的使用重构为 enum_helper 类，以使用静态字典，而非 Enum 中生成的反射和 python。

### 分析 Enum 与“询问”模式下对话助手视图结合使用

GitHub Copilot 的对话助手视图有三种模式：“询问”、“编辑”和“代理”************。 每个模式专为与 GitHub Copilot 进行不同类型的交互而设计。

- 询问****：使用此模式可以向 GitHub Copilot 询问关于代码库的问题。 可以要求 GitHub Copilot 解释代码、提供更改建议或提供有关代码库的信息。
- 编辑****：使用此模式编辑所选的代码文件。 可以使用 GitHub Copilot 重构代码、添加备注或对代码执行其他更改。
- 代理****：使用此模式以代理身份运行 GitHub Copilot。 可以使用 GitHub Copilot 运行命令、执行代码或在工作区中执行其他任务。

**** 在该练习的本部分中，将在“询问”模式下使用“对话助手”视图来分析编码分配。

请使用以下步骤完成本练习的这一部分：

1. **** 在资源管理器视图中，展开 library 文件夹。

1. 打开 GitHub Copilot 对话助手视图。

    对话助手视图提供一个托管对话界面，用于与 GitHub Copilot 交互。

    可以使用位于 Visual Studio Code 窗口顶部的“切换对话助手”按钮（位于搜索文本框右侧）将“对话助手”视图切换为打开或关闭****。

    ![显示 Copilot“切换对话助手”按钮的屏幕截图。](./Media/m01-github-copilot-toggle-chat.png)

    还可以使用键盘快捷方式 Ctrl+Alt+I 切换“对话助手”视图****。

1. 请注意，“对话助手”视图默认在“询问”模式下打开****。

    当前对话助手模式显示在对话助手视图右下角附近。 **** 在“询问”模式下工作时，“对话助手”视图中会显示对话助手响应。

1. 查看然后提交以下提示：

    ```plaintext
    @workspace Provide an explanation of how the current Python code can be improved with code refactoring to:
    - avoid reflection
    - avoid repeated `if/elif`
    - make the code more explicit and efficient.
    Then provide suggestion for refactoring this Python project to implement the improvement suggestions.
    ```

1. 花一分钟时间查看响应，例如以下响应（已删除代码建议）：

    >**注意** 看到迥然不同的响应是正常的，但它们应该都涉及类似的主题。

    -----

    >### 代码改进说明
    >
    > 要改进当前代码，可考虑以下重构策略：
    >
    > 1. **** 避免反射：  
    >    反射可能会很缓慢且容易出错。 应使用显式方法调用、接口或字典将操作映射到函数。 这可使代码更易于维护且性能更高。
    >
    > 2. **** 避免重复的 `if/elif` 链：  
    >    长 `if/elif` 链难以读取和维护。 将其替换为字典（在 Python 中）或 switch 表达式（在 C# 8+ 中），或使用多态性（面向对象的设计）来委托行为。
    >
    > 3. **** 使代码更加明确且高效：  
    >    显式代码更易于理解和调试。 使用清晰的变量名，避免魔法字符串/数字，并合理组织逻辑以确保意图明确。 通常可以通过减少不必要的检查或查找提高效率。
    >
    >#### 重构建议
    >
    > 假设你的代码如下所示（Python 示例）：
    >
    > ```python
    > # ...existing code...
    > def handle_action(action, data):
    >     if action == "create":
    >         return create_item(data)
    >     elif action == "update":
    >         return update_item(data)
    >     elif action == "delete":
    >         return delete_item(data)
    >     else:
    >         raise ValueError("Unknown action")
    > # ...existing code...
    > ```
    >
    > 重构版本（避免反射以及 if/elif，增加显式程度）****
    >
    > ```python
    > # ...existing code...
    > def handle_action(action, data):
    >     action_map = {
    >         "create": create_item,
    >         "update": update_item,
    >         "delete": delete_item,
    >     }
    >     try:
    >         return action_map[action](data)
    >     except KeyError:
    >         raise ValueError(f"Unknown action: {action}")
    > # ...existing code...
    > ```
    >
    > **优点：**
    >
    > - 不使用反射。
    > - 无重复的 `if/elif`。
    > - 映射是显式的且易于扩展。
    > - 由于是直接的函数查找，效率更高。
    >
    >### 总结
    >
    > - 使用字典或 switch 表达式，而不是反射或长 `if/elif` 链。
    > - 为提升可维护性和效率，要采用显式映射。
    > - 针对未知情况使用明确的错误处理。
    >
    > 这些更改将使代码库更易于维护、更高效且不易出错。

1. 查看然后提交以下提示：

    ```plaintext
    @workspace which Python files in this workspace use reflection or long `if/elif` chains?
    ```

    ******** 响应应告知 console/console_app.py  (`getattr`)，且 console/console_app.py 也使用长 `if/elif` 链来处理命令和输入。

1. **** 将 console/console_app.py 文件添加到对话助手上下文：

    可以使用拖放操作将 Visual Studio Code 资源管理器视图中的文件添加到对话助手视图。 还可以使用“对话助手”视图中的“添加上下文”按钮添加文件和其他资源****。

    > 注意****：将文件添加到对话助手上下文可确保 GitHub Copilot 在生成响应时考虑这些文件。 当 GitHub Copilot 了解与提示关联的上下文时，响应的相关性和准确性会提高。

1. 查看然后提交以下提示：

    ```plaintext

    @workspace I need to refactor the `library\console\console_app.py` Python file to: Use dictionaries or switch expressions instead of reflection or long `if/elif` chains; Make mappings explicit for maintainability and efficiency; Use clear error handling for unknown cases; Provide refactored code to apply.

    ```

    编写任何提示时，清晰度和上下文都非常重要。 使用对话助手参与者、斜杠命令和对话助手变量有助于定义 GitHub Copilot 可以理解的上下文。

    对于询问 GitHub Copilot 如何解决问题的提示，请首先说明要解决的问题。 使用简洁的句子来描述详细信息、指定约束和标识资源。 最后，请务必告诉 GitHub Copilot 响应中要包含的内容。

    在本例中，提示中应首先描述问题/目标。 告知 GitHub Copilot 需要将 `library\console\console_app.py` 文件重构为：

    - 使用字典或 switch 表达式，而不是反射或长 `if/elif` 链。
    - 为提升可维护性和效率，要采用显式映射。
    - 针对未知情况使用明确的错误处理。

    为清晰起见，请以“提供可应用的重构代码”这一指令作为提示词的结尾。

1. 请花些时间查看 GitHub Copilot 提供的响应。 **** 此步骤中不应用编辑。

    > ### 代码改进说明
    >
    > 要改进当前代码，可考虑以下重构策略：
    >
    > 1. **** 避免反射：  
    >    反射可能会很缓慢且容易出错。 应使用显式方法调用、接口或字典将操作映射到函数。 这可使代码更易于维护且性能更高。
    >
    > 2. **** 避免重复的 `if/elseif` 链：  
    >    长 `if/elseif` 链难以读取和维护。 将其替换为字典（在 Python 中）或 switch 表达式（在 C# 8+ 中），或使用多态性（面向对象的设计）来委托行为。
    >
    > 3. **** 使代码更加明确且高效：  
    >    显式代码更易于理解和调试。 使用清晰的变量名，避免魔法字符串/数字，并合理组织逻辑以确保意图明确。 通常可以通过减少不必要的检查或查找提高效率。
    >
    >
    > ### 重构建议
    >
    > 假设你的代码如下所示（Python 示例）：
    >
    > ```python
    > # ...existing code...
    >    def _handle_patron_details_selection(self, selection, patron, valid_loans):
    >        if selection == 'q':
    >            return ConsoleState.QUIT
    >        elif selection == 's':
    >            return ConsoleState.PATRON_SEARCH
    >        elif selection == 'm':
    >            status = self._patron_service.renew_membership(patron.id)
    >            print(status)
    >            self.selected_patron_details = self._patron_repository.get_patron(patron.id)
    >            return ConsoleState.PATRON_DETAILS
    > # ...existing code...
    > ```
    >
    > #### 重构版本（避免反射以及 if/elif，增加显式程度）
    >
    > ```python
    > # ...existing code...
    >    def _handle_patron_details_selection(self, selection, patron, valid_loans):
    >        def renew_membership():
    >           status = self._patron_service.renew_membership(patron.id)
    >           print(status)
    >           self.selected_patron_details = self._patron_repository.get_patron(patron.id)
    >           return ConsoleState.PATRON_DETAILS
    > # ...existing code...
    > ```
    >
    > **优点：**
    >
    > - 不使用反射。
    > - 无重复的 `if/elif`。
    > - 映射是显式的且易于扩展。
    > - 由于是直接的函数查找，效率更高。
    >
    >
    > ### 总结
    >
    > - 使用字典或 switch 表达式，而不是反射或长 `if/elseif` 链。
    > - 为提升可维护性和效率，要采用显式映射。
    > - 针对未知情况使用明确的错误处理。
    >
    > 这些更改将使代码库更易于维护、更高效且不易出错。

1. 在“对话助手”视图中，将鼠标指针悬停在响应中包含的代码示例上。

1. 请注意代码片段右上角显示的三个按钮。

1. 将鼠标指针悬停在每个按钮上，查看描述操作的工具提示。

    前两个按钮可将代码复制到编辑器。 第三个按钮可将代码复制到剪贴板。 **** 此步骤中不应用编辑。

> 注意****：可以使用“询问”模式更新代码。 但是，编辑模式直接在代码编辑器中重构代码，并提供用于接受更新的更多选项。

### 使用“编辑”模式下的对话助手视图重构类 ConsoleApp (console/console_app.py) 文件

对话助手视图的“编辑”模式旨在用于在工作区中编辑代码。 可以使用“编辑”模式重构代码、添加注释或对代码执行其他更改。

1. 在“对话助手”视图中，选择“设置模式”，然后选择“编辑”********。

    **** 当系统提示在“编辑”模式下启动新会话时，选择“是”。

    **** 在“编辑”模式下，GitHub Copilot 在代码编辑器中以代码更新建议的形式显示响应。 实现新功能、修复 bug 或重构代码时，通常会使用编辑模式。

1. 将 console/console_app.py 文件添加到对话助手上下文：

1. 查看然后提交以下提示：

    ```plaintext

    @codebase I need to refactor the ConsoleApp class. Use static dictionaries to supply enum description attributes. Use dictionaries or switch expressions instead of reflection or long `if/elif` chains. Make mappings explicit for maintainability and efficiency. Use clear error handling for unknown cases.

    ```

    “编辑”模式的响应应在 `ConsoleApp` 类中提出更新，并提供与以下内容类似的响应：

    ```plaintext
    The ConsoleApp class has been refactored to use static dictionaries for enum descriptions, explicit dictionaries for state and input handling, and clear error handling for unknown cases, improving maintainability and efficiency.
    ```

1. **** 花一些时间查看 console_app.py 中建议的代码更新。 应获得以下结果：

    - 为枚举说明（`CONSOLE_STATE_DESCRIPTIONS` 和 `COMMON_ACTIONS_DESCRIPTIONS`）引入了静态字典来替换反射和长 `if/elif` 链。
    - 将 `write_input_options` 重构为使用静态字典来显示输入选项。
    - 将 `run` 中的主状态循环替换为基于字典的处理程序查找，以提高可维护性和效率。
    - 添加了针对未知状态和后续状态的显式错误处理。
    - 输入处理程序中的所有操作映射现已改为显式字典，以提升清晰度和可维护性。

1. **** 在对话助手视图中，若要接受所有更新，可选择“保留”。

    还可以使用代码编辑器选项卡底部附近的“对话助手编辑”工具栏来接受或拒绝代码更新。

1. 花些时间查看更新后的 **** run 方法。

    **** GitHub Copilot 应已更新 run 方法，将长 if/elif 链替换为将每个 ConsoleState 映射到其相应的处理程序函数的 state_handlers 字典。 更新的方法应类似于以下示例之一：

    ```python

    def run(self) -> None:
        state_handlers = {
            ConsoleState.PATRON_SEARCH: self.patron_search,
            ConsoleState.PATRON_SEARCH_RESULTS: self.patron_search_results,
            ConsoleState.PATRON_DETAILS: self.patron_details,
            ConsoleState.LOAN_DETAILS: self.loan_details,
            ConsoleState.QUIT: lambda: ConsoleState.QUIT
        }
        while True:
            handler = state_handlers.get(self._current_state)
            if handler is None:
                print(f"Unknown state: {self._current_state}")
                break
            next_state = handler()
            if next_state == ConsoleState.QUIT:
                print("Exiting application.")
                break
            if next_state not in state_handlers:
                print(f"Unknown next state: {next_state}")
                break
            self._current_state = next_state
    ```

    此代码使用 `state_handlers` 字典将每个 `ConsoleState` 映射到其相应的处理程序函数。 它现在从字典中检索处理程序，针对未知状态引发错误，并根据处理程序的返回值更新当前状态。

1. 运行 Pytest 并手动测试，以确保未引入任何错误。

    你将看到在本练习开始时出现的相同警告，但不应显示任何错误消息。

## 在“编辑”和“代理”模式下使用内联对话助手和对话助手视图来重构代码

通过采用 Python 的列表推导式****、生成器表达式**** 和内置函数****（如 `any()`、`all()`、`sum()`、`map()`、`filter()` 和 `sorted()`），代码库可以更简洁、更具表达力且效率更高，减少与手动迭代和数据处理相关的样板代码及潜在错误。

本练习的本部分包括以下任务：

- 内联对话助手：**** 生成器表达式重构
- ****“编辑”模式对话助手：列表推导式和 Python 内置方法重构
- 代理模式：**** 内置的函数重构

### 使用内联对话助手重构生成器表达式

1. ************ 在资源管理器视图中，展开 infrastructure/json_patron_repository.py 项目，然后打开 json_loan_repository.py 文件并检查 `JsonLoanRepository` 类。

1. **** 要选择 `JsonLoanRepository` 类，请突出显示整个类。

    ```python

    class JsonPatronRepository(IPatronRepository):
        def __init__(self, json_data: JsonData):
            self._json_data = json_data
    
        def get_patron(self, patron_id: int) -> Optional[Patron]:
            for patron in self._json_data.patrons:
                if patron.id == patron_id:
                    return patron
            return None
    
        def search_patrons(self, search_input: str) -> List[Patron]:
            results = []
            for p in self._json_data.patrons:
                if search_input.lower() in p.name.lower():
                    results.append(p)
            n = len(results)
            for i in range(n):
                for j in range(0, n - i - 1):
                    if results[j].name > results[j + 1].name:
                        results[j], results[j + 1] = results[j + 1], results[j]
            return results
    
        def update_patron(self, patron: Patron) -> None:
            for idx in range(len(self._json_data.patrons)):
                if self._json_data.patrons[idx].id == patron.id:
                    self._json_data.patrons[idx] = patron
                    self._json_data.save_patrons(self._json_data.patrons)
                    return
    
        def add_patron(self, patron: Patron) -> None:
            self._json_data.patrons.append(patron)
            self._json_data.save_patrons(self._json_data.patrons)
            self._json_data.load_data()
    
        def get_all_patrons(self) -> List[Patron]:
            return self._json_data.patrons
    
        def find_patrons_by_name(self, name: str) -> List[Patron]:
            result = []
            for patron in self._json_data.patrons:
                if patron.name.lower() == name.lower():
                    result.append(patron)
            return result
    
        def get_all_books(self):
            return self._json_data.books
    
        def get_all_book_items(self):
            return self._json_data.book_items

    ```

1. 打开内联 Copilot 对话助手，然后输入用于重构方法的提示。

    ```plaintext
    #selection Refactor any manual aggregation or search over loans in this method to use generator expressions with built-in functions like any(), all(), sum(), or max() for improved readability and performance.
    ```

1. 请花点时间来查看建议的更新。

    建议的更新看起来应类似于以下代码：

    ```python
    # --code continues-- 
    def get_patron(self, patron_id: int) -> Optional[Patron]:
        return next((patron for patron in self._json_data.patrons if patron.id == patron_id), None)

    def search_patrons(self, search_input: str) -> List[Patron]:
        results = [p for p in self._json_data.patrons if search_input.lower() in p.name.lower()]
        results.sort(key=lambda p: p.name)
        return results

    def update_patron(self, patron: Patron) -> None:
        for idx, existing_patron in enumerate(self._json_data.patrons):
            if existing_patron.id == patron.id:
                self._json_data.patrons[idx] = patron
                self._json_data.save_patrons(self._json_data.patrons)
                return

    # --code continues--    
    def find_patrons_by_name(self, name: str) -> List[Patron]:
        return [patron for patron in self._json_data.patrons if patron.name.lower() == name.lower()]
    # --code continues-- 
    ```

1. 若要接受建议的更新，请选择“接受”****。

#### **** 使用“内联对话助手‘说明’智能操作”检查方法

1. 使用“说明”**** 智能操作查看 `search_patrons` 方法的说明。

1. 从 `search_patrons` 方法中选择以下代码行（注释后面）：

    ```python
    # def search_patrons(self, search_input: str) -> List[Patron]:
        results = [p for p in self._json_data.patrons if search_input.lower() in p.name.lower()]
        results.sort(key=lambda p: p.name)
    ```

    ******** 要打开“说明”智能操作，请在编辑器中选择 `search_patrons` 方法代码，右键单击所选代码，选择 Copilot，然后选择“说明”。 “说明”**** 智能操作提供了所选代码的详细说明。

    例如，说明可能类似这样：

    ```plaintext

    The `search_patrons` method is responsible for finding patrons whose names contain a given search string, regardless of case. It takes a single argument, `search_input`, which is the string to search for. The method iterates over all patrons stored in `self._json_data.patrons` and uses a list comprehension to filter those whose `name` attribute contains the `search_input` substring. Both the patron's name and the search input are converted to lowercase to ensure the search is case-insensitive.
    
    After collecting the matching patrons, the method sorts the results alphabetically by the patron's name using the `sort()` method with a key function that extracts the `name` attribute from each patron. Finally, the sorted list of matching patrons is returned. This ensures that users receive a case-insensitive, alphabetically ordered list of patrons matching their search criteria.
    
    A potential consideration is that the search will match any part of the name, not just the beginning, which may lead to more results than expected. Also, the sorting is done in-place on a new list,

    ```

1. 测试并运行项目，以确保未引入任何错误。

#### 使用内联对话助手继续进行生成器表达式重构

1. **** 接下来继续，执行 loan_service.py 的重构过程。

1. ************ 在资源管理器视图中，展开 application_core\services\loan_service.py 项目，然后打开 loan_service.py 文件并检查 `LoanService` 类。

1. **** 要选择 `LoanService` 类，请突出显示整个类，并检查 `checkout_book` 方法。

    ```python

    class LoanService(ILoanService):
        EXTEND_BY_DAYS = 14
    
        def __init__(self, loan_repository: ILoanRepository):
            self._loan_repository = loan_repository
    
        def return_loan(self, loan_id: int) -> LoanReturnStatus:
            loan = self._loan_repository.get_loan(loan_id)
            if loan is None:
                return LoanReturnStatus.LOAN_NOT_FOUND
            if loan.return_date is not None:
                return LoanReturnStatus.ALREADY_RETURNED
            loan.return_date = datetime.now()
            try:
                self._loan_repository.update_loan(loan)
                return LoanReturnStatus.SUCCESS
            except Exception:
                return LoanReturnStatus.ERROR
    
        def extend_loan(self, loan_id: int) -> LoanExtensionStatus:
            loan = self._loan_repository.get_loan(loan_id)
            if loan is None:
                return LoanExtensionStatus.LOAN_NOT_FOUND
            if loan.patron and loan.patron.membership_end < datetime.now():
                return LoanExtensionStatus.MEMBERSHIP_EXPIRED
            if loan.return_date is not None:
                return LoanExtensionStatus.LOAN_RETURNED
            if loan.due_date < datetime.now():
                return LoanExtensionStatus.LOAN_EXPIRED
            try:
                loan.due_date = loan.due_date + timedelta(days=self.EXTEND_BY_DAYS)
                self._loan_repository.update_loan(loan)
                return LoanExtensionStatus.SUCCESS
            except Exception:
                return LoanExtensionStatus.ERROR
    
        def checkout_book(self, patron, book_item, loan_id=None) -> None:
            from ..entities.loan import Loan
            from datetime import datetime, timedelta
            # Generate a new loan ID if not provided
            if loan_id is None:
                all_loans = getattr(self._loan_repository, 'get_all_loans', lambda: [])()
                max_id = 0
                for l in all_loans:
                    if l.id > max_id:
                        max_id = l.id
                loan_id = max_id + 1 if all_loans else 1
            now = datetime.now()
            due = now + timedelta(days=14)
            new_loan = Loan(
                id=loan_id,
                book_item_id=book_item.id,
                patron_id=patron.id,
                patron=patron,
                loan_date=now,
                due_date=due,
                return_date=None,
                book_item=book_item
            )
            self._loan_repository.add_loan(new_loan)
            return new_loan

    ```

1. 打开内联对话助手，然后输入重构方法的提示。

    ```plaintext
    #selection Refactor any manual aggregation or search over loans in this method to use generator expressions with built-in functions like any(), all(), sum(), or max() for improved readability and performance.
    ```

1. 花些时间查看在 `checkout_book` 方法中添加代码时建议的更新：

    ```python

            loan_id = max((l.id for l in all_loans), default=0) + 1 if all_loans else 1
    ```

    完整的 `checkout_book` 方法应与以下代码类似：

    ```python
    # --code continues-- 

    def checkout_book(self, patron, book_item, loan_id=None) -> None:
            from ..entities.loan import Loan
            from datetime import datetime, timedelta
            if loan_id is None:
                all_loans = getattr(self._loan_repository, 'get_all_loans', lambda: [])()
                loan_id = max((l.id for l in all_loans), default=0) + 1 if all_loans else 1
            now = datetime.now()
            due = now + timedelta(days=14)
            new_loan = Loan(
                id=loan_id,
                book_item_id=book_item.id,
                patron_id=patron.id,
                patron=patron,
                loan_date=now,
                due_date=due,
                return_date=None,
                book_item=book_item
            )
            self._loan_repository.add_loan(new_loan)
            return new_loan
    ```

    重构为使用生成器表达式后，会以简洁、省内存的结构替代手动迭代，这种结构能与 `max()`、`any()` 和 `sum()` 等** 内置函数无缝协作。 这不仅减少了样板代码和潜在错误，还提升了性能（尤其是在处理大型数据集时），因为生成器表达式不会在内存中创建中间列表。 总体而言，这些修改使代码库更符合 Python 风格、更易读且更易维护。

### 在编辑模式下使用对话助手视图重构 JsonPatronRepository 类

**** JsonPatronRepository 类包含以下三种方法：

- SearchPatrons：SearchPatrons 方法用于按姓名搜索借阅者。 此方法返回已排序的借阅者列表。
- GetPatron：GetPatron 方法用于按 ID 检索借阅者。 此方法返回填充的 patron 对象。
- UpdatePatron：UpdatePatron 方法用于更新借阅者的信息。 此方法使用新数据更新现有借阅者，并保存更新后的借阅者集合。

这三种方法均使用 foreach 循环来循环访问借阅者，并根据搜索输入或 ID 查找匹配项。

请使用以下步骤完成本练习的这一部分：

1. **** 打开 json_loan_repository.py 文件。

    **** JsonPatronRepository 类旨在用于管理图书馆借阅者。

1. 花些时间查看 JsonPatronRepository 类中包含的一些方法****。

    **** SearchPatrons 方法旨在用于按姓名搜索借阅者。

    ```python

    #  ----code continues----   
    class JsonLoanRepository(ILoanRepository):
        def **init**(self, json_data: JsonData):
            self._json_data = json_data
    
        #  ----code continues----   
        def get_loans_by_patron_id(self, patron_id: int):
            result = []
            for loan in self._json_data.loans:
                if loan.patron_id == patron_id:
                    result.append(loan)
            return result
    
        #  ----code continues----  
        def get_overdue_loans(self, current_date):
            overdue = []
            for loan in self._json_data.loans:
                if loan.return_date is None and loan.due_date < current_date:
                    overdue.append(loan)
            return overdue
    
        def sort_loans_by_due_date(self):
            # Manual bubble sort for demonstration
            n = len(self._json_data.loans)
            for i in range(n):
                for j in range(0, n - i - 1):
                    if self._json_data.loans[j].due_date > self._json_data.loans[j + 1].due_date:
                        self._json_data.loans[j], self._json_data.loans[j + 1] = self._json_data.loans[j + 1], self._json_data.loans[j]
            return self._json_data.loans

        #  ----code continues----  
    ```

1. 在“对话助手”视图中，选择“设置模式”，然后选择“编辑”********。

    **** 当系统提示在“编辑”模式下启动新会话时，选择“是”。

    **** 在“编辑”模式下，GitHub Copilot 在代码编辑器中以代码更新建议的形式显示响应。 实现新功能、修复 bug 或重构代码时，通常会使用编辑模式。

1. 输入提示

    ```plaintext
    @codebase Refactor methods in the JsonLoanRepository class with for-loops to use list comprehensions 
    or Python built-in methods to improve code clarity, conciseness, and efficiency.
    ```

1. 请注意，在下文更新后的代码中，`get_loans_by_patron_id` 和 `get_overdue_loans` 方法现已使用列表推导式，而 `sort_loans_by_due_date` 方法现在使用了一个 Python 内置的 `sorted` 函数。

    ```python
    #  ----code continues----

    def get_loan(self, loan_id: int) -> Optional[Loan]:
        return next((loan for loan in self._json_data.loans if loan.id == loan_id), None)

    def update_loan(self, loan: Loan) -> None:
        idx = next((i for i, l in enumerate(self._json_data.loans) if l.id == loan.id), None)
        if idx is not None:
            self._json_data.loans[idx] = loan
            self._json_data.save_loans(self._json_data.loans)
            return

    #  ----code continues----
    def get_loans_by_patron_id(self, patron_id: int):
        return [loan for loan in self._json_data.loans if loan.patron_id == patron_id]
    
    #  ----code continues----
    def get_overdue_loans(self, current_date):
        return [loan for loan in self._json_data.loans if loan.return_date is None and loan.due_date < current_date]

    def sort_loans_by_due_date(self):
        # Use built-in sorted for clarity and efficiency
        self._json_data.loans = sorted(self._json_data.loans, key=lambda l: l.due_date)
        return self._json_data.loans
    ```

    列表推导式和内置函数取代了用于筛选与排序的手动 for 循环，使代码更简短、更清晰且效率更高。

### 使用“代理”模式下的对话助手视图重构 JsonLoanRepository 和 JsonPatronRepository 类

1. **** 在“对话助手”视图中，将模式更改为“代理”

    代理模式旨在将 GitHub Copilot 作为代理运行。 可以使用自然语言来指定高级任务。 代理将评估分配的任务、规划所需的工作，并将更改应用到代码库。

    代理模式结合使用代码编辑和工具调用来完成指定任务。 处理请求时，它会监视编辑和工具的结果，并循环访问以解决出现的任何问题。 如果代理无法解决问题，它会请你进行干预。 例如，如果代理使用多个迭代来解决同一问题，它将暂停进程，并请你提供其他上下文来阐明请求或取消进程。

    > **重要说明**：在代理模式下使用聊天视图时，GitHub Copilot 可能会发出多个高级请求来完成单个任务。 高级请求可由用户发起的提示和 Copilot 代表你采取的后续操作使用。 使用的高级请求总数取决于任务的复杂性、所涉及的步骤数和所选的模型。

1. 请花点时间考虑需要分配给代理的任务。

    ******** 任务是重构 JsonLoanRepository & JsonPatronRepository 类。 **** 目标是找到手动循环以使用内置的 Python 函数进行重构，生成与原来的 foreach 代码相同的结果。

    可以使用之前重构的体验来帮助为代理编写任务。

1. **** 若要分配代理任务，请使用代理模式输入以下提示：

    ```plaintext

    #codebase Review the manual loop code used in the methods of the JsonLoanRepository and JsonPatronRepository classes to make them more pythonic. Refactor with Built-in Python Functions that produce the same result as the original foreach code.

    ```

    **** 此提示告知代理重构 JsonPatronRepository 类。 它指定应将 foreach 循环替换为内置的 Python 函数。

1. 在代理重构代码时监视智能体的进度。

    请注意，代理在多次迭代中完成任务。 每次代码编辑后均会进行评审，以检查是否存在问题。 如果代理遇到问题，它将重构代码以解决该问题。 如果代理无法解决问题，它会请你进行干预。

1. 代理完成后，请花些时间查看建议的更新。

    JsonLoanRepository 中所做的更改应为：- `update_patron` 现在配合使用 `next` 和 `enumerate` 实现高效索引查找。
        - `search_patrons` 使用生成器表达式和 `sorted` 编写简洁易读的代码。
        - 已删除所有手动气泡排序和冗余代码。

    > **注意：** JsonPatronRepository 无需修改，因为它已经使用内置函数和推导式进行筛选和搜索。

    建议的更新看起来应类似于以下代码：

    ```python
    #  ----code continues----
    def search_patrons(self, search_input: str) -> List[Patron]:
        return sorted(
            (p for p in self._json_data.patrons if search_input.lower() in p.name.lower()),
            key=lambda patron: patron.name
        )

    def update_patron(self, patron: Patron) -> None:
        idx = next((i for i, existing_patron in enumerate(self._json_data.patrons) if existing_patron.id == patron.id), None)
        if idx is not None:
            self._json_data.patrons[idx] = patron
            self._json_data.save_patrons(self._json_data.patrons)

        #  ----code continues----

    ```

1. **** 要接受所有更新，请选择“保留”。

这些更新使代码更加简洁、易读且更符合 Python 风格，同时保留了原有的功能。

### 测试和运行应用程序

现在，你已重构代码，接下来可以测试和运行应用程序，以确保一切正常运行。 你还将测试应用程序，以确保重构的代码按预期运行。

1. 要在******** Visual Studio Code 中使用 Python 运行应用程序，请在编辑器中打开 console/main.py，按 CTRL+Shift+D 打开“运行和调试”面板，选择“Python：**** 当前文件”或其他调试配置，然后按 F5**** 开始调试。

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

## 总结

在本练习中，重点是借助 GitHub Copilot 对现有代码进行重构和改进。 你通过使用列表推导式、生成器表达式和内置函数等符合 Python 风格的结构替代手动循环和重复模式，系统地增强了代码库。 你运用了 GitHub Copilot 的多种模式（询问、内联、编辑和代理）来分析代码、生成重构建议，并直接在 Visual Studio Code 中应用改进。 这些方法提高了代码质量、可读性、可维护性和性能。 掌握了这些技能后，你现在可以自信地对 Python 项目进行重构和现代化改造，在持续开发新功能的同时，使代码库更加健壮和高效。

## 清理

现已完成练习，请花点时间确保你未更改不想保留的 GitHub 帐户或 GitHub Copilot 订阅。 如果进行了任何不需要的更改，请立即还原。
