---
title: 使用 Visual Studio Code 發佈 .NET Core Hello World 應用程式
description: 發行會建立一組執行 .NET Core 應用程式所需的檔案。
ms.date: 05/28/2020
ms.openlocfilehash: b49b12bf41e3ea7be8dbc459eb7d9b1fbef25790
ms.sourcegitcommit: a241301495a84cc8c64fe972330d16edd619868b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2020
ms.locfileid: "84246679"
---
# <a name="tutorial-publish-a-net-core-console-application-with-visual-studio-code"></a><span data-ttu-id="3c9f4-103">教學課程：使用 Visual Studio Code 發行 .NET Core 主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="3c9f4-103">Tutorial: Publish a .NET Core console application with Visual Studio Code</span></span>

<span data-ttu-id="3c9f4-104">本教學課程說明如何發佈主控台應用程式，讓其他使用者可以執行它。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-104">This tutorial shows how to publish a console app so that other users can run it.</span></span> <span data-ttu-id="3c9f4-105">發行會建立一組執行應用程式所需的檔案。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-105">Publishing creates the set of files that are needed to run an application.</span></span> <span data-ttu-id="3c9f4-106">若要部署檔案，請將檔案複製到目的電腦。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-106">To deploy the files, copy them to the target machine.</span></span>

<span data-ttu-id="3c9f4-107">.NET Core CLI 是用來發佈應用程式，因此如果您想要的話，也可以依照此教學課程，使用 Visual Studio Code 以外的程式碼編輯器。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-107">The .NET Core CLI is used to publish the app, so you can follow this tutorial with a code editor other than Visual Studio Code if you prefer.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c9f4-108">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="3c9f4-108">Prerequisites</span></span>

- <span data-ttu-id="3c9f4-109">本教學課程適用于您在 Visual Studio Code 中建立[.Net Core 主控台應用程式](with-visual-studio-code.md)中建立的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-109">This tutorial works with the console app that you create in [Create a .NET Core console application in Visual Studio Code](with-visual-studio-code.md).</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="3c9f4-110">發佈應用程式</span><span class="sxs-lookup"><span data-stu-id="3c9f4-110">Publish the app</span></span>

1. <span data-ttu-id="3c9f4-111">開啟 Visual Studio Code。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-111">Open Visual Studio Code.</span></span>

1. <span data-ttu-id="3c9f4-112">在 Visual Studio Code 中，開啟您在[建立 .Net Core 主控台應用程式](with-visual-studio-code.md)中建立的*HelloWorld*專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-112">Open the *HelloWorld* project folder that you created in [Create a .NET Core console application in Visual Studio Code](with-visual-studio-code.md).</span></span>

1. <span data-ttu-id="3c9f4-113">從主功能表選擇 [**視圖**] [  >  **終端**機]。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-113">Choose **View** > **Terminal** from the main menu.</span></span>

   <span data-ttu-id="3c9f4-114">終端機會在*HelloWorld*資料夾中開啟。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-114">The terminal opens in the *HelloWorld* folder.</span></span>

1. <span data-ttu-id="3c9f4-115">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="3c9f4-115">Run the following command:</span></span>

   ```dotnetcli
   dotnet publish --configuration Release
   ```

   <span data-ttu-id="3c9f4-116">預設的組建設定為*Debug*，因此此命令會指定*發行*組建設定。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-116">The default build configuration is *Debug*, so this command specifies the *Release* build configuration.</span></span> <span data-ttu-id="3c9f4-117">發行組建設定的輸出具有最小的符號偵錯工具資訊，並已完全優化。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-117">The output from the Release build configuration has minimal symbolic debug information and is fully optimized.</span></span>

   <span data-ttu-id="3c9f4-118">命令輸出會類似下列範例：</span><span class="sxs-lookup"><span data-stu-id="3c9f4-118">The command output is similar to the following example:</span></span>

   ```
   Microsoft (R) Build Engine version 16.6.0+5ff7b0c9e for .NET Core
   Copyright (C) Microsoft Corporation. All rights reserved.

   Determining projects to restore...
   All projects are up-to-date for restore.
   HelloWorld -> C:\Projects\HelloWorld\bin\Release\netcoreapp3.1\HelloWorld.dll
   HelloWorld -> C:\Projects\HelloWorld\bin\Release\netcoreapp3.1\publish\
   ```

## <a name="inspect-the-files"></a><span data-ttu-id="3c9f4-119">檢查檔案</span><span class="sxs-lookup"><span data-stu-id="3c9f4-119">Inspect the files</span></span>

