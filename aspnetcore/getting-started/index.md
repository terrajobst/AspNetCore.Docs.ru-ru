---
title: Начало работы с ASP.NET Core
author: rick-anderson
description: Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World.
ms.author: riande
ms.custom: mvc
ms.date: 01/07/2020
uid: getting-started
ms.openlocfilehash: 4f7e67e1e422afe3f7e2970e0c40380f065390ac
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928316"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="0e4a5-103">Учебник. Начало работы с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e4a5-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="0e4a5-104">В этом руководстве показано, как использовать .NET Core CLI для создания и запуска веб-приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0e4a5-104">This tutorial shows how to use the .NET Core CLI to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="0e4a5-105">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="0e4a5-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0e4a5-106">создавать проект веб-приложения;</span><span class="sxs-lookup"><span data-stu-id="0e4a5-106">Create a web app project.</span></span>
> * <span data-ttu-id="0e4a5-107">устанавливать доверие к сертификату разработки;</span><span class="sxs-lookup"><span data-stu-id="0e4a5-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="0e4a5-108">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="0e4a5-108">Run the app.</span></span>
> * <span data-ttu-id="0e4a5-109">редактировать страницу Razor.</span><span class="sxs-lookup"><span data-stu-id="0e4a5-109">Edit a Razor page.</span></span>

<span data-ttu-id="0e4a5-110">В итоге вы получите рабочее веб-приложение на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="0e4a5-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Домашняя страница веб-приложения](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="0e4a5-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0e4a5-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/3.1-SDK.md)]

## <a name="create-a-web-app-project"></a><span data-ttu-id="0e4a5-113">Создание проекта веб-приложения</span><span class="sxs-lookup"><span data-stu-id="0e4a5-113">Create a web app project</span></span>

<span data-ttu-id="0e4a5-114">Откройте окно командной оболочки и введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="0e4a5-114">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

<span data-ttu-id="0e4a5-115">Предыдущая команда позволяет:</span><span class="sxs-lookup"><span data-stu-id="0e4a5-115">The preceding command:</span></span>

* <span data-ttu-id="0e4a5-116">создать веб-сайт;</span><span class="sxs-lookup"><span data-stu-id="0e4a5-116">Creates a new web app.</span></span>  
* <span data-ttu-id="0e4a5-117">с помощью параметра `-o aspnetcoreapp` создать каталог *aspnetcoreapp* с исходными файлами приложения.</span><span class="sxs-lookup"><span data-stu-id="0e4a5-117">The `-o aspnetcoreapp` parameter creates a directory named *aspnetcoreapp* with the source files for the app.</span></span>

### <a name="trust-the-development-certificate"></a><span data-ttu-id="0e4a5-118">Установка доверия к сертификату разработки</span><span class="sxs-lookup"><span data-stu-id="0e4a5-118">Trust the development certificate</span></span>

<span data-ttu-id="0e4a5-119">Установите доверие к сертификату разработки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0e4a5-119">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="0e4a5-120">Windows</span><span class="sxs-lookup"><span data-stu-id="0e4a5-120">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="0e4a5-121">Приведенная выше команда отображает следующее диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="0e4a5-121">The preceding command displays the following dialog:</span></span>

![Диалоговое окно "Предупреждение о безопасности"](~/getting-started/_static/cert.png)

<span data-ttu-id="0e4a5-123">Выберите **Да**, если согласны доверять сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="0e4a5-123">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="0e4a5-124">macOS</span><span class="sxs-lookup"><span data-stu-id="0e4a5-124">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="0e4a5-125">Приведенная выше команда отображает следующее сообщение.</span><span class="sxs-lookup"><span data-stu-id="0e4a5-125">The preceding command displays the following message:</span></span>

<span data-ttu-id="0e4a5-126">*Запрошено доверие к сертификату разработки HTTPS. Если сертификат не является доверенным, выполните следующую команду:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="0e4a5-126">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted, we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="0e4a5-127">Эта команда может запросить пароль для установки сертификата в системной цепочке ключей.</span><span class="sxs-lookup"><span data-stu-id="0e4a5-127">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="0e4a5-128">Введите пароль, если согласны доверять сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="0e4a5-128">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="0e4a5-129">Linux</span><span class="sxs-lookup"><span data-stu-id="0e4a5-129">Linux</span></span>](#tab/linux)

<span data-ttu-id="0e4a5-130">Просмотрите документацию по дистрибутиву Linux, чтобы узнать, как установить отношение доверия к сертификату разработки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0e4a5-130">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="0e4a5-131">Дополнительные сведения см. в разделе [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) (Настройка доверия к сертификату разработки HTTPS ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="0e4a5-131">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="0e4a5-132">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="0e4a5-132">Run the app</span></span>

<span data-ttu-id="0e4a5-133">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="0e4a5-133">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet watch run
```

<span data-ttu-id="0e4a5-134">Когда интерпретатор команд покажет, что приложение запущено, откройте страницу [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="0e4a5-134">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="0e4a5-135">Редактирование страницы Razor</span><span class="sxs-lookup"><span data-stu-id="0e4a5-135">Edit a Razor page</span></span>

<span data-ttu-id="0e4a5-136">Откройте *Pages/Index.cshtml*, а затем измените и сохраните страницу, добавив выделенное исправление:</span><span class="sxs-lookup"><span data-stu-id="0e4a5-136">Open *Pages/Index.cshtml* and modify and save the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="0e4a5-137">Перейдите к [https://localhost:5001](https://localhost:5001), обновите страницу и проверьте, отобразились ли изменения.</span><span class="sxs-lookup"><span data-stu-id="0e4a5-137">Browse to [https://localhost:5001](https://localhost:5001), refresh the page, and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0e4a5-138">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="0e4a5-138">Next steps</span></span>

<span data-ttu-id="0e4a5-139">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="0e4a5-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0e4a5-140">создавать проект веб-приложения;</span><span class="sxs-lookup"><span data-stu-id="0e4a5-140">Create a web app project.</span></span>
> * <span data-ttu-id="0e4a5-141">устанавливать доверие к сертификату разработки;</span><span class="sxs-lookup"><span data-stu-id="0e4a5-141">Trust the development certificate.</span></span>
> * <span data-ttu-id="0e4a5-142">Запустите проект.</span><span class="sxs-lookup"><span data-stu-id="0e4a5-142">Run the project.</span></span>
> * <span data-ttu-id="0e4a5-143">вносить изменения.</span><span class="sxs-lookup"><span data-stu-id="0e4a5-143">Make a change.</span></span>

<span data-ttu-id="0e4a5-144">Дополнительные сведения об ASP.NET Core см. в разделе рекомендуемой схемы обучения в вводной статье:</span><span class="sxs-lookup"><span data-stu-id="0e4a5-144">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
