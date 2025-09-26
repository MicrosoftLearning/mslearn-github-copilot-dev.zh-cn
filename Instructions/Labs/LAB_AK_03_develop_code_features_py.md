---
lab:
  title: 练习 - 使用 GitHub Copilot 开发新代码功能 (Python)
  description: 了解如何在 Visual Studio Code 中使用 GitHub Copilot 加速开发新代码功能。
---

# 使用 GitHub Copilot 开发新代码功能

GitHub Copilot 的代码补全和交互式聊天功能可帮助开发人员更快地编写代码，并减少错误。 它根据所编写代码的上下文为代码片段、函数甚至整个类提供建议。 在本练习中，你将使用 GitHub Copilot 在 Visual Studio Code 中加快新代码功能的开发。

完成此练习大约需要 30 分钟****。

> **重要说明**：若要完成本练习，必须提供自己的 GitHub 帐户和 GitHub Copilot 订阅。 如果没有 GitHub 帐户，可以<a href="https://go.microsoft.com/fwlink/?linkid=2320148" target="_blank">注册</a>免费的个人帐户，并使用 GitHub Copilot 免费版计划来完成练习。 如果可以从实验室环境中访问 GitHub Copilot Pro、GitHub Copilot Pro+、GitHub Copilot Business 或 GitHub Copilot Enterprise 订阅，则可以使用你现有的 GitHub Copilot 订阅来完成本练习。

## 开始之前

实验室环境必须包括以下内容：Git 2.48 或更高版本、Python 3.10 或更高版本、带有 Microsoft Python 扩展的 Visual Studio Code，以及启用了 GitHub Copilot 的 GitHub 帐户的访问权限。

如果你将本地电脑用作本练习的实验室环境：

- 有关将本地电脑配置为实验室环境的帮助，请在浏览器中打开以下链接：<a href="https://microsoftlearning.github.io/mslearn-github-copilot-dev/Instructions/Labs/LAB_AK_00_configure_lab_environment_py.html" target="_blank">配置实验室环境资源</a>。

- 有关在 Visual Studio Code 中启用 GitHub Copilot 订阅的帮助，请在浏览器中打开以下链接：<a href="https://go.microsoft.com/fwlink/?linkid=2320158" target="_blank">在 Visual Studio Code 中启用 GitHub Copilot</a>。

如果你将在本练习中使用托管实验室环境：

- 有关在 Visual Studio Code 中启用 GitHub Copilot 订阅的帮助，请将以下 URL 粘贴到浏览器的网站导航栏中：<a href="https://go.microsoft.com/fwlink/?linkid=2320158" target="_blank">在 Visual Studio Code 中启用 GitHub Copilot</a>。

- 打开命令终端，并运行以下命令：

    若要确保将 Visual Studio Code 配置为使用正确的 Python 版本，请验证 Python 安装是否为 3.10 或更高版本：

    ```bash
    python --version
    ```

    若要确保 Git 配置为使用你的姓名和电子邮件地址，请使用你的信息更新以下命令，然后运行这些命令：

    ```bash

    git config --global user.name "John Doe"

    ```

    ```bash

    git config --global user.email johndoe@example.com

    ```

## 练习场景

你是一名在当地社区 IT 部门工作的开发人员。 支持公共图书馆的后端系统在一场火灾中被烧毁。 你的团队需要开发一个临时项目，以帮助图书馆员工管理他们的运营，直到系统可以被替换为止。 你的团队已选择使用 GitHub Copilot 来加速开发流程。

你的图书馆应用程序的初始版本已经由最终用户测试，并且要求添加一些附加功能。 你的团队同意开发以下功能：

- 书籍可借状态：可让图书管理员确定书籍的可借状态。 此功能应显示一条消息，指出某书籍可供借阅，如果该书籍当前已借给其他借阅者，该消息会显示归还截止日期。

- 书籍借阅记录：可让图书管理员将书籍借给借阅者（如果该书籍可供借阅）。 此功能应显示借阅者接收借阅书籍的选项、使用新的借阅记录更新 Loans.json，并显示借阅者的已更新借阅详细信息。

- 书籍预留：可让图书管理员为借阅者预留书籍（除非该书籍已预留）。 此功能应实现新的书籍预留过程。 此功能可能需要创建新的 Reservations.json 文件，以及支持预留过程所需的新类和接口。

每个团队成员将负责开发其中的一项新功能，然后重新组合。 你将负责开发用于确定书籍可借状态的功能。 你的同事负责开发向借阅者借书的功能。 最后一项功能（为借阅者预留书籍）将在上述两项功能完成后进行开发。

本练习包括以下任务：

1. 在 Visual Studio Code 中设置图书馆应用程序。

1. 使用 Visual Studio Code 为图书馆应用程序创建一个 GitHub 仓库。

1. 在代码存储库中创建“书籍可借状态”分支。

1. 开发新的“书籍可借状态”功能。

    - 使用 GitHub Copilot 建议来帮助更快、更准确地实现代码。
    - 将代码更新同步到远程存储库的“书籍可借状态”分支。

1. 将“书籍可借状态”更新合并到仓库的主分支中。

## 在 Visual Studio Code 中设置图书馆应用程序

你需要下载现有的应用程序、提取代码文件，然后在 Visual Studio Code 中打开项目。

使用以下步骤设置图书馆应用程序：

1. 在实验室环境中打开浏览器窗口。

