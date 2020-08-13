---
title: 調試鎖死-.NET Core
description: 本教學課程會逐步引導您在 .NET Core 中偵測鎖定問題。
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 247521176297254180d794d4d4fc850f30e343b0
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/22/2020
ms.locfileid: "86926397"
---
# <a name="debug-a-deadlock-in-net-core"></a><span data-ttu-id="42f47-103">在 .NET Core 中調試鎖死</span><span class="sxs-lookup"><span data-stu-id="42f47-103">Debug a deadlock in .NET Core</span></span>

<span data-ttu-id="42f47-104">**本文適用于：✔️** .net CORE 3.1 SDK 和更新版本</span><span class="sxs-lookup"><span data-stu-id="42f47-104">**This article applies to: ✔️** .NET Core 3.1 SDK and later versions</span></span>

<span data-ttu-id="42f47-105">在本教學課程中，您將瞭解如何調試鎖死案例。</span><span class="sxs-lookup"><span data-stu-id="42f47-105">In this tutorial, you'll learn how to debug a deadlock scenario.</span></span> <span data-ttu-id="42f47-106">使用提供的範例[ASP.NET Core web 應用程式](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios)原始程式碼存放庫，您可能會刻意造成鎖死。</span><span class="sxs-lookup"><span data-stu-id="42f47-106">Using the provided example [ASP.NET Core web app](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios) source code repository, you can cause a deadlock intentionally.</span></span> <span data-ttu-id="42f47-107">端點會遇到停止回應和執行緒累積的情況。</span><span class="sxs-lookup"><span data-stu-id="42f47-107">The endpoint will experience a hang and thread accumulation.</span></span> <span data-ttu-id="42f47-108">您將瞭解如何使用各種工具來分析問題，例如核心傾印、核心傾印分析和進程追蹤。</span><span class="sxs-lookup"><span data-stu-id="42f47-108">You'll learn how you can use various tools to analyze the problem, such as core dumps, core dump analysis, and process tracing.</span></span>

<span data-ttu-id="42f47-109">在本教學課程中，您將：</span><span class="sxs-lookup"><span data-stu-id="42f47-109">In this tutorial, you will:</span></span>

> [!div class="checklist"]
>
> - <span data-ttu-id="42f47-110">調查應用程式停止回應</span><span class="sxs-lookup"><span data-stu-id="42f47-110">Investigate an app hang</span></span>
> - <span data-ttu-id="42f47-111">產生核心傾印檔案</span><span class="sxs-lookup"><span data-stu-id="42f47-111">Generate a core dump file</span></span>
> - <span data-ttu-id="42f47-112">分析傾印檔案中的處理常式執行緒</span><span class="sxs-lookup"><span data-stu-id="42f47-112">Analyze process threads in the dump file</span></span>
> - <span data-ttu-id="42f47-113">分析呼叫堆疊和同步區塊</span><span class="sxs-lookup"><span data-stu-id="42f47-113">Analyze callstacks and sync blocks</span></span>
> - <span data-ttu-id="42f47-114">診斷並解決鎖死</span><span class="sxs-lookup"><span data-stu-id="42f47-114">Diagnose and solve a deadlock</span></span>

## <a name="prerequisites"></a><span data-ttu-id="42f47-115">先決條件</span><span class="sxs-lookup"><span data-stu-id="42f47-115">Prerequisites</span></span>

<span data-ttu-id="42f47-116">教學課程會使用：</span><span class="sxs-lookup"><span data-stu-id="42f47-116">The tutorial uses:</span></span>

- <span data-ttu-id="42f47-117">[.Net Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core)或更新版本</span><span class="sxs-lookup"><span data-stu-id="42f47-117">[.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core) or a later version</span></span>
- <span data-ttu-id="42f47-118">用於觸發案例的[範例 debug 目標-web 應用程式](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios)</span><span class="sxs-lookup"><span data-stu-id="42f47-118">[Sample debug target - web app](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios) to trigger the scenario</span></span>
- <span data-ttu-id="42f47-119">[dotnet-列出進程的追蹤](dotnet-trace.md)</span><span class="sxs-lookup"><span data-stu-id="42f47-119">[dotnet-trace](dotnet-trace.md) to list processes</span></span>
- <span data-ttu-id="42f47-120">[dotnet-](dotnet-dump.md)用來收集和分析傾印檔案的傾印</span><span class="sxs-lookup"><span data-stu-id="42f47-120">[dotnet-dump](dotnet-dump.md) to collect, and analyze a dump file</span></span>

