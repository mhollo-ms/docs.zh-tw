---
title: '如何使用 My 命名空間-c # 程式設計手冊'
description: 瞭解如何「我的」命名空間。 「我的」命名空間可讓您以簡單且直覺的方式存取許多 .NET 類別。
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, My namespace access
ms.assetid: e7152414-0ea5-4c8e-bf02-c8d5bbe45ff4
ms.openlocfilehash: 7abd5049a979d5a15d123052cba0cfdb35bf3fb7
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381706"
---
# <a name="how-to-use-the-my-namespace-c-programming-guide"></a>如何使用 My 命名空間（c # 程式設計手冊）

<xref:Microsoft.VisualBasic.MyServices>命名空間（ `My` 在 Visual Basic 中）可讓您輕鬆且直覺地存取許多 .net 類別，讓您撰寫程式碼來與電腦、應用程式、設定、資源等等互動。 雖然原本設計為搭配使用 Visual Basic，但 `MyServices` 命名空間可以在 C# 應用程式中使用。  
  
 如需從 Visual Basic 中使用 `MyServices` 命名空間的詳細資訊，請參閱[使用 My 開發](../../../visual-basic/developing-apps/development-with-my/index.md)。  
  
## <a name="add-a-reference"></a>加入參考

 您必須新增 Visual Basic 程式庫的參考，才能在您的方案中使用 `MyServices` 類別。  
  
### <a name="add-a-reference-to-the-visual-basic-library"></a>將參考新增至 Visual Basic 程式庫  
  
1. 在**方案總管**中，以滑鼠右鍵按一下 [**參考**] 節點，然後選取 [**新增參考**]。  
  
2. 當 [參考]**** 對話方塊出現時，向下捲動清單，然後選取 Microsoft.VisualBasic.dll。  
  
     您也可以在程式開頭處的 `using` 區段中包含下列這行。  
  
     [!code-csharp[csProgGuideNamespaces#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces3.cs#18)]  
  
## <a name="example"></a>範例  
 這個範例會呼叫 `MyServices` 命名空間中所包含的各種靜態方法。 為了要編譯這個程式碼，必須將 Microsoft.VisualBasic.DLL 的參考新增至專案。  
  
 [!code-csharp[csProgGuideNamespaces#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces3.cs#19)]  
  
 不是 `MyServices` 命名空間中的所有類別都可以從 C# 應用程式呼叫：例如，<xref:Microsoft.VisualBasic.MyServices.FileSystemProxy> 類別不相容。 在此特殊情況下，可以改為使用屬於 <xref:Microsoft.VisualBasic.FileIO.FileSystem> 的靜態方法，這些靜態方法也包含於 VisualBasic.dll 中。 例如，以下是如何使用一個這類方法來複製目錄：  
  
 [!code-csharp[csProgGuideNamespaces#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideNamespaces/CS/Namespaces3.cs#20)]  
  
## <a name="see-also"></a>另請參閱

- [C # 程式設計指南](../index.md)
- [命名空間](./index.md)
- [使用命名空間](./using-namespaces.md)
