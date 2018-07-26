---
title: Начало работы с ASP.NET Core
author: rick-anderson
description: Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 7ab9f303d74786c4ac76f002d0f2c66371e78cb8
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228586"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="537ac-103">Начало работы с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="537ac-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="537ac-104">Установите [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="537ac-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="537ac-105">Создайте проект ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="537ac-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="537ac-106">Откройте окно командной оболочки и введите следующую команду.</span><span class="sxs-lookup"><span data-stu-id="537ac-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="537ac-107">Установите доверие к сертификату разработки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="537ac-107">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="537ac-108">Windows</span><span class="sxs-lookup"><span data-stu-id="537ac-108">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="537ac-109">Приведенная выше команда отображает следующее диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="537ac-109">The preceding command displays the following dialog:</span></span>

   ![Диалоговое окно "Предупреждение о безопасности"](_static/cert.png)

   <span data-ttu-id="537ac-111">Выберите **Да**, если согласны доверять сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="537ac-111">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="537ac-112">macOS</span><span class="sxs-lookup"><span data-stu-id="537ac-112">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="537ac-113">Приведенная выше команда отображает следующее сообщение.</span><span class="sxs-lookup"><span data-stu-id="537ac-113">The preceding command displays the following message:</span></span>

   <span data-ttu-id="537ac-114">*Запрошено доверие к сертификату разработки HTTPS. Если сертификат не является доверенным, выполните следующую команду:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *Эта команда может запросить пароль для установки сертификата в системной цепочке ключей.    Пароль:* </span><span class="sxs-lookup"><span data-stu-id="537ac-114">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *This command might prompt you for your password to install the certificate on the system keychain.    Password:*</span></span>

   <span data-ttu-id="537ac-115">Введите пароль, если согласны доверять сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="537ac-115">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="537ac-116">Linux</span><span class="sxs-lookup"><span data-stu-id="537ac-116">Linux</span></span>](#tab/linux)

   <a name="see-the-documentation-for-your-linux-distribution-on-how-to-trust-the-https-development-certificate"></a><span data-ttu-id="537ac-117">Просмотрите документацию по дистрибутиву Linux, чтобы узнать, как установить отношение доверия к сертификату разработки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="537ac-117">See the documentation for your Linux distribution on how to trust the HTTPS development certificate</span></span>
---

4. <span data-ttu-id="537ac-118">Запустите приложение:</span><span class="sxs-lookup"><span data-stu-id="537ac-118">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="537ac-119">Перейдите по адресу [http://localhost:5001](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="537ac-119">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="537ac-120">Щелкните **Принять**, чтобы принять политику конфиденциальности и использования файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="537ac-120">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="537ac-121">Это приложение не хранит персональные данные.</span><span class="sxs-lookup"><span data-stu-id="537ac-121">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="537ac-122">Откройте *Pages/About.cshtml* и измените страницу, добавив выделенное исправление.</span><span class="sxs-lookup"><span data-stu-id="537ac-122">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="537ac-123">Перейдите к [http://localhost:5001/About](http://localhost:5001/About) и проверьте, отобразились ли изменения.</span><span class="sxs-lookup"><span data-stu-id="537ac-123">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="537ac-124">Установите [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="537ac-124">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="537ac-125">Создайте новый проект ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="537ac-125">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="537ac-126">Откройте командную оболочку.</span><span class="sxs-lookup"><span data-stu-id="537ac-126">Open a command shell.</span></span> <span data-ttu-id="537ac-127">Введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="537ac-127">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="537ac-128">Запустите приложение с помощью следующих команд:</span><span class="sxs-lookup"><span data-stu-id="537ac-128">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="537ac-129">Перейдите по адресу [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="537ac-129">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="537ac-130">Откройте файл *Pages/About.cshtml* и измените страницу, чтобы на ней отображалось сообщение "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="537ac-130">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="537ac-131">Время на сервере — @DateTime.Now" :</span><span class="sxs-lookup"><span data-stu-id="537ac-131">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="537ac-132">Перейдите к [http://localhost:5000/About](http://localhost:5000/About) и проверьте изменения.</span><span class="sxs-lookup"><span data-stu-id="537ac-132">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="537ac-133">Установите **установщик пакета SDK** версии 1.0.4 для .NET Core со [страницы "Все загрузки .NET Core"](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="537ac-133">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="537ac-134">Создайте папку для нового проекта ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="537ac-134">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="537ac-135">Откройте командную оболочку.</span><span class="sxs-lookup"><span data-stu-id="537ac-135">Open a command shell.</span></span> <span data-ttu-id="537ac-136">Введите следующие команды.</span><span class="sxs-lookup"><span data-stu-id="537ac-136">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="537ac-137">Если на компьютере установлена более поздняя версия пакета SDK, создайте файл *global.json*, чтобы выбрать пакет SDK версии 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="537ac-137">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="537ac-138">Создайте новый проект ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="537ac-138">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="537ac-139">Восстановите пакеты.</span><span class="sxs-lookup"><span data-stu-id="537ac-139">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="537ac-140">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="537ac-140">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="537ac-141">При необходимости команда [dotnet run](/dotnet/core/tools/dotnet-run) сначала выполняет сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="537ac-141">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="537ac-142">Перейдите по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="537ac-142">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
