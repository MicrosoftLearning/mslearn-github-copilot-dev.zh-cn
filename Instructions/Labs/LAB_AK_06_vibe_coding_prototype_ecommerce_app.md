---
lab:
  title: 练习 - 开始使用 GitHub Copilot 智能体进行 Vibe 编码
  description: 了解如何使用 Vibe 编码过程和 GitHub Copilot 智能体创建原型应用。
---

# 开始使用 GitHub Copilot 智能体进行 Vibe 编码

Vibe 编码是一种使用 AI 工具（如 GitHub Copilot 智能体）来生成软件的编程方法。 用户无需手动编写代码，而是提供预期应用的自然语言说明，AI 就会生成相应的代码。 这将程序员的角色从传统的编码转变为指导、测试和优化 AI 生成的输出。

在本练习中，使用 Vibe 编码过程和 GitHub Copilot 智能体创建在线购物应用的原型版本。 原型应用包含以下页面：产品、产品详细信息、购物车和结帐。该应用包括页面之间的基本导航和有助于演示应用功能的有限数据集。 原型不包括任何后端功能，例如用户身份验证、付款处理或数据库集成。

完成此练习大约需要 30 分钟****。

> **重要说明**：要完成本练习，必须提供自己的 GitHub 帐户和 GitHub Copilot 订阅。 如果没有 GitHub 帐户，可以<a href="https://github.com/" target="_blank">注册</a>免费的个人帐户，并使用 GitHub Copilot 免费版计划来完成练习。 如果可从实验室环境中访问 GitHub Copilot Pro、GitHub Copilot Pro+、GitHub Copilot Business 或 GitHub Copilot Enterprise 订阅，则可以使用现有的 GitHub Copilot 订阅完成本练习。

## 开始之前

实验室环境必须包括以下内容：

- Visual Studio Code。
- 对启用了 GitHub Copilot 的 GitHub 帐户的访问。

如果将本地电脑用作本练习的实验室环境：

- 可以从以下 URL 下载 Visual Studio Code 安装程序文件：<a href="https://code.visualstudio.com/download" target="_blank">下载 Visual Studio Code</a>。

- 有关在 Visual Studio Code 中启用 GitHub Copilot 订阅的帮助，请在浏览器中打开以下链接：<a href="https://go.microsoft.com/fwlink/?linkid=2320158" target="_blank">在 Visual Studio Code 中启用 GitHub Copilot</a>。

如果使用支持本练习的托管实验室环境：

- 有关在 Visual Studio Code 中启用 GitHub Copilot 订阅的帮助，请打开一个浏览器并将以下 URL 粘贴到站点导航栏中：<a href="https://go.microsoft.com/fwlink/?linkid=2320158" target="_blank">在 Visual Studio Code 中启用 GitHub Copilot</a>。

## 练习场景

你是一位企业家，想要使用 Vibe 编码过程来创建一个原型购物应用。 你的初始原型需要展示用户期望联机购物应用具备的基本功能，以及你所设想的特定功能。

确定以下基本规范以开始开发过程：

1. 使用 HTML、CSS 和 JavaScript 创建一个客户端 Web 应用。
2. 包括以下网页：产品、产品详细信息、购物车和结账。
3. 启用页面之间的导航。

本练习包括以下任务：

1. 定义产品需求：**** 使用 GitHub Copilot，帮助将基本规范转换为更详细的产品需求。

1. 创建初始原型应用：**** 使用 GitHub Copilot 智能体和你的产品需求创建一个初始原型应用。

1. 优化原型应用：**** 使用 GitHub Copilot 智能体完成一系列迭代更新，以优化用户体验并确保应用满足预期要求。

> 注意****：原型应用是演示其视觉设计和用户体验的应用程序的早期交互式模型。 在本练习中，原型应用应实现基本功能并满足少量高级用例的需求。

## 定义产品需求

要让 AI 智能体开发你设想的应用，它需要理解你的产品需求和预期的用户体验。 可以使用以下任一过程，将你的意图传达给 GitHub Copilot 智能体：

