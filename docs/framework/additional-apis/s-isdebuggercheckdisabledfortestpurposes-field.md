---
title: s_isDebuggerCheckDisabledForTestPurposes 欄位
ms.date: 03/30/2017
ms.technology: dotnet-wpf
topic_type:
- apiref
api_name:
- System.Windows.Diagnostics.VisualDiagnostics.s_isDebuggerCheckDisabledForTestPurposes
api_location:
- PresentationCore.dll
api_type:
- Assembly
ms.assetid: 9033a513-c255-4f31-b6d7-09b8d8c50e2d
ms.openlocfilehash: ad9bc0ecf4b7a8e5f3ef13fdff5aa59ca8915922
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/15/2019
ms.locfileid: "65634663"
---
# <a name="sisdebuggercheckdisabledfortestpurposes-field"></a>s_isDebuggerCheckDisabledForTestPurposes 欄位

在此私用欄位`System.Windows.Diagnostics.VisualDiagnostics`類別可由 Visual Studio 來判斷是否要執行的內部檢查作用中的偵錯工具。

## <a name="syntax"></a>語法

```csharp
private static bool s_isDebuggerCheckDisabledForTestPurposes
```

> [!WARNING]
> 中的 Api`System.Windows.Diagnostics.VisualDiagnostics`偵錯工具下執行應用程式時，才可用類別。 設定`s_isDebuggerCheckDisabledForTestPurposes`至`true`來存取偵錯工具外部 Api。
>
> Microsoft 不支援在生產環境應用程式中任何情況下使用此欄位。

## <a name="requirements"></a>需求

**命名空間︰** <xref:System.Windows.Diagnostics>

**組件：** PresentationCore （在 PresentationCore.dll 中)

**.NET framework 版本：** 自 4.6 起可用。
