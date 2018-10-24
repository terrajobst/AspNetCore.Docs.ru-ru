---
title: Начало работы с ASP.NET Core
author: rick-anderson
description: Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 4a5a0cc5a5dab2171ab8ef43818185a4ee91af0e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912570"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="12563-103">Руководство. Начало работы с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12563-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="12563-104">В этом руководстве показано, как использовать интерфейс командной строки .NET Core для создания веб-приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12563-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span> <span data-ttu-id="12563-105">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="12563-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="12563-106">создавать проект веб-приложения;</span><span class="sxs-lookup"><span data-stu-id="12563-106">Create a web app project.</span></span>
> * <span data-ttu-id="12563-107">включать локальный HTTPS;</span><span class="sxs-lookup"><span data-stu-id="12563-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="12563-108">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="12563-108">Run the app.</span></span>
> * <span data-ttu-id="12563-109">редактировать страницу Razor.</span><span class="sxs-lookup"><span data-stu-id="12563-109">Edit a Razor page.</span></span>

<span data-ttu-id="12563-110">В итоге вы получите рабочее веб-приложение на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="12563-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Домашняя страница веб-приложения](_static/home-page.png)


## <a name="prerequisites"></a><span data-ttu-id="12563-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="12563-112">Prerequisites</span></span>

* <span data-ttu-id="12563-113">Установите [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="12563-113">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

## <a name="create-a-web-app-project"></a><span data-ttu-id="12563-114">Создание проекта веб-приложения</span><span class="sxs-lookup"><span data-stu-id="12563-114">Create a web app project</span></span>

* <span data-ttu-id="12563-115">Откройте окно командной оболочки и введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="12563-115">Open a command shell, and enter the following command:</span></span>

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

## <a name="enable-local-https"></a><span data-ttu-id="12563-116">Включение локального HTTPS</span><span class="sxs-lookup"><span data-stu-id="12563-116">Enable local HTTPS</span></span>

* <span data-ttu-id="12563-117">Установите доверие к сертификату разработки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="12563-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="12563-118">Windows</span><span class="sxs-lookup"><span data-stu-id="12563-118">Windows</span></span>](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="12563-119">Приведенная выше команда отображает следующее диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="12563-119">The preceding command displays the following dialog:</span></span>

  ![Диалоговое окно "Предупреждение о безопасности"](_static/cert.png)

  <span data-ttu-id="12563-121">Выберите **Да**, если согласны доверять сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="12563-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="12563-122">macOS</span><span class="sxs-lookup"><span data-stu-id="12563-122">macOS</span></span>](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="12563-123">Приведенная выше команда отображает следующее сообщение.</span><span class="sxs-lookup"><span data-stu-id="12563-123">The preceding command displays the following message:</span></span>

  <span data-ttu-id="12563-124">*Запрошено доверие к сертификату разработки HTTPS. Если сертификат не является доверенным, выполните следующую команду:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="12563-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
  <span data-ttu-id="12563-125">\* Эта команда может запросить пароль для установки сертификата в системной цепочке ключей.</span><span class="sxs-lookup"><span data-stu-id="12563-125">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>
  
  <span data-ttu-id="12563-126">Пароль: \*</span><span class="sxs-lookup"><span data-stu-id="12563-126">Password:\*</span></span>

  <span data-ttu-id="12563-127">Введите пароль, если согласны доверять сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="12563-127">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="12563-128">Linux</span><span class="sxs-lookup"><span data-stu-id="12563-128">Linux</span></span>](#tab/linux)

  <span data-ttu-id="12563-129">Просмотрите документацию по дистрибутиву Linux, чтобы узнать, как установить отношение доверия к сертификату разработки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="12563-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>
   
---

## <a name="run-the-app"></a><span data-ttu-id="12563-130">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="12563-130">Run the app</span></span>

* <span data-ttu-id="12563-131">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="12563-131">Run the following commands:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

* <span data-ttu-id="12563-132">Перейдите по адресу [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="12563-132">Browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="12563-133">Щелкните **Принять**, чтобы принять политику конфиденциальности и использования файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="12563-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="12563-134">Это приложение не хранит персональные данные.</span><span class="sxs-lookup"><span data-stu-id="12563-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="12563-135">Редактирование страницы Razor</span><span class="sxs-lookup"><span data-stu-id="12563-135">Edit a Razor page</span></span>

* <span data-ttu-id="12563-136">Откройте *Pages/About.cshtml* и измените страницу, добавив выделенное исправление.</span><span class="sxs-lookup"><span data-stu-id="12563-136">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

* <span data-ttu-id="12563-137">Перейдите к [https://localhost:5001/About](https://localhost:5001/About) и проверьте, отобразились ли изменения.</span><span class="sxs-lookup"><span data-stu-id="12563-137">Browse to [https://localhost:5001/About](https://localhost:5001/About) and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="12563-138">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="12563-138">Next steps</span></span>

<span data-ttu-id="12563-139">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="12563-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="12563-140">создавать проект веб-приложения;</span><span class="sxs-lookup"><span data-stu-id="12563-140">Create a web app project.</span></span>
> * <span data-ttu-id="12563-141">включать локальный HTTPS;</span><span class="sxs-lookup"><span data-stu-id="12563-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="12563-142">Запустите проект.</span><span class="sxs-lookup"><span data-stu-id="12563-142">Run the project.</span></span>
> * <span data-ttu-id="12563-143">вносить изменения.</span><span class="sxs-lookup"><span data-stu-id="12563-143">Make a change.</span></span>

<span data-ttu-id="12563-144">Дополнительные сведения об ASP.NET Core см. во введении:</span><span class="sxs-lookup"><span data-stu-id="12563-144">To learn more about ASP.NET Core, see the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index>