- 先编码，然后迭代以定义需求：**** 此方法从最少的基本规范开始，直接进入编码。 随着开发的进行，应用通过迭代周期有组织地发展，逐渐塑造了产品的功能和用户体验。 当你探索 AI 实现的功能时，此方法可能会偏离你的原始愿景（无论情况好坏）。 AI 主导的过程可能会意外耗时，并且可能不会产生所需的结果（尤其是在初始规范模糊或开放的情况下）。

- 编码前探索需求：**** 此方法从一开始就强调明确性。 在编写任何代码之前，与 AI 协作起草产品需求文档 (PRD)。 PRD 概述了应用的目的、目标用户、关键功能和技术约束。 通过预先确立一个明确的愿景，你为 AI 提供了一个坚实的基础，以生成与你的目标一致的代码——减少歧义，提高构建你真正想要的应用的机会。

在此任务中，使用 GitHub Copilot 评估基本规范并开发原型应用的产品需求。

请使用以下步骤完成本练习的这一部分：

1. 打开 Visual Studio Code。

1. 在“文件”菜单上，选择“向工作区添加文件夹”****。

1. 在“向工作区添加文件夹”对话框中，导航到易于查找的文件夹位置，创建一个名为 VibeCoding-PrototypeApp 的新文件夹，然后选择“添加”************。

    该文件夹位置应位于任何现有 Git 存储库之外，并且应该易于查找。 例如，如果你使用 Windows 电脑，可以在“桌面”或“文档”目录中创建一个名为 VibeCoding-PrototypeApp 的新文件夹************。

    完成本实验室练习后，可以存档或删除该代码项目。

1. 打开 GitHub Copilot 的“聊天”视图。

    可以通过选择位于 Visual Studio Code 窗口顶部中心附近的 GitHub Copilot 图标（就在搜索文本框右侧）来打开“聊天”视图。

1. 确保聊天模式设置为“提问”，且已选择“GPT-4.1”模型********。

    “设置模式”和“选取模型”下拉菜单位于“聊天”视图的左下角****。

    GitHub Copilot 模式：**** 尽管它们的功能有所重叠，但每种聊天模式（提问、编辑和智能体）都针对特定目的进行了优化：

    - 提问：**** 使用此模式可以向 GitHub Copilot 询问关于代码库的问题。 可以使用“提问”模式解释代码、提供更改建议或提供有关代码库的信息。
    - 编辑：**** 使用此模式编辑工作区中的特定代码文件。 可以使用“编辑”模式重构代码、添加注释、实现测试或向应用添加新功能。
    - 代理****：使用此模式以智能体身份运行 GitHub Copilot。 可以使用“智能体”模式自主执行编码任务。

    支持的模型：**** GitHub Copilot 支持多个模型，每个模型具有不同的优势。 某些模型优先考虑速度和成本效益，而另一些模型则针对准确性、推理或处理多模态输入（如图像和代码）进行了优化。 GitHub Copilot 的免费计划目前支持 GPT-4.1、GPT-4o、o3-mini、Claude Sonnet 3.5 和 Gemini 2.0 Flash。 GPT-4.1 模型对于本练习是一个不错的选择，因为它生成快速、准确的代码补全和解释，支持可视化输入，并有效地处理复杂任务。

    > 注意****：选择其他模型会影响从 GitHub Copilot 接收的响应。 建议对此练习使用 GPT-4.1 模型，但如果你想了解其他模型对你的提示的响应，可以使用其他模型重复本练习。

1. 在“聊天”视图中，输入以下提示：

    ```plaintext
    I want to create a product requirements document (PRD) for an app that I'll develop using a vibe coding process. I want the PRD to include information about the app's purpose, target audience, features, and technical requirements. I've defined the following high-level parameters for my app: 1 - Use HTML, CSS, and JavaScript to create a client-side web app. 2 - Include the following web pages: Products, ProductDetails, ShoppingCart, and Checkout. 3 - Enable navigation between pages. I want the prototype app to implement basic features and satisfy a small number of high-level use cases. The prototype should implement the following: basic use case functionality, simple navigation, a sample dataset, and basic styling. I'll be adding the PRD to the chat context, then asking GitHub Copilot Agent to create the prototype app. What sections should I include in the PRD to enable GitHub Copilot Agent to create my envisioned prototype?
    ```