## <a name="core-dump-generation"></a><span data-ttu-id="42f47-121">核心傾印產生</span><span class="sxs-lookup"><span data-stu-id="42f47-121">Core dump generation</span></span>

<span data-ttu-id="42f47-122">若要調查應用程式無回應，核心傾印或記憶體傾印可讓您檢查其執行緒的狀態，以及可能發生爭用問題的任何可能鎖定。</span><span class="sxs-lookup"><span data-stu-id="42f47-122">To investigate application unresponsiveness, a core dump or memory dump allows you to inspect the state of its threads and any possible locks that may have contention issues.</span></span> <span data-ttu-id="42f47-123">使用範例根目錄中的下列命令，執行[範例 debug](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios)應用程式：</span><span class="sxs-lookup"><span data-stu-id="42f47-123">Run the [sample debug](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios) application using the following command from the sample root directory:</span></span>

```dotnetcli
dotnet run
```

<span data-ttu-id="42f47-124">若要尋找處理序識別碼，請使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="42f47-124">To find the process ID, use the following command:</span></span>

```dotnetcli
dotnet-trace ps
```

<span data-ttu-id="42f47-125">請記下命令輸出中的處理序識別碼。</span><span class="sxs-lookup"><span data-stu-id="42f47-125">Take note of the process ID from your command output.</span></span> <span data-ttu-id="42f47-126">我們的處理序識別碼是 `4807` ，但您的程式識別碼不同。</span><span class="sxs-lookup"><span data-stu-id="42f47-126">Our process ID was `4807`, but yours will be different.</span></span> <span data-ttu-id="42f47-127">流覽至下列 URL，這是範例網站上的 API 端點：</span><span class="sxs-lookup"><span data-stu-id="42f47-127">Navigate to the following URL, which is an API endpoint on the sample site:</span></span>

