---
title: Начало работы с ASP.NET Core
author: rick-anderson
description: Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World.
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: getting-started
ms.openlocfilehash: 0f05ab120d64832a4bc2fd70921efc7238ee9eac
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187059"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="3c3ac-103">Учебник. Начало работы с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c3ac-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="3c3ac-104">В этом учебнике показано, как использовать интерфейс командной строки .NET Core для создания и запуска веб-приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-104">This tutorial shows how to use the .NET Core command-line interface to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="3c3ac-105">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="3c3ac-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3c3ac-106">создавать проект веб-приложения;</span><span class="sxs-lookup"><span data-stu-id="3c3ac-106">Create a web app project.</span></span>
> * <span data-ttu-id="3c3ac-107">устанавливать доверие к сертификату разработки;</span><span class="sxs-lookup"><span data-stu-id="3c3ac-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="3c3ac-108">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-108">Run the app.</span></span>
> * <span data-ttu-id="3c3ac-109">редактировать страницу Razor.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-109">Edit a Razor page.</span></span>

<span data-ttu-id="3c3ac-110">В итоге вы получите рабочее веб-приложение на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Домашняя страница веб-приложения](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="3c3ac-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="3c3ac-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/3.0-SDK.md)]

## <a name="create-a-web-app-project"></a><span data-ttu-id="3c3ac-113">Создание проекта веб-приложения</span><span class="sxs-lookup"><span data-stu-id="3c3ac-113">Create a web app project</span></span>

<span data-ttu-id="3c3ac-114">Откройте окно командной оболочки и введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="3c3ac-114">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

<span data-ttu-id="3c3ac-115">Предыдущая команда позволяет:</span><span class="sxs-lookup"><span data-stu-id="3c3ac-115">The preceding command:</span></span>

* <span data-ttu-id="3c3ac-116">создать веб-сайт;</span><span class="sxs-lookup"><span data-stu-id="3c3ac-116">Creates a new web app.</span></span>  
* <span data-ttu-id="3c3ac-117">с помощью параметра `-o` создать каталог *aspnetcoreapp* с исходными файлами приложения.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-117">The `-o` parameter creates a directory named *aspnetcoreapp* with the source files for the app.</span></span>

### <a name="trust-the-development-certificate"></a><span data-ttu-id="3c3ac-118">Установка доверия к сертификату разработки</span><span class="sxs-lookup"><span data-stu-id="3c3ac-118">Trust the development certificate</span></span>

<span data-ttu-id="3c3ac-119">Установите доверие к сертификату разработки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-119">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="3c3ac-120">Windows</span><span class="sxs-lookup"><span data-stu-id="3c3ac-120">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="3c3ac-121">Приведенная выше команда отображает следующее диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-121">The preceding command displays the following dialog:</span></span>

![Диалоговое окно "Предупреждение о безопасности"](~/getting-started/_static/cert.png)

<span data-ttu-id="3c3ac-123">Выберите **Да**, если согласны доверять сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-123">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="3c3ac-124">macOS</span><span class="sxs-lookup"><span data-stu-id="3c3ac-124">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="3c3ac-125">Приведенная выше команда отображает следующее сообщение.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-125">The preceding command displays the following message:</span></span>

<span data-ttu-id="3c3ac-126">*Запрошено доверие к сертификату разработки HTTPS. Если сертификат не является доверенным, выполните следующую команду:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-126">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="3c3ac-127">Эта команда может запросить пароль для установки сертификата в системной цепочке ключей.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-127">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="3c3ac-128">Введите пароль, если согласны доверять сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-128">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="3c3ac-129">Linux</span><span class="sxs-lookup"><span data-stu-id="3c3ac-129">Linux</span></span>](#tab/linux)

<span data-ttu-id="3c3ac-130">Дополнительные сведения о подсистеме Windows для Linux см. в разделе [Trust HTTPS certificate from Windows Subsystem for Linux](xref:security/enforcing-ssl#wsl) (Установка доверия к сертификату HTTPS из подсистемы Windows для Linux).</span><span class="sxs-lookup"><span data-stu-id="3c3ac-130">For Windows Subsystem for Linux, see [Trust HTTPS certificate from Windows Subsystem for Linux](xref:security/enforcing-ssl#wsl).</span></span>

<span data-ttu-id="3c3ac-131">Просмотрите документацию по дистрибутиву Linux, чтобы узнать, как установить отношение доверия к сертификату разработки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-131">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="3c3ac-132">Дополнительные сведения см. в разделе [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) (Настройка доверия к сертификату разработки HTTPS ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="3c3ac-132">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="3c3ac-133">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="3c3ac-133">Run the app</span></span>

<span data-ttu-id="3c3ac-134">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="3c3ac-134">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="3c3ac-135">Когда интерпретатор команд покажет, что приложение запущено, откройте страницу [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="3c3ac-135">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="3c3ac-136">Щелкните **Принять**, чтобы принять политику конфиденциальности и использования файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-136">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="3c3ac-137">Это приложение не хранит персональные данные.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-137">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="3c3ac-138">Редактирование страницы Razor</span><span class="sxs-lookup"><span data-stu-id="3c3ac-138">Edit a Razor page</span></span>

<span data-ttu-id="3c3ac-139">Откройте *Pages/Index.cshtml* и измените страницу, добавив выделенное исправление.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-139">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="3c3ac-140">Перейдите к [https://localhost:5001](https://localhost:5001) и проверьте, отобразились ли изменения.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-140">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c3ac-141">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="3c3ac-141">Next steps</span></span>

<span data-ttu-id="3c3ac-142">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="3c3ac-142">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3c3ac-143">создавать проект веб-приложения;</span><span class="sxs-lookup"><span data-stu-id="3c3ac-143">Create a web app project.</span></span>
> * <span data-ttu-id="3c3ac-144">устанавливать доверие к сертификату разработки;</span><span class="sxs-lookup"><span data-stu-id="3c3ac-144">Trust the development certificate.</span></span>
> * <span data-ttu-id="3c3ac-145">Запустите проект.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-145">Run the project.</span></span>
> * <span data-ttu-id="3c3ac-146">вносить изменения.</span><span class="sxs-lookup"><span data-stu-id="3c3ac-146">Make a change.</span></span>

<span data-ttu-id="3c3ac-147">Дополнительные сведения об ASP.NET Core см. в разделе рекомендуемой схемы обучения в вводной статье:</span><span class="sxs-lookup"><span data-stu-id="3c3ac-147">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
