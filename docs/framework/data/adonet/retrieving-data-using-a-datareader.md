---
title: 使用 DataReader 擷取資料
description: 瞭解如何使用 ADO.NET 中的 DataReader 和此範例程式碼來抓取資料。 DataReader 提供未緩衝的資料流程。
ms.date: 10/29/2018
dev_langs:
- csharp
- vb
ms.assetid: 97afc121-fb8b-465b-bab3-6d844420badb
ms.openlocfilehash: 6e5161cc325bf0379bb9241b99c473c539ad1081
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286594"
---
# <a name="retrieve-data-using-a-datareader"></a>使用 DataReader 取出資料
若要使用**DataReader**來抓取資料，請建立**command**物件的實例，然後呼叫**ExecuteReader**來建立**DataReader** ，以從資料來源中取出資料列。 **DataReader**提供了未緩衝的資料流程，可讓程式邏輯依序有效率地處理來自資料來源的結果。 當您正在抓取大量資料時， **DataReader**是不錯的選擇，因為資料不會快取到記憶體中。

下列範例說明如何使用**DataReader**，其中 `reader` 代表有效的 datareader，並 `command` 代表有效的命令物件。  

```csharp
reader = command.ExecuteReader();  
```

```vb
reader = command.ExecuteReader()
```  