1. 若要下载包含图书馆应用程序的 zip 文件，请将以下 URL 粘贴到浏览器的地址栏中：[GitHub Copilot 实验室 - 开发代码功能](https://github.com/MicrosoftLearning/mslearn-github-copilot-dev/raw/refs/heads/main/DownloadableCodeProjects/Downloads/AZ2007LabAppM3Python.zip)

    zip 文件命名为 AZ2007LabAppM3Python.zip。****

1. 从 AZ2007LabAppM3Python.zip 文件中解压缩文件。****

    例如：

    1. 导航到实验室环境中的下载文件夹。

    1. 右键单击“AZ2007LabAppM3Python.zip”，然后选择“全部解压缩”。********

    1. 选择“完成时显示解压缩的文件”，然后选择“解压缩”。

1. 打开解压缩的文件文件夹，然后将 AccelerateDevGHCopilot 文件夹复制到易于访问的位置，例如 Windows 桌面文件夹。****

1. 在 Visual Studio Code 中打开 AccelerateDevGHCopilot 文件夹。****

    例如：

    1. 在实验室环境中打开 Visual Studio Code。

    1. 在 Visual Studio Code 中的“文件”菜单上，选择“打开文件夹” 。

    1. 导航到 Windows 桌面文件夹，选择“AccelerateDevGHCopilot”，然后选择“选择文件夹”********。

1. 在 Visual Studio Code 的“资源管理器”视图中，验证以下项目结构：

    - AccelerateDevGHCopilot/library   ├── application_core   ├── console   ├── infrastructure   └── tests   └── readme.md

1. 确保应用程序成功运行。

    例如，在 Visual Studio Code 中打开终端，导航到 AccelerateDevGHCopilot/library 目录，然后运行以下命令：****

    ```bash
    python -m unittest discover -v tests
    ```

    你将看到多个警告，但不应看到任何错误。

## 为代码创建 GitHub 存储库

为代码创建 GitHub 存储库，可以方便你与他人分享工作成果并协作处理项目。

> 注意****：使用自己的 GitHub 帐户为图书馆应用程序创建一个专用 GitHub 存储库。

请使用以下步骤完成本练习的这一部分：

1. 打开浏览器窗口并导航到 GitHub 帐户。

    GitHub 登录页面为：[https://github.com/login](https://github.com/login)。

1. 登录到 GitHub 帐户。

1. 打开 GitHub 帐户菜单，然后选择“你的存储库”****。

1. 切换到“Visual Studio Code”窗口。

1. 在 Visual Studio Code 中，打开“源代码管理”视图。

1. 选择“发布到 GitHub”****。

1. 仓库 AccelerateDevGHCopilot 的名称。****

    > 注意****：如果未在 Visual Studio Code 中登录到 GitHub，系统会提示你登录。 登录后，使用请求的权限授权 Visual Studio Code。

1. 选择“发布到 GitHub 专用仓库”。****

1. 请注意，Visual Studio Code 会在发布过程中显示状态消息。

    发布过程完成后，你将看到一条消息，告知你代码已成功发布到指定的 GitHub 存储库。

1. 切换到 GitHub 帐户的浏览器窗口。

1. 在 GitHub 帐户中打开新的 AccelerateDevGHCopilot 仓库。

    如果未看到 AccelerateDevGHCopilot 仓库，请刷新页面。 如果仍然看不到存储库，请尝试以下步骤：

    1. 切换到“Visual Studio Code”。
    1. 打开通知（发布新存储库时会生成一个通知）。
    1. 选择“在 GitHub 上打开”以打开存储库****。

## 在存储库中创建新分支

在开始开发新的“书籍可借状态”功能之前，需要在存储库中创建新分支。 这样便可以开发新功能且不会影响存储库的 main 分支。 代码准备就绪后，可以将新功能合并到主分支中。

请使用以下步骤完成本练习的这一部分：

1. 确保已在 Visual Studio Code 中打开 AccelerateDevGHCopilot 项目。

1. 选择“源代码管理”视图，并确保本地存储库已与远程存储库同步（“拉取”或“同步”）。

1. 在窗口左下角，选择“main”****。

1. 若要创建新分支，请键入“**书籍可借状态**”，然后选择“**+ 创建新分支**”。

1. 若要将新分支推送到远程存储库，请选择“发布分支”****。

## 使用 GitHub Copilot 开发新的“书籍可借状态”功能

在本练习的本部分中，你将使用 GitHub Copilot 来为图书馆应用程序开发一项新功能。 请求的功能将使图书馆管理员能够检查书籍是否可供借阅，这是当前的图书馆应用程序当前不支持的常见场景。

若要实现书籍可借状态功能，需要完成以下更新：

- 在 library/console/common_actions.py 中将新的 SEARCH_BOOKS 操作添加到 CommonActions 枚举。************

- 更新 library/console/console_app.py 中的 WriteInputOptions 方法。********

  - 为新的 CommonActions.SEARCH_BOOKS 选项添加支持。****
  - 显示用于检查书籍是否可供借阅的选项。

- 更新 library/console/console_app.py 中的 ReadInputOptions 方法。********

  - 为新的 CommonActions.SEARCH_BOOKS 选项添加支持。****

- 更新 library/console/console_app.py 中的 PatronDetails 方法。********

  - 在调用 ReadInputOptions 之前，将 CommonActions.SEARCH_BOOKS 添加到选项。************
  - 添加 else if 来处理 SEARCH_BOOKS 操作。********
  - else if 块应调用名为 SEARCH_BOOKS 的新方法。********

- 在 library/console/console_app.py 中创建新的 SEARCH_BOOKS 方法。********

  - SEARCH_BOOKS 方法应读取用户提供的书名。****
  - 检查某书籍是否可供借阅，并显示一条消息，指出：

    - “book.title 可供借阅”，或****
    - “book.title 已借给其他借阅者”。**** 归还截止日期为“loan.DueDate”。****

GitHub Copilot 聊天可帮助实现完成新功能所需的代码更新。

- 可以使用内联聊天会话根据要求实现更小、更谨慎的代码更新。
- 可以使用“聊天”视图处理可能需要更多对话和迭代方法的大型代码更新。

### 使用 Copilot 内联聊天实现“书籍可借状态”更新

通过 Copilot 内联聊天会话，可以直接在代码编辑器中与 GitHub Copilot 交互。**** 可以使用内联聊天提问、请求代码建议，以及获取 GitHub Copilot 生成的代码的说明。

请使用以下步骤完成本练习的这一部分：

1. 打开“资源管理器”视图。

1. 展开 library/console 项目。****

1. 打开 console/common_actions.py 文件，然后选择 CommonActions 类。************

    需要向 CommonActions 添加新的 SEARCH_BOOKS 操作。********

1. 打开内联聊天：将鼠标悬停在所选内容上，右键单击打开菜单，选择“Copilot”，然后选择“编辑器内联聊天”。********

1. 输入以下提示：

    ```plaintext
    Update selection to include a new `SEARCH_BOOKS` action.
    ```

    GitHub Copilot 应该建议进行代码更新，以将新的 SEARCH_BOOK 操作添加到 CommonActions 类。********

1. 查看建议的更新，然后选择“接受”****。

    更新后的代码应类似于以下代码片段：

    ```python

    class CommonActions(Flag):
        REPEAT = 0
        SELECT = auto()
        QUIT = auto()
        SEARCH_PATRONS = auto()
        SEARCH_BOOKS = auto() # added
        RENEW_PATRON_MEMBERSHIP = auto()
        RETURN_LOANED_BOOK = auto()
        EXTEND_LOANED_BOOK = auto()
    ```

    注意将 `SEARCH_BOOKS = auto()` 添加到 `CommonActions`。

1. 打开 library/console/console_app.py 文件。****

1. 找到并选择 `ConsoleApp` 类中的 `write_input_options` 方法。 将鼠标悬停在所选内容上，右键单击打开菜单，选择“Copilot”，然后选择“编辑器内联聊天”。********

    需要添加对新 `CommonActions.SEARCH_BOOKS` 选项的支持。 如果存在 `SEARCH_BOOKS` 选项，则显示检查书籍是否可供借阅的选项。

1. 打开内联聊天，然后输入以下提示：

    ```plaintext
    Update selection to include an option for the `CommonActions.SEARCH_BOOKS` action. Use the letter "b" and the message "to check for book availability".
    ```

    GitHub Copilot 应该建议进行代码更新，以便为 `SEARCH_BOOKS` 操作添加新的 `if` 块。

1. 查看建议的更新，然后选择“接受”****。

    建议的更新应类似于以下代码片段：

    ```python
        def write_input_options(self, options):
        print("Input Options:")
        if options & CommonActions.RETURN_LOANED_BOOK:
            print(' - "r" to mark as returned')
        if options & CommonActions.EXTEND_LOANED_BOOK:
            print(' - "e" to extend the book loan')
        if options & CommonActions.RENEW_PATRON_MEMBERSHIP:
            print(' - "m" to extend patron\'s membership')
        if options & CommonActions.SEARCH_PATRONS:
            print(' - "s" for new search')
        if options & CommonActions.SEARCH_BOOKS:
            print(' - "b" to check for book availability')
        if options & CommonActions.QUIT:
            print(' - "q" to quit')
        if options & CommonActions.SELECT:
            print(' - type a number to select a list item.')
    ```

1. 向下滚动以找到并选择 library/console/console_app.py 文件中的 `_handle_patron_details_selection` 方法（或输入处理部分）。****

    你再次需要添加对新 `CommonActions.SEARCH_BOOKS` 选项的支持。 包含处理用户选择 `SEARCH_BOOKS` 操作的一个用例（例如，当用户输入“b”时）。

1. 打开内联聊天，然后输入以下提示：

    ```plaintext
    Update selection to include an option for the `CommonActions.SEARCH_BOOKS` action.
    ```

    GitHub Copilot 应该建议进行更新，以添加新的 `elif` 块来处理用户选择 `SEARCH_BOOKS` 操作的情况。

1. 查看建议的更新，然后选择“接受”****。

    建议的更新应类似于以下代码片段：

    ```python
    def _handle_patron_details_selection(self, selection, patron, valid_loans):
    # ...existing code...

        elif selection == 'b':
            # Placeholder for book search functionality
            print("Book search functionality is not implemented yet.")
            return ConsoleState.PATRON_DETAILS
        elif selection.isdigit():
            idx = int(selection)
            if 1 <= idx <= len(valid_loans):
                self.selected_loan_details = valid_loans[idx - 1][1]
                return ConsoleState.LOAN_DETAILS
            print("Invalid selection. Please enter a number shown in the list above.")
            return ConsoleState.PATRON_DETAILS
        else:
            print("Invalid input. Please enter a number, 'm', 's', 'b', or 'q'.")
            return ConsoleState.PATRON_DETAILS
    ```

#### 使用 Copilot 内联聊天将代码选择共享到 GitHub Copilot 聊天

1. 确保 GitHub Copilot 聊天在询问模式下打开。****

1. 需要完成以下两项操作：

    - 在调用 `_get_patron_details_input` 之前，需要将 `CommonActions.SEARCH_BOOKS` 添加到 `options`。
    - 还需要添加一个 `if` 或 `elif` 块来处理 `SEARCH_BOOKS` 操作的 `"b"` 选择。 该块应该调用名为 `search_books` 的新方法。

    可以使用同一提示来满足这两个要求。

1. 在 library/console/console_app.py 文件中找到并选择 `patron_details` 方法。********

1. 在仍然选择 `patron_details` 方法的情况下，将鼠标悬停在所选内容上。 右键单击打开一个菜单，选择“Copilot”，然后选择“添添加选择到聊天”。************

1. 输入以下提示：

    ```plaintext
    @workspace Update selection to add `CommonActions.SEARCH_BOOKS` to `options` before calling `_get_patron_details_input`. Add an `if` or `elif` block to handle the `"b"` selection for the `SEARCH_BOOKS` action. The block should call a new method named `search_books`.
    ```

    GitHub Copilot 聊天应该建议进行代码更新，以便在调用 `_get_patron_details_input` 之前将 `CommonActions.SEARCH_BOOKS` 添加到 `options`。 对于提供的代码，请选择“在编辑器中应用”图标。

    ![显示 GitHub Copilot 询问模式的屏幕截图 - 在编辑器中应用图标。](./Media/m03-github-copilot-chat-view-response-ask-mode.png)

1. 查看建议的更新，然后选择“接受”****。
    ```python
    def patron_details(self) -> ConsoleState:
        patron = self.selected_patron_details
        print(f"\nName: {patron.name}")
        print(f"Membership Expiration: {patron.membership_end}")
        loans = self._loan_repository.get_loans_by_patron_id(patron.id)
        print("\nBook Loans History:")

        valid_loans = self._print_loans(loans)

        if valid_loans:
            options = (
                CommonActions.RENEW_PATRON_MEMBERSHIP
                | CommonActions.SEARCH_PATRONS
                | CommonActions.QUIT
                | CommonActions.SELECT
                | CommonActions.SEARCH_BOOKS  # Added SEARCH_BOOKS to options
            )
            selection = self._get_patron_details_input(options)
            return self._handle_patron_details_selection(selection, patron, valid_loans)
        else:
            print("No valid loans for this patron.")
            options = (
                CommonActions.SEARCH_PATRONS
                | CommonActions.QUIT
                | CommonActions.SEARCH_BOOKS  # Added SEARCH_BOOKS to options
            )
            selection = self._get_patron_details_input(options)
            return self._handle_no_loans_selection(selection)

    def _handle_patron_details_selection(self, selection, patron, valid_loans):
        if selection == 'q':
            return ConsoleState.QUIT
        elif selection == 's':
            return ConsoleState.PATRON_SEARCH
        elif selection == 'm':
            status = self._patron_service.renew_membership(patron.id)
            print(status)
            self.selected_patron_details = self._patron_repository.get_patron(patron.id)
            return ConsoleState.PATRON_DETAILS
        elif selection == 'b':
            return self.search_books()  # Call the new search_books method
        elif selection.isdigit():
            idx = int(selection)
            if 1 <= idx <= len(valid_loans):
                self.selected_loan_details = valid_loans[idx - 1][1]
                return ConsoleState.LOAN_DETAILS
            print("Invalid selection. Please enter a number shown in the list above.")
            return ConsoleState.PATRON_DETAILS
        else:
            print("Invalid input. Please enter a number, 'm', 's', 'b', or 'q'.")
            return ConsoleState.PATRON_DETAILS

    def _handle_no_loans_selection(self, selection):
        if selection == 'q':
            return ConsoleState.QUIT
        elif selection == 's':
            return ConsoleState.PATRON_SEARCH
        elif selection == 'b':
            return self.search_books()  # Handle SEARCH_BOOKS when no loans
        else:
            print("Invalid input.")
            return ConsoleState.PATRON_DETAILS

    def search_books(self) -> ConsoleState:
        print("Book search functionality is not implemented yet.")
        return ConsoleState.PATRON_DETAILS

    ```
<!-- TODO: Delete or restore? Previous version that resulted in the Agent bug fix
     ```python

    def patron_details(self) -> ConsoleState:
    patron = self.selected_patron_details
    print(f"\nName: {patron.name}")
    print(f"Membership Expiration: {patron.membership_end}")
    loans = self._loan_repository.get_loans_by_patron_id(patron.id)
    print("\nBook Loans History:")

    valid_loans = self._print_loans(loans)

    if valid_loans:
        options = (
            CommonActions.RENEW_PATRON_MEMBERSHIP
            | CommonActions.SEARCH_PATRONS
            | CommonActions.QUIT
            | CommonActions.SELECT
            | CommonActions.SEARCH_BOOKS
        )
        selection = self._get_patron_details_input(options)
        if selection == 'b':
            return self.search_books()
        return self._handle_patron_details_selection(selection, patron, valid_loans)
    else:
        print("No valid loans for this patron.")
        options = (
            CommonActions.SEARCH_PATRONS
            | CommonActions.QUIT
            | CommonActions.SEARCH_BOOKS
        )
        selection = self._get_patron_details_input(options)
        if selection == 'b':
            return self.search_books()
        return self._handle_no_loans_selection(selection)

    def search_books(self) -> ConsoleState:
        print("Book search option selected.")
        return ConsoleState.PATRON_DETAILS
    ``` 

-->

    > **NOTE**: The code suggested by Inline chat may include stub code for the **search_books()** method as in the previous code sample. You can accept that code stub, but you'll implement the **search_books** method in the next section.

### 使用 Copilot 聊天询问模式实现 SEARCH_BOOKS 方法

实现“书籍可借状态”更新还剩一个步骤，即创建 search_books 方法。**** search_books 方法将读取用户提供的书名，检查此书是否可供借阅，并显示一条消息，指出该书籍的可借状态。**** 你将使用“聊天”视图来评估要求并实现 search_books 方法。****

GitHub Copilot 的“聊天”视图提供了使用内联聊天时不可用的对话和交互式环境。 可以使用“聊天”视图提问、请求代码建议，以及获取 GitHub Copilot 生成的代码的说明。 “聊天”视图支持以下三种模式：

- 要求模式：使用询问模式更好地了解代码库、集思广益和编码任务的帮助。 在询问模式下生成的代码建议可以直接实现到代码库中或复制到剪贴板。
- 编辑模式：编辑模式用于更改代码，例如重构或添加新功能。 编辑模式可以跨项目中的多个文件进行编辑。
- 智能体模式：智能体模式用于定义高级任务，并启动智能体代码编辑会话来完成该任务。 在智能体模式下，Copilot 会自主规划所需的工作，并确定相关的文件和上下文。 代理可以更改代码、运行测试，甚至部署应用程序。
<!-- TODO: line below do we use all 3 listed modes? -->
你将使用内联聊天、询问模式和编辑模式实现 search_books 方法。****************

请使用以下步骤完成本练习的这一部分：

1. 花点时间思考一下 search_books 方法的过程要求。****

    方法需要完成的过程是什么？ 此方法的返回类型是什么？ 它是否需要参数？

    search_books 方法应实现以下过程：****

    1. 提示用户输入书籍标题。
    1. 读取用户提供的书籍标题。
    1. 检查书籍是否可供借阅。
    1. 显示一条消息，其中指明了以下选项之一：

        - “{book.title} 可供借阅”****
        - “{book.title}”已借给其他借阅者。**** 归还截止日期为“{loan.due_date}”。****

    若要生成消息选项，代码需要访问以下 JSON 文件：

    - 需要 Books.json 才能找到匹配的 Title 和 Id。************
    - 需要使用 BookItems.json 来查找每本实体书的 BookId（其中 BookId 与 Books.json 中的 Id 匹配）。********************
    - 需要使用 Loans.json 来查找匹配的 BookItemId 的 ReturnDate 和 DueDate（其中 BookItemId 与 BookItems.json 中的 Id 匹配）。****************************

> **注意:**  
> Loans.json 中的 BookItemId 指的是 BookItems.json 中的 Id。****************  
> BookItems.json 中的 BookId 指的是 Books.json 中的 Id。****************

1. 确保在 console_app.py 文件中创建了以下 search_books 方法：********

    ```python

    def search_books(self) -> ConsoleState:
        print("Book search functionality is not implemented yet.")
        return ConsoleState.PATRON_DETAILS

    ```

    > 注意****：请务必删除 GitHub Copilot 创建的所有代码注释。 不必要和不准确的注释会对 GitHub Copilot 的建议产生负面影响。

1. 在 VSCode 中打开 console_app.py，选择 search_books 方法。********

1. 打开“聊天”视图，然后输入以下提示：

    ```plaintext
    @workspace Update selection to obtain a book title. Prompt the user to "Enter a book title to search for". Read the user input and ensure the book title isn't null.
    ```

1. 查看建议的更新。

    建议的更新应类似于以下代码片段：

    ```python

    def search_books(self) -> ConsoleState:
        while True:
            book_title = input("Enter a book title to search for: ").strip()
            if not book_title:
                print("No input provided. Please enter a book title.")
            else:
                # Placeholder for future book search logic
                print(f"Searching for book titled: {book_title}")
                break
        return ConsoleState.PATRON_DETAILS

    ```

1. 将鼠标指针悬停在建议的代码上，然后选择“应用到 library\console\console_app.py”。****

    建议的代码应显示在代码编辑器中，其中包含“保留”或“撤消”选项。********

1. 在代码编辑器中，若要接受建议的代码，请选择“保留”。****

1. 选择“search_books”方法。****

1. 使用拖放操作将以下文件添加到聊天上下文：

    - library/console/console_app.py（已选择当前文件）****
    - library/console/main.py****
    - library/infrastructure/Json/Books.json****
    - library/infrastructure/Json/Loans.json****
    - library/infrastructure/Json/BookItems.json****
    - library/infrastructure/json_data.py****
    - library/infrastructure/json_loan_repository.py****

1. 输入以下提示：

    ```plaintext
    @workspace provide a simple text overview plan of how to update the search_books method and ConsoleApps class to find a matching book title in the Books.json file. Use the user supplied book title to find a matching book. If a book is found, use Loans.json to check if the book is available for loan. If the book has been returned, display a message stating "book.title is available for loan". If the book is on loan, display a message stating "book.title is on loan to another patron. The return due date is loan.DueDate". Don't provide code yet. List the general change recommendations for each high level item as a numbered list of paragraphs.
    ```

1. 请花点时间来查看建议的更新。

    回复应包含以下信息：

    ```plaintext

    1. Update the ConsoleApp class to access book data:  
    Add a reference to the book repository or the loaded books data from JsonData within the ConsoleApp class. This will allow the search_books method to access the list of all books and perform searches based on user input.
    
    2. Modify the search_books method to search for a book:  
    Update the search_books method to use the user-supplied book title to search for a matching book in the loaded books data (from Books.json). The search should be case-insensitive and match the title exactly or partially, depending on requirements.
    
    3. Check book availability using loan data:  
    Once a matching book is found, use the book's ID to find all related book items (copies) and then check the Loans.json data to determine if any copies are currently on loan (i.e., have a loan record with a null ReturnDate). If all copies are returned, the book is available for loan.
    
    4. Display book availability status to the user:  
    If at least one copy of the book is available (not currently on loan), display a message such as "book.title is available for loan." If all copies are on loan, display a message such as "book.title is on loan to another patron. The return due date is loan.DueDate," using the due date from the active loan.
    
    5. Handle no match or multiple matches:  
    If no book matches the search, display an appropriate message to the user. Optionally, if multiple books match, prompt the user to refine their search or select from a list.
    
    6. Keep user in the book search loop:  
    After displaying the result, allow the user to search for another book or return to the previous menu, maintaining a smooth user experience.

    ```

    > **注意**：你可能已在 GitHub Copilot 回复中收到了代码更新。 
    > 代码更新将在下一步中解决。

    还可以使用“聊天”视图的询问模式分析代码更新，然后使用编辑模式实现代码更新。********

1. 查看示例提示（你稍后将提示）。

    ```plaintext

    Update the search_books method and ConsoleApp class so that when a user enters a book title, the app searches 
    Books.json for a matching title (case-insensitive, partial match allowed). If a match is found, check all 
    related BookItem records and their loan status in Loans.json. If any copy is not currently on loan (no active 
    loan or has a ReturnDate), display "book.title is available for loan". If all copies are on loan, display 
    "book.title is on loan to another patron. The return due date is loan.DueDate" (show the soonest due date). 
    Integrate this logic into the user flow and ensure clear user messaging.
    ```

    可以通过要求 Copilot 从 Copilot 反馈生成提示来调整提示以达到特定要求。 以下是 Copilot 生成的示例提示，以供查看：

    ```plaintext

    Update the ConsoleApp class and its search_books method to implement the following:
    
    1. Add access to the loaded books data (from JsonData or a book repository) in ConsoleApp.
    2. In search_books, use the user-supplied book title to search for a matching book in Books.json (case-insensitive, partial or exact match).
    3. If a book is found, use its ID to find all related book items (copies) and check Loans.json to see if any copies are currently on loan (ReturnDate is null).
    4. If at least one copy is available, display: "book.title is available for loan." If all are on loan, display: "book.title is on loan to another patron. The return due date is loan.DueDate."
    5. If no book matches, inform the user. If multiple books match, prompt for refinement or selection.
    6. After displaying the result, allow the user to search again or return to the previous menu.
        
    Please provide the complete implementation for this book search and availability feature.
    ```

### 使用 Copilot 聊天编辑模式实现 search_books 方法

1. 若要将“聊天”视图切换到编辑模式，请选择“设置模式”，然后选择“编辑”。********

    当系统提示开始新会话时，请选择“是”。****

1. 通过使用聊天编辑模式，使用拖放操作将以下文件添加到聊天上下文：****

    - library/console/console_app.py****
    - library/console/main.py****
    - library/infrastructure/json_data.py****
    - library/infrastructure/Json/Books.json****
    - library/infrastructure/Json/Loans.json****
    - library/infrastructure/Json/BookItems.json****

1. 从 console_app.py 中选择 search_books 方法。********

1. 输入以下提示：

    ```plaintext

    @workspace Update the ConsoleApp class so it can access the loaded books data from JsonData or a book repository, and update main.py to instantiate ConsoleApp with the loaded JsonData instance by passing json_data=json_data. In the search_books method, prompt the user for a book title and search for a matching book in Books.json using a case-insensitive, partial or exact match. If a book is found, use its ID to find all related book items (copies) and check Loans.json to determine if any copies are currently on loan (ReturnDate is null). If at least one copy is available, display a message stating the book is available for loan; if all are on loan, display a message with the book title and the due date of the loan. If no book matches, inform the user, and if multiple books match, prompt for refinement or selection. After displaying the result, allow the user to search again or return to the previous menu.
    ```

1. 花点时间查看 console_app.py 文件中建议的更新。

    可以使用“上一页”和“下一页”浏览建议的代码更新，也可以手动滚动浏览文件。********

    **console_app.py**

    在 ConsoleApp 类顶部附近可以找到将 json_data 依赖项添加到 console_app 构造函数的代码更新。********

    ```python
    class ConsoleApp:
        def **init**(
            self,
            loan_service: ILoanService,
            patron_service: IPatronService,
            patron_repository: IPatronRepository,
            loan_repository: ILoanRepository,
            json_data=None,  # <-- Add json_data for direct access to books/items
            book_repository=None  # <-- Optionally allow a book repo
        ):
            self._current_state: ConsoleState = ConsoleState.PATRON_SEARCH
            self.matching_patrons = []
            self.selected_patron_details = None
            self.selected_loan_details = None
            self._patron_repository = patron_repository
            self._loan_repository = loan_repository
            self._loan_service = loan_service
            self._patron_service = patron_service
            self._json_data = json_data   # <-- store json_data
            self._book_repository = book_repository
    ```

    代码完成了 `search_books` 方法以实现存根代码中的功能，检查是否使用 json_data 按书名查找书籍、检索其 BookItem、检查有效借阅情况以及根据要求显示可借状态或借阅状态。****

    ```python
        def search_books(self) -> ConsoleState:
        while True:
            book_title = input("Enter a book title to search for: ").strip()
            if not book_title:
                print("No book title provided. Please try again.")
                continue

            # Case-insensitive, partial or exact match
            books = self._json_data.books
            matches = [b for b in books if book_title.lower() in b.title.lower()]

            if not matches:
                print("No matching books found.")
                again = input("Search again? (y/n): ").strip().lower()
                if again == 'y':
                    continue
                else:
                    return ConsoleState.PATRON_DETAILS

            if len(matches) == 1:
                book = matches[0]
            else:
                print("\nMultiple books found:")
                for idx, b in enumerate(matches, 1):
                    print(f"{idx}) {b.title}")
                selection = input("Select a book by number or 'r' to refine search: ").strip().lower()
                if selection == 'r':
                    continue
                if not selection.isdigit() or not (1 <= int(selection) <= len(matches)):
                    print("Invalid selection.")
                    continue
                book = matches[int(selection) - 1]

            # Find all book items (copies) for this book
            book_items = [bi for bi in self._json_data.book_items if bi.book_id == book.id]
            if not book_items:
                print("No copies of this book are in the library.")
                again = input("Search again? (y/n): ").strip().lower()
                if again == 'y':
                    continue
                else:
                    return ConsoleState.PATRON_DETAILS

            # Find all loans for these book items
            loans = self._json_data.loans
            on_loan = []
            available = []
            for item in book_items:
                # Find latest loan for this item (if any)
                item_loans = [l for l in loans if l.book_item_id == item.id]
                if item_loans:
                    # Get the most recent loan (by LoanDate)
                    latest_loan = max(item_loans, key=lambda l: l.loan_date or l.due_date or l.return_date or 0)
                    if latest_loan.return_date is None:
                        on_loan.append(latest_loan)
                    else:
                        available.append(item)
                else:
                    available.append(item)

            if available:
                print(f"Book '{book.title}' is available for loan.")
            else:
                # All copies are on loan, show due dates
                due_dates = [l.due_date for l in on_loan if l.due_date]
                if due_dates:
                    next_due = min(due_dates)
                    print(f"All copies of '{book.title}' are currently on loan. Next due date: {next_due}")
                else:
                    print(f"All copies of '{book.title}' are currently on loan.")

            again = input("Search for another book? (y/n): ").strip().lower()
            if again == 'y':
                continue
            else:
                return ConsoleState.PATRON_DETAILS 
    ```

    **main.py**

    修改了 `ConsoleApp` 的实例化以传入 `json_data`，这样应用程序就可以访问 JSON 数据文件。

    ```python
    # ...existing code...
            app = ConsoleApp(
            loan_service=loan_service,
            patron_service=patron_service,
            patron_repository=patron_repo,
            loan_repository=loan_repo,
            json_data=json_data  # <-- Pass json_data so ConsoleApp can access books/items/loans
        )
    # ...existing code...
    
    ```

1. 在“聊天”视图中，若要保留所有编辑，请选择“保留”。****

    在接受更新之前，请务必查看 GitHub Copilot 建议。

    如果不确定建议的更新，则可以接受更改，然后要求 GitHub Copilot 给出解释。 如果拒绝接受更新，则可以还原编辑。

    > 注意****：如果 GitHub Copilot 建议使用特定区域设置或格式设置返回日期的格式，请确保使用 Python 的 datetime.strftime() 方法来根据需要设置日期的格式。

1. 确保已接受 console_app.py 和 main.py 文件中的更新。

1. 打开 Visual Studio Code 的“资源管理器”视图。

1. 运行你的测试或启动应用程序，以确保你的代码更新未引入任何错误。

    你可能会看到警告消息，但不应出现任何错误。

    若要运行测试，请在 Visual Studio Code 中打开终端，导航到 AccelerateDevGHCopilot/library 目录，然后运行：

    ```bash
    python -m unittest discover tests
    ```

## 将“书籍可借状态”更新合并到仓库的主分支中

在将代码合并到仓库的主分支之前，请务必对代码进行测试。 测试可确保代码按预期工作，且不会引入任何新问题。 在本练习中，你将使用手动测试来验证“书籍可借状态”功能是否按预期工作。

在本练习的本部分，你需要完成以下任务：

1. 测试“书籍可借状态”功能。
1. 将更改与远程仓库同步。
1. 创建一个拉取请求，以将更改合并到存储库的 main 分支中。

### 测试“书籍可借状态”功能

手动测试可用于验证新功能是否按预期工作。 必须使用可验证的数据源。 在这种情况下，使用 Books.json 和 Loans.json 文件来验证新功能是否正确报告书籍的可借状态。********

请使用以下步骤完成本练习的这一部分：

1. 若要运行应用程序，请在 Visual Studio Code 中打开终端，导航到 `AccelerateDevGHCopilot/library` 目录，然后运行：

```bash
python console/main.py
```

1. 当系统提示你输入借阅者姓名时，请键入 1，然后按 Enter****。

    你应会看到与搜索查询匹配的借阅者列表。

1. 在“输入选项”提示处键入 2（选择“Patron One”），然后按 Enter 键。****

    - 输入 2 会选择列表中的第二位借阅者****。
    - 你应会看到借阅者的姓名和会员状态，以及书籍的借阅详细信息。

1. 若要选择“Book One”，其中显示了“Returned:False”，请输入：1。****

   - 请注意，状态为“Returned:**False”**
   - 或者查找以下状态的书籍的借阅者：“Returned:**False”。** 记下该书的名称。

1. 若要归还书籍，请选择：r****

    - 请注意，状态已更新为“Returned:**真实**

1. 再次检查“Patron One”的书籍：

    - 若要搜索借阅者，请选择：s****
    - 针对 Patron One 重复步骤搜索借阅者。********
    - 若要选择“Book One”，其中显示了“Returned:True”，请输入：**1**

    请注意，显示了书名，但没有可借状态信息。

1. 检查书籍可借状态并借阅可用书籍。

    - 检查书籍可借状态：b****
    - 搜索可借阅书籍，输入：One****

    请注意，显示了书名，可借状态显示为“Book One is available for loan.” 另请注意，没有签出“Book One”的选项。

1. 你会被问及“Search for another book? (y/n)”，请输入：n****

1. 输入“q”退出以停止应用程序。

**应用程序存在问题**。 正在报告书籍可借状态，但没有签出书籍的选项。

### 使用智能体模式修复签出书籍 bug

Copilot 是一种 AI 工具，就像人类一样，它也会犯错误，因此你需要检查结果。 若要修复此 bug，请使用智能体模式。

1. 打开 Github Copilot 聊天并选择“智能体模式”。****
1. 若要将手动测试从终端添加到聊天中，请执行以下操作：

    - 在“聊天”窗口中，选择 Copilot 聊天中的“添加上下文”（确保它处于智能体模式！）****
    - 键入“Tools”，然后键入“Tools”，并使用鼠标或“Enter”键进行选择。
    - 输入“terminalSelection”并选择“terminalLastCommand”终端的最后运行命令****

1. 通过打开 VSCode 编辑器中列出的文件，将文件添加到聊天内容。 然后在 Copilot 聊天窗口中，选择“添加上下文”，然后选择“打开编辑器”：********

    - **console_app.py**（包含主要应用程序逻辑和用户交互）
    - **loan_service.py**（借阅逻辑）
    - **json_data.py**（处理书籍、书籍条目、借阅者、作者、借阅的加载/保存）
    - **json_loan_repository.py**（处理检索、添加和更新借阅记录）
    - **Books.json**（示例书籍数据）
    - **BookItems.json**（示例书籍条目/复制数据）
    - **Patrons.json** （示例借阅者数据）
    - **Loans.json**（示例借阅/签出数据）
    - **Authors.json**（示例作者数据）

1. 查看并输入解决 bug 的提示（确保 Copilot 聊天显示“智能体”）：****

    ```plaintext
    @workspace My goal is to make it possible for a patron to check out an available book directly from the console app menu. Right now, when I search for a book and it is available, the app only tells me "...is available for loan" but doesn’t let me check it out. Please help me update the code so that when a book is available, the app prompts me to check it out, and if I choose yes, it creates a new loan for the current patron and book. Make sure the checkout works smoothly in the menu flow. For the loan_service add checkout functionality and ensure the console app calls this method. Only use the existing enum and menu option names as defined in the codebase. Only use enum values and menu option names that are already defined in the codebase. Do not invent, rename, or add new enum values or menu options.
    ```

    你应该注意到智能体读取多个文件的特定行。 它会进行更新，然后检查并进行进一步的更新。

1. 查看 console-app.py 中靠近 `search_books` 方法底部的智能体模式更新，该更新应类似于以下代码：****

    ```python
    def search_books(self) -> ConsoleState:
    # ...existing code...

           if available:
                print(f"Book '{book.title}' is available for loan.")
                # Prompt to check out
                checkout = input("Would you like to check out this book? (y/n): ").strip().lower()
                if checkout == 'y':
                    if not self.selected_patron_details:
                        print("No patron selected. Please select a patron first.")
                        return ConsoleState.PATRON_SEARCH
                    # Use the first available copy
                    book_item = available[0]
                    # Create a new loan using the loan service
                    status = self._loan_service.create_loan(
                        patron_id=self.selected_patron_details.id,
                        book_item_id=book_item.id
                    )
                    print(status)
                    # Reload loans to reflect the new loan
                    if hasattr(self._json_data, 'load_data'):
                        self._json_data.load_data()
                    print(f"Book '{book.title}' checked out to {self.selected_patron_details.name}.")
                    return ConsoleState.PATRON_DETAILS
        # ...existing code...
    ```

1. 查看 loan_service.create_loan 的更新。****

```python
    def create_loan(self, patron_id: int, book_item_id: int):
        """Create a new loan for the given patron and book item, and add it to the repository."""
        from ..entities.loan import Loan
        from datetime import datetime, timedelta
        # Check if the book item is already on loan
        all_loans = getattr(self._loan_repository, 'get_all_loans', lambda: [])()
        for loan in all_loans:
            if loan.book_item_id == book_item_id and loan.return_date is None:
                return "This copy is already on loan."
        # Generate a new loan ID
        max_id = max((l.id for l in all_loans if hasattr(l, 'id')), default=0)
        new_id = max_id + 1
        now = datetime.now()
        due = now + timedelta(days=14)
        new_loan = Loan(
            id=new_id,
            book_item_id=book_item_id,
            patron_id=patron_id,
            loan_date=now,
            due_date=due,
            return_date=None
        )
        self._loan_repository.add_loan(new_loan)
        return f"Loan created. Due date: {due.date()}"

```

1. 请注意 ConsoleApp.search_books 和 LoanService 的更新：

    search_books 和 loan_service 方法现已更新，以便在书籍可借阅时，应用程序会提示用户签出。如果用户选择“是”，则会为当前借阅者和所选书籍条目创建新的借阅，并且菜单流会返回到借阅者详细信息。 签出过程现已顺利集成到菜单流中。

1. 接受更改。

1. 重新测试前面的测试步骤并完成书籍签出，以确保修复正确。**** 如果问题仍然存在，请根据需要继续使用智能体或询问模式进行故障排除。

### 将更改与远程存储库同步

1. 选择“源代码管理”视图。

1. 确保更新的文件列在“更改”下****。

    你应该会看到 common_actions.py 和 console_app.py 文件列在“更改”下。**** 还会列出 main.py 文件。

1. 使用 GitHub Copilot 为“提交”生成消息****。

    ![显示“使用 Copilot 生成提交消息”按钮的屏幕截图。](./Media/m03-github-copilot-commit-message-python.png)

1. 若要暂存并提交更改，请选择“提交”，然后选择“是”********。

1. 若要将更改同步到远程仓库，请选择“同步更改”。****

### 创建拉取请求以将更改合并到 main 分支

你已实现帮助图书管理员确定书籍可借状态的功能。 现在需要将更改合并到存储库的 main 分支中。 可以创建一个拉取请求，以将更改合并到 main 分支中。

请使用以下步骤完成本练习的这一部分：

1. 在 Web 浏览器中打开 GitHub 存储库。

    若要从 Visual Studio Code 打开 GitHub 仓库，请执行以下操作：

    1. 在 Visual Studio Code 窗口左下角，选择“book-availability”。****
    1. 在上下文菜单中，在 book-availability 分支右侧，选择“在 GitHub 中打开”图标。********

1. 在 GitHub 仓库页上，选择“比较和拉取请求”选项卡。****

1. 确保“Base”中指定了“main”，“compare”指定了“book-availability”，并选中“Able to merge”。********************

1. 在“添加说明”下，选择“Copilot Actions”按钮（GitHub Copilot 图标），然后选择生成摘要选项。****

    > 注意****：GitHub Copilot 免费版计划目前不支持拉取请求摘要功能。

    如果使用 GitHub Copilot 免费版计划，则可以编写自己的摘要，或使用下面的摘要来完成拉取请求。

    ```plaintext

    This pull request introduces a new feature to the library console application: the ability to search for books and check their availability. It also includes updates to dependency injection and the CommonActions enumeration to support this functionality. Below are the most important changes grouped by theme.

    New Feature: Book Search

    Added a new SEARCH_BOOKS action to the CommonActions enumeration (library\console\common_actions.py).

    Updated PatronDetails method to handle the SEARCH_BOOKS action, including a new SEARCH_BOOKS method that allows users to search for a book by title and check its availability (library\console\console_app.py).

    Modified ReadInputOptions and WriteInputOptions methods to include the new SEARCH_BOOKS option (library/console/console_app.py).

    Dependency Injection Updates

    Added JsonData as a dependency in the ConsoleApp constructor and ensured it is registered in the DI container before ConsoleApp (library/console/console_app.py, library/console/main.py).

    ```

1. 生成摘要后，选择“预览”。****

1. 花点时间查看摘要。

    GitHub Copilot 生成的拉取请求摘要应类似于以下示例：

    ![显示使用 GitHub Copilot Enterprise 帐户生成的拉取请求摘要的屏幕截图。](./Media/m03-github-copilot-pull-request-summary.png)

1. 选择“创建拉取请求”****。

1. 如果所有检查都通过并且与基分支没有冲突，请选择“合并拉取请求”，然后选择“确认合并”。********

    请注意，合并更改后可以删除 book-availability 分支。**** 若要删除分支，请选择“删除分支”。****

1. 切换回“Visual Studio Code”窗口。

1. 切换到存储库的 main 分支****。

1. 打开“源代码管理”视图，然后从远程仓库拉取更改。****

1. 验证 book-availability 功能是否在主分支中可用。****

## 总结

在本练习中，你学习了如何使用 GitHub Copilot 为 Python 应用程序开发新的代码功能。 你使用 GitHub Copilot 的内联聊天和“聊天”视图在新分支中开发了功能，测试了代码，然后将更改合并到仓库的主分支中。 还使用了 GitHub Copilot 生成提交消息和拉取请求摘要。

## 清理

现在，你已经完成了本练习，请花点时间确保你没有对 GitHub 帐户或 GitHub Copilot 订阅做出任何你不希望保留的更改。 如果做出了任何更改，请立即还原。