1. 查看 GitHub Copilot 生成的响应。

    例如，响应可能类似于以下示例：

    ```md
    To enable GitHub Copilot Agent to create your envisioned prototype app, your Product Requirements Document (PRD) should include the following sections:
    
    1. App Overview / Purpose  
       - Briefly describe what the app does and its main goal.
    
    2. Target Audience  
       - Define who will use the app (e.g., general consumers, online shoppers, etc.).
    
    3. Core Features  
       - List and describe the main features (e.g., view products, view product details, add to cart, checkout).
    
    4. Page Descriptions  
       - Detail each required page (Products, ProductDetails, ShoppingCart, Checkout) and what should be displayed or possible on each.
    
    5. Navigation  
       - Explain how users move between pages (e.g., navigation bar, links, buttons).
    
    6. Sample Data  
       - Describe the sample dataset (e.g., a few example products with name, price, description, image).
    
    7. Technical Requirements  
       - Specify technologies to use (HTML, CSS, JavaScript, client-side only, no backend).
       - Mention any libraries or frameworks (if any) or state "vanilla JS".
    
    8. Styling  
       - Indicate the level of styling (e.g., basic, clean, minimal, responsive).
    
    9. Use Cases / User Flows  
       - List a few high-level use cases (e.g., browse products, view details, add to cart, proceed to checkout).
    
    10. Out of Scope  
        - Clarify what is not required (e.g., user authentication, payment processing, backend integration).
    
    Including these sections will give clear, actionable guidance for building your prototype app.
    ```

    > 注意****：如果你的响应包含不同的部分标题，或者这些部分按不同的顺序显示，请不要担心。 AI 工具生成的响应通常在不同的聊天会话中略有不同。 所选的 AI 模型、聊天历史记录和聊天会话的上下文也会影响响应。

1. 花几分钟时间考虑完成 PRD 的每个部分所需的信息。

    定义明确的 PRD 有助于确保 GitHub Copilot 智能体清楚地理解你对应用的愿景。 PRD 应提供足够的详细信息，使智能体能够创建满足你的需求和预期用户体验的原型应用。 PRD 应建立在本练习前面列出的基本规范之上。

    如果不确定要包含在特定部分中的信息，可以要求 GitHub Copilot 智能体帮助你生成该部分的内容。 例如，可以向 GitHub Copilot 询问有关“核心功能”或“用例”部分应包含哪些内容的想法。

    > **提示**：可以提供描述应用需求的自然语言文本，并让 GitHub Copilot 将该信息格式化为 PRD。 还可以使用 GitHub Copilot 来帮助查看和更新 PRD，并确保它提供 GitHub Copilot 智能体创建原型所需的详细程度。

1. 在“聊天”视图中，输入以下提示：

    ```plaintext
    The PRD sections that you suggested look good. Here's some information that should help you construct the PRD:

    My prototype app targets online shoppers interested in ordering my products. The prototype should include the following:

    - A dynamic user interface that scales automatically to appear correctly on large or small screens (desktop and phone devices).
    - A simple dataset that defines 10 fruit products. The dataset should include: product name, description, price per unit (where unit could be the number of items, ounces, pounds, etc.). If possible, I want to include a simple image (an emoji) that represents the product.
    - A navigation menu on the left side of the screen that allows users to navigate between the Products, ProductDetails, ShoppingCart, and Checkout pages.
    - Basic styling that makes the user interface visually appealing, but it doesn't need to be fully responsive or polished.

    The prototype app won't include any backend functionality, such as user authentication, payment processing, or database integration. It will be a static prototype that demonstrates the basic concepts.

    Here's a description of the user interface:

    - The Products page should display a list of products with basic information such as product name, price per unit, and an image (an emoji). The Products page should also provide a way to select a desired quantity of a product and an option to add selected items to the shopping cart.
    - The ProductDetails page should display detailed information about a product when the product is selected from the Products page. The ProductDetails page should display the product name, a description of the product, the price per unit, and an image (an emoji) representing the product. The ProductDetails page should also provide a way to navigate back to the Products page.
    - The ShoppingCart page should display the list of products added to the cart. The list should include the product name, quantity, and total price for that product. The ShoppingCart page should also provide a way to update the quantity of each product that's in the cart, and an option to remove products from the cart.
    - The Checkout page should display a summary of the products being purchased, including product name, quantity, and price. The total price should be clearly displayed along with the option to "Process Order".
    - The left-side navigation menu should provide basic navigation between pages. The navigation bar should collapse down to display a one or two letter abbreviation when the display width drops below 300 pixels. The navigation bar should allow users to navigate between the app pages.
    ```

    GitHub Copilot 应根据你提供的信息生成一个包含建议的 PRD 的响应。 响应应包括你之前查看的部分，并且应根据你提供的信息包括每个部分的内容。

