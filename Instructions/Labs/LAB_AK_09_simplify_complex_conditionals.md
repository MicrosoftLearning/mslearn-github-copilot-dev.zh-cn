---
lab:
  title: 练习 - 使用 GitHub Copilot 简化复杂条件
  description: 学习如何使用 GitHub Copilot 工具重构 C# 代码库中的复杂条件逻辑。
---

# 使用 GitHub Copilot 简化复杂条件

业务应用程序中的条件逻辑通常会随着时间推移而变得更加复杂和深度嵌套，从而使代码难以阅读、维护和测试。 遗憾的是，这种导致代码难以维护的复杂性，也会让代码的重构变得困难。 当代码与业务逻辑高度耦合时，难度进一步增加。

在本练习中，你将使用 GitHub Copilot 分析包含深度嵌套条件逻辑的代码、重构代码逻辑，然后测试重构的代码，以确保其按预期工作。 你将在“提问”模式下使用 GitHub Copilot 来了解代码并探索简化逻辑的选项。 你将在“智能体”模式下使用 GitHub Copilot 来重构代码，将复杂的条件逻辑提取到更小、更专注的辅助方法中，并减少嵌套层级。 简化复杂的条件可以更轻松地阅读、维护和测试代码。

完成此练习大约需要 30 分钟****。

> **重要说明**：要完成本练习，需要提供自己的 GitHub 帐户和 GitHub Copilot 订阅。 如果没有 GitHub 帐户，可以<a href="https://github.com/" target="_blank">注册</a>免费的个人帐户，并使用 GitHub Copilot 免费版计划来完成练习。 如果可以从实验室环境中访问 GitHub Copilot Pro、GitHub Copilot Pro+、GitHub Copilot Business 或 GitHub Copilot Enterprise 订阅，则可以使用你现有的 GitHub Copilot 订阅来完成本练习。

## 开始之前

实验室环境必须具备以下条件：Git 2.48 或更高版本、.NET SDK 9.0 或更高版本、带有 C# Dev Kit 扩展的 Visual Studio Code，以及启用了 GitHub Copilot 的 GitHub 帐户的访问权限。

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

使用以下步骤下载示例项目并在 Visual Studio Code 中打开：

1. 在实验室环境中打开浏览器窗口。

