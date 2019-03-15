---
title: Начало работы с ASP.NET Core
author: rick-anderson
description: 'Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World.'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2019
uid: getting-started
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="20cd7-103">Учебник. Начало работы с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="20cd7-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="20cd7-104">В этом руководстве показано, как использовать интерфейс командной строки .NET Core для создания веб-приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="20cd7-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span>

<span data-ttu-id="20cd7-105">Вы научитесь:</span><span class="sxs-lookup"><span data-stu-id="20cd7-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="20cd7-106">создавать проект веб-приложения;</span><span class="sxs-lookup"><span data-stu-id="20cd7-106">Create a web app project.</span></span>
> * <span data-ttu-id="20cd7-107">включать локальный HTTPS;</span><span class="sxs-lookup"><span data-stu-id="20cd7-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="20cd7-108">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="20cd7-108">Run the app.</span></span>
> * <span data-ttu-id="20cd7-109">редактировать страницу Razor.</span><span class="sxs-lookup"><span data-stu-id="20cd7-109">Edit a Razor page.</span></span>

<span data-ttu-id="20cd7-110">В итоге вы получите рабочее веб-приложение на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="20cd7-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Домашняя страница веб-приложения](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="20cd7-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="20cd7-112">Prerequisites</span></span>

* [<span data-ttu-id="20cd7-113">Пакет SDK для .NET Core 2.2</span><span class="sxs-lookup"><span data-stu-id="20cd7-113">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a><span data-ttu-id="20cd7-114">Создание проекта веб-приложения</span><span class="sxs-lookup"><span data-stu-id="20cd7-114">Create a web app project</span></span>

<span data-ttu-id="20cd7-115">Откройте окно командной оболочки и введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="20cd7-115">Open a command shell, and enter the following command:</span></span>

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a><span data-ttu-id="20cd7-116">Включение локального HTTPS</span><span class="sxs-lookup"><span data-stu-id="20cd7-116">Enable local HTTPS</span></span>

<span data-ttu-id="20cd7-117">Установите доверие к сертификату разработки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="20cd7-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="20cd7-118">Windows</span><span class="sxs-lookup"><span data-stu-id="20cd7-118">Windows</span></span>](#tab/windows)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="20cd7-119">Приведенная выше команда отображает следующее диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="20cd7-119">The preceding command displays the following dialog:</span></span>

![Диалоговое окно "Предупреждение о безопасности"](~/getting-started/_static/cert.png)

<span data-ttu-id="20cd7-121">Выберите **Да**, если согласны доверять сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="20cd7-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="20cd7-122">macOS</span><span class="sxs-lookup"><span data-stu-id="20cd7-122">macOS</span></span>](#tab/macos)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="20cd7-123">Приведенная выше команда отображает следующее сообщение.</span><span class="sxs-lookup"><span data-stu-id="20cd7-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="20cd7-124">*Запрошено доверие к сертификату разработки HTTPS. Если сертификат не является доверенным, выполните следующую команду:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="20cd7-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="20cd7-125">Эта команда может запросить пароль для установки сертификата в системной цепочке ключей.</span><span class="sxs-lookup"><span data-stu-id="20cd7-125">This command command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="20cd7-126">Введите пароль, если согласны доверять сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="20cd7-126">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="20cd7-127">Linux</span><span class="sxs-lookup"><span data-stu-id="20cd7-127">Linux</span></span>](#tab/linux)

<span data-ttu-id="20cd7-128">Просмотрите документацию по дистрибутиву Linux, чтобы узнать, как установить отношение доверия к сертификату разработки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="20cd7-128">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="20cd7-129">Дополнительные сведения см. в разделе [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) (Настройка доверия к сертификату разработки HTTPS ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="20cd7-129">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="20cd7-130">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="20cd7-130">Run the app</span></span>

<span data-ttu-id="20cd7-131">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="20cd7-131">Run the following commands:</span></span>

```console
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="20cd7-132">Когда интерпретатор команд покажет, что приложение запущено, откройте страницу [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="20cd7-132">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="20cd7-133">Щелкните **Принять**, чтобы принять политику конфиденциальности и использования файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="20cd7-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="20cd7-134">Это приложение не хранит персональные данные.</span><span class="sxs-lookup"><span data-stu-id="20cd7-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="20cd7-135">Редактирование страницы Razor</span><span class="sxs-lookup"><span data-stu-id="20cd7-135">Edit a Razor page</span></span>

<span data-ttu-id="20cd7-136">Откройте *Pages/Index.cshtml* и измените страницу, добавив выделенное исправление.</span><span class="sxs-lookup"><span data-stu-id="20cd7-136">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="20cd7-137">Перейдите к [https://localhost:5001](https://localhost:5001) и проверьте, отобразились ли изменения.</span><span class="sxs-lookup"><span data-stu-id="20cd7-137">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="20cd7-138">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="20cd7-138">Next steps</span></span>

<span data-ttu-id="20cd7-139">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="20cd7-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="20cd7-140">создавать проект веб-приложения;</span><span class="sxs-lookup"><span data-stu-id="20cd7-140">Create a web app project.</span></span>
> * <span data-ttu-id="20cd7-141">включать локальный HTTPS;</span><span class="sxs-lookup"><span data-stu-id="20cd7-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="20cd7-142">Запустите проект.</span><span class="sxs-lookup"><span data-stu-id="20cd7-142">Run the project.</span></span>
> * <span data-ttu-id="20cd7-143">вносить изменения.</span><span class="sxs-lookup"><span data-stu-id="20cd7-143">Make a change.</span></span>

<span data-ttu-id="20cd7-144">Дополнительные сведения об ASP.NET Core см. в разделе рекомендуемой схемы обучения в вводной статье:</span><span class="sxs-lookup"><span data-stu-id="20cd7-144">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
