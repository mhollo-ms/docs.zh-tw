---
ms.openlocfilehash: c6c897c13c84ce4b2be21da18e5702365518f286
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/01/2020
ms.locfileid: "85620025"
---
### <a name="ef-no-longer-throws-for-queryviews-with-specific-characteristics"></a>針對具有特定字元的 QueryView，不再擲回 EF

#### <a name="details"></a>詳細資料

當應用程式執行與 QueryView (導覽屬性為 0..1) 相關的查詢，嘗試在查詢中包含相關的項目時，Entity Framework 不再擲回 <xref:System.StackOverflowException?displayProperty=fullName> 例外狀況。 例如，藉由呼叫 <code>.Include(e =&gt; e.RelatedNavProp)</code>。

#### <a name="suggestion"></a>建議

這項變更只會影響在執行呼叫 .Include 的查詢時，使用 QueryView (關聯性為 1-0..1) 的程式碼。 這項功能可提高可靠性，對於幾乎所有應用程式應該都是透明的。 不過，如果這項功能造成未預期的行為，您可以在應用程式組態檔的 <code>&lt;appSettings&gt;</code> 區段中加入下列項目，來停用這項功能：<pre><code class="lang-xml">&lt;add key=&quot;EntityFramework_SimplifyUserSpecifiedViews&quot; value=&quot;false&quot; /&gt;&#13;&#10;</code></pre>

| 名稱    | 值       |
|:--------|:------------|
| 影響範圍   |Edge|
|版本|4.5.2|
|類型|執行階段|
