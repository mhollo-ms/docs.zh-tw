---
title: '#warning - C# 參考'
ms.date: 07/20/2015
f1_keywords:
- '#warning'
helpviewer_keywords:
- '#warning directive [C#]'
ms.assetid: e6fb496d-bb8b-4018-baf6-5b60a0c8902b
ms.openlocfilehash: 38c3807a696599390667060d3bf374c68845fed0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2020
ms.locfileid: "75715062"
---
# <a name="warning-c-reference"></a>#warning (C# 參考)
`#warning` 可讓您從程式碼中的特定位置產生 [CS1030](../../misc/cs1030.md) 層級一的編譯器警告。 例如：  
  
```csharp
#warning Deprecated code in this method.  
```  
  
## <a name="remarks"></a>備註
 `#warning` 常見於條件指示詞中。 也可以使用 [#error](./preprocessor-error.md) 產生使用者定義錯誤。  
  
## <a name="example"></a>範例  

```csharp
// preprocessor_warning.cs  
// CS1030 expected  
#define DEBUG  
class MainClass
{  
    static void Main()
    {  
#if DEBUG  
#warning DEBUG is defined  
#endif  
    }  
}  
```  

## <a name="see-also"></a>另請參閱

- [C# 參考](../index.md)
- [C# 程式設計指南](../../programming-guide/index.md)
- [C# 預處理器指令](./index.md)
