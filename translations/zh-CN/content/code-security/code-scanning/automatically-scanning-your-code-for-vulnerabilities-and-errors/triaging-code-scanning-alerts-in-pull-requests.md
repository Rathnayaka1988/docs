---
title: 鉴定拉取请求中的代码扫描警报
shortTitle: Triage alerts in pull requests
intro: '当 {% data variables.product.prodname_code_scanning %} 在拉取请求中发现问题时，您可以审查高亮的代码并解决警报。'
product: '{% data reusables.gated-features.code-scanning %}'
permissions: 'If you have read permission for a repository, you can see annotations on pull requests. With write permission, you can see detailed information and resolve {% data variables.product.prodname_code_scanning %} alerts for that repository.'
redirect_from:
  - /github/finding-security-vulnerabilities-and-errors-in-your-code/triaging-code-scanning-alerts-in-pull-requests
  - /code-security/secure-coding/triaging-code-scanning-alerts-in-pull-requests
  - /code-security/secure-coding/automatically-scanning-your-code-for-vulnerabilities-and-errors/triaging-code-scanning-alerts-in-pull-requests
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
type: how_to
topics:
  - Advanced Security
  - Code scanning
  - Pull requests
  - Alerts
  - Repositories
ms.openlocfilehash: 0b5fd364bcc9da7b0334214980ea943a4f12f74b
ms.sourcegitcommit: 478f2931167988096ae6478a257f492ecaa11794
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2022
ms.locfileid: '147760868'
---
{% data reusables.code-scanning.beta %}

## 关于拉取请求上的 {% data variables.product.prodname_code_scanning %} 结果

在仓库中，如果 {% data variables.product.prodname_code_scanning %} 被配置为拉取请求检查，则 {% data variables.product.prodname_code_scanning %} 将检查拉取请求中的代码。 默认情况下，这仅限于针对默认分支的拉取请求，但是您可以在 {% data variables.product.prodname_actions %} 或第三方 CI/CD 系统中更改此配置。 如果合并更改会向目标分支引入新的 {% data variables.product.prodname_code_scanning %} 警报，这些警报将在多个位置报告。

- 检查拉取请求 {% ifversion code-scanning-pr-conversations-tab %} 中的结果
- 拉取请求的“对话”选项卡是拉取请求审查的一部分 {% endif %} 
- 拉取请求的“已更改的文件”选项卡

如果你拥有存储库的写入权限，可以在“安全性”选项卡上看到任何现有的 {% data variables.product.prodname_code_scanning %} 警报。有关存储库警报的信息，请参阅“[管理存储库的 {% data variables.product.prodname_code_scanning %} 警报](/code-security/secure-coding/managing-code-scanning-alerts-for-your-repository)”。

