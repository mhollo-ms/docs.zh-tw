---
ms.openlocfilehash: f561550d57e98a515fa3bdf56eea1dc1759b4e69
ms.sourcegitcommit: 1e6439ec4d5889fc08cf3bfb4dac2b91931eb827
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88025001"
---
## <a name="available-counters"></a>可用計數器

在各種 .NET 封裝中，垃圾收集 (GC) 的基本計量、及時 (JIT) 、元件、例外狀況、執行緒、網路和 web 要求都是使用 EventCounters 發行。

### <a name="systemruntime-counters"></a>"System.object" 計數器

下列計數器會發行為 .NET 執行時間的一部分，並在中維護 [`RuntimeEventSource.cs`](https://github.com/dotnet/coreclr/blob/master/src/System.Private.CoreLib/src/System/Diagnostics/Eventing/RuntimeEventSource.cs) 。

| 計數器 | 描述 |
|--|--|
| :::no-loc text="% Time in GC since last GC"::: (`time-in-gc`) | 自上次 GC 後 GC 中的時間百分比 |
| :::no-loc text="Allocation Rate"::: (`alloc-rate`) | 配置的速率（以位元組為單位） |
| :::no-loc text="CPU Usage"::: (`cpu-usage`) | CPU 使用量百分比 |
| :::no-loc text="Exception Count"::: (`exception-count`) | 已發生的例外狀況數目 |
| :::no-loc text="GC Heap Size"::: (`gc-heap-size`) | 考慮要配置的位元組數目，依據<xref:System.GC.GetTotalMemory(System.Boolean)?displayProperty=nameWithType> |
| :::no-loc text="Gen 0 GC Count"::: (`gen-0-gc-count`) | Gen 0 發生 GC 的次數 |
| :::no-loc text="Gen 0 Size"::: (`gen-0-size`) | Gen 0 GC 的位元組數目 |
| :::no-loc text="Gen 1 GC Count"::: (`gen-1-gc-count`) | Gen 1 發生 GC 的次數 |
| :::no-loc text="Gen 1 Size"::: (`gen-1-size`) | Gen 1 GC 的位元組數目 |
| :::no-loc text="Gen 2 GC Count"::: (`gen-2-gc-count`) | Gen 2 發生 GC 的次數 |
| :::no-loc text="Gen 2 Size"::: (`gen-2-size`) | Gen 2 GC 的位元組數目 |
| :::no-loc text="LOH Size"::: (`loh-size`) | Gen 3 GC 的位元組數目 |
| :::no-loc text="Monitor Lock Contention Count"::: (`monitor-lock-contention-count`) | 嘗試採用監視器鎖定時的爭用次數，依據<xref:System.Threading.Monitor.LockContentionCount?displayProperty=nameWithType> |
| :::no-loc text="Number of Active Timers"::: (`active-timer-count`) | <xref:System.Threading.Timer>目前作用中的實例數目，依據<xref:System.Threading.Timer.ActiveCount?displayProperty=nameWithType> |
| :::no-loc text="Number of Assemblies Loaded"::: (`assembly-count`) | 在 <xref:System.Reflection.Assembly> 某個時間點載入至進程的實例數目 |
| :::no-loc text="ThreadPool Completed Work Item Count"::: (`threadpool-completed-items-count`) | 到目前為止已處理的工作專案數<xref:System.Threading.ThreadPool> |
| :::no-loc text="ThreadPool Queue Length"::: (`threadpool-queue-length`) | 目前排入佇列中要處理的工作專案數<xref:System.Threading.ThreadPool> |
| :::no-loc text="ThreadPool Thread Count"::: (`threadpool-thread-count`) | 目前存在於中的執行緒集區執行緒數目 <xref:System.Threading.ThreadPool> ，依據<xref:System.Threading.ThreadPool.ThreadCount?displayProperty=nameWithType> |
| :::no-loc text="Working Set"::: (`working-set`) | 在時間基底中，對應至進程內容的實體記憶體數量<xref:System.Environment.WorkingSet?displayProperty=nameWithType> |

### <a name="microsoftaspnetcorehosting-counters"></a>"AspNetCore. 裝載" 計數器

下列計數器會發佈為[ASP.NET Core](/aspnet/core)的一部分，而且會在中維護 [`HostingEventSource.cs`](https://github.com/dotnet/aspnetcore/blob/master/src/Hosting/Hosting/src/Internal/HostingEventSource.cs) 。

| 計數器 | 描述 |
|--|--|
| :::no-loc text="Current Requests"::: (`current-requests`) | 已啟動但尚未停止的要求總數 |
| :::no-loc text="Failed Requests"::: (`failed-requests`) | 應用程式生命週期中發生的失敗要求總數 |
| :::no-loc text="Request Rate"::: (`requests-per-second`) | 每秒發生的要求數 |
| :::no-loc text="Total Requests"::: (`total-requests`) | 應用程式生命週期中所發生的要求總數 |

### <a name="microsoftaspnetcorehttpconnections-counters"></a>"AspNetCore" 計數器

下列計數器會發佈為[ASP.NET Core SignalR](/aspnet/core/signalr/introduction)的一部分，並在中維護 [`HttpConnectionsEventSource.cs`](https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/common/Http.Connections/src/Internal/HttpConnectionsEventSource.cs) 。

| 計數器 | 描述 |
|--|--|
| :::no-loc text="Average Connection Duration"::: (`connections-duration`) | 連接的平均持續時間（以毫秒為單位） |
| :::no-loc text="Current Connections"::: (`current-connections`) | 已啟動但尚未停止的作用中連接數目 |
| :::no-loc text="Total Connections Started"::: (`connections-started`) | 已啟動的連接總數 |
| :::no-loc text="Total Connections Stopped"::: (`connections-stopped`) | 已停止的連接總數 |
| :::no-loc text="Total Connections Timed Out"::: (`connections-timed-out`) | 已超時的連接總數 |

### <a name="microsoft-aspnetcore-server-kestrel-counters"></a>"AspNetCore-Server-Kestrel" 計數器

下列計數器會發佈為[ASP.NET Core Kestrel web 伺服器](/aspnet/core/fundamentals/servers/kestrel)的一部分，而且會在中進行維護 [`KestrelEventSource.cs`](https://github.com/dotnet/aspnetcore/blob/master/src/Servers/Kestrel/Core/src/Internal/Infrastructure/KestrelEventSource.cs) 。

| 計數器 | 描述 |
|--|--|
| :::no-loc text="Connection Queue Length"::: (`connection-queue-length`) | 連接佇列的目前長度 |
| :::no-loc text="Connection Rate"::: (`connections-per-second`) | 每秒與 web 伺服器的連接數目 |
| :::no-loc text="Current Connections"::: (`current-connections`) | 目前網頁伺服器的使用中連接數目 |
| :::no-loc text="Current TLS Handshakes"::: (`current-tls-handshakes`) | 目前的 TLS 交握數 |
| :::no-loc text="Current Upgraded Requests (WebSockets)"::: (`current-upgraded-requests`) |  (Websocket 的目前已升級要求數)  |
| :::no-loc text="Failed TLS Handshakes"::: (`failed-tls-handshakes`) | 失敗的 TLS 交握總數 |
| :::no-loc text="Request Queue Length"::: (`request-queue-length`) | 要求佇列的目前長度 |
| :::no-loc text="TLS Handshake Rate"::: (`tls-handshakes-per-second`) | 每秒的 TLS 交握數 |
| :::no-loc text="Total Connections"::: (`total-connections`) | 與網頁伺服器的連接總數 |
| :::no-loc text="Total TLS Handshakes"::: (`total-tls-handshakes`) | 與網頁伺服器的 TLS 交握總數 |
