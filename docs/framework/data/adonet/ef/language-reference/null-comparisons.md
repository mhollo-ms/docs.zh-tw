---
title: Null 比較
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: ef88af8c-8dfe-4556-8b56-81df960a900b
ms.openlocfilehash: 697c933daeb3c68fb4ea89a957b639a79a9407f8
ms.sourcegitcommit: 99b153b93bf94d0fecf7c7bcecb58ac424dfa47c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/25/2020
ms.locfileid: "80249652"
---
# <a name="null-comparisons"></a>Null 比較
資料來源中的 `null` 值表示該值未知。 在 LINQ 到實體查詢中，可以檢查空值，以便某些計算或比較僅對具有有效或非空資料的行執行。 不過，CLR null 語意 (Semantics) 可能與資料來源的 null 語意不同。 大部分的資料庫都使用三值邏輯來處理 null 比較， 也就是說，與空值的比較不計算 到`true`或`false`，它計算到`unknown`。 通常，這是 ANSI NULLS 的實作，可是實際情況不一定如此。  
  
 根據預設，SQL Server 中 null 等於 null 的比較會傳回 null 值。 在下面的示例中，結果集中排除了空`ShipDate`行，Transact-SQL 語句將返回 0 行。  
  
```sql  
-- Find order details and orders with no ship date.  
SELECT h.SalesOrderID  
FROM Sales.SalesOrderHeader h  
JOIN Sales.SalesOrderDetail o ON o.SalesOrderID = h.SalesOrderID  
WHERE h.ShipDate IS Null  
```  
  
 這與 CLR null 語意大不相同，其中 null 等於 null 的比較則是會傳回 true。  
  
 下列 LINQ 查詢是以 CLR 表示，但卻在資料來源中執行。 因為無法保證 CLR 語意在資料來源能被接受，預期的行為是不確定的。  
  
 [!code-csharp[DP L2E Conceptual Examples#JoinOnNull](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#joinonnull)]
 [!code-vb[DP L2E Conceptual Examples#JoinOnNull](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#joinonnull)]  
  
## <a name="key-selectors"></a>索引鍵選擇器  
 *鍵選擇器*是標準查詢運算子中用於從元素中提取金鑰的函數。 在索引鍵選擇器函式中，可以將運算式與常數進行比較。 如果比較運算式與 null 常數，或比較兩個 null 常數，則會顯示 CLR null 語意。 如果比較資料來源中兩個具有 null 值的資料行，則會顯示存放區 null 語意。 索引鍵選擇器存在於許多群組和排序的標準查詢運算子 (如 <xref:System.Linq.Queryable.GroupBy%2A>)，而且是用來選取做為排序或群組依據的索引鍵。  
  
## <a name="null-property-on-a-null-object"></a>Null 物件上的 Null 屬性  
 在實體框架中，空物件的屬性為空。 當您嘗試參考 CLR 中 null 物件的屬性時，將會收到 <xref:System.NullReferenceException>。 當 LINQ 查詢包含 null 物件的屬性時，可能會產生不一致的行為。  
  
 舉例來說，下列查詢中 `NewProduct` 的轉型會在命令樹層完成，這可能導致 `Introduced` 屬性為 null。 如果資料庫已定義 null 比較而使 <xref:System.DateTime> 的比較評估為 true，則會包含資料列。  
  
 [!code-csharp[DP L2E Conceptual Examples#CastResultsIsNull](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#castresultsisnull)]
 [!code-vb[DP L2E Conceptual Examples#CastResultsIsNull](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#castresultsisnull)]  
  
## <a name="passing-null-collections-to-aggregate-functions"></a>將 Null 集合傳遞至彙總函式  
 在 LINQ 到實體中，當您將支援`IQueryable`彙總函式的集合傳遞給彙總函式時，聚合操作將在資料庫中執行。 在記憶體中執行的查詢和在資料庫執行的查詢可能會產生不同的結果。 在記憶體中查詢時，如果沒有相符結果，查詢會傳回零。 同樣的查詢在資料庫中則會傳回 `null`。 如果將`null`值傳遞給 LINQ 彙總函式，將引發異常。 要接受可能`null`的值，請將接收查詢結果的類型和屬性轉換為空數值型別。  
  
## <a name="see-also"></a>另請參閱

- [LINQ to Entities 查詢中的運算式](expressions-in-linq-to-entities-queries.md)