1. 要下载包含示例应用项目的 zip 文件，请在浏览器中打开以下 URL：[GitHub Copilot 实验室 - 开发代码功能](https://github.com/MicrosoftLearning/mslearn-github-copilot-dev/raw/refs/heads/main/DownloadableCodeProjects/Downloads/GHCopilotEx9LabApps.zip)

    zip 文件名为 GHCopilotEx9LabApps.zip****。

1. 从 GHCopilotEx9LabApps.zip 文件中提取文件****。

    例如：

    1. 导航到实验室环境中的“下载”文件夹。

    1. 右键单击 GHCopilotEx9LabApps.zip，然后选择“全部提取”********。

    1. 选择“完成时显示解压缩的文件”，然后选择“解压缩”。

1. 将 GHCopilotEx9LabApps 文件夹复制到易于访问的位置，例如 Windows“桌面”文件夹****。

1. 在 Visual Studio Code 中打开 GHCopilotEx9LabApps 文件夹****。

    例如：

    1. 在实验室环境中打开 Visual Studio Code。

    1. 在 Visual Studio Code 中的“文件”菜单上，选择“打开文件夹” 。

    1. 导航到 Windows“桌面”文件夹，选择 GHCopilotEx9LabApps，然后选择“选择文件夹”********。

1. 在 Visual Studio Code 的“解决方案资源管理器”视图中，验证以下项目结构：

    - GHCopilotEx9LabApps\
        - ECommercePricingEngine\
            - Dependencies\
            - ECommercePricingDemo.cs
            - Output-ECommercePricingEngine.txt
            - SecurityTest.cs
        - LoanApprovalWorkflow\
            - Dependencies\
            - LoanApprovalDemo.cs
            - Output-LoanApprovalWorkflow.txt
            - SecurityTest.cs

## 练习场景

你是一家咨询公司的软件开发人员。 你的客户需要重构复杂的条件逻辑方面的帮助，以提升代码的可读性和可维护性。 你被分配到以下应用：

- 电商定价引擎：第一个应用是电商定价引擎，它基于各种业务规则计算动态定价。 条件包括会员等级、订单金额、优惠券码、产品类别以及配送规则。
- 贷款审批工作流：第二个应用是贷款审批工作流，它基于各种因素评估贷款申请。 条件包括收入、就业状况、负债率、抵押物和信用记录。

本练习包括以下任务：

1. 查看电商定价引擎代码库。
1. 使用 GitHub Copilot 明确电商定价代码中的重构机会。
1. 使用 GitHub Copilot 智能体重构电商定价代码。
1. 测试重构的电商定价代码。
1. （可选）简化 LoanApprovalWorkflow 演示应用中的复杂条件。

### 查看电商定价引擎代码库

任何重构工作的第一步都是确保了解现有的代码库。

在此任务中，你将打开电商定价引擎项目，并使用 GitHub Copilot 来帮助分析复杂的条件逻辑。

使用以下步骤完成此任务：

1. 确保在 Visual Studio Code 中打开了 GHCopilotEx9LabApps 文件夹。

    如果未下载示例代码项目，请参阅“开始之前”部分****。

1. 验证 ECommercePricingEngine 代码项目是否成功生成****。

    例如，在“解决方案资源管理器”视图中，右键单击 ECommercePricingEngine，然后选择“生成”********。

    将看到警告“无法将 null 字面量转换为非 null 的引用类型”。 生成项目时，不应有任何错误。 在本练习中，可以忽略这些警告。

1. 打开 GitHub Copilot“聊天”视图。

    如果“聊天”视图尚未打开，可以通过选择 Visual Studio Code 窗口顶部“搜索”文本框右侧的“聊天”图标来打开****。

1. 在“聊天”视图中，确保聊天模式设置为“提问”，模型设置为 GPT-4.1********。

    这些设置可在“聊天”视图的左下角找到。 GitHub Copilot 的“提问”模式用于询问一般编码问题并生成代码相关解释****。 GPT-4.1 模型（包含在 GitHub Copilot 免费版计划中），是进行代码分析、代码解释以及代码重构指导的不错选择****。

    在本练习的后面部分，将使用 GitHub Copilot 的“智能体”模式，但现在，应该使用“提问”模式来进行代码分析和解释********。

    > [!NOTE]
    > 某些模型比其他模型更适合特定任务。 选择的模型会影响 GitHub Copilot 生成的响应。 使用建议的设置完成本实验室练习后，可能需要使用不同的模型重复练习，并比较结果。

1. 在 Visual Studio Code 中，打开 ECommercePricingDemo.cs 文件****。

    此文件包含以下类：

    - 用户:表示客户，包含会员等级、购买历史记录以及特殊身份（学生、员工、企业等）属性。 用于确定各种折扣和权益的资格。
    - Coupon：表示折扣或配送优惠券，包含券码、有效期、类型（百分比折扣型或配送减免型）及优惠金额属性。 用于定价计算，以应用额外折扣或免运费。
    - 物料：表示订单中的产品，包含名称、类别和价格属性。 用于生成订单并计算小计金额与品类专属折扣。
    - 订单：表示客户的订单，包含商品列表、配送区域、优惠券、活动、支付方式以及其他订单特定标志。 提供用于计算小计金额、检查品类存在性以及确定订单特征（例如，高价值、混合品类）的方法。
    - PricingEngine：包含用于计算订单最终价格的主要逻辑。 根据用户身份、订单详情、优惠券及品类专属规则应用折扣。 处理安全校验，并确保折扣和价格维持在合理范围内。
    - Program：入口点。 创建测试用户、优惠券和订单，然后使用上述类运行一系列复杂的定价场景。 演示定价引擎在不同情况下如何应用其逻辑。

    每个类都对电商定价系统中的某个现实实体或流程进行建模，这些类在 Program 类中协同工作，以模拟和测试定价计算。 例如，User、Order 和 Coupon 实例将传递给 PricingEngine.CalculateFinalPrice，以计算并显示包含所有适用折扣的最终价格。

1. 找到 PricingEngine 类，然后选择整个 CalculateFinalPrice 方法********。

    CalculateFinalPrice 方法包含复杂且深层嵌套的条件逻辑，用于评估会员等级、季节性活动、公司帐户、订阅服务以及各类折扣场景。

1. 花点时间浏览 CalculateFinalPrice 方法中的条件逻辑****。

    该方法复杂且可读性差，包含多个处理不同折扣场景的嵌套条件。 这种复杂性源于需要根据用户身份、订单详情和优惠券码来应用的各种业务规则。

1. 右键单击所选代码，然后选择“Copilot” >“解释”********。

    GitHub Copilot 将分析 CalculateFinalPrice 方法，并提供代码功能的详细解释。

1. 花几分钟时间查看 GitHub Copilot 的解释。

    该解释应当重点突出主要的折扣类别，例如会员等级、优惠券应用、批量购买激励以及品类专属规则。

    例如：

    ```md
    The CalculateFinalPrice method is a comprehensive pricing engine for an e-commerce scenario. It starts by validating its inputs, ensuring that both the user and order objects are not null and that the order data is valid. This prevents null reference exceptions and guards against invalid or malicious input. The method then calculates the base total of the order and checks that it falls within a reasonable range, aborting the calculation if the total is suspiciously low or high.
    
    Next, the method initializes variables for the discount percentage, shipping cost, and a list to track all applied discounts. The core of the function is a series of nested conditional blocks that determine which discounts the user is eligible for. These are based on membership level (Premium, Gold, Silver), user attributes (such as being a student, employee, or first-time buyer), order characteristics (like high value, bulk purchase, or mixed categories), and special events (such as Black Friday or Back to School). Each eligible discount is added using the SafeAddDiscount helper, which ensures the total discount does not exceed a maximum cap and records a description of the discount.
    
    The method also handles coupon codes, applying either a percentage discount or free shipping, with additional enhancements for certain membership levels or order types. For example, Premium members may receive a boosted coupon value, and further stacking is possible during special events or for corporate accounts.
    
    After all discounts are considered, the method applies additional bulk purchase incentives if the order contains a large number of items. It then enforces a final cap on the total discount to prevent excessive reductions. The final price is calculated using ApplyCategorySpecificDiscounts, which applies category-based discount caps (e.g., electronics discounts are limited to 15%). The shipping cost is added, and the final price is ensured to be above a minimum threshold.
    
    Finally, the method outputs a summary of the calculation, including the base total, all applied discounts, the total discount percentage (with a note about category caps), the shipping cost, and the final price. This approach ensures transparency and traceability in how the final price is determined, while also enforcing business rules and security best practices.
    ```

    花时间理解现有代码将有助于你在本练习的后续步骤中识别并选择重构方案。

1. 在“聊天”视图中，若要更深入地分析计算过程，请输入以下提示：

    ```plaintext
    @workspace Explain the business logic flow in the CalculateFinalPrice method. What are the different discount paths and how do they interact with each other? What are the key business rules that govern pricing calculations?
    ```

    此分析应帮助你理解不同折扣类别的叠加方式，以及在定价计算的每个步骤中应用了哪些业务规则。

1. 花几分钟时间查看 GitHub Copilot 生成的解释。

    响应应明确主要折扣路径及它们的叠加方式。 例如：

    ```md
    The `CalculateFinalPrice` method in `ECommercePricing.PricingEngine` implements a layered, rule-driven approach to pricing calculation for an e-commerce order. Here’s how the business logic flows and how the discount paths interact:
    
    ### 1. **Input Validation**
    - The method first validates that both `user` and `order` are not null and that the order data is valid (e.g., no negative prices, all required fields present).
    - It checks that the order subtotal is within allowed bounds (greater than zero and less than $1,000,000).
    
    ### 2. **Base Discount Initialization**
    - The method initializes the discount percentage (`discountPercent`) to 0 and calculates the base shipping cost.
    
    ### 3. **Membership-Based Discount Paths**
    - The first major decision point is the user's membership level:
      - **Premium:** Starts with a 15% discount, then applies additional discounts for high-value orders, seasonal events, corporate accounts, subscriptions, loyalty (years as member), lifetime spending, and express shipping. Each condition is nested, so deeper benefits require all previous conditions to be met.
      - **Gold:** Starts with a 12% discount, then adds bonuses for seasonal events, bulk purchases, category diversity, employee status, pre-orders, and alternative payment methods. These are also nested, but less deeply than Premium.
      - **Silver:** Starts with 8%, with further bonuses for students, back-to-school events, bulk purchases, electronics focus, gift wrap, and express shipping. These are nested within the student path.
      - **First-Time Buyer:** If the user is a first-time buyer (and not a member), a 10% discount is applied, with further bonuses for seasonal events, order size, payment method, high-value orders, premium shipping regions, and express shipping.
    
    ### 4. **Coupon Application**
    - If a coupon is present:
      - **Valid Percent Coupon:** The coupon value is capped at 50%. Premium and Gold members get a multiplier (Premium: 1.3x up to 50%, Gold: 1.2x up to 40%). Additional stacking occurs for Premium members on Black Friday, corporate accounts, and bulk orders, with each step further capping the discount.
      - **Valid Shipping Coupon:** Shipping is set to zero for domestic orders or Premium members.
      - **Expired Coupon:** No discount is applied, and a message is logged.
    
    ### 5. **Bulk Purchase Incentives**
    - Additional discounts are applied for orders with 10+ items (5%) or 20+ items (8%).
    
    ### 6. **Discount Capping**
    - The total discount percentage is capped at 95% to prevent excessive reductions.
    
    ### 7. **Category-Specific Discount Rules**
    - The final discount is applied with category-specific caps:
      - **Electronics:** Discount capped at 15%.
      - **Clothing:** Discount capped at 20% (25% during Back-to-School).
      - **Accessories/Other:** Full discount applies.
    
    ### 8. **Final Price Calculation**
    - The final price is the sum of all discounted category subtotals plus shipping, with a minimum enforced price of $0.01.
    
    ### 9. **Result Output**
    - The method prints the base total, all applied discounts, the total discount percentage (with a note about the electronics cap), shipping cost, and the final price.
    
    ---
    
    ## **Discount Path Interactions**
    - **Nesting:** Many discounts are nested, meaning deeper discounts require all previous conditions to be true (e.g., Premium members with high-value orders, during Black Friday, who are also corporate accounts, etc.).
    - **Stacking:** Discounts from different sources (membership, coupons, bulk, etc.) are added together, but always subject to the overall cap.
    - **Category Caps:** Even if the total discount is high, certain categories (like electronics) have stricter caps, so the effective discount for those items may be lower than the overall discount.
    
    ---
    
    ## **Key Business Rules**
    - **Validation:** Orders and users must be valid; otherwise, pricing is aborted.
    - **Membership Drives Discounts:** Higher membership levels unlock deeper and more complex discount paths.
    - **Coupons Are Enhanced by Membership:** Premium and Gold members get more value from percent-off coupons.
    - **Bulk and Seasonal Bonuses:** Large orders and seasonal events can trigger additional discounts.
    - **Category Protection:** Electronics and clothing have maximum discount caps to protect margins.
    - **Shipping Can Be Discounted:** Shipping can be free with certain coupons or for Premium members.
    - **Discount Caps:** No combination of discounts can exceed 95% off the order.
    - **Transparency:** All applied discounts are tracked and displayed for auditability.
    
    For more details, see the full implementation in ECommercePricingDemo.cs, especially the `ECommercePricing.PricingEngine.CalculateFinalPrice` method.
    ```

1. 比较 GitHub Copilot 的解释与你自己对`CalculateFinalPrice` 方法中复杂条件逻辑的观察结果。

    自己的观察结果或许已经明确了主要的折扣来源：会员等级、优惠券码以及批量购买激励。 GitHub Copilot 的解释应当能印证你的观察结果，同时提供额外见解，包括这些折扣路径的叠加方式，以及支配定价计算的关键业务规则。

    - 注意不同会员等级（高级、金卡、银卡）的折扣应用方式，以及首次购买者的权益。
    - 注意优惠券的验证与应用方式，以及优惠券折扣与会员折扣的叠加方式。
    - 请注意基于商品数量的阶梯折扣是如何应用的。

    业务规则通过嵌套条件来执行，确保能根据用户身份、订单详情和优惠券码正确应用折扣。 这些规则在重构过程中必须保留下来。

1. 运行 ECommercePricingEngine 项目并查看输出。

    输出应显示不同用户、订单和优惠券组合对应的基准价格和折扣后价格。 在本练习结束时，重构的代码必须生成与原始代码相同的结果。

    你可能注意到，输出中包含了针对无效输入和恶意操纵定价行为的基础安全测试。 尽管这些测试并未涵盖所有可能的攻击途径，也未达到生产应用所需的测试级别，但它们足以提醒我们：确保代码质量与安全性是一项必须满足的要求。 SecurityTest.cs 文件是 ECommercePricingEngine 项目的一部分****。

    > [!NOTE]
    > 输出的副本可在 ECommercePricingEngine 文件夹中包含的 Output-ECommercePricingEngine.txt 文件内找到****。 如果需要对比结果，或者修改了示例数据，也可以创建自己的输出文件。 完成本练习后，将使用输出文件来确保重构的代码生成与原始代码相同的结果。

    运行项目：如果在 Visual Studio Code 编辑器中打开了 ECommercePricingDemo.cs 文件，可以通过选择编辑器窗格右上角的运行按钮来运行项目（运行与此文件关联的项目）。 若要从“解决方案资源管理器”视图运行项目，请右键单击 ECommercePricingEngine，选择“调试”，然后选择“启动新实例”************。

### 使用 GitHub Copilot 明确电商定价代码中的重构机会

GitHub Copilot 是分析复杂代码并明确代码重构机会的出色工具。

在此任务中，你将使用 GitHub Copilot 明确特定的重构机会，并建议简化复杂条件的辅助方法。 你将使用上一个任务的观察结果来帮助构造提供给 GitHub Copilot 的提示。

使用以下步骤完成此任务：

1. 确保 GitHub Copilot“聊天”视图已打开，并处于“提问”模式，且已选择 GPT-4.1 模型********。

1. 使用拖放操作将 ECommercePricingDemo.cs 文件添加到“聊天”上下文****。

    尽管 ECommercePricingDemo.cs 文件已在 Visual Studio Code 编辑器中打开，但将其添加到“聊天”上下文能促使 GitHub Copilot 分析整个代码文件，进而生成更精准的建议。 使用 GitHub Copilot 时，将相关文件添加到“聊天”上下文是最佳做法，即使在提示中包含@workspace 或 #codebase 标记也是如此********。

1. 提交提示，让 GitHub Copilot 明确可改善与主要折扣路径相关的代码模块化的重构机会。

    构造提示时，请考虑以下各项：

    - 告诉 GitHub Copilot 专注于 CalculateFinalPrice 方法及其嵌套条件****。
    - 让 GitHub Copilot 建议将复杂逻辑移动到更易于管理的单责任辅助方法的选项。
    - 列出应考虑提取到辅助方法的主要折扣路径。
    - 强调在简化代码结构的同时维护业务逻辑的重要性。

    在分析代码时，让 GitHub Copilot 考虑代码交互通常是有益的。 可以使用@workspace 或 #codebase 标记告诉 GitHub Copilot 在其分析中包含整个代码库********。

    例如：

    ```plaintext
    @workspace Analyze the CalculateFinalPrice method and suggest refactoring opportunities. Include options that move the nested conditional logic into more manageable, single-responsibility helper methods. Focus on the main discount paths: membership discounts, coupon discounts, and bulk discounts. Maintain the business logic while simplifying the code structure.
    ```

    GitHub Copilot 将分析该方法并建议特定的重构机会，明确可提取到辅助方法的嵌套条件中的模式。

1. 花几分钟时间查看 GitHub Copilot 的建议。

    响应应明确提高代码可管理性和模块化的机会，例如将主要折扣路径提取到辅助方法中，以及采用一种或多种方法来减少嵌套。

    例如：

    ```md
    Collecting workspace informationThe `ECommercePricing.PricingEngine.CalculateFinalPrice` method is functionally rich but contains deeply nested conditional logic, especially for membership, coupon, and bulk discounts. This makes the code harder to read, maintain, and test. Here are refactoring opportunities and suggestions:
    
    ## **Refactoring Opportunities**
    
    ### 1. **Extract Membership Discount Logic**
    Move the entire membership-based discount logic into a helper method, e.g., `ApplyMembershipDiscounts`. This method would encapsulate all the nested membership, event, and user-attribute checks, returning the updated discount and applied discounts.
    
    ### 2. **Extract Coupon Discount Logic**
    Move coupon handling into a method like `ApplyCouponDiscounts`. This would handle coupon validation, membership-based coupon boosts, and shipping coupon logic.
    
    ### 3. **Extract Bulk Discount Logic**
    Move the bulk purchase logic into a method such as `ApplyBulkDiscounts`.
    
    ### 4. **Reduce Nesting**
    Within each helper, use early returns or guard clauses where possible to reduce nesting.
    
    ### 5. **Encapsulate Discount State**
    Consider using a small struct or class (e.g., `DiscountContext`) to pass and update the discount percent and applied discounts list, reducing parameter clutter.
    ```

1. 提交跟进提示，询问 GitHub Copilot 如何简化 CalculateFinalPrice 方法中的复杂条件逻辑并减少嵌套层级。

    跟进提示是优化分析并从 GitHub Copilot 获取更具体建议的好方法。 在这种情况下，需要的是关于减少嵌套层级的更多补充信息，且这些信息仍需围绕 CalculateFinalPrice 方法中的复杂条件逻辑展开。 还可以使用跟进提示来询问最佳做法，或了解建议的更改可能对代码产生的影响。

    例如：

    ```plaintext
    How can I simplify the complex conditional logic and reduce nesting levels in the CalculateFinalPrice method? For example, the method has multiple nested conditionals that apply different membership discounts based on user status and order details. What are some best practices for reducing nesting levels and improving readability? Explain the benefit of each approach when applied to the CalculateFinalPrice method.
    ```

1. 花点时间查看 GitHub Copilot 关于简化条件逻辑和减少嵌套层级的建议。

    GitHub Copilot 应提供关于简化条件逻辑和减少嵌套层级的建议列表。 一些建议可能会与之前的建议重复，这种情况是正常的，无需意外。 不过，你应该会看到一些与减少嵌套层级和提升可读性相关的新建议，例如：

    - 使用提前返回或卫语句来扁平化逻辑、减少缩进并明确流程。
    - 使用局部辅助函数来消除代码重复、明确代码意图。
    - 使用开关表达式或模式匹配来让会员逻辑更明确、更易于维护。
    - 为每个会员等级使用更小的方法来模块化逻辑，使其更易于测试和更新。
    - 使用布尔变量来表示条件，以提高复杂检查的可读性。

> [!NOTE]
> 你将使用分析的结果，来帮助构造本练习下一节中分配给 GitHub Copilot 智能体的任务。

### 使用 GitHub Copilot 智能体重构电商定价代码

GitHub Copilot 有三种模式：“提问”、“编辑”和“智能体”************。 在“智能体”模式下运行时，GitHub Copilot 作为自主 AI 智能体。

在“智能体”模式下：

- 提示指定分配给 GitHub Copilot 智能体的任务。
- GitHub Copilot 使用任务信息和代码库来明确相关代码文件，并为任务建立上下文。
- GitHub Copilot 制定一个可以用来完成任务的流程。 智能体使用迭代方法和代码评审来帮助确保任务成功完成。
- GitHub Copilot 使用“聊天”视图，让你在它处理分配的任务时随时了解进度。 它还可能提供所做更改的解释或理由。
- GitHub Copilot 可以调用工具来帮助其完成任务或验证代码更改是否正常工作。
- GitHub Copilot 在处理任务时可能会暂停，并向你请求帮助或澄清。 监视聊天界面并在收到提示时做出响应，为自主智能体提供协助，这一点非常重要。
- GitHub Copilot 在 Visual Studio Code 编辑器中更新代码文件。 任务完成后，应先查看 GitHub Copilot 所做的更改，然后再将它们应用于代码库（单独或集体）。

在本练习的本节中，你将使用 GitHub Copilot 智能体重构 PricingEngine 类，并简化 CalculateFinalPrice 方法中的复杂条件逻辑。 分配给 GitHub Copilot 智能体的任务将基于 GitHub Copilot 在代码分析（上一任务）期间提供的建议。

使用以下步骤完成此任务：

1. 确保 GitHub Copilot“聊天”视图已在 Visual Studio Code 中打开。

1. 在“聊天”视图中，选择“智能体”模式****。

    “设置模式”下拉菜单位于“聊天”视图的左下角****。 选择“智能体”时，GitHub Copilot 将切换到“智能体”模式，这样它就可以自主处理你分配的任务****。

1. 花点时间明确分配给 GitHub Copilot 智能体完成的任务要求。

    需要编写一个任务来解释目标，并描述 GitHub Copilot 智能体可用于实现该目标的步骤。

    在这种情况下，目标包括以下各项：

    - 简化复杂的条件逻辑并减少嵌套层级。
    - 提高代码可维护性和可读性。
    - 保留现有功能，并确保重构的代码生成与原始代码相同的输出。

    在上一任务中，你要求 GitHub Copilot 分析代码并明确重构机会。 你生成了两组建议：一组建议是将复杂逻辑提取到辅助方法中，以提升代码的模块化程度；另一组建议是降低嵌套层级，从而改善代码的可读性。

    上一任务中识别的重构机会可能包括：

    - 将会员等级折扣逻辑提取到辅助方法中。
    - 将优惠券验证和应用逻辑提取到辅助方法中。
    - 将阶梯折扣逻辑提取到辅助方法中。
    - 使用提前返回或卫语句来扁平化逻辑、减少缩进并明确流程。
    - 使用局部辅助函数来消除代码重复、明确代码意图。
    - 使用开关表达式或模式匹配来让会员逻辑更明确、更易于维护。
    - 为每个会员等级使用更小的方法来模块化逻辑，使其更易于测试和更新。
    - 使用布尔变量来表示条件，以提高复杂检查的可读性。

    为 GitHub Copilot 智能体创建的任务应结合目标语句和你识别的重构机会。

    该任务应实现以下组织结构：

    1. 目标语句：解释任务的总体目标。
    1. 代码结构/模块化更新：明确可以提取以提升模块化程度的任何大型代码块。
    1. 小的或细节上的代码改进：描述减少嵌套层级并提高可读性的具体方法。
    1. 结果：描述重构过程的预期结果。

1. 构造满足要求的任务。

    你的任务应包括目标和建议的重构机会，这些机会使用建议的组织结构来排列。

    例如：

    ```plaintext
    I need to simplify the complex conditional logic and reduce nesting levels in the CalculateFinalPrice method. The main areas of complexity are associated with membership discounts, coupon processing, and bulk discounts. Each of these areas could be moved into separate helper methods that are called from CalculateFinalPrice. There are several options to reduce nesting levels in the helper methods. Use early returns or guard clauses to flatten logic, reduce indentation, and clarify flow. Use local helper functions to remove duplication and clarify intent. Use switch expressions or pattern matching to make membership logic explicit and maintainable. Use smaller methods per membership to modularize logic and make it easier to test and update. Use Boolean variables for conditions to improve readability of complex checks. The goal is to improve code maintainability and readability while preserving the existing functionality and generated output.
    ```

    此任务使用自然语言文本来描述目标、主要重构机会以及用于减少嵌套层级和提升可读性的具体方法。 任务还描述了重构过程的预期结果。

1. 使用“聊天”视图将任务分配给 GitHub Copilot 智能体。

    GitHub Copilot 智能体将评估任务和代码库，以开发用于重构 CalculateFinalPrice 并将会员折扣逻辑提取到新的帮助器方法的方法。 智能体将在各个阶段测试代码更新，以确保重构成功且业务逻辑保持不变。

    提交任务后，GitHub Copilot 智能体将开始处理任务。 可以在“聊天”视图中监视其进度。

    > [!NOTE]
    > 如果需要对任务进行更改，可以编辑“聊天”视图中的文本并重新提交。 GitHub Copilot 智能体将重新评估任务并继续处理该任务。

1. 在 GitHub Copilot 智能体处理任务时监视“聊天”视图。

    智能体实时更新其任务进展，包括遇到的任何挑战以及计划如何解决这些挑战。 如果需要，它还可能要求澄清或补充信息。 如有必要，请通过在“聊天”视图中响应智能体的提示来提供帮助。

1. 重构任务完成后，在 Visual Studio Code 编辑器中查看建议的更新。

    在接受之前，请始终查看 GitHub Copilot 建议的更新。 验证更新是否符合预期业务逻辑、应用功能和编码标准。

    重构的代码应包含一个极大简化的 CalculateFinalPrice 方法，该方法主要由一系列对辅助方法的方法调用构成，这些辅助方法分别用于处理会员折扣、优惠券折扣和批量折扣****。 原始方法的复杂度应显著降低，使其更易于阅读和维护。

    还应重构会员折扣辅助方法，以处理会员等级的复杂逻辑，并为每个等级（高级、金卡、银卡和首次购买者）提供额外的辅助方法。

1. 在“聊天”视图中，若要接受所有编辑，请选择“保留”****。

    如果建议的更改不符合预期，可以拒绝，也可以接受部分更改并拒绝其他更改。 使用 GitHub Copilot 重构代码通常是一个迭代过程，包括优化提示以实现预期结果。

    如果接受 GitHub Copilot 的建议更新，然后意识到建议引入的问题可能难以继续解决，则可以通过在“聊天”视图中选择“撤消上次请求”或使用 Visual Studio Code 的撤消功能来还原更改****。

1. 让 GitHub Copilot 解释它实现的代码重构更新。

    例如：

    ```plaintext
    Explain the refactoring updates that you implemented to simplify the complex conditional logic, reduce nesting levels, and improve code maintainability and readability.
    ```

    GitHub Copilot 将对其所做的修改进行解释，包括复杂条件逻辑是如何简化的，以及代码结构是如何优化的。

    例如：

    ```md
    Here’s an explanation of the refactoring updates made to simplify the complex conditional logic, reduce nesting, and improve maintainability and readability in the `CalculateFinalPrice` method:
    
    ### 1. **Separation of Concerns with Helper Methods**
    - The main discount logic was split into three focused helper methods: `ApplyMembershipDiscounts`, `ApplyCouponDiscounts`, and `ApplyBulkDiscounts`.
    - Each helper method is responsible for a single aspect of the discount calculation, making the main method concise and easy to follow.
    
    ### 2. **Flattened Membership Discount Logic**
    - The deeply nested membership discount logic was moved into `ApplyMembershipDiscounts`, which uses a `switch` statement to dispatch to smaller, membership-specific methods (`ApplyPremiumDiscounts`, `ApplyGoldDiscounts`, `ApplySilverDiscounts`, `ApplyFirstTimeBuyerDiscounts`).
    - Each of these methods uses guard clauses (early returns) and Boolean variables to flatten the logic, reducing indentation and clarifying the flow.
    
    ### 3. **Guard Clauses and Early Returns**
    - Instead of multiple nested `if` statements, each helper method uses guard clauses to exit early when a condition is not met. This keeps the main logic path at the top level and reduces cognitive load.
    
    ### 4. **Switch Expressions and Pattern Matching**
    - The use of `switch` in `ApplyMembershipDiscounts` makes the membership logic explicit and easy to extend or modify.
    - This approach also makes it clear which logic applies to which membership type.
    
    ### 5. **Boolean Variables for Readability**
    - Boolean variables (e.g., `altPayment`, `premiumPayment`) are used to clarify complex conditions, making the code more self-documenting and easier to read.
    
    ### 6. **Local Helper Methods for Duplication and Intent**
    - Smaller, focused methods encapsulate repeated or complex logic, such as safe discount addition and category percentage calculation.
    - This reduces code duplication and clarifies the intent of each operation.
    
    ### 7. **Preserved Business Logic and Output**
    - All business rules, discount caps, and output formatting are preserved, ensuring that the refactoring does not change the program’s behavior.
    
    ## **Benefits**
    - **Readability:** The main method is now a clear sequence of high-level steps.
    - **Maintainability:** Each discount rule is isolated, making it easier to update or test.
    - **Extensibility:** Adding or modifying discount rules is straightforward.
    - **Testability:** Each helper method can be unit tested independently.
    
    This refactoring transforms a complex, deeply nested method into a modular, maintainable, and readable pricing engine, while preserving all business requirements and output.
    ```

1. 花点时间查看 PricingEngine 类中的重构代码。

1. 提交跟进提示，让 GitHub Copilot 解释重构代码的特定部分或比较原始版本和重构的版本。

    例如：

    ```plaintext
    Compare the original PricingEngine class with the new refactored version of the PricingEngine class. Explain the relationship between methods in the two versions. Explain the implementation of business rules in the two versions.
    ```

### 测试重构的电商定价代码

测试对于确保重构不会更改业务逻辑行为至关重要。 你将使用各种场景来运行代码，以验证计算是否保持一致。

使用以下步骤完成此任务：

1. 生成项目以确保不存在任何编译错误。

    例如，在“解决方案资源管理器”视图中，右键单击 ECommercePricingEngine，然后选择“生成”********。

    如果存在任何编译错误，请查看重构的代码并修复任何问题。 如果需要，GitHub Copilot 可以帮助解决编译错误。

1. 运行应用程序以测试重构的定价逻辑。

    如果在 Visual Studio Code 编辑器中打开了 ECommercePricingDemo.cs 文件，可以通过选择编辑器窗格右上角的运行按钮来运行项目（运行与此文件关联的项目）。 若要从“解决方案资源管理器”视图运行项目，请右键单击 ECommercePricingEngine，选择“调试”，然后选择“启动新实例”************。

    应用程序应无错误地执行，并能针对各种测试场景显示定价计算。

    > [!IMPORTANT]
    > 如果重构的代码遇到运行时错误，请查看 GitHub Copilot 智能体所做的更改，并使用 GitHub Copilot 解决问题。 如有必要，可以使用“聊天”视图中的“撤消上次请求”选项还原最后一组更改，然后更新分配给 GitHub Copilot 智能体的任务****。 使用具备 AI 功能的工具时，通过迭代优化提示/任务来改进结果，是一种标准做法。

1. 让 GitHub Copilot 比较重构代码生成的输出与原始输出。

    原始输出 Output-ECommercePricingEngine.txt 包含在 ECommercePricingEngine 文件夹中****。

    可以创建第二个输出文件，然后让 GitHub Copilot 明确这两个文件之间的任何差异。

    输出应相同，以确认在提升代码可维护性与可读性的同时，业务逻辑保持不变。

### （可选）简化 LoanApprovalWorkflow 演示应用中的复杂条件

如果时间允许，请使用你在贷款审批工作流示例应用中简化定价逻辑时所采用的相同流程，来简化 LoanApprovalWorkflow 演示应用中的复杂条件。

在此可选任务中，你将应用在电商定价引擎中使用的相同技术来重构贷款审批工作流演示应用中的复杂条件逻辑。 你将分析贷款审批代码，明确重构机会，然后使用 GitHub Copilot 智能体将复杂的条件提取到更小、更专注的辅助方法中。

> [!IMPORTANT]
> 此任务的说明包含概括流程步骤，但不包含建议的提示或详细解释。 有关如何有效地构造提示和使用 GitHub Copilot 的示例，可参考本练习中前面的任务。

使用以下步骤完成此任务：

1. 展开 LoanApprovalWorkflow 项目文件夹，打开 LoanApprovalDemo.cs 文件，然后查看代码********。

1. 使用 GitHub Copilot 解释 LoanApprovalDemo.cs 代码，包括 LoanEvaluator 类的 Evaluate 方法中的复杂条件逻辑****。

1. 运行 LoanApprovalWorkflow 项目以测试初始贷款审批逻辑并创建输出记录。

1. 使用 GitHub Copilot 明确代码重构机会，以简化 LoanEvaluator 类中的复杂条件，减少嵌套层级，并提升可读性与可维护性。

1. 将任务分配给 GitHub Copilot 智能体，以简化 LoanEvaluator 类中的复杂条件逻辑、减少嵌套层级，并提升可读性与可维护性（重构 Evaluate 方法）。

    例如，可以让 GitHub Copilot 智能体创建用于信用评分评估、收入与就业验证、财务比率计算、政府项目资格审查以及贷款条款确定的辅助方法。

1. 使用 GitHub Copilot 智能体简化复杂的条件逻辑，并减少辅助方法中的嵌套层级。

1. 测试重构的代码，并确保贷款审批演示应用程序生成与原始实现相同的结果。

## 总结

在本练习中，你学习了如何使用 GitHub Copilot 来简化代码库中的复杂条件逻辑。 你研究了电商定价引擎和贷款审批工作流演示应用，识别了重构机会，并使用 GitHub Copilot 智能体将复杂的条件提取到了更小、更专注的辅助方法中。 你还学习了如何减少代码中的嵌套层级以提高可读性，同时保持相同的业务逻辑。

## 清理

现在，你已经完成了本练习，请花点时间确保你没有对 GitHub 帐户或 GitHub Copilot 订阅作出任何你不希望保留的更改。 如果你进行了任何更改，请根据需要还原。 如果你将本地电脑用作实验室环境，则可以存档或删除为此练习创建的示例项目文件夹。