1. 在“聊天”视图中，选择“智能体”模式。****

    “设置模式”下拉菜单位于“聊天”视图的左下角。

1. 在“聊天”视图中，输入以下提示：

    ```md
    Create a markdown file named VibeCodingPRD.md using your suggested sections and the inputs that I've provided.
    ```

1. 在“聊天”视图中，若要保存建议的 VibeCodingPRD.md 文件，请选择“保留”****。

1. 花几分钟来查看 PRD 文件。

    对于本练习，建议的 PRD 文件应该足够了。 但是，如果你发现任何差距或差异，或者想要添加更多信息，可以直接在 Visual Studio Code 中编辑该文件。 还可以要求 GitHub Copilot 通过生成附加信息或实施澄清来帮助优化 PRD。

1. 打开 Visual Studio Code 的“资源管理器”视图。

1. 在“聊天”视图中，输入以下提示：

    ```md
    Can you create low-fidelity wireframe diagrams (or text-based layouts) that represent the web pages and navigation bar (expanded and collapsed) for my prototype app? Use the PRD that I've provided as a reference.
    ```

    GitHub Copilot 智能体应生成基于文本的低保真布局，这些布局表示原型应用的用户界面。 这些“线框图”应有助于定义用户界面以及用户与应用交互的方式。

    例如，响应可能如下所示：

    ```md
    Certainly! Here are text-based, low-fidelity wireframe layouts for each main page of your prototype app, based on your PRD:

    ---

    ### 1. Products Page
    

    +------------------------------------------------------+
    | [Nav] Products | ProductDetails | Cart | Checkout    |
    +----------------+-------------------------------------+
    | 🍎 Apple         $1.00/each   [Qty: - 1 +] [Add]     |
    | 🍌 Banana        $0.50/each   [Qty: - 1 +] [Add]     |
    | 🍊 Orange        $0.80/each   [Qty: - 1 +] [Add]     |
    | ... (other fruits)                                  |
    +------------------------------------------------------+
    
    ---
    
    ### 2. ProductDetails Page
    
    +------------------------------------------------------+
    | [Nav] Products | ProductDetails | Cart | Checkout    |
    +----------------+-------------------------------------+
    | 🍎 Apple                                         |
    | Fresh, crisp apples                              |
    | Price: $1.00/each                               |
    | [Qty: - 1 +] [Add to Cart]                      |
    | [Back to Products]                              |
    +------------------------------------------------------+
    
    ---
    
    ### 3. ShoppingCart Page
    
    +------------------------------------------------------+
    | [Nav] Products | ProductDetails | Cart | Checkout    |
    +----------------+-------------------------------------+
    | Cart Items:                                       |
    | 🍎 Apple   Qty: [ - 2 + ]  $2.00   [Remove]        |
    | 🍌 Banana  Qty: [ - 1 + ]  $0.50   [Remove]        |
    | -------------------------------------------------- |
    | [Update Cart]                                     |
    | [Proceed to Checkout]                             |
    +------------------------------------------------------+
    
    ---
    
    ### 4. Checkout Page
    
    +------------------------------------------------------+
    | [Nav] Products | ProductDetails | Cart | Checkout    |
    +----------------+-------------------------------------+
    | Order Summary:                                     |
    | 🍎 Apple   x2   $2.00                              |
    | 🍌 Banana  x1   $0.50                              |
    | -------------------------------------------------- |
    | Total: $2.50                                      |
    | [Process Order]                                   |
    +------------------------------------------------------+
    
    ---
    
    ### 5. Navigation Bar (Left Side, Collapsible)
    
    +-------------------+      +-----+
    | Products          |      | P   |
    | ProductDetails    |      | D   |
    | Cart              |      | C   |
    | Checkout          |      | CO  |
    +-------------------+      +-----+
    (Full width)             (Collapsed <300px)
    
    ---
    
    These wireframes provide a clear, simple structure for each page and the navigation menu, matching your requirements. If you want these added to your PRD or as a separate markdown file, let me know!
    ```

    > 注意****：可通过多种其他方法创建线框图。 对于与 Vibe 编码过程一致的基于 AI 的方法，可以使用 Microsoft 的 M365 Copilot。 只需向 M365 Copilot 提供应用的说明（PRD 的内容），并要求 AI 创建低保真线框图的图像即可。 对于手动创建的高保真线框图，可以使用 UI/UX 设计工具（如 Figma）。