使用**DataReader. Read**方法，從查詢結果取得資料列。 您可以將資料行的名稱或序數傳遞至**DataReader**，以存取傳回之資料列的每個資料行。 不過，為了達到最佳效能， **DataReader**提供一系列的方法，可讓您存取其原生資料類型（**GetDateTime**、 **GetDouble**、 **GetGuid**、 **GetInt32**等）中的資料行值。 如需資料提供者特定**datareader**之具類型存取子方法的清單，請參閱 <xref:System.Data.OleDb.OleDbDataReader> 和 <xref:System.Data.SqlClient.SqlDataReader> 。 當您知道基礎資料類型時，使用具類型的存取子方法，可減少在抓取資料行值時所需的類型轉換數量。  
  
 下列範例會逐一查看**DataReader**物件，並傳回每個資料列中的兩個數據行。  
  
 [!code-csharp[DataWorks SqlClient.HasRows#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.HasRows/CS/source.cs#1)]
 [!code-vb[DataWorks SqlClient.HasRows#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.HasRows/VB/source.vb#1)]  
  
## <a name="closing-the-datareader"></a>關閉 DataReader  
 當您完成使用**DataReader**物件時，請一律呼叫**Close**方法。  
  
 如果您的**命令**包含輸出參數或傳回值，則在**DataReader**關閉之前，這些值都無法使用。  
  
 當**datareader**開啟時，該**datareader**會以獨佔方式使用**連接**。 您無法執行**連接**的任何命令，包括建立另一個**datareader**，直到原始**DataReader**關閉為止。  
  
> [!NOTE]
> 請勿在**連接**、 **DataReader**或您類別的**Finalize**方法中的任何其他 Managed 物件上呼叫**Close**或**Dispose** 。 在完成項中，只需釋放類別直接擁有的 Unmanaged 資源。 如果您的類別未擁有任何非受控資源，請勿在您的類別定義中包含**Finalize**方法。 如需詳細資訊，請參閱[垃圾收集](../../../standard/garbage-collection/index.md)。  
  
## <a name="retrieving-multiple-result-sets-using-nextresult"></a>使用 NextResult 抓取多個結果集  
 如果**DataReader**傳回多個結果集，請呼叫**NextResult**方法，依序逐一查看結果集。 下列範例顯示 <xref:System.Data.SqlClient.SqlDataReader> 使用 <xref:System.Data.SqlClient.SqlCommand.ExecuteReader%2A> 方法，處理兩個 SELECT 陳述式的結果。  
  
 [!code-csharp[DataWorks SqlClient.NextResult#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.NextResult/CS/source.cs#1)]
 [!code-vb[DataWorks SqlClient.NextResult#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.NextResult/VB/source.vb#1)]  
  
## <a name="getting-schema-information-from-the-datareader"></a>從 DataReader 取得架構資訊  
 當**DataReader**開啟時，您可以使用**GetSchemaTable**方法來抓取目前結果集的架構資訊。 **GetSchemaTable**會傳回一個物件，其中 <xref:System.Data.DataTable> 填入包含目前結果集之架構資訊的資料列和資料行。 **DataTable**會針對結果集的每個資料行，各包含一個資料列。 架構資料表的每個資料行都會對應到結果集資料列中所傳回之資料行的屬性，其中**ColumnName**是屬性的名稱，而資料行的值是屬性的值。 下列範例會寫出**DataReader**的架構資訊。  
  
 [!code-csharp[DataWorks SqlClient.GetSchemaTable#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.GetSchemaTable/CS/source.cs#1)]
 [!code-vb[DataWorks SqlClient.GetSchemaTable#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.GetSchemaTable/VB/source.vb#1)]  
  
## <a name="working-with-ole-db-chapters"></a>使用 OLE DB 章節  
 階層式資料列集或章節（OLE DB 類型**DBTYPE_HCHAPTER**，ADO 類型**adChapter**）可以使用來抓取 <xref:System.Data.OleDb.OleDbDataReader> 。 當包含章節的查詢當做**datareader**傳回時，該章節會**當做 datareader 中**的資料行傳回，並公開為**datareader**物件。  
  
 ADO.NET**資料集**也可以使用資料表之間的父子式關聯性，來表示階層資料列集。 如需詳細資訊，請參閱[dataset、datatable 和 dataview](./dataset-datatable-dataview/index.md)。  
  
 下列程式碼範例使用 MSDataShape 提供者，替客戶清單中每位客戶的訂單產生章節資料行。  
  
```vb  
Using connection As OleDbConnection = New OleDbConnection(
    "Provider=MSDataShape;Data Provider=SQLOLEDB;" &
    "Data Source=localhost;Integrated Security=SSPI;Initial Catalog=northwind")

    Using custCMD As OleDbCommand = New OleDbCommand(
        "SHAPE {SELECT CustomerID, CompanyName FROM Customers} " &
        "APPEND ({SELECT CustomerID, OrderID FROM Orders} AS CustomerOrders " &
        "RELATE CustomerID TO CustomerID)", connection)

        connection.Open()

        Using custReader As OleDbDataReader = custCMD.ExecuteReader()

            Do While custReader.Read()
                Console.WriteLine("Orders for " & custReader.GetString(1))
                ' custReader.GetString(1) = CompanyName  

                Using orderReader As OleDbDataReader = custReader.GetValue(2)
                    ' custReader.GetValue(2) = Orders chapter as DataReader  

                    Do While orderReader.Read()
                        Console.WriteLine(vbTab & orderReader.GetInt32(1))
                        ' orderReader.GetInt32(1) = OrderID  
                    Loop
                    orderReader.Close()
                End Using
            Loop
            ' Make sure to always close readers and connections.  
            custReader.Close()
        End Using
    End Using
End Using
```  
  
```csharp  
using (OleDbConnection connection = new OleDbConnection(
    "Provider=MSDataShape;Data Provider=SQLOLEDB;" +
    "Data Source=localhost;Integrated Security=SSPI;Initial Catalog=northwind"))
{
    using (OleDbCommand custCMD = new OleDbCommand(
        "SHAPE {SELECT CustomerID, CompanyName FROM Customers} " +
        "APPEND ({SELECT CustomerID, OrderID FROM Orders} AS CustomerOrders " +
        "RELATE CustomerID TO CustomerID)", connection))
    {
        connection.Open();

        using (OleDbDataReader custReader = custCMD.ExecuteReader())
        {

            while (custReader.Read())
            {
                Console.WriteLine("Orders for " + custReader.GetString(1));
                // custReader.GetString(1) = CompanyName  

                using (OleDbDataReader orderReader = (OleDbDataReader)custReader.GetValue(2))
                {
                    // custReader.GetValue(2) = Orders chapter as DataReader  

                    while (orderReader.Read())
                        Console.WriteLine("\t" + orderReader.GetInt32(1));
                    // orderReader.GetInt32(1) = OrderID  
                    orderReader.Close();
                }
            }
            // Make sure to always close readers and connections.  
            custReader.Close();
        }
    }
}
```  
  
## <a name="returning-results-with-oracle-ref-cursors"></a>使用 Oracle REF 資料指標傳回結果  
 Oracle 的 .NET Framework 資料提供者支援使用 Oracle REF CURSOR 傳回查詢結果。 Oracle REF CURSOR 以 <xref:System.Data.OracleClient.OracleDataReader> 傳回。  
  
 您可以 <xref:System.Data.OracleClient.OracleDataReader> 使用方法來抓取代表 ORACLE REF 資料指標的物件 <xref:System.Data.OracleClient.OracleCommand.ExecuteReader%2A> 。 您也可以指定 <xref:System.Data.OracleClient.OracleCommand> ，它會傳回一個或多個 ORACLE REF 資料**SelectCommand**指標做為 <xref:System.Data.OracleClient.OracleDataAdapter> 用來填滿之的 SelectCommand <xref:System.Data.DataSet> 。  
  
 若要存取從 Oracle 資料來源傳回的 REF CURSOR，請 <xref:System.Data.OracleClient.OracleCommand> 為您的查詢建立，並將參考參考游標的輸出參數加入至的 <xref:System.Data.OracleClient.OracleCommand.Parameters> 集合 <xref:System.Data.OracleClient.OracleCommand> 。 參數的名稱必須符合查詢中 REF CURSOR 參數的名稱。 將參數的類型設定為 <xref:System.Data.OracleClient.OracleType.Cursor?displayProperty=nameWithType> 。 的 <xref:System.Data.OracleClient.OracleCommand.ExecuteReader?displayProperty=nameWithType> 方法會 <xref:System.Data.OracleClient.OracleCommand> <xref:System.Data.OracleClient.OracleDataReader> 針對 REF 資料指標傳回。  
  
 如果您傳回 <xref:System.Data.OracleClient.OracleCommand> 多個 REF 資料指標，請加入多個輸出參數。 您可以藉由呼叫方法來存取不同的 REF 資料指標 <xref:System.Data.OracleClient.OracleCommand.ExecuteReader?displayProperty=nameWithType> 。 對的呼叫會傳回 <xref:System.Data.OracleClient.OracleCommand.ExecuteReader> <xref:System.Data.OracleClient.OracleDataReader> 參考第一個 REF 資料指標的。 接著，您可以呼叫 <xref:System.Data.OracleClient.OracleDataReader.NextResult?displayProperty=nameWithType> 方法來存取後續的 REF 資料指標。 雖然集合中的參數 <xref:System.Data.OracleClient.OracleCommand.Parameters?displayProperty=nameWithType> 符合 REF CURSOR 輸出參數的名稱，但會依其 <xref:System.Data.OracleClient.OracleDataReader> 加入集合的順序來存取它們 <xref:System.Data.OracleClient.OracleCommand.Parameters> 。  
  
 例如，請考慮下列的 Oracle Package 和 Package 內容。  
  
```sql
CREATE OR REPLACE PACKAGE CURSPKG AS
  TYPE T_CURSOR IS REF CURSOR;
  PROCEDURE OPEN_TWO_CURSORS (EMPCURSOR OUT T_CURSOR,
    DEPTCURSOR OUT T_CURSOR);
END CURSPKG;  
  
CREATE OR REPLACE PACKAGE BODY CURSPKG AS
  PROCEDURE OPEN_TWO_CURSORS (EMPCURSOR OUT T_CURSOR,
    DEPTCURSOR OUT T_CURSOR)
  IS
  BEGIN
    OPEN EMPCURSOR FOR SELECT * FROM DEMO.EMPLOYEE;
    OPEN DEPTCURSOR FOR SELECT * FROM DEMO.DEPARTMENT;
  END OPEN_TWO_CURSORS;
END CURSPKG;
```  
  
 下列程式碼會建立 <xref:System.Data.OracleClient.OracleCommand> ，它會將類型的兩個參數加入至集合，以傳回先前 Oracle 封裝的 REF 資料指標 <xref:System.Data.OracleClient.OracleType.Cursor?displayProperty=nameWithType> <xref:System.Data.OracleClient.OracleCommand.Parameters?displayProperty=nameWithType> 。  
  
```vb  
Dim cursCmd As OracleCommand = New OracleCommand("CURSPKG.OPEN_TWO_CURSORS", oraConn)  
cursCmd.Parameters.Add("EMPCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output  
cursCmd.Parameters.Add("DEPTCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output  
```  
  
```csharp  
OracleCommand cursCmd = new OracleCommand("CURSPKG.OPEN_TWO_CURSORS", oraConn);  
cursCmd.Parameters.Add("EMPCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output;  
cursCmd.Parameters.Add("DEPTCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output;  
```  
  
 下列程式碼會使用的和方法，傳回上一個命令的結果 <xref:System.Data.OracleClient.OracleDataReader.Read> <xref:System.Data.OracleClient.OracleDataReader.NextResult> <xref:System.Data.OracleClient.OracleDataReader> 。 REF CURSOR 參數會依順序傳回。  
  
```vb  
oraConn.Open()  
  
Dim cursCmd As OracleCommand = New OracleCommand("CURSPKG.OPEN_TWO_CURSORS", oraConn)  
cursCmd.CommandType = CommandType.StoredProcedure  
cursCmd.Parameters.Add("EMPCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output  
cursCmd.Parameters.Add("DEPTCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output  
  
Dim reader As OracleDataReader = cursCmd.ExecuteReader()  
  
Console.WriteLine(vbCrLf & "Emp ID" & vbTab & "Name")  
  
Do While reader.Read()  
  Console.WriteLine("{0}" & vbTab & "{1}, {2}", reader.GetOracleNumber(0), reader.GetString(1), reader.GetString(2))  
Loop  
  
reader.NextResult()  
  
Console.WriteLine(vbCrLf & "Dept ID" & vbTab & "Name")  
  
Do While reader.Read()  
  Console.WriteLine("{0}" & vbTab & "{1}", reader.GetOracleNumber(0), reader.GetString(1))  
Loop  
' Make sure to always close readers and connections.  
reader.Close()  
oraConn.Close()  
```  
  
```csharp  
oraConn.Open();  
  
OracleCommand cursCmd = new OracleCommand("CURSPKG.OPEN_TWO_CURSORS", oraConn);  
cursCmd.CommandType = CommandType.StoredProcedure;  
cursCmd.Parameters.Add("EMPCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output;  
cursCmd.Parameters.Add("DEPTCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output;  
  
OracleDataReader reader = cursCmd.ExecuteReader();  
  
Console.WriteLine("\nEmp ID\tName");  
  
while (reader.Read())  
  Console.WriteLine("{0}\t{1}, {2}", reader.GetOracleNumber(0), reader.GetString(1), reader.GetString(2));  
  
reader.NextResult();  
  
Console.WriteLine("\nDept ID\tName");  
  
while (reader.Read())  
  Console.WriteLine("{0}\t{1}", reader.GetOracleNumber(0), reader.GetString(1));  
// Make sure to always close readers and connections.  
reader.Close();  
oraConn.Close();  
```  
  
 下列範例會使用先前的命令，將 <xref:System.Data.DataSet> Oracle 封裝的結果填入。  
  
```vb  
Dim ds As DataSet = New DataSet()  
  
Dim adapter As OracleDataAdapter = New OracleDataAdapter(cursCmd)  
adapter.TableMappings.Add("Table", "Employees")  
adapter.TableMappings.Add("Table1", "Departments")  
  
adapter.Fill(ds)  
```  
  
```csharp  
DataSet ds = new DataSet();  
  
OracleDataAdapter adapter = new OracleDataAdapter(cursCmd);  
adapter.TableMappings.Add("Table", "Employees");  
adapter.TableMappings.Add("Table1", "Departments");  
  
adapter.Fill(ds);  
```

> [!NOTE]
> 若要避免**OverflowException**，建議您同時處理從 Oracle NUMBER 類型到有效 .NET Framework 類型的任何轉換，然後再將值儲存在中 <xref:System.Data.DataRow> 。 您可以使用 <xref:System.Data.Common.DataAdapter.FillError> 事件來判斷是否已發生**OverflowException** 。 如需事件的詳細資訊 <xref:System.Data.Common.DataAdapter.FillError> ，請參閱[處理 DataAdapter 事件](handling-dataadapter-events.md)。  
  
## <a name="see-also"></a>另請參閱

- [DataAdapter 和 DataReader](dataadapters-and-datareaders.md)
- [命令和參數](commands-and-parameters.md)
- [擷取資料庫結構描述資訊](retrieving-database-schema-information.md)
- [ADO.NET 概觀](ado-net-overview.md) \(部分機器翻譯\)
