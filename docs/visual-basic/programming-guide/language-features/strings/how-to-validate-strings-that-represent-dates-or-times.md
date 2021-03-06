---
title: 如何：驗證代表日期或時間的字串
ms.date: 07/20/2015
helpviewer_keywords:
- strings [Visual Basic], validating
- String data type [Visual Basic], validation
ms.assetid: ae7d4b29-3436-4032-bdbf-4650eb1c8e19
ms.openlocfilehash: 5d153ac29039375386b3cd5f03609b1e4bd1d5bf
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410563"
---
# <a name="how-to-validate-strings-that-represent-dates-or-times-visual-basic"></a>如何：驗證代表日期或時間的字串 (Visual Basic)
下列程式碼範例會設定一個 `Boolean` 值，指出字串是否代表有效的日期或時間。  
  
## <a name="example"></a>範例  
 [!code-vb[VbVbcnRegEx#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnRegEx/VB/Class1.vb#2)]  
  
## <a name="compile-the-code"></a>編譯程式碼  
 `("01/01/03")`將和取代為 `"9:30 PM"` 您想要驗證的日期和時間。 您可以使用 `String` 變數或傳回字串的方法（例如），將字串取代為另一個硬式編碼的字串。 `InputBox`  
  
## <a name="robust-programming"></a>穩固程式設計  
 嘗試將轉換成變數之前，請使用這個方法來驗證字串 `String` `DateTime` 。 藉由先檢查日期或時間，您可以避免在執行時間產生例外狀況。  
  
## <a name="see-also"></a>另請參閱

- <xref:Microsoft.VisualBasic.Information.IsDate%2A>
- <xref:Microsoft.VisualBasic.Interaction.InputBox%2A>
- [在 Visual Basic 中驗證字串](validating-strings.md)