1. 在“聊天”视图中，输入以下提示：

    ```md
    Save the low-fidelity wireframe diagrams as text files, one file for each web page and one for navigation.
    ```

1. 监视“聊天”视图以确保所有文件都已保存，然后选择“保留”****。

1. 花几分钟时间来查看线框图。

    如果你发现任何要更正的明显问题，可以直接在 Visual Studio Code 中编辑线框图。 还可以要求 GitHub Copilot 帮助优化线框图。

    在本练习中，线框图（文本布局）不需要精确，建议的线框应该足够了，无需修改。 但是，如果你在练习中遇到归因于线框图的问题，可以要求 GitHub Copilot 智能体帮助优化这些线框图。

    > **提示**：如果你不确定如何解释线框图，或者如果你认为其中一个图可能不正确，可要求 GitHub Copilot 解释该图。 例如，可以要求 GitHub Copilot 智能体“查看线框图，并使用它们来解释用户界面的布局以及用户与应用交互的方式”。 如果 GitHub Copilot 的解释不符合你的预期，可以要求 GitHub Copilot 智能体帮助更新线框图，以更好地匹配预期用户体验。

## 创建初始原型应用

GitHub Copilot 智能体可以使用产品需求和线框图来开发原型应用程序。 提供足够详细的产品需求和线框图有助于智能体理解你打算为应用实现的用户体验、应用功能和设计目标。

- PRD 提供有关应用的用途、目标受众、功能和技术需求的详细信息。
- 线框图显示预期的用户界面，并帮助描述用户交互。

在此任务中，使用 GitHub Copilot 智能体，基于创建的 PRD 和线框图创建初始原型应用。

请使用以下步骤完成本练习的这一部分：