[https://localhost:5001/api/diagscenario/deadlock](https://localhost:5001/api/diagscenario/deadlock)

<span data-ttu-id="42f47-128">網站的 API 要求將會停止回應，而不會回應。</span><span class="sxs-lookup"><span data-stu-id="42f47-128">The API request to the site will hang and not respond.</span></span> <span data-ttu-id="42f47-129">讓要求執行約10-15 秒。</span><span class="sxs-lookup"><span data-stu-id="42f47-129">Let the request run for about 10-15 seconds.</span></span> <span data-ttu-id="42f47-130">然後使用下列命令建立核心傾印：</span><span class="sxs-lookup"><span data-stu-id="42f47-130">Then create the core dump using the following command:</span></span>

### <a name="linux"></a>[<span data-ttu-id="42f47-131">Linux</span><span class="sxs-lookup"><span data-stu-id="42f47-131">Linux</span></span>](#tab/linux)

```bash
sudo dotnet-dump collect -p 4807
```

### <a name="windows"></a>[<span data-ttu-id="42f47-132">Windows</span><span class="sxs-lookup"><span data-stu-id="42f47-132">Windows</span></span>](#tab/windows)

```console
dotnet-dump collect -p 4807
```

---

## <a name="analyze-the-core-dump"></a><span data-ttu-id="42f47-133">分析核心傾印</span><span class="sxs-lookup"><span data-stu-id="42f47-133">Analyze the core dump</span></span>

<span data-ttu-id="42f47-134">若要啟動核心傾印分析，請使用下列命令來開啟核心傾印 `dotnet-dump analyze` 。</span><span class="sxs-lookup"><span data-stu-id="42f47-134">To start the core dump analysis, open the core dump using the following `dotnet-dump analyze` command.</span></span> <span data-ttu-id="42f47-135">引數是稍早收集到的核心傾印檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="42f47-135">The argument is the path to the core dump file that was collected earlier.</span></span>

```dotnetcli
dotnet-dump analyze  ~/.dotnet/tools/core_20190513_143916
```

<span data-ttu-id="42f47-136">因為您正在查看可能的停止回應，所以您想要在程式中進行執行緒活動的整體操作。</span><span class="sxs-lookup"><span data-stu-id="42f47-136">Since you're looking at a potential hang, you want an overall feel for the thread activity in the process.</span></span> <span data-ttu-id="42f47-137">您可以使用命令，如下 `threads` 所示：</span><span class="sxs-lookup"><span data-stu-id="42f47-137">You can use the `threads` command as shown below:</span></span>

```console
> threads
*0 0x1DBFF (121855)
 1 0x1DC01 (121857)
 2 0x1DC02 (121858)
 3 0x1DC03 (121859)
 4 0x1DC04 (121860)
 5 0x1DC05 (121861)
 6 0x1DC06 (121862)
 7 0x1DC07 (121863)
 8 0x1DC08 (121864)
 9 0x1DC09 (121865)
 10 0x1DC0A (121866)
 11 0x1DC0D (121869)
 12 0x1DC0E (121870)
 13 0x1DC10 (121872)
 14 0x1DC11 (121873)
 15 0x1DC12 (121874)
 16 0x1DC13 (121875)
 17 0x1DC14 (121876)
 18 0x1DC15 (121877)
 19 0x1DC1C (121884)
 20 0x1DC1D (121885)
 21 0x1DC1E (121886)
 22 0x1DC21 (121889)
 23 0x1DC22 (121890)
 24 0x1DC23 (121891)
 25 0x1DC24 (121892)
 26 0x1DC25 (121893)
...
...
 317 0x1DD48 (122184)
 318 0x1DD49 (122185)
 319 0x1DD4A (122186)
 320 0x1DD4B (122187)
 321 0x1DD4C (122188)
 ```

<span data-ttu-id="42f47-138">輸出會顯示目前在進程中執行的所有線程，及其相關聯的偵錯工具執行緒識別碼和作業系統執行緒識別碼。</span><span class="sxs-lookup"><span data-stu-id="42f47-138">The output shows all the threads currently running in the process with their associated debugger thread ID and operating system thread ID.</span></span> <span data-ttu-id="42f47-139">根據輸出，有超過300個執行緒。</span><span class="sxs-lookup"><span data-stu-id="42f47-139">Based on the output, there are over 300 threads.</span></span>

<span data-ttu-id="42f47-140">下一步是取得每個執行緒的呼叫堆疊，以進一步瞭解執行緒目前執行的作業。</span><span class="sxs-lookup"><span data-stu-id="42f47-140">The next step is to get a better understanding of what the threads are currently doing by getting each thread's callstack.</span></span> <span data-ttu-id="42f47-141">`clrstack`命令可以用來輸出呼叫堆疊。</span><span class="sxs-lookup"><span data-stu-id="42f47-141">The `clrstack` command can be used to output callstacks.</span></span> <span data-ttu-id="42f47-142">它可以輸出單一呼叫堆疊或所有呼叫堆疊。</span><span class="sxs-lookup"><span data-stu-id="42f47-142">It can either output a single callstack or all the callstacks.</span></span> <span data-ttu-id="42f47-143">使用下列命令，輸出進程中所有線程的所有呼叫堆疊：</span><span class="sxs-lookup"><span data-stu-id="42f47-143">Use the following command to output all the callstacks for all the threads in the process:</span></span>

```console
clrstack -all
```

<span data-ttu-id="42f47-144">輸出的代表性部分如下所示：</span><span class="sxs-lookup"><span data-stu-id="42f47-144">A representative portion of the output looks like:</span></span>

```console
  ...
  ...
  ...
        Child SP               IP Call Site
00007F2AE37B5680 00007f305abc6360 [GCFrame: 00007f2ae37b5680]
00007F2AE37B5770 00007f305abc6360 [GCFrame: 00007f2ae37b5770]
00007F2AE37B57D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae37b57d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE37B5920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE37B5950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE37B5CA0 00007f30593044af [GCFrame: 00007f2ae37b5ca0]
00007F2AE37B5D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae37b5d70]
OS Thread Id: 0x1dc82
        Child SP               IP Call Site
00007F2AE2FB4680 00007f305abc6360 [GCFrame: 00007f2ae2fb4680]
00007F2AE2FB4770 00007f305abc6360 [GCFrame: 00007f2ae2fb4770]
00007F2AE2FB47D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae2fb47d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE2FB4920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE2FB4950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE2FB4CA0 00007f30593044af [GCFrame: 00007f2ae2fb4ca0]
00007F2AE2FB4D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae2fb4d70]
OS Thread Id: 0x1dc83
        Child SP               IP Call Site
00007F2AE27B3680 00007f305abc6360 [GCFrame: 00007f2ae27b3680]
00007F2AE27B3770 00007f305abc6360 [GCFrame: 00007f2ae27b3770]
00007F2AE27B37D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae27b37d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE27B3920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE27B3950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE27B3CA0 00007f30593044af [GCFrame: 00007f2ae27b3ca0]
00007F2AE27B3D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae27b3d70]
OS Thread Id: 0x1dc84
        Child SP               IP Call Site
00007F2AE1FB2680 00007f305abc6360 [GCFrame: 00007f2ae1fb2680]
00007F2AE1FB2770 00007f305abc6360 [GCFrame: 00007f2ae1fb2770]
00007F2AE1FB27D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae1fb27d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE1FB2920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE1FB2950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE1FB2CA0 00007f30593044af [GCFrame: 00007f2ae1fb2ca0]
00007F2AE1FB2D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae1fb2d70]
OS Thread Id: 0x1dc85
        Child SP               IP Call Site
00007F2AE17B1680 00007f305abc6360 [GCFrame: 00007f2ae17b1680]
00007F2AE17B1770 00007f305abc6360 [GCFrame: 00007f2ae17b1770]
00007F2AE17B17D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae17b17d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE17B1920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE17B1950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE17B1CA0 00007f30593044af [GCFrame: 00007f2ae17b1ca0]
00007F2AE17B1D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae17b1d70]
OS Thread Id: 0x1dc86
        Child SP               IP Call Site
00007F2AE0FB0680 00007f305abc6360 [GCFrame: 00007f2ae0fb0680]
00007F2AE0FB0770 00007f305abc6360 [GCFrame: 00007f2ae0fb0770]
00007F2AE0FB07D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae0fb07d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE0FB0920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE0FB0950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE0FB0CA0 00007f30593044af [GCFrame: 00007f2ae0fb0ca0]
00007F2AE0FB0D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae0fb0d70]
OS Thread Id: 0x1dc87
        Child SP               IP Call Site
00007F2AE07AF680 00007f305abc6360 [GCFrame: 00007f2ae07af680]
00007F2AE07AF770 00007f305abc6360 [GCFrame: 00007f2ae07af770]
00007F2AE07AF7D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae07af7d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE07AF920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE07AF950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE07AFCA0 00007f30593044af [GCFrame: 00007f2ae07afca0]
00007F2AE07AFD70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae07afd70]
OS Thread Id: 0x1dc88
        Child SP               IP Call Site
00007F2ADFFAE680 00007f305abc6360 [GCFrame: 00007f2adffae680]
00007F2ADFFAE770 00007f305abc6360 [GCFrame: 00007f2adffae770]
00007F2ADFFAE7D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2adffae7d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2ADFFAE920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2ADFFAE950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2ADFFAECA0 00007f30593044af [GCFrame: 00007f2adffaeca0]
00007F2ADFFAED70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2adffaed70]
...
...
```

<span data-ttu-id="42f47-145">觀察所有300多個執行緒的呼叫堆疊，會顯示大多數執行緒共用通用呼叫堆疊的模式：</span><span class="sxs-lookup"><span data-stu-id="42f47-145">Observing the callstacks for all 300+ threads shows a pattern where a majority of the threads share a common callstack:</span></span>

```console
OS Thread Id: 0x1dc88
        Child SP               IP Call Site
00007F2ADFFAE680 00007f305abc6360 [GCFrame: 00007f2adffae680]
00007F2ADFFAE770 00007f305abc6360 [GCFrame: 00007f2adffae770]
00007F2ADFFAE7D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2adffae7d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2ADFFAE920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2ADFFAE950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2ADFFAECA0 00007f30593044af [GCFrame: 00007f2adffaeca0]
00007F2ADFFAED70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2adffaed70]
```

<span data-ttu-id="42f47-146">呼叫堆疊似乎會顯示要求到達我們的鎖死方法中，而後者又會進行的呼叫 `Monitor.ReliableEnter` 。</span><span class="sxs-lookup"><span data-stu-id="42f47-146">The callstack seems to show that the request arrived in our deadlock method that in turn makes a call to `Monitor.ReliableEnter`.</span></span> <span data-ttu-id="42f47-147">這個方法表示執行緒正嘗試進入監視器鎖定。</span><span class="sxs-lookup"><span data-stu-id="42f47-147">This method indicates that the threads are trying to enter a monitor lock.</span></span> <span data-ttu-id="42f47-148">他們正在等候鎖定的可用性。</span><span class="sxs-lookup"><span data-stu-id="42f47-148">They're waiting on the availability of the lock.</span></span> <span data-ttu-id="42f47-149">這可能是由不同的執行緒鎖定。</span><span class="sxs-lookup"><span data-stu-id="42f47-149">It's likely locked by a different thread.</span></span>

<span data-ttu-id="42f47-150">接下來的步驟是找出哪個執行緒實際持有監視器鎖定。</span><span class="sxs-lookup"><span data-stu-id="42f47-150">The next step then is to find out which thread is actually holding the monitor lock.</span></span> <span data-ttu-id="42f47-151">因為監視器通常會將鎖定資訊儲存在同步處理區塊資料表中，所以我們可以使用 `syncblk` 命令來取得詳細資訊：</span><span class="sxs-lookup"><span data-stu-id="42f47-151">Since monitors typically store lock information in the sync block table, we can use the `syncblk` command to get more information:</span></span>

```console
> syncblk
Index         SyncBlock MonitorHeld Recursion Owning Thread Info          SyncBlock Owner
   43 00000246E51268B8          603         1 0000024B713F4E30 5634  28   00000249654b14c0 System.Object
   44 00000246E5126908            3         1 0000024B713F47E0 51d4  29   00000249654b14d8 System.Object
-----------------------------
Total           344
CCW             1
RCW             2
ComClassFactory 0
Free            0
```

<span data-ttu-id="42f47-152">這兩個有趣的資料行是**MonitorHeld**和**擁有線程資訊**。</span><span class="sxs-lookup"><span data-stu-id="42f47-152">The two interesting columns are **MonitorHeld** and **Owning Thread Info**.</span></span> <span data-ttu-id="42f47-153">**MonitorHeld**資料行會顯示執行緒是否取得監視器鎖定，以及等候中線程的數目。</span><span class="sxs-lookup"><span data-stu-id="42f47-153">The **MonitorHeld** column shows whether a monitor lock is acquired by a thread and the number of waiting threads.</span></span> <span data-ttu-id="42f47-154">[**擁有線程資訊**] 資料行會顯示目前擁有監視器鎖定的執行緒。</span><span class="sxs-lookup"><span data-stu-id="42f47-154">The **Owning Thread Info** column shows which thread currently owns the monitor lock.</span></span> <span data-ttu-id="42f47-155">執行緒資訊有三個不同的個子。</span><span class="sxs-lookup"><span data-stu-id="42f47-155">The thread info has three different subcolumns.</span></span> <span data-ttu-id="42f47-156">第二個 subcolumn 顯示作業系統執行緒識別碼。</span><span class="sxs-lookup"><span data-stu-id="42f47-156">The second subcolumn shows operating system thread ID.</span></span>

<span data-ttu-id="42f47-157">此時，我們知道兩個不同的執行緒（0x5634 和0x51d4）會保存監視器鎖定。</span><span class="sxs-lookup"><span data-stu-id="42f47-157">At this point, we know two different threads (0x5634 and 0x51d4) hold a monitor lock.</span></span> <span data-ttu-id="42f47-158">下一步是查看這些執行緒的作用。</span><span class="sxs-lookup"><span data-stu-id="42f47-158">The next step is to take a look at what those threads are doing.</span></span> <span data-ttu-id="42f47-159">我們需要檢查它們是否在無限期地停滯住。</span><span class="sxs-lookup"><span data-stu-id="42f47-159">We need to check if they're stuck indefinitely holding the lock.</span></span> <span data-ttu-id="42f47-160">讓我們使用 `setthread` 和 `clrstack` 命令來切換至每個執行緒，並顯示呼叫堆疊。</span><span class="sxs-lookup"><span data-stu-id="42f47-160">Let's use the `setthread` and `clrstack` commands to switch to each of the threads and display the callstacks.</span></span>

<span data-ttu-id="42f47-161">若要查看第一個執行緒，請執行 `setthread` 命令，並尋找0x5634 執行緒的索引（我們的索引是28）。</span><span class="sxs-lookup"><span data-stu-id="42f47-161">To look at the first thread, run the `setthread` command, and find the index of the 0x5634 thread (our index was 28).</span></span> <span data-ttu-id="42f47-162">鎖死函式正在等候取得鎖定，但它已經擁有鎖定。</span><span class="sxs-lookup"><span data-stu-id="42f47-162">The deadlock function is waiting to acquire a lock, but it already owns the lock.</span></span> <span data-ttu-id="42f47-163">它處於鎖死狀態，等待它已經保留的鎖定。</span><span class="sxs-lookup"><span data-stu-id="42f47-163">It's in deadlock waiting for the lock it already holds.</span></span>

```console
> setthread 28
> clrstack
OS Thread Id: 0x5634 (28)
        Child SP               IP Call Site
0000004E46AFEAA8 00007fff43a5cbc4 [GCFrame: 0000004e46afeaa8]
0000004E46AFEC18 00007fff43a5cbc4 [GCFrame: 0000004e46afec18]
0000004E46AFEC68 00007fff43a5cbc4 [HelperMethodFrame_1OBJ: 0000004e46afec68] System.Threading.Monitor.Enter(System.Object)
0000004E46AFEDC0 00007FFE5EAF9C80 testwebapi.Controllers.DiagScenarioController.DeadlockFunc() [C:\Users\dapine\Downloads\Diagnostic_scenarios_sample_debug_target\Controllers\DiagnosticScenarios.cs @ 58]
0000004E46AFEE30 00007FFE5EAF9B8C testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_0() [C:\Users\dapine\Downloads\Diagnostic_scenarios_sample_debug_target\Controllers\DiagnosticScenarios.cs @ 26]
0000004E46AFEE80 00007FFEBB251A5B System.Threading.ThreadHelper.ThreadStart_Context(System.Object) [/_/src/System.Private.CoreLib/src/System/Threading/Thread.CoreCLR.cs @ 44]
0000004E46AFEEB0 00007FFE5EAEEEEC System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/_/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
0000004E46AFEF20 00007FFEBB234EAB System.Threading.ThreadHelper.ThreadStart() [/_/src/System.Private.CoreLib/src/System/Threading/Thread.CoreCLR.cs @ 93]
0000004E46AFF138 00007ffebdcc6b63 [GCFrame: 0000004e46aff138]
0000004E46AFF3A0 00007ffebdcc6b63 [DebuggerU2MCatchHandlerFrame: 0000004e46aff3a0]
```

<span data-ttu-id="42f47-164">第二個執行緒是相似的。</span><span class="sxs-lookup"><span data-stu-id="42f47-164">The second thread is similar.</span></span> <span data-ttu-id="42f47-165">它也會嘗試取得其已擁有的鎖定。</span><span class="sxs-lookup"><span data-stu-id="42f47-165">It's also trying to acquire a lock that it already owns.</span></span> <span data-ttu-id="42f47-166">所有等待的剩餘300個以上執行緒，很可能也會等待造成鎖死的其中一個鎖定。</span><span class="sxs-lookup"><span data-stu-id="42f47-166">The remaining 300+ threads that are all waiting are most likely also waiting on one of the locks that caused the deadlock.</span></span>

## <a name="see-also"></a><span data-ttu-id="42f47-167">另請參閱</span><span class="sxs-lookup"><span data-stu-id="42f47-167">See also</span></span>

- <span data-ttu-id="42f47-168">[dotnet-列出進程的追蹤](dotnet-trace.md)</span><span class="sxs-lookup"><span data-stu-id="42f47-168">[dotnet-trace](dotnet-trace.md) to list processes</span></span>
- <span data-ttu-id="42f47-169">[dotnet-](dotnet-counters.md)檢查 managed 記憶體使用量的計數器</span><span class="sxs-lookup"><span data-stu-id="42f47-169">[dotnet-counters](dotnet-counters.md) to check managed memory usage</span></span>
- <span data-ttu-id="42f47-170">[dotnet-](dotnet-dump.md)用來收集和分析傾印檔案的傾印</span><span class="sxs-lookup"><span data-stu-id="42f47-170">[dotnet-dump](dotnet-dump.md) to collect and analyze a dump file</span></span>
- [<span data-ttu-id="42f47-171">dotnet/診斷</span><span class="sxs-lookup"><span data-stu-id="42f47-171">dotnet/diagnostics</span></span>](https://github.com/dotnet/diagnostics/tree/master/documentation/tutorial)

## <a name="next-steps"></a><span data-ttu-id="42f47-172">後續步驟</span><span class="sxs-lookup"><span data-stu-id="42f47-172">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="42f47-173">.NET Core 中提供哪些診斷工具</span><span class="sxs-lookup"><span data-stu-id="42f47-173">What diagnostic tools are available in .NET Core</span></span>](index.md)