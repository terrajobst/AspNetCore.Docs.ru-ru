---
title: Начало работы с ASP.NET Core
author: rick-anderson
description: Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: a6a5023594aec01370143e7d1f35fb45c109122a
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860944"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="c3c13-103">Начало работы с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c3c13-103">Get started with ASP.NET Core</span></span>

<span data-ttu-id="c3c13-104">В этом документе приводятся инструкции по созданию и запуску приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3c13-104">This document provides steps for creating and running an ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="c3c13-105">Установите [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="c3c13-105">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="c3c13-106">Создайте проект ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3c13-106">Create an ASP.NET Core project.</span></span> <span data-ttu-id="c3c13-107">Откройте окно командной оболочки и введите следующую команду.</span><span class="sxs-lookup"><span data-stu-id="c3c13-107">Open a command shell and enter the following command:</span></span>

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

3. <span data-ttu-id="c3c13-108">Установите доверие к сертификату разработки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c3c13-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="c3c13-109">Windows</span><span class="sxs-lookup"><span data-stu-id="c3c13-109">Windows</span></span>](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="c3c13-110">Приведенная выше команда отображает следующее диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="c3c13-110">The preceding command displays the following dialog:</span></span>

  ![Диалоговое окно "Предупреждение о безопасности"](_static/cert.png)

  <span data-ttu-id="c3c13-112">Выберите **Да**, если согласны доверять сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="c3c13-112">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="c3c13-113">macOS</span><span class="sxs-lookup"><span data-stu-id="c3c13-113">macOS</span></span>](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="c3c13-114">Приведенная выше команда отображает следующее сообщение.</span><span class="sxs-lookup"><span data-stu-id="c3c13-114">The preceding command displays the following message:</span></span>

  <span data-ttu-id="c3c13-115">*Запрошено доверие к сертификату разработки HTTPS. Если сертификат не является доверенным, выполните следующую команду:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="c3c13-115">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
  <span data-ttu-id="c3c13-116">\* Эта команда может запросить пароль для установки сертификата в системной цепочке ключей.</span><span class="sxs-lookup"><span data-stu-id="c3c13-116">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>
  
  <span data-ttu-id="c3c13-117">Пароль: \*</span><span class="sxs-lookup"><span data-stu-id="c3c13-117">Password:\*</span></span>

  <span data-ttu-id="c3c13-118">Введите пароль, если согласны доверять сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="c3c13-118">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="c3c13-119">Linux</span><span class="sxs-lookup"><span data-stu-id="c3c13-119">Linux</span></span>](#tab/linux)

  <span data-ttu-id="c3c13-120">Просмотрите документацию по дистрибутиву Linux, чтобы узнать, как установить отношение доверия к сертификату разработки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c3c13-120">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>
   
---

4. <span data-ttu-id="c3c13-121">Запустите приложение:</span><span class="sxs-lookup"><span data-stu-id="c3c13-121">Run the app:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

5. <span data-ttu-id="c3c13-122">Перейдите по адресу [http://localhost:5001](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="c3c13-122">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="c3c13-123">Щелкните **Принять**, чтобы принять политику конфиденциальности и использования файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="c3c13-123">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="c3c13-124">Это приложение не хранит персональные данные.</span><span class="sxs-lookup"><span data-stu-id="c3c13-124">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="c3c13-125">Откройте *Pages/About.cshtml* и измените страницу, добавив выделенное исправление.</span><span class="sxs-lookup"><span data-stu-id="c3c13-125">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="c3c13-126">Перейдите к [http://localhost:5001/About](http://localhost:5001/About) и проверьте, отобразились ли изменения.</span><span class="sxs-lookup"><span data-stu-id="c3c13-126">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="c3c13-127">Установите [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="c3c13-127">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="c3c13-128">Создайте новый проект ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3c13-128">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="c3c13-129">Откройте командную оболочку.</span><span class="sxs-lookup"><span data-stu-id="c3c13-129">Open a command shell.</span></span> <span data-ttu-id="c3c13-130">Введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="c3c13-130">Enter the following command:</span></span>

   ```console
   dotnet new razor -o aspnetcoreapp
   ```

3. <span data-ttu-id="c3c13-131">Запустите приложение с помощью следующих команд:</span><span class="sxs-lookup"><span data-stu-id="c3c13-131">Run the app with the following commands:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

4. <span data-ttu-id="c3c13-132">Перейдите по адресу [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="c3c13-132">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="c3c13-133">Откройте файл *Pages/About.cshtml* и измените страницу, чтобы на ней отображалось сообщение "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="c3c13-133">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="c3c13-134">Время на сервере — @DateTime.Now" :</span><span class="sxs-lookup"><span data-stu-id="c3c13-134">The time on the server is @DateTime.Now":</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="c3c13-135">Перейдите к [http://localhost:5000/About](http://localhost:5000/About) и проверьте изменения.</span><span class="sxs-lookup"><span data-stu-id="c3c13-135">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="c3c13-136">Установите **установщик пакета SDK** версии 1.0.4 для .NET Core со [страницы "Все загрузки .NET Core"](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="c3c13-136">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="c3c13-137">Создайте папку для нового проекта ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3c13-137">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="c3c13-138">Откройте командную оболочку.</span><span class="sxs-lookup"><span data-stu-id="c3c13-138">Open a command shell.</span></span> <span data-ttu-id="c3c13-139">Введите следующие команды.</span><span class="sxs-lookup"><span data-stu-id="c3c13-139">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="c3c13-140">Если на компьютере установлена более поздняя версия пакета SDK, создайте файл *global.json*, чтобы выбрать пакет SDK версии 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="c3c13-140">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="c3c13-141">Создайте новый проект ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3c13-141">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="c3c13-142">Восстановите пакеты.</span><span class="sxs-lookup"><span data-stu-id="c3c13-142">Restore the packages.</span></span>

   ```console
   dotnet restore
   ```

6. <span data-ttu-id="c3c13-143">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="c3c13-143">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="c3c13-144">При необходимости команда [dotnet run](/dotnet/core/tools/dotnet-run) сначала выполняет сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="c3c13-144">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="c3c13-145">Перейдите по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c3c13-145">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end
