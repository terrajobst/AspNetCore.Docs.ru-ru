---
title: "Подтверждение учетной записи и пароль восстановления в ASP.NET Core"
author: rick-anderson
description: "Сведения о создании приложения ASP.NET Core с помощью сброса пароля и подтверждение по электронной почте."
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: b236b4e5d3a4fa7212453f2aec209d145f5f5e32
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="ecd32-103">Подтверждение учетной записи и пароль восстановления в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ecd32-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="ecd32-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)</span><span class="sxs-lookup"><span data-stu-id="ecd32-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="ecd32-105">Этого учебника показано, как создать приложение ASP.NET Core с помощью сброса пароля и подтверждение по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="ecd32-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="ecd32-106">Этот учебник содержит **не** начало раздела.</span><span class="sxs-lookup"><span data-stu-id="ecd32-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="ecd32-107">Вы должны быть знакомы с:</span><span class="sxs-lookup"><span data-stu-id="ecd32-107">You should be familiar with:</span></span>

* [<span data-ttu-id="ecd32-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ecd32-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="ecd32-109">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="ecd32-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="ecd32-110">Подтверждение учетной записи и восстановление пароля</span><span class="sxs-lookup"><span data-stu-id="ecd32-110">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="ecd32-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ecd32-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="ecd32-112">В разделе [этот файл PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) для версии ASP.NET Core MVC 1.1 и 2.x.</span><span class="sxs-lookup"><span data-stu-id="ecd32-112">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ecd32-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="ecd32-113">Prerequisites</span></span>

<span data-ttu-id="ecd32-114">[.NET core 2.1.4 Установка пакета SDK](https://www.microsoft.com/net/core) или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="ecd32-114">[.NET Core 2.1.4 SDK](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="ecd32-115">Создайте новый проект ASP.NET Core с .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ecd32-115">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ecd32-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ecd32-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

* <span data-ttu-id="ecd32-117">`--auth Individual` Указывает шаблон проекта отдельных учетных записей пользователей.</span><span class="sxs-lookup"><span data-stu-id="ecd32-117">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="ecd32-118">В Windows, добавить `-uld` параметр.</span><span class="sxs-lookup"><span data-stu-id="ecd32-118">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="ecd32-119">Он указывает, что LocalDB должен использоваться вместо SQLite.</span><span class="sxs-lookup"><span data-stu-id="ecd32-119">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="ecd32-120">Запустите `new mvc --help` для получения справки по этой команде.</span><span class="sxs-lookup"><span data-stu-id="ecd32-120">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ecd32-121">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ecd32-121">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ecd32-122">Если вы используете CLI или SQLite, в окно командной строки выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ecd32-122">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="ecd32-123">`--auth Individual` Указывает шаблон проекта отдельных учетных записей пользователей.</span><span class="sxs-lookup"><span data-stu-id="ecd32-123">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="ecd32-124">В Windows, добавить `-uld` параметр.</span><span class="sxs-lookup"><span data-stu-id="ecd32-124">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="ecd32-125">Он указывает, что LocalDB должен использоваться вместо SQLite.</span><span class="sxs-lookup"><span data-stu-id="ecd32-125">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="ecd32-126">Запустите `new mvc --help` для получения справки по этой команде.</span><span class="sxs-lookup"><span data-stu-id="ecd32-126">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="ecd32-127">Кроме того можно создать новый проект ASP.NET Core с помощью Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ecd32-127">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="ecd32-128">В Visual Studio создайте новый **веб-приложение** проекта.</span><span class="sxs-lookup"><span data-stu-id="ecd32-128">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="ecd32-129">Выберите **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="ecd32-129">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="ecd32-130">**.NET core** установлен на следующем рисунке, но вы можете выбрать **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="ecd32-130">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="ecd32-131">Выберите **изменить аутентификацию** и задайте **индивидуальные учетные записи**.</span><span class="sxs-lookup"><span data-stu-id="ecd32-131">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="ecd32-132">Оставьте значение по умолчанию **хранилище учетные записи в приложении**.</span><span class="sxs-lookup"><span data-stu-id="ecd32-132">Keep the default **Store user accounts in-app**.</span></span>

![Отображение «Radio отдельных учетных записей пользователей» выбран диалоговое окно нового проекта](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="ecd32-134">Регистрация нового пользователя теста</span><span class="sxs-lookup"><span data-stu-id="ecd32-134">Test new user registration</span></span>

<span data-ttu-id="ecd32-135">Запустите приложение, выберите **зарегистрировать** ссылку и регистрации пользователя.</span><span class="sxs-lookup"><span data-stu-id="ecd32-135">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="ecd32-136">Следуйте инструкциям для выполнения миграции Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ecd32-136">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="ecd32-137">На этом этапе является проверку только в электронном письме с [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) атрибута.</span><span class="sxs-lookup"><span data-stu-id="ecd32-137">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="ecd32-138">После прохождения процедуры регистрации, вы вошли в приложение.</span><span class="sxs-lookup"><span data-stu-id="ecd32-138">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="ecd32-139">Далее в этом учебнике код обновляется, таким образом, новые пользователи не могут войти пока не был проверен электронной почте.</span><span class="sxs-lookup"><span data-stu-id="ecd32-139">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="ecd32-140">Представление базы данных удостоверений</span><span class="sxs-lookup"><span data-stu-id="ecd32-140">View the Identity database</span></span>

<span data-ttu-id="ecd32-141">В разделе [работа SQLite в проект ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) инструкции о том, как просмотреть базу данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="ecd32-141">See [Working with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="ecd32-142">Для Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ecd32-142">For Visual Studio:</span></span>

* <span data-ttu-id="ecd32-143">Из **представление** последовательно выберите пункты **обозреватель объектов SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="ecd32-143">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="ecd32-144">Перейдите к **(localdb) (SQL Server 13) MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="ecd32-144">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="ecd32-145">Щелкните правой кнопкой мыши **dbo. AspNetUsers** > **просмотра данных**:</span><span class="sxs-lookup"><span data-stu-id="ecd32-145">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Контекстное меню таблицы AspNetUsers в обозревателе объектов SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="ecd32-147">Обратите внимание, таблицы `EmailConfirmed` поле является `False`.</span><span class="sxs-lookup"><span data-stu-id="ecd32-147">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="ecd32-148">Можно использовать это сообщение электронной почты снова на следующем шаге, когда приложение отправляет сообщение с подтверждением.</span><span class="sxs-lookup"><span data-stu-id="ecd32-148">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="ecd32-149">Щелкните правой кнопкой мыши в строке и выберите **удалить**.</span><span class="sxs-lookup"><span data-stu-id="ecd32-149">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="ecd32-150">Удаление псевдонимов электронной почты помогает в следующих шагах.</span><span class="sxs-lookup"><span data-stu-id="ecd32-150">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="ecd32-151">Требовать использования протокола HTTPS</span><span class="sxs-lookup"><span data-stu-id="ecd32-151">Require HTTPS</span></span>

<span data-ttu-id="ecd32-152">В разделе [обязательного использования протокола HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="ecd32-152">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="ecd32-153">Требовать подтверждения по электронной почте</span><span class="sxs-lookup"><span data-stu-id="ecd32-153">Require email confirmation</span></span>

<span data-ttu-id="ecd32-154">Рекомендуется для подтверждения адреса электронной почты Регистрация нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="ecd32-154">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="ecd32-155">Отправить по электронной почте подтверждение помогает проверить, они не использовании олицетворения кто-то другой (то есть они еще не зарегистрированы с помощью электронной почты другого пользователя).</span><span class="sxs-lookup"><span data-stu-id="ecd32-155">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="ecd32-156">Предположим, что имеется дискуссионный форум и избежать "yli@example.com«от регистрации, как»nolivetto@contoso.com.»</span><span class="sxs-lookup"><span data-stu-id="ecd32-156">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="ecd32-157">Без подтверждения по электронной почте»nolivetto@contoso.com» может получать нежелательные сообщения от приложения.</span><span class="sxs-lookup"><span data-stu-id="ecd32-157">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="ecd32-158">Предположим, что пользователь случайно зарегистрированы как «ylo@example.com» и не заметили ошибочное «yli».</span><span class="sxs-lookup"><span data-stu-id="ecd32-158">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="ecd32-159">Они не смогут использовать пароль восстановления, так как у приложения нет электронной почте правильно.</span><span class="sxs-lookup"><span data-stu-id="ecd32-159">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="ecd32-160">Подтверждение по электронной почте предоставляет ограниченную защиту от программы-роботы.</span><span class="sxs-lookup"><span data-stu-id="ecd32-160">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="ecd32-161">Подтверждение по электронной почте не обеспечивает защиты от злоумышленников с многих учетные записи электронной почты.</span><span class="sxs-lookup"><span data-stu-id="ecd32-161">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="ecd32-162">Как правило, требуется запретить пользователям новых учетных данных для веб-узла до того, как они подтверждения по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="ecd32-162">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="ecd32-163">Обновление `ConfigureServices` требовать подтверждения по электронной почте:</span><span class="sxs-lookup"><span data-stu-id="ecd32-163">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="ecd32-164">`config.SignIn.RequireConfirmedEmail = true;` зарегистрированные пользователю войти в систему, пока не будет подтверждена электронной почте.</span><span class="sxs-lookup"><span data-stu-id="ecd32-164">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="ecd32-165">Настройка поставщика услуг электронной почты</span><span class="sxs-lookup"><span data-stu-id="ecd32-165">Configure email provider</span></span>

<span data-ttu-id="ecd32-166">В этом учебнике SendGrid используется для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="ecd32-166">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="ecd32-167">Требуется учетной записи SendGrid и ключ для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="ecd32-167">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="ecd32-168">Можно использовать другие поставщики электронной почты.</span><span class="sxs-lookup"><span data-stu-id="ecd32-168">You can use other email providers.</span></span> <span data-ttu-id="ecd32-169">Включает 2.x ASP.NET Core `System.Net.Mail`, позволяющий отправлять электронную почту из приложения.</span><span class="sxs-lookup"><span data-stu-id="ecd32-169">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="ecd32-170">Мы рекомендуем использовать SendGrid или другая служба электронной почты для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="ecd32-170">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="ecd32-171">Для защиты и правильно настроить сложно SMTP.</span><span class="sxs-lookup"><span data-stu-id="ecd32-171">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="ecd32-172">[Параметры шаблона](xref:fundamentals/configuration/options) используется для доступа к параметрам учетной записи и ключа пользователя.</span><span class="sxs-lookup"><span data-stu-id="ecd32-172">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="ecd32-173">Дополнительные сведения см. в разделе [конфигурации](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="ecd32-173">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="ecd32-174">Создание класса для получения ключа защиты электронной почты.</span><span class="sxs-lookup"><span data-stu-id="ecd32-174">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="ecd32-175">Для этого образца `AuthMessageSenderOptions` класса создается в *Services/AuthMessageSenderOptions.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="ecd32-175">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="ecd32-176">Задать `SendGridUser` и `SendGridKey` с [секрет диспетчера](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="ecd32-176">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="ecd32-177">Пример:</span><span class="sxs-lookup"><span data-stu-id="ecd32-177">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="ecd32-178">В Windows, диспетчер секрет хранит пары ключей и значений в *secrets.json* файл `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` каталог.</span><span class="sxs-lookup"><span data-stu-id="ecd32-178">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="ecd32-179">Содержимое *secrets.json* файл не зашифрован.</span><span class="sxs-lookup"><span data-stu-id="ecd32-179">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="ecd32-180">*Secrets.json* ниже приведен файл ( `SendGridKey` значение был удален.)</span><span class="sxs-lookup"><span data-stu-id="ecd32-180">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="ecd32-181">Настройка запуска для использования AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="ecd32-181">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="ecd32-182">Добавить `AuthMessageSenderOptions` в контейнер службы, в конце `ConfigureServices` метод в *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="ecd32-182">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ecd32-183">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ecd32-183">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ecd32-184">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ecd32-184">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="ecd32-185">Настроить класс AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="ecd32-185">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="ecd32-186">Этот учебник демонстрирует добавление уведомления по электронной почте через [SendGrid](https://sendgrid.com/), однако вы можете отправлять электронную почту с помощью SMTP и другие механизмы.</span><span class="sxs-lookup"><span data-stu-id="ecd32-186">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="ecd32-187">Установить `SendGrid` пакет NuGet:</span><span class="sxs-lookup"><span data-stu-id="ecd32-187">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="ecd32-188">Из командной строки:</span><span class="sxs-lookup"><span data-stu-id="ecd32-188">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="ecd32-189">В консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ecd32-189">From the Package Manager Console, enter the following command:</span></span>

 `Install-Package SendGrid`

<span data-ttu-id="ecd32-190">В разделе [начните бесплатно с помощью SendGrid](https://sendgrid.com/free/) для регистрации бесплатной учетной записи SendGrid.</span><span class="sxs-lookup"><span data-stu-id="ecd32-190">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="ecd32-191">Настройка SendGrid</span><span class="sxs-lookup"><span data-stu-id="ecd32-191">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ecd32-192">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ecd32-192">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ecd32-193">Чтобы настроить SendGrid, добавьте код, аналогичный следующему *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="ecd32-193">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ecd32-194">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ecd32-194">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
* <span data-ttu-id="ecd32-195">Добавьте код в *Services/MessageServices.cs* аналогичный приведенному ниже, чтобы настроить SendGrid:</span><span class="sxs-lookup"><span data-stu-id="ecd32-195">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="ecd32-196">Включить учетную запись, пароль и Подтверждение восстановления</span><span class="sxs-lookup"><span data-stu-id="ecd32-196">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="ecd32-197">Шаблон содержит код для восстановления и подтверждение пароля учетной записи.</span><span class="sxs-lookup"><span data-stu-id="ecd32-197">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="ecd32-198">Найти `OnPostAsync` метод в *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="ecd32-198">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ecd32-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ecd32-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ecd32-200">Запретить пользователям вновь зарегистрированного выполняется автоматический вход в систему, преобразуйте следующую строку:</span><span class="sxs-lookup"><span data-stu-id="ecd32-200">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="ecd32-201">С измененной выделенной строке показан полный метод.</span><span class="sxs-lookup"><span data-stu-id="ecd32-201">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ecd32-202">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ecd32-202">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ecd32-203">Чтобы включить подтверждение учетной записи, раскомментируйте следующий код:</span><span class="sxs-lookup"><span data-stu-id="ecd32-203">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="ecd32-204">**Примечание:** код препятствует вновь зарегистрированного пользователя выполняется автоматический вход в систему, преобразуйте следующую строку:</span><span class="sxs-lookup"><span data-stu-id="ecd32-204">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="ecd32-205">Включить восстановление пароля Раскомментировать код в `ForgotPassword` действие *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="ecd32-205">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="ecd32-206">Раскомментируйте элемента form в *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ecd32-206">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="ecd32-207">Может потребоваться удалить `<p> For more information on how to enable reset password ... </p>` элемент, который содержит ссылку на эту статью.</span><span class="sxs-lookup"><span data-stu-id="ecd32-207">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="ecd32-208">Регистрация, подтверждение по электронной почте и сброс пароля</span><span class="sxs-lookup"><span data-stu-id="ecd32-208">Register, confirm email, and reset password</span></span>

<span data-ttu-id="ecd32-209">Запустите веб-приложения и тестов подтверждение учетной записи и пароль восстановления потока.</span><span class="sxs-lookup"><span data-stu-id="ecd32-209">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="ecd32-210">Запустите приложение и регистрации нового пользователя</span><span class="sxs-lookup"><span data-stu-id="ecd32-210">Run the app and register a new user</span></span>

 ![Веб-приложения, зарегистрируйте учетную запись-представление](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="ecd32-212">Проверьте свою почту ссылку для подтверждения учетной записи.</span><span class="sxs-lookup"><span data-stu-id="ecd32-212">Check your email for the account confirmation link.</span></span> <span data-ttu-id="ecd32-213">В разделе [Отладка электронной почты](#debug) Если вы не получаете сообщение электронной почты.</span><span class="sxs-lookup"><span data-stu-id="ecd32-213">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="ecd32-214">Щелкните ссылку, чтобы подтвердить ваш адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="ecd32-214">Click the link to confirm your email.</span></span>
* <span data-ttu-id="ecd32-215">Войдите в систему электронной почты и пароль.</span><span class="sxs-lookup"><span data-stu-id="ecd32-215">Log in with your email and password.</span></span>
* <span data-ttu-id="ecd32-216">Выйдите из системы.</span><span class="sxs-lookup"><span data-stu-id="ecd32-216">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="ecd32-217">Страница управления представления</span><span class="sxs-lookup"><span data-stu-id="ecd32-217">View the manage page</span></span>

<span data-ttu-id="ecd32-218">Выберите имя пользователя в браузере: ![окно браузера с именем пользователя](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="ecd32-218">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="ecd32-219">Может потребоваться развернуть панель навигации, чтобы увидеть имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="ecd32-219">You might need to expand the navbar to see user name.</span></span>

![панель переходов](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ecd32-221">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ecd32-221">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ecd32-222">Страница управления отображается с **профиль** с выбранной вкладкой.</span><span class="sxs-lookup"><span data-stu-id="ecd32-222">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="ecd32-223">**Электронной почты** показывает подтвердила типа "флажок", указывающее, сообщения электронной почты.</span><span class="sxs-lookup"><span data-stu-id="ecd32-223">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![страница «Управление»](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ecd32-225">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ecd32-225">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ecd32-226">Это упоминается далее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="ecd32-226">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="ecd32-227">![страница «Управление»](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="ecd32-227">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="ecd32-228">Сброс пароля для тестирования</span><span class="sxs-lookup"><span data-stu-id="ecd32-228">Test password reset</span></span>

* <span data-ttu-id="ecd32-229">Если вы выполнили вход, выберите **выхода**.</span><span class="sxs-lookup"><span data-stu-id="ecd32-229">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="ecd32-230">Выберите **входа** ссылку и выберите **забыли пароль?** ссылку.</span><span class="sxs-lookup"><span data-stu-id="ecd32-230">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="ecd32-231">Введите адрес электронной почты, который использовался для регистрации учетной записи.</span><span class="sxs-lookup"><span data-stu-id="ecd32-231">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="ecd32-232">Отправляется сообщение электронной почты со ссылкой для сброса пароля.</span><span class="sxs-lookup"><span data-stu-id="ecd32-232">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="ecd32-233">Проверьте свою почту и щелкните ссылку, чтобы сбросить пароль.</span><span class="sxs-lookup"><span data-stu-id="ecd32-233">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="ecd32-234">После успешного сброса пароля можно войти электронной почты и новый пароль.</span><span class="sxs-lookup"><span data-stu-id="ecd32-234">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="ecd32-235">Отладка электронной почты</span><span class="sxs-lookup"><span data-stu-id="ecd32-235">Debug email</span></span>

<span data-ttu-id="ecd32-236">Если не удается получить рабочей электронной почты:</span><span class="sxs-lookup"><span data-stu-id="ecd32-236">If you can't get email working:</span></span>

* <span data-ttu-id="ecd32-237">Создание [консольного приложения для отправки электронной почты](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="ecd32-237">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="ecd32-238">Просмотрите [действия электронной почты](https://sendgrid.com/docs/User_Guide/email_activity.html) страницы.</span><span class="sxs-lookup"><span data-stu-id="ecd32-238">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="ecd32-239">Проверьте папку нежелательной почты.</span><span class="sxs-lookup"><span data-stu-id="ecd32-239">Check your spam folder.</span></span>
* <span data-ttu-id="ecd32-240">Попробуйте другой псевдоним электронной почты на другой поставщик (Microsoft, Yahoo, Gmail, и т. д.)</span><span class="sxs-lookup"><span data-stu-id="ecd32-240">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="ecd32-241">Повторите отправку учетным записям другое сообщение.</span><span class="sxs-lookup"><span data-stu-id="ecd32-241">Try sending to different email accounts.</span></span>

<span data-ttu-id="ecd32-242">**Рекомендации по безопасности** — **не** используйте секреты производства в тестирования и разработки.</span><span class="sxs-lookup"><span data-stu-id="ecd32-242">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="ecd32-243">При публикации приложения в Azure, секреты SendGrid можно задать как параметры приложения на портале веб-приложения Azure.</span><span class="sxs-lookup"><span data-stu-id="ecd32-243">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="ecd32-244">Система конфигурации настраивается для чтения ключи из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="ecd32-244">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="ecd32-245">Объединение социальных сетей и локальных учетных записей</span><span class="sxs-lookup"><span data-stu-id="ecd32-245">Combine social and local login accounts</span></span>

<span data-ttu-id="ecd32-246">Для выполнения в этом разделе, необходимо сначала включить внешнего поставщика аутентификации.</span><span class="sxs-lookup"><span data-stu-id="ecd32-246">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="ecd32-247">В разделе [Facebook, Google и внешнего поставщика проверки подлинности](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="ecd32-247">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="ecd32-248">Можно объединять учетные записи локальных и социальных сетей, щелкнув ссылку по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="ecd32-248">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="ecd32-249">В следующей последовательности «RickAndMSFT@gmail.com» сначала создается как локальное имя входа; тем не менее, можно сначала создать учетную запись входа на социальных сетей, а затем добавить локальный вход.</span><span class="sxs-lookup"><span data-stu-id="ecd32-249">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Веб-приложения: RickAndMSFT@gmail.com пользователь прошел проверку подлинности](accconfirm/_static/rick.png)

<span data-ttu-id="ecd32-251">Щелкните **управление** ссылку.</span><span class="sxs-lookup"><span data-stu-id="ecd32-251">Click on the **Manage** link.</span></span> <span data-ttu-id="ecd32-252">Примечание внешних 0 (социальных имена входа), связанные с этой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="ecd32-252">Note the 0 external (social logins) associated with this account.</span></span>

![Управление представления](accconfirm/_static/manage.png)

<span data-ttu-id="ecd32-254">Щелкните ссылку на другую службу входа в систему и принимать запросы приложений.</span><span class="sxs-lookup"><span data-stu-id="ecd32-254">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="ecd32-255">На следующем рисунке Facebook является внешнего поставщика проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="ecd32-255">In the following image, Facebook is the external authentication provider:</span></span>

![Управление листинг Facebook Просмотр внешних имен входа](accconfirm/_static/fb.png)

<span data-ttu-id="ecd32-257">Объединены две учетные записи.</span><span class="sxs-lookup"><span data-stu-id="ecd32-257">The two accounts have been combined.</span></span> <span data-ttu-id="ecd32-258">Имеется возможность войти в систему с любой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="ecd32-258">You are able to log on with either account.</span></span> <span data-ttu-id="ecd32-259">Может потребоваться пользователям добавлять локальные учетные записи, в случае их службы проверки подлинности имени входа социальных сетей не работает или более вероятно, они потеряли доступ к учетной записи социальных сетей.</span><span class="sxs-lookup"><span data-stu-id="ecd32-259">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="ecd32-260">Включить подтверждение учетной записи после сайт имеет пользователей</span><span class="sxs-lookup"><span data-stu-id="ecd32-260">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="ecd32-261">Включение подтверждение учетной записи на узле с пользователями блокирует работу всех существующих пользователей.</span><span class="sxs-lookup"><span data-stu-id="ecd32-261">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="ecd32-262">Существующие пользователи заблокированы, так как учетные записи не подтверждено.</span><span class="sxs-lookup"><span data-stu-id="ecd32-262">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="ecd32-263">Чтобы обойти выход из блокировки пользователя, используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="ecd32-263">To work around exiting user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="ecd32-264">Обновление базы данных для пометки всех существующих пользователей как подтверждения</span><span class="sxs-lookup"><span data-stu-id="ecd32-264">Update the database to mark all existing users as being confirmed</span></span>
* <span data-ttu-id="ecd32-265">Подтвердите для существующих пользователей.</span><span class="sxs-lookup"><span data-stu-id="ecd32-265">Confirm exiting users.</span></span> <span data-ttu-id="ecd32-266">Например пакет отправки сообщения электронной почты с подтверждением ссылки.</span><span class="sxs-lookup"><span data-stu-id="ecd32-266">For example, batch-send emails with confirmation links.</span></span>
