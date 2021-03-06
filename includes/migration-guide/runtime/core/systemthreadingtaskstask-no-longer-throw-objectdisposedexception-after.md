---
ms.openlocfilehash: 3eab96149be1e40d528cfd552bbe05ca766cf4e8
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619994"
---
### <a name="systemthreadingtaskstask-no-longer-throw-objectdisposedexception-after-object-is-disposed"></a>System.Threading.Tasks.Task 不會再於處置物件之後擲回 ObjectDisposedException

#### <a name="details"></a>詳細資料

除了 <xref:System.Threading.Tasks.Task.System%23IAsyncResult%23AsyncWaitHandle> 以外，<xref:System.Threading.Tasks.Task?displayProperty=fullName> 方法將不再於處置物件之後擲回 <xref:System.ObjectDisposedException?displayProperty=fullName> 例外狀況。這項變更支援使用快取的工作。 例如，方法可能傳回快取的工作來表示已完成的作業，而不配置新的工作。 舊版的 .NET Framework 並未提供這項功能，因為工作的任何消費者都可能會處置它，使它變得無法使用。

#### <a name="suggestion"></a>建議

請注意，Task 方法可能不會再於處置物件之後擲回 <xref:System.ObjectDisposedException?displayProperty=fullName>。 如果應用程式根據此例外狀況得知某個工作已處置，則應該將它更新，以使用 <xref:System.Threading.Tasks.Task.Status> 明確檢查工作的狀態。

| 名稱    | 值       |
|:--------|:------------|
| 影響範圍   |Minor|
|版本|4.5|
|類型|執行階段|