{% ifversion fpt or ghes > 3.2 or ghae or ghec %} 当 {% data variables.product.prodname_code_scanning %} 配置为在每次推送代码时进行扫描的存储库中，{% data variables.product.prodname_code_scanning %} 还会将结果映射到任何打开的拉取请求，并将警报作为注释添加到与其他拉取请求检查相同的位置。 有关详细信息，请参阅“[推送时扫描](/code-security/code-scanning/automatically-scanning-your-code-for-vulnerabilities-and-errors/configuring-code-scanning#scanning-on-push)”。
{% endif %}

如果拉取请求针对使用 {% data variables.product.prodname_code_scanning %} 的受保护分支，并且存储库所有者已配置所需的状态检查，则“{% data variables.product.prodname_code_scanning_capc %} 结果”检查必须先通过，然后才能合并拉取请求。 有关详细信息，请参阅“[关于受保护的分支](/github/administering-a-repository/about-protected-branches#require-status-checks-before-merging)”。

## 关于 {% data variables.product.prodname_code_scanning %} 作为拉取请求检查

有许多选项可将 {% data variables.product.prodname_code_scanning %} 配置为拉取请求检查，因此每个仓库的确切设置会有所不同，有些仓库还会有多个检查。 

### {% data variables.product.prodname_code_scanning_capc %} 结果检查

对于 {% data variables.product.prodname_code_scanning %} 的所有配置，包含 {% data variables.product.prodname_code_scanning %} 结果的检查为：{% data variables.product.prodname_code_scanning_capc %} 结果。 所使用的每个分析工具的结果将单独显示。 由拉取请求中的更改引起的任何新警报都显示为注释。 

{% ifversion fpt or ghes > 3.2 or ghae or ghec %} 要查看所分析分支的完整警报集，请单击“查看所有分支警报”。 这将打开完整的警报视图，你可以在其中按类型、严重性、标记等筛选分支上的所有警报。有关详细信息，请参阅“[管理存储库的代码扫描警报](/code-security/code-scanning/automatically-scanning-your-code-for-vulnerabilities-and-errors/managing-code-scanning-alerts-for-your-repository#filtering-and-searching-for-code-scanning-alerts)”。

![拉取请求的 {% data variables.product.prodname_code_scanning_capc %} 结果检查](/assets/images/help/repository/code-scanning-results-check.png) {% endif %}

### {% data variables.product.prodname_code_scanning_capc %} 结果检查失败

如果 {% data variables.product.prodname_code_scanning %} 结果检查发现严重性为 `error``critical` 或 `high` 的问题，检查将失败并在检查结果中报告错误。 如果 {% data variables.product.prodname_code_scanning %} 发现的所有结果的严重性都较低，则警报将被视为警告或通知，检查成功。

![拉取请求上失败的 {% data variables.product.prodname_code_scanning %} 检查](/assets/images/help/repository/code-scanning-check-failure.png)

你可以通过指定会导致拉取请求检查失败的严重级别和安全严重性来覆盖仓库设置中的默认行为。 有关详细信息，请参阅“[定义导致拉取请求检查失败的严重性](/code-security/secure-coding/configuring-code-scanning#defining-the-severities-causing-pull-request-check-failure)”。

### 其他 {% data variables.product.prodname_code_scanning %} 检查

根据您的配置，您可能会看到在配置了 {% data variables.product.prodname_code_scanning %} 的拉取请求上运行其他检查。 这些通常是分析代码或上传 {% data variables.product.prodname_code_scanning %} 结果的工作流程。 当分析出现问题时，这些检查对于故障排除非常有用。 

例如，如果存储库使用 {% data variables.product.prodname_codeql_workflow %}，则在结果检查运行之前，将针对每种语言运行 {% data variables.product.prodname_codeql %} / Analyze (LANGUAGE) 检查。 如果存在配置问题，或者拉取请求中断了分析需要编译的语言（例如 C/C ++、C# 或 Java）的构建，则分析检查可能会失败。 

与其他拉取请求检查一样，可以在“检查”选项卡上看到检查失败的完整详细信息。有关配置和故障排除的详细信息，请参阅“[配置 {% data variables.product.prodname_code_scanning %}](/code-security/secure-coding/configuring-code-scanning)”或“[对 {% data variables.product.prodname_codeql %} 工作流进行故障排除](/code-security/secure-coding/troubleshooting-the-codeql-workflow)”。

## 查看拉取请求上的警报

{% ifversion code-scanning-pr-conversations-tab %} 通过查看“对话”选项卡，可以看到拉取请求中引入的任何 {% data variables.product.prodname_code_scanning %} 警报。{% data variables.product.prodname_code_scanning_capc %} 会发布拉取请求审查，该审查将每个警报作为触发警报的代码行上的注释显示。 可以对警报进行注释、关闭警报并直接从注释中查看警报的路径。 可以通过单击“显示更多详细信息”链接来查看警报的完整详细信息，该链接将带你进入警报详细信息页面。

![拉取请求“对话”选项卡中的警报注释](/assets/images/help/repository/code-scanning-pr-conversation-tab.png)

还可以在拉取请求的“已更改的文件”选项卡中查看所有 {% data variables.product.prodname_code_scanning %} 警报。 拉取请求中引入的更改差异之外的文件上的现有 {% data variables.product.prodname_code_scanning %} 警报将只显示在“已更改的文件”选项卡中。

{% else %} 通过显示“已更改的文件”选项卡，可以看到拉取请求中引入的任何 {% data variables.product.prodname_code_scanning %} 警报。每个警报都在触发警报的代码行上显示为一条注释。 警报的严重性显示在注释中。 

![拉取请求差异中的警报注释](/assets/images/help/repository/code-scanning-pr-annotation.png) {% endif %}

如果您拥有仓库的写入权限，则某些注释将包含警报额外上下文的链接。 在上例中，可以在 {% data variables.product.prodname_codeql %} 分析中单击“用户提供的值”，以查看不受信任的数据进入数据流的位置（这称为源）。 在此例中，还可以通过单击“显示路径”来查看从源到使用数据的代码（池）的完整路径。 这样就很容易检查数据是否不受信任，或者分析是否无法识别源与池之间的数据净化步骤。 有关使用 {% data variables.product.prodname_codeql %} 分析数据流的信息，请参阅“[关于数据流分析](https://codeql.github.com/docs/writing-codeql-queries/about-data-flow-analysis/)”。

要查看有关警报的更多信息，拥有写入权限的用户可单击注释中所示的“显示更多详情”链接。 这允许您在警报视图中查看工具提供的所有上下文和元数据。 在下例中，您可以查看显示问题的严重性、类型和相关通用缺陷枚举 (CWE) 的标记。 该视图还显示哪个提交引入了问题。

{% ifversion fpt or ghec or ghes > 3.4 or ghae-issue-6249 %} {% data reusables.code-scanning.alert-default-branch %} {% endif %}

在警报的详细视图中，有些 {% data variables.product.prodname_code_scanning %} 工具（例如 {% data variables.product.prodname_codeql %} 分析）还包括问题描述和“显示更多”链接以指导你如何修复代码。

{% ifversion fpt or ghec or ghes > 3.4 or ghae-issue-6249 %} ![警报说明和链接以显示详细信息](/assets/images/help/repository/code-scanning-pr-alert.png) {% else %} ![警报说明和链接以显示详细信息](/assets/images/enterprise/3.4/repository/code-scanning-pr-alert.png) {% endif %}

{% ifversion code-scanning-pr-conversations-tab %}
## 对拉取请求中的警报进行注释

可以对由拉取请求中更改引入的任何 {% data variables.product.prodname_code_scanning %} 警报进行注释。 警报在拉取请求的“对话”选项卡中显示为注释，作为拉取请求审查的一部分，也显示在“已更改的文件”选项卡中。只能对拉取请求中的更改引入的警报进行注释 。 拉取请求中引入的更改之外的文件上的现有 {% data variables.product.prodname_code_scanning %} 警报将显示在“文件已更改”选项卡中，但不能注释。

可以选择要求拉取请求中的所有对话（包括 {% data variables.product.prodname_code_scanning %} 警报上的对话），以在合并拉取请求之前被解析。 有关详细信息，请参阅“[关于受保护的分支](/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/about-protected-branches#require-conversation-resolution-before-merging)”。
{% endif %}
## 修复拉取请求上的警报

任何对拉取请求具有推送权限的人都可以修复在该拉取请求上已识别的 {% data variables.product.prodname_code_scanning %} 警报。 如果将更改提交到拉取请求，这将触发拉取请求检查的新运行。 如果您的更改修复了问题，则警报将被关闭，注释将被删除。

## 忽略拉取请求上的警报

关闭警报的另一种办法是忽略它。 您可以忽略您认为不需要修复的警报。 {% data reusables.code-scanning.close-alert-examples %} 如果你拥有存储库的写入权限，则“忽略”按钮在代码注释和警报摘要中可用。 单击“忽略”时，系统会提示你选择关闭警报的原因。
{% ifversion comment-dismissed-code-scanning-alert %} ![代码扫描警报的屏幕截图，其中选择关闭原因的下拉列表突出显示](/assets/images/help/repository/code-scanning-alert-dropdown-reason.png) {% else %} ![选择关闭警报的原因](/assets/images/help/repository/code-scanning-alert-close-drop-down.png) {% endif %} {% data reusables.code-scanning.choose-alert-dismissal-reason %}

{% data reusables.code-scanning.false-positive-fix-codeql %}

有关消除警报的详细信息，请参阅 {% ifversion delete-code-scanning-alerts %}“[管理存储库的 {% data variables.product.prodname_code_scanning %} 警报](/code-security/secure-coding/managing-code-scanning-alerts-for-your-repository#dismissing-or-deleting-alerts)”。{% else %}“[管理存储库的 {% data variables.product.prodname_code_scanning %} 警报](/code-security/code-scanning/automatically-scanning-your-code-for-vulnerabilities-and-errors/managing-code-scanning-alerts-for-your-repository#dismissing--alerts)”。{% endif %}
