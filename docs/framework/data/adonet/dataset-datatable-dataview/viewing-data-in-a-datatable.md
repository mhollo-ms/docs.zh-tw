---
title: 在 DataTable 中檢視資料
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1d26e0fb-f6e0-4afa-9a9c-b8d55b8f20dc
ms.openlocfilehash: c13f0b802b2714a17ea4014625a65ebd1b0011f4
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2019
ms.locfileid: "70785860"
---
# <a name="viewing-data-in-a-datatable"></a>在 DataTable 中檢視資料

您<xref:System.Data.DataTable>可以使用**DataTable**的資料**列**和資料**行**集合來存取的內容。 您也可以使用<xref:System.Data.DataTable.Select%2A>方法，根據包含搜尋條件、排序次序和資料列狀態的準則，傳回**DataTable**中的資料子集。 此外，您可以在使用<xref:System.Data.DataRowCollection.Find%2A>主鍵值搜尋特定資料列時，使用**DataRowCollection**的方法。

**選取**方法**DataTable**物件會傳回一組<xref:System.Data.DataRow>符合指定的準則的物件。 **選取** 採用的篩選條件運算式，排序運算式的選擇性引數和**DataViewRowState**。 篩選條件運算式會根據**DataColumn**值識別要傳回的資料列，例如`LastName = 'Smith'`。 排序運算式會遵照標準的 SQL 慣例排序資料行，例如 `LastName ASC, FirstName ASC`。 如需撰寫運算式的相關規則， <xref:System.Data.DataColumn.Expression%2A>請參閱**DataColumn**類別的屬性。

> [!TIP]
> 如果您正在執行的呼叫次數 **選取** 方法 **DataTable**，您可以藉由先建立提升效能 <xref:System.Data.DataView> 如 **DataTable**。 建立**DataView**索引資料表的資料列。 然後， **Select**方法會使用該索引，大幅減少產生查詢結果的時間。 如需建立**DataTable**的**DataView**的詳細資訊，請參閱[dataview](dataviews.md)。

**選取**方法會判斷要檢視或管理的資料列的版本將會根據<xref:System.Data.DataViewRowState>。 下表描述可能的**DataViewRowState**列舉值。

|DataViewRowState 值|說明|
|----------------------------|-----------------|
|**CurrentRows**|目前資料列，包括未變更、加入和修改過的資料列。|
|**刪除**|刪除的資料列。|
|**ModifiedCurrent**|目前的版本，即為原始資料的修改版本 （請參閱**ModifiedOriginal**）。|
|**ModifiedOriginal**|所有修改資料列的原始版本。 您可以使用**ModifiedCurrent**來取得目前的版本。|
|**加入**|新的資料列。|
|**無**|無。|
|**OriginalRows**|原始資料列，包括未變更和刪除的資料列。|
|**任何**|未變更的資料列。|

在下列範例中，會篩選 DataSet 物件，讓您只能使用其**DataViewRowState**設定為**CurrentRows**的**資料**列。

```vb
Dim column As DataColumn
Dim row As DataRow

Dim currentRows() As DataRow = _
    workTable.Select(Nothing, Nothing, DataViewRowState.CurrentRows)

If (currentRows.Length < 1 ) Then
  Console.WriteLine("No Current Rows Found")
Else
  For Each column in workTable.Columns
    Console.Write(vbTab & column.ColumnName)
  Next

  Console.WriteLine(vbTab & "RowState")

  For Each row In currentRows
    For Each column In workTable.Columns
      Console.Write(vbTab & row(column).ToString())
    Next

    Dim rowState As String = _
        System.Enum.GetName(row.RowState.GetType(), row.RowState)
    Console.WriteLine(vbTab & rowState)
  Next
End If
```

```csharp
DataRow[] currentRows = workTable.Select(
    null, null, DataViewRowState.CurrentRows);

if (currentRows.Length < 1 )
  Console.WriteLine("No Current Rows Found");
else
{
  foreach (DataColumn column in workTable.Columns)
    Console.Write("\t{0}", column.ColumnName);

  Console.WriteLine("\tRowState");

  foreach (DataRow row in currentRows)
  {
    foreach (DataColumn column in workTable.Columns)
      Console.Write("\t{0}", row[column]);

    Console.WriteLine("\t" + row.RowState);
  }
}
```

**選取 **方法可用來傳回具有不同的資料列**RowState**值或欄位值。 下列範例會傳回一個**datarow**陣列，它會參考所有已刪除的資料列，並傳回另一個以**CustLName**排序的**datarow**陣列，其中**CustID**資料行大於5。 如需如何在**已刪除**的資料列中查看資訊的詳細資訊，請參閱資料[列狀態和資料列版本](row-states-and-row-versions.md)。

```vb
' Retrieve all deleted rows.
Dim deletedRows() As DataRow = workTable.Select(Nothing, Nothing, DataViewRowState.Deleted)

' Retrieve rows where CustID > 5, and order by CustLName.
Dim custRows() As DataRow = workTable.Select( _
    "CustID > 5", "CustLName ASC")
```

```csharp
// Retrieve all deleted rows.
DataRow[] deletedRows = workTable.Select(
    null, null, DataViewRowState.Deleted);

// Retrieve rows where CustID > 5, and order by CustLName.
DataRow[] custRows = workTable.Select("CustID > 5", "CustLName ASC");
```

## <a name="see-also"></a>另請參閱

- <xref:System.Data.DataRow>
- <xref:System.Data.DataSet>
- <xref:System.Data.DataTable>
- <xref:System.Data.DataViewRowState>
- [在 DataTable 中操作資料](manipulating-data-in-a-datatable.md)
- [資料列狀態和資料列版本](row-states-and-row-versions.md)
- [ADO.NET 概觀](../ado-net-overview.md)