<span data-ttu-id="3c9f4-120">根據預設，發佈程式會建立與 framework 相依的部署，這是一種部署類型，其中已發佈的應用程式會在已安裝 .NET Core 執行時間的電腦上執行。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-120">By default, the publishing process creates a framework-dependent deployment, which is a type of deployment where the published application runs on a machine that has the .NET Core runtime installed.</span></span> <span data-ttu-id="3c9f4-121">若要執行已發佈的應用程式，您可以使用可執行檔，或 `dotnet HelloWorld.dll` 從命令提示字元執行命令。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-121">To run the published app you can use the executable file or run the `dotnet HelloWorld.dll` command from a command prompt.</span></span>

<span data-ttu-id="3c9f4-122">在下列步驟中，您將查看發行程式所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-122">In the following steps, you'll look at the files created by the publish process.</span></span>

1. <span data-ttu-id="3c9f4-123">在左側導覽列中選取 [ **Explorer** ]。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-123">Select the **Explorer** in the left navigation bar.</span></span>

1. <span data-ttu-id="3c9f4-124">展開 [ *bin/Release/netcoreapp 3.1/* 發行]。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-124">Expand *bin/Release/netcoreapp3.1/publish*.</span></span>

   :::image type="content" source="media/publishing-with-visual-studio-code/published-files-output.png" alt-text="顯示已發行檔案的 Explorer":::

   <span data-ttu-id="3c9f4-126">如圖所示，已發行的輸出會包含下列檔案：</span><span class="sxs-lookup"><span data-stu-id="3c9f4-126">As the image shows, the published output includes the following files:</span></span>

   * <span data-ttu-id="3c9f4-127">*HelloWorld.deps.json*</span><span class="sxs-lookup"><span data-stu-id="3c9f4-127">*HelloWorld.deps.json*</span></span>

      <span data-ttu-id="3c9f4-128">這是應用程式的執行時間相依性檔案。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-128">This is the application's runtime dependencies file.</span></span> <span data-ttu-id="3c9f4-129">它會定義執行應用程式所需的 .NET Core 元件和程式庫（包括包含您應用程式的動態連結程式庫）。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-129">It defines the .NET Core components and the libraries (including the dynamic link library that contains your application) needed to run the app.</span></span> <span data-ttu-id="3c9f4-130">如需詳細資訊，請參閱[執行時間設定檔](https://github.com/dotnet/cli/blob/85ca206d84633d658d7363894c4ea9d59e515c1a/Documentation/specs/runtime-configuration-file.md)。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-130">For more information, see [Runtime configuration files](https://github.com/dotnet/cli/blob/85ca206d84633d658d7363894c4ea9d59e515c1a/Documentation/specs/runtime-configuration-file.md).</span></span>

   * <span data-ttu-id="3c9f4-131">*HelloWorld.dll*</span><span class="sxs-lookup"><span data-stu-id="3c9f4-131">*HelloWorld.dll*</span></span>

      <span data-ttu-id="3c9f4-132">這是與[framework 相依](../deploying/deploy-with-cli.md#framework-dependent-deployment)的應用程式部署版本。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-132">This is the [framework-dependent deployment](../deploying/deploy-with-cli.md#framework-dependent-deployment) version of the application.</span></span> <span data-ttu-id="3c9f4-133">若要執行此動態連結程式庫，請 `dotnet HelloWorld.dll` 在命令提示字元中輸入。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-133">To execute this dynamic link library, enter `dotnet HelloWorld.dll` at a command prompt.</span></span> <span data-ttu-id="3c9f4-134">這種執行應用程式的方法可在已安裝 .NET Core 執行時間的任何平臺上運作。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-134">This method of running the app works on any platform that has the .NET Core runtime installed.</span></span>

   * <span data-ttu-id="3c9f4-135">*HelloWorld* （在 Linux 上為*HelloWorld* ，而不是在 macOS 上建立）。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-135">*HelloWorld.exe* (*HelloWorld* on Linux, not created on macOS.)</span></span>

      <span data-ttu-id="3c9f4-136">這是與[framework 相依的應用程式可執行檔](../deploying/deploy-with-cli.md#framework-dependent-executable)版本。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-136">This is the [framework-dependent executable](../deploying/deploy-with-cli.md#framework-dependent-executable) version of the application.</span></span> <span data-ttu-id="3c9f4-137">檔案是作業系統特定的檔案。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-137">The file is operating-system-specific.</span></span>

   * <span data-ttu-id="3c9f4-138">*HelloWorld.pdb* (對於部署為選用)</span><span class="sxs-lookup"><span data-stu-id="3c9f4-138">*HelloWorld.pdb* (optional for deployment)</span></span>

      <span data-ttu-id="3c9f4-139">這是 debug 符號檔案。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-139">This is the debug symbols file.</span></span> <span data-ttu-id="3c9f4-140">此檔案不需要隨您的應用程式部署，但當您需要對應用程式發行的版本進行偵錯，則應該儲存它。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-140">You aren't required to deploy this file along with your application, although you should save it in the event that you need to debug the published version of your application.</span></span>

   * <span data-ttu-id="3c9f4-141">*HelloWorld.runtimeconfig.json*</span><span class="sxs-lookup"><span data-stu-id="3c9f4-141">*HelloWorld.runtimeconfig.json*</span></span>

      <span data-ttu-id="3c9f4-142">這是應用程式的執行時間設定檔。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-142">This is the application's run-time configuration file.</span></span> <span data-ttu-id="3c9f4-143">它會識別建置您應用程式以在其上執行的 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-143">It identifies the version of .NET Core that your application was built to run on.</span></span> <span data-ttu-id="3c9f4-144">您也可以在其中新增設定選項。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-144">You can also add configuration options to it.</span></span> <span data-ttu-id="3c9f4-145">如需詳細資訊，請參閱[.Net Core 執行時間設定](../run-time-config/index.md#runtimeconfigjson)。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-145">For more information, see [.NET Core run-time configuration settings](../run-time-config/index.md#runtimeconfigjson).</span></span>

## <a name="run-the-published-app"></a><span data-ttu-id="3c9f4-146">執行已發佈的應用程式</span><span class="sxs-lookup"><span data-stu-id="3c9f4-146">Run the published app</span></span>

1. <span data-ttu-id="3c9f4-147">在**Explorer**中，以滑鼠右鍵按一下 [*發行*] 資料夾（或<kbd>Ctrl</kbd>+ 按一下 [macOS]），然後選取 [**在終端機中開啟**]。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-147">In **Explorer**, right-click the *publish* folder (or <kbd>Ctrl</kbd>+click on macOS), and select **Open in Terminal**.</span></span>

   :::image type="content" source="media/publishing-with-visual-studio-code/open-in-terminal.png" alt-text="顯示在終端機中開啟的內容功能表":::

1. <span data-ttu-id="3c9f4-149">在 Windows 或 Linux 上，使用可執行檔來執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-149">On Windows or Linux, run the app by using the executable.</span></span>

   1. <span data-ttu-id="3c9f4-150">在 Windows 上輸入， `.\HelloWorld.exe` 然後按<kbd>enter</kbd>鍵。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-150">On Windows, enter `.\HelloWorld.exe` and press <kbd>Enter</kbd>.</span></span>

   1. <span data-ttu-id="3c9f4-151">在 Linux 上，輸入 `./HelloWorld` ，然後按<kbd>enter</kbd>。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-151">On Linux, enter `./HelloWorld` and press <kbd>Enter</kbd>.</span></span>

   1. <span data-ttu-id="3c9f4-152">輸入名稱以回應提示，然後按任意鍵結束。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-152">Enter a name in response to the prompt, and press any key to exit.</span></span>

1. <span data-ttu-id="3c9f4-153">在任何平臺上，使用命令執行應用程式 [`dotnet`](../tools/dotnet.md) ：</span><span class="sxs-lookup"><span data-stu-id="3c9f4-153">On any platform, run the app by using the  [`dotnet`](../tools/dotnet.md) command:</span></span>

   1. <span data-ttu-id="3c9f4-154">輸入 `dotnet HelloWorld.dll` ，然後按<kbd>enter</kbd>鍵。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-154">Enter `dotnet HelloWorld.dll` and press <kbd>Enter</kbd>.</span></span>

   1. <span data-ttu-id="3c9f4-155">輸入名稱以回應提示，然後按任意鍵結束。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-155">Enter a name in response to the prompt, and press any key to exit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3c9f4-156">其他資源</span><span class="sxs-lookup"><span data-stu-id="3c9f4-156">Additional resources</span></span>

- [<span data-ttu-id="3c9f4-157">.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="3c9f4-157">.NET Core application deployment</span></span>](../deploying/index.md)

## <a name="next-steps"></a><span data-ttu-id="3c9f4-158">後續步驟</span><span class="sxs-lookup"><span data-stu-id="3c9f4-158">Next steps</span></span>

<span data-ttu-id="3c9f4-159">在本教學課程中，您已發佈主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-159">In this tutorial, you published a console app.</span></span> <span data-ttu-id="3c9f4-160">若要瞭解如何建立程式庫，請參閱[使用 .NET Core CLI 開發程式庫](libraries.md)。</span><span class="sxs-lookup"><span data-stu-id="3c9f4-160">To learn how to build libraries, see [Develop libraries with the .NET Core CLI](libraries.md).</span></span>

<!--In the next tutorial, you create a class library.

> [!div class="nextstepaction"]
> [Create a .NET Standard library in Visual Studio](library-with-visual-studio.md)
-->