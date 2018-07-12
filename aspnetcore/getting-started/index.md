---
title: Начало работы с ASP.NET Core
author: rick-anderson
description: Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 22e9c982921cc03d89506e18ff99bf481027dda6
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38216217"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="d90f8-103">Начало работы с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d90f8-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="d90f8-104">Установите [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="d90f8-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="d90f8-105">Создайте проект ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d90f8-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="d90f8-106">Откройте окно командной оболочки и введите следующую команду.</span><span class="sxs-lookup"><span data-stu-id="d90f8-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    <span data-ttu-id="d90f8-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span><span class="sxs-lookup"><span data-stu-id="d90f8-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span></span>

3. <span data-ttu-id="d90f8-108">Установите доверие к сертификату разработки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d90f8-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="d90f8-109">Windows</span><span class="sxs-lookup"><span data-stu-id="d90f8-109">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="d90f8-110">macOS</span><span class="sxs-lookup"><span data-stu-id="d90f8-110">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="d90f8-111">Linux</span><span class="sxs-lookup"><span data-stu-id="d90f8-111">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="d90f8-112">Запустите приложение:</span><span class="sxs-lookup"><span data-stu-id="d90f8-112">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="d90f8-113">Перейдите по адресу [http://localhost:5001](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="d90f8-113">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="d90f8-114">Щелкните **Принять**, чтобы принять политику конфиденциальности и использования файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="d90f8-114">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="d90f8-115">Это приложение не хранит персональные данные.</span><span class="sxs-lookup"><span data-stu-id="d90f8-115">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="d90f8-116">Откройте *Pages/About.cshtml* и измените страницу, добавив выделенное исправление.</span><span class="sxs-lookup"><span data-stu-id="d90f8-116">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="d90f8-117">Перейдите к [http://localhost:5001/About](http://localhost:5001/About) и проверьте, отобразились ли изменения.</span><span class="sxs-lookup"><span data-stu-id="d90f8-117">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="d90f8-118">Установите [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="d90f8-118">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="d90f8-119">Создайте новый проект ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d90f8-119">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="d90f8-120">Откройте командную оболочку.</span><span class="sxs-lookup"><span data-stu-id="d90f8-120">Open a command shell.</span></span> <span data-ttu-id="d90f8-121">Введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d90f8-121">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="d90f8-122">Запустите приложение с помощью следующих команд:</span><span class="sxs-lookup"><span data-stu-id="d90f8-122">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="d90f8-123">Перейдите по адресу [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="d90f8-123">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="d90f8-124">Откройте файл *Pages/About.cshtml* и измените страницу, чтобы на ней отображалось сообщение "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="d90f8-124">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="d90f8-125">Время на сервере — @DateTime.Now" :</span><span class="sxs-lookup"><span data-stu-id="d90f8-125">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="d90f8-126">Перейдите к [http://localhost:5000/About](http://localhost:5000/About) и проверьте изменения.</span><span class="sxs-lookup"><span data-stu-id="d90f8-126">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="d90f8-127">Установите **установщик пакета SDK** версии 1.0.4 для .NET Core со [страницы "Все загрузки .NET Core"](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="d90f8-127">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="d90f8-128">Создайте папку для нового проекта ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d90f8-128">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="d90f8-129">Откройте командную оболочку.</span><span class="sxs-lookup"><span data-stu-id="d90f8-129">Open a command shell.</span></span> <span data-ttu-id="d90f8-130">Введите следующие команды.</span><span class="sxs-lookup"><span data-stu-id="d90f8-130">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="d90f8-131">Если на компьютере установлена более поздняя версия пакета SDK, создайте файл *global.json*, чтобы выбрать пакет SDK версии 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="d90f8-131">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="d90f8-132">Создайте новый проект ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d90f8-132">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="d90f8-133">Восстановите пакеты.</span><span class="sxs-lookup"><span data-stu-id="d90f8-133">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="d90f8-134">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="d90f8-134">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="d90f8-135">При необходимости команда [dotnet run](/dotnet/core/tools/dotnet-run) сначала выполняет сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="d90f8-135">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="d90f8-136">Перейдите по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d90f8-136">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
