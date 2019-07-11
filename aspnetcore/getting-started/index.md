---
title: Начало работы с ASP.NET Core
author: rick-anderson
description: Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World.
ms.author: riande
ms.custom: mvc
ms.date: 05/15/2019
uid: getting-started
ms.openlocfilehash: c35251a0e49fbbffee7b8f5ea6905322b9042261
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814935"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="95dc4-103">Учебник. Начало работы с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="95dc4-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="95dc4-104">В этом учебнике показано, как использовать интерфейс командной строки .NET Core для создания и запуска веб-приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="95dc4-104">This tutorial shows how to use the .NET Core command-line interface to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="95dc4-105">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="95dc4-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="95dc4-106">создавать проект веб-приложения;</span><span class="sxs-lookup"><span data-stu-id="95dc4-106">Create a web app project.</span></span>
> * <span data-ttu-id="95dc4-107">устанавливать доверие к сертификату разработки;</span><span class="sxs-lookup"><span data-stu-id="95dc4-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="95dc4-108">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="95dc4-108">Run the app.</span></span>
> * <span data-ttu-id="95dc4-109">редактировать страницу Razor.</span><span class="sxs-lookup"><span data-stu-id="95dc4-109">Edit a Razor page.</span></span>

<span data-ttu-id="95dc4-110">В итоге вы получите рабочее веб-приложение на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="95dc4-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Домашняя страница веб-приложения](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="95dc4-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="95dc4-112">Prerequisites</span></span>

* [<span data-ttu-id="95dc4-113">Пакет SDK для .NET Core 2.2</span><span class="sxs-lookup"><span data-stu-id="95dc4-113">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a><span data-ttu-id="95dc4-114">Создание проекта веб-приложения</span><span class="sxs-lookup"><span data-stu-id="95dc4-114">Create a web app project</span></span>

<span data-ttu-id="95dc4-115">Откройте окно командной оболочки и введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="95dc4-115">Open a command shell, and enter the following command:</span></span>

```console
dotnet new webapp -o aspnetcoreapp
```

### <a name="trust-the-development-certificate"></a><span data-ttu-id="95dc4-116">Установка доверия к сертификату разработки</span><span class="sxs-lookup"><span data-stu-id="95dc4-116">Trust the development certificate</span></span>

<span data-ttu-id="95dc4-117">Установите доверие к сертификату разработки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="95dc4-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="95dc4-118">Windows</span><span class="sxs-lookup"><span data-stu-id="95dc4-118">Windows</span></span>](#tab/windows)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="95dc4-119">Приведенная выше команда отображает следующее диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="95dc4-119">The preceding command displays the following dialog:</span></span>

![Диалоговое окно "Предупреждение о безопасности"](~/getting-started/_static/cert.png)

<span data-ttu-id="95dc4-121">Выберите **Да**, если согласны доверять сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="95dc4-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="95dc4-122">macOS</span><span class="sxs-lookup"><span data-stu-id="95dc4-122">macOS</span></span>](#tab/macos)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="95dc4-123">Приведенная выше команда отображает следующее сообщение.</span><span class="sxs-lookup"><span data-stu-id="95dc4-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="95dc4-124">*Запрошено доверие к сертификату разработки HTTPS. Если сертификат не является доверенным, выполните следующую команду:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="95dc4-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="95dc4-125">Эта команда может запросить пароль для установки сертификата в системной цепочке ключей.</span><span class="sxs-lookup"><span data-stu-id="95dc4-125">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="95dc4-126">Введите пароль, если согласны доверять сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="95dc4-126">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="95dc4-127">Linux</span><span class="sxs-lookup"><span data-stu-id="95dc4-127">Linux</span></span>](#tab/linux)

<span data-ttu-id="95dc4-128">Дополнительные сведения о подсистеме Windows для Linux см. в разделе [Trust HTTPS certificate from Windows Subsystem for Linux](xref:security/enforcing-ssl#wsl) (Установка доверия к сертификату HTTPS из подсистемы Windows для Linux).</span><span class="sxs-lookup"><span data-stu-id="95dc4-128">For Windows Subsystem for Linux, see [Trust HTTPS certificate from Windows Subsystem for Linux](xref:security/enforcing-ssl#wsl).</span></span>

<span data-ttu-id="95dc4-129">Просмотрите документацию по дистрибутиву Linux, чтобы узнать, как установить отношение доверия к сертификату разработки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="95dc4-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="95dc4-130">Дополнительные сведения см. в разделе [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) (Настройка доверия к сертификату разработки HTTPS ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="95dc4-130">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="95dc4-131">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="95dc4-131">Run the app</span></span>

<span data-ttu-id="95dc4-132">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="95dc4-132">Run the following commands:</span></span>

```console
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="95dc4-133">Когда интерпретатор команд покажет, что приложение запущено, откройте страницу [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="95dc4-133">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="95dc4-134">Щелкните **Принять**, чтобы принять политику конфиденциальности и использования файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="95dc4-134">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="95dc4-135">Это приложение не хранит персональные данные.</span><span class="sxs-lookup"><span data-stu-id="95dc4-135">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="95dc4-136">Редактирование страницы Razor</span><span class="sxs-lookup"><span data-stu-id="95dc4-136">Edit a Razor page</span></span>

<span data-ttu-id="95dc4-137">Откройте *Pages/Index.cshtml* и измените страницу, добавив выделенное исправление.</span><span class="sxs-lookup"><span data-stu-id="95dc4-137">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="95dc4-138">Перейдите к [https://localhost:5001](https://localhost:5001) и проверьте, отобразились ли изменения.</span><span class="sxs-lookup"><span data-stu-id="95dc4-138">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="95dc4-139">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="95dc4-139">Next steps</span></span>

<span data-ttu-id="95dc4-140">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="95dc4-140">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="95dc4-141">создавать проект веб-приложения;</span><span class="sxs-lookup"><span data-stu-id="95dc4-141">Create a web app project.</span></span>
> * <span data-ttu-id="95dc4-142">устанавливать доверие к сертификату разработки;</span><span class="sxs-lookup"><span data-stu-id="95dc4-142">Trust the development certificate.</span></span>
> * <span data-ttu-id="95dc4-143">Запустите проект.</span><span class="sxs-lookup"><span data-stu-id="95dc4-143">Run the project.</span></span>
> * <span data-ttu-id="95dc4-144">вносить изменения.</span><span class="sxs-lookup"><span data-stu-id="95dc4-144">Make a change.</span></span>

<span data-ttu-id="95dc4-145">Дополнительные сведения об ASP.NET Core см. в разделе рекомендуемой схемы обучения в вводной статье:</span><span class="sxs-lookup"><span data-stu-id="95dc4-145">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