1. 在 Visual Studio Code 中，在 VibeCoding-PrototypeApp 文件夹中创建一个名为 ShoppingApp 的新文件夹****。

    GitHub Copilot 智能体需要一个空文件夹用作新应用文件的工作区。

    Visual Studio Code 中的“资源管理器”视图应如下所示：

    ```plaintext
    UNTITLED (WORKSPACE)
    └── VibeCoding-PrototypeApp
        ├── ShoppingApp
        ├── VibeCodingPRD.md
        ├── wireframe-checkout.txt
        ├── wireframe-navigation.txt
        ├── wireframe-product-details.txt
        ├── wireframe-products.txt
            ```└── wireframe-shopping-cart.txt
    ```

1. 将 PRD 和线框图添加到聊天上下文。

    将这些文件添加到聊天上下文会告知 GitHub Copilot 智能体在生成响应时引用这些文件。

    可以通过将文件从“资源管理器”视图拖放到“聊天”视图，或使用位于“聊天”视图左下角区域的“添加上下文”按钮，将文件添加到聊天上下文****。

1. 在“资源管理器”视图中，选择“ShoppingApp”文件夹****。

1. 在“聊天”视图中，输入以下提示：

    ```md
    I want you to create a prototype shopping app using the information in my PRD and wireframe diagrams. Create the prototype app in the selected 'ShoppingApp' folder. The prototype should implement the following: basic use case functionality, simple navigation, a sample dataset, and basic styling. After creating the prototype app, add a '.github/copilot-instructions.md' file to the workspace. Add the contents of the PRD and wireframe files to the 'copilot-instructions.md' file.
    ```

    GitHub Copilot 智能体使用此提示，根据你已定义的需求生成初始原型应用。

    - 智能体会检查“ShoppingApp”文件夹，以确保该文件夹为空且已准备好用作工作区****。
    - 智能体使用 PRD 和线框图来创建原型应用文件。 在“ShoppingApp”文件夹中创建以下文件：****

        - app.js：**** 包含实现应用功能（例如管理产品目录、购物车和导航）的 JavaScript 代码。
        - index.html：**** 充当 Web 应用程序的入口点，设置基本结构并链接样式和脚本。
        - styles.css：**** 为原型 Web 应用提供可视化布局和响应式设计。

    - 智能体将“.github/copilot-instructions.md”文件添加到工作区，然后将 PRD 和线框文件的内容添加到“copilot-instructions.md”文件中。********

    > **提示**：可以将自定义说明存储在“.github/copilot-instructions.md”文件中的工作区或存储库中。 使用自定义说明，可以描述通用准则或规则，以获取与特定编码做法和技术堆栈匹配的响应。 无需手动将此上下文包含在每个聊天查询中，自定义说明会自动将此信息与每个聊天请求合并。 这些说明仅适用于文件所在的工作区。

1. 监视“聊天”视图，以跟踪智能体在原型应用上工作的进度。

    > 注意****：虽然 GitHub Copilot 智能体作为自主智能体执行任务，但在执行某些任务时，它可能会请求帮助。 若要协助智能体，请响应“聊天”视图中出现的任何提示。 例如，如果智能体请求在终端中运行一个命令的权限，请选择“运行”以允许智能体运行该命令****。 如果智能体请求提供有关你的需求的澄清，请提供有助于智能体理解你的需求的响应。

1. 在“聊天”视图中，若要保存原型应用文件，请选择“保留”****。

1. 展开“ShoppingApp”文件夹****。

    该文件夹应包含以下文件：

    ```plaintext
    ShoppingApp
    ├── .github
    │   └── copilot-instructions.md
    ├── app.js
    ├── index.html
    ├── styles.css
    ```

1. 花几分钟时间来查看每个代码文件。

    - index.html 文件充当 Web 应用程序的入口点****。 它设置应用的基本结构，并链接样式和脚本文件。
    - styles.css 文件为原型 Web 应用提供可视化布局和响应式设计****。
    - app.js 文件包含用于管理产品目录、购物车、导航和 UI 呈现的 JavaScript 代码****。

    如果时间允许，请考虑要求 GitHub Copilot 生成每个文件的详细说明。

1. 在 Visual Studio Code 编辑器中打开 index.html 文件****。

1. 在“运行”菜单上，选择“运行但不调试”********。

    如果出现提示，请选择你选择的浏览器来运行应用。

1. 在浏览器中打开原型应用后，测试 PRD 中列出的用例，并验证原型应用是否提供了预期功能。

    这些用例描述了原型应用应实现的基本功能。 例如：

    - 作为用户，我可以浏览水果产品列表。
    - 作为用户，我可以查看有关所选产品的详细信息。
    - 作为用户，我可以向购物车添加产品并调整数量。
    - 作为用户，我可以在结帐前查看并更新购物车。
    - 作为用户，我可以查看订单摘要并“处理”它（无实际交易）。
    - 作为用户，我可以在“产品”、“产品详细信息”、“购物车”和“结帐”页面之间导航。

1. 验证这些用例后，通过调整浏览器窗口的大小来测试应用的动态行为。

    原型应用应具有动态用户界面，该界面自动缩放以适应在桌面和手机设备上的查看。

1. 尝试测试折叠的导航栏。

    你指定了当页面宽度降至 300 像素以下时，导航栏应折叠。 折叠时，导航栏应显示一两个字母来表示应用中的每个网页。

    > 注意****：大多数桌面浏览器（包括 Microsoft Edge）强制实施大于 300px（通常约为 320-400px）的最小窗口宽度。 这意味着你可能无法手动调整浏览器窗口的大小，使其小到足以触发导航栏的折叠。

1. （可选）执行附加测试，以确保原型应用符合预期。

    如果需要，请在测试期间记笔记。 可以在下一个任务中使用笔记来帮助优化原型应用。

1. 关闭浏览器窗口或在 Visual Studio Code 中停止应用。

## 优化原型应用

初始原型应用应已提供产品需求的基本实现。 但是，它可能会被优化和改进，并且可能未完全实现预期的用户体验。

在此任务中，使用 GitHub Copilot 智能体来优化原型应用的功能和行为。

请使用以下步骤完成本练习的这一部分：

1. 在“聊天”视图中，若要调整折叠的导航栏的断点，请输入以下提示：

    ```md
    #codebase Refactor the prototype app to use a higher breakpoint for the collapsed navigation bar. Change from 300 to 600px. Update the copilot-instructions.md file to explain the updated 600px requirement.
    ```

    如果智能体实现了一个在屏幕缩小时更改方向的导航栏（从垂直切换到水平），请使用以下命令更新导航栏的行为：

    ```md
    #codebase Refactor the code to ensure that the navigation bar stays on the left-side of the app for all devices types and sizes. The navigation bar should be responsive and maintain its position, in either an expanded or collapsed mode.
    ```

1. 花点时间查看 GitHub Copilot 智能体为响应你的提示而生成的代码更新。

1. 在“聊天”视图中，选择“保留”以保存更新的原型应用文件****。

1. 再次运行应用程序，并确保当宽度低于 600 像素时导航栏折叠。

1. 关闭浏览器窗口或在 Visual Studio Code 中停止应用。

1. 在“聊天”视图中，输入以下提示，然后监视智能体的进度：

    ```md
    #codebase Update the prototype app to display an emoji in the nav bar for each of the web pages. Ensure that the emoji is centered horizontally in the nav bar when the nav bar is collapsed. Update the copilot-instructions.md file to include this product requirement.
    ```

1. 花点时间查看代码更新。

1. 在“聊天”视图中，若要保存更新的原型应用文件，请选择“保留”****。

1. 再次运行应用程序，并验证表情符号是否在导航栏中正确显示。

    导航栏应显示表示每个网页的表情符号。 导航栏折叠时，表情符号应在导航栏中水平居中。

    如果发现导航栏的任何其他问题，可以要求 GitHub Copilot 智能体帮助你优化导航栏的行为。 例如，可以要求智能体“#codebase 重构代码，以确保导航栏始终可见，并且只有两个阶段（展开或折叠）”。

1. 关闭浏览器窗口或在 Visual Studio Code 中停止应用。

1. 在“聊天”视图中，若要确定其他改进机会，请输入以下提示：

    ```md
    #codebase Review the product requirements and wireframe diagrams in the copilot-instructions.md file. Are there any features or requirements that are missing from the implementation? Are there obvious opportunities to improve the user experience?
    ```

1. 查看 GitHub Copilot 智能体的响应。

    确定想要实现的三项或更多建议的改进。

1. 创建描述想要实现的改进的提示。

    使用 GitHub Copilot 的建议以及你创建的任何测试笔记来实现改进。 例如，可以要求 GitHub Copilot 智能体帮助实现以下更改：

    ```md
    #codebase Implement the following improvements to the prototype app:

    - Replace alert() popups with in-app notification banners or toasts.
    - Add a confirmation/thank you message after processing an order.
    - Add a visual indicator (badge) for the number of items in the cart on the nav bar.

    Ensure that the copilot-instructions.md file is updated to reflect any changes to the product features, technical requirements, user experience, or other measurable characteristics.
    ```

    > **提示**：可以从 GitHub Copilot 的响应中复制信息，以帮助构建新提示。 还可以在提示中引用前一个响应的各部分。

1. 如果时间允许，请使用 GitHub Copilot 的建议和自己的想法继续优化应用。

1. 在“文件”菜单上，选择“将工作区另存为...”****。

1. 若要在“VibeCoding-PrototypeApp”文件夹中保存工作区配置文件 (VibeCoding-PrototypeApp.code-workspace)，请选择“保存”********。

    此文件使你能够保存并重新打开具有相同文件夹结构和设置的工作区。

## 总结

在本练习中，你学习了如何使用 GitHub Copilot 智能体通过 Vibe 编码过程创建原型应用。 你定义了产品需求，创建了一个初始原型应用，并优化了该原型应用，以更好地满足预期的用户体验和功能。

## 清理

完成本练习后，请花点时间确保你未更改不想保留的 GitHub 帐户或 GitHub Copilot 订阅。 如果你进行了任何更改，请根据需要还原。 如果你将本地电脑用作实验室环境，则可以存档或删除为此练习创建的原型应用文件夹。
