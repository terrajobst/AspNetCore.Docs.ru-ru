---
title: "Подтверждение учетной записи и пароль восстановления в ASP.NET Core"
author: rick-anderson
description: "Показано, как создать приложение ASP.NET Core с помощью сброса пароля и подтверждение по электронной почте."
manager: wpickett
ms.author: riande
ms.date: 12/1/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: 14c7fdfc1ed8b87aac8ca937298c7da6373bf06d
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="62df3-103">Подтверждение учетной записи и пароль восстановления в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62df3-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="62df3-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)</span><span class="sxs-lookup"><span data-stu-id="62df3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="62df3-105">Этого учебника показано, как создать приложение ASP.NET Core с помощью сброса пароля и подтверждение по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="62df3-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="62df3-106">Создание проекта ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62df3-106">Create a New ASP.NET Core Project</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62df3-107">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62df3-107">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="62df3-108">Этот шаг применяется Visual Studio в Windows.</span><span class="sxs-lookup"><span data-stu-id="62df3-108">This step applies to Visual Studio on Windows.</span></span> <span data-ttu-id="62df3-109">В следующем разделе инструкции CLI.</span><span class="sxs-lookup"><span data-stu-id="62df3-109">See the next section for CLI instructions.</span></span>

<span data-ttu-id="62df3-110">Учебник по требуется 2017 г предварительной версии Visual Studio 2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="62df3-110">The tutorial requires Visual Studio 2017 Preview 2 or later.</span></span>

* <span data-ttu-id="62df3-111">В Visual Studio создайте новый проект веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="62df3-111">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="62df3-112">Выберите **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="62df3-112">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="62df3-113">На следующем рисунке show **.NET Core** установлен, но вы можете выбрать **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="62df3-113">The following image show **.NET Core** selected, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="62df3-114">Выберите **изменить аутентификацию** и задайте **индивидуальные учетные записи**.</span><span class="sxs-lookup"><span data-stu-id="62df3-114">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="62df3-115">Оставьте значение по умолчанию **хранилище учетные записи в приложении**.</span><span class="sxs-lookup"><span data-stu-id="62df3-115">Keep the default **Store user accounts in-app**.</span></span>

![Отображение «Radio отдельных учетных записей пользователей» выбран диалоговое окно нового проекта](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62df3-117">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62df3-117">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="62df3-118">Учебник по требуется Visual Studio 2017 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="62df3-118">The tutorial requires Visual Studio 2017 or later.</span></span>

* <span data-ttu-id="62df3-119">В Visual Studio создайте новый проект веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="62df3-119">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="62df3-120">Выберите **изменить аутентификацию** и задайте **индивидуальные учетные записи**.</span><span class="sxs-lookup"><span data-stu-id="62df3-120">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>

![Отображение «Radio отдельных учетных записей пользователей» выбран диалоговое окно нового проекта](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a><span data-ttu-id="62df3-122">Создание проекта .NET core CLI для macOS и Linux</span><span class="sxs-lookup"><span data-stu-id="62df3-122">.NET Core CLI project creation for macOS and Linux</span></span>

<span data-ttu-id="62df3-123">Если вы используете CLI или SQLite, в окно командной строки выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="62df3-123">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="62df3-124">`--auth Individual`Задает шаблон отдельных учетных записей пользователей.</span><span class="sxs-lookup"><span data-stu-id="62df3-124">`--auth Individual` specifies the Individual User Accounts template.</span></span>
* <span data-ttu-id="62df3-125">В Windows, добавить `-uld` параметр.</span><span class="sxs-lookup"><span data-stu-id="62df3-125">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="62df3-126">`-uld` Параметр создает строку подключения LocalDB, а не базы данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="62df3-126">The `-uld` option creates a LocalDB connection string rather than a SQLite DB.</span></span>
* <span data-ttu-id="62df3-127">Запустите `new mvc --help` для получения справки по этой команде.</span><span class="sxs-lookup"><span data-stu-id="62df3-127">Run `new mvc --help` to get help on this command.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="62df3-128">Регистрация нового пользователя теста</span><span class="sxs-lookup"><span data-stu-id="62df3-128">Test new user registration</span></span>

<span data-ttu-id="62df3-129">Запустите приложение, выберите **зарегистрировать** ссылку и регистрации пользователя.</span><span class="sxs-lookup"><span data-stu-id="62df3-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="62df3-130">Следуйте инструкциям для выполнения миграции Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="62df3-130">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="62df3-131">На этом этапе является проверку только в электронном письме с [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) атрибута.</span><span class="sxs-lookup"><span data-stu-id="62df3-131">At this  point, the only validation on the email is with the [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="62df3-132">После отправки регистрации вы вошли в приложение.</span><span class="sxs-lookup"><span data-stu-id="62df3-132">After you submit the registration, you are logged into the app.</span></span> <span data-ttu-id="62df3-133">Далее в этом учебнике мы изменим это, не может входить новых пользователей до проверила электронной почте.</span><span class="sxs-lookup"><span data-stu-id="62df3-133">Later in the tutorial, we'll change this so new users cannot log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="62df3-134">Представление базы данных удостоверений</span><span class="sxs-lookup"><span data-stu-id="62df3-134">View the Identity database</span></span>

# <a name="sql-servertabsql-server"></a>[<span data-ttu-id="62df3-135">SQL Server</span><span class="sxs-lookup"><span data-stu-id="62df3-135">SQL Server</span></span>](#tab/sql-server)

* <span data-ttu-id="62df3-136">Из **представление** последовательно выберите пункты **обозреватель объектов SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="62df3-136">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span> 
* <span data-ttu-id="62df3-137">Перейдите к **(localdb) (SQL Server 13) MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="62df3-137">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="62df3-138">Щелкните правой кнопкой мыши **dbo. AspNetUsers** > **просмотра данных**:</span><span class="sxs-lookup"><span data-stu-id="62df3-138">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Контекстное меню таблицы AspNetUsers в обозревателе объектов SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="62df3-140">Примечание `EmailConfirmed` поле является `False`.</span><span class="sxs-lookup"><span data-stu-id="62df3-140">Note the `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="62df3-141">Можно использовать это сообщение электронной почты снова на следующем шаге, когда приложение отправляет сообщение с подтверждением.</span><span class="sxs-lookup"><span data-stu-id="62df3-141">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="62df3-142">Щелкните правой кнопкой мыши в строке и выберите **удалить**.</span><span class="sxs-lookup"><span data-stu-id="62df3-142">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="62df3-143">Удаление сообщения электронной почты теперь псевдоним облегчит в следующих шагах.</span><span class="sxs-lookup"><span data-stu-id="62df3-143">Deleting the email alias now will make it easier in the following steps.</span></span>

# <a name="sqlitetabsqlite"></a>[<span data-ttu-id="62df3-144">SQLite</span><span class="sxs-lookup"><span data-stu-id="62df3-144">SQLite</span></span>](#tab/sqlite)

<span data-ttu-id="62df3-145">В разделе [работа SQLite в проект ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) инструкции о том, как просмотреть базу данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="62df3-145">See [Working with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite DB.</span></span> 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a><span data-ttu-id="62df3-146">Обязательное использование SSL и настроить IIS Express для SSL</span><span class="sxs-lookup"><span data-stu-id="62df3-146">Require SSL and setup IIS Express for SSL</span></span>

<span data-ttu-id="62df3-147">В разделе [применения SSL](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="62df3-147">See [Enforcing SSL](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="62df3-148">Требовать подтверждения по электронной почте</span><span class="sxs-lookup"><span data-stu-id="62df3-148">Require email confirmation</span></span>

<span data-ttu-id="62df3-149">Рекомендуется для подтверждения адреса электронной почты Регистрация нового пользователя, чтобы проверить, они не использовании олицетворения кто-то другой (то есть они еще не зарегистрированы с помощью электронной почты другого пользователя).</span><span class="sxs-lookup"><span data-stu-id="62df3-149">It's a best practice to confirm the email of a new user registration to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="62df3-150">Предположим, что имеется дискуссионный форум и избежать "yli@example.com«от регистрации, как»nolivetto@contoso.com.»</span><span class="sxs-lookup"><span data-stu-id="62df3-150">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="62df3-151">Без подтверждения по электронной почте»nolivetto@contoso.com» удалось получить нежелательных электронной почты из приложения.</span><span class="sxs-lookup"><span data-stu-id="62df3-151">Without email confirmation, "nolivetto@contoso.com" could get unwanted email from your app.</span></span> <span data-ttu-id="62df3-152">Предположим, что пользователь случайно зарегистрированы как «ylo@example.com» и не заметили неверное написание из «yli», они не смогут использовать пароль восстановления, так как у приложения нет электронной почте правильно.</span><span class="sxs-lookup"><span data-stu-id="62df3-152">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli," they wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="62df3-153">Предоставляет ограниченную защиту от программы-роботы подтверждение по электронной почте и не обеспечивает защиту от определяется нежелательных сообщений, которые имеют много рабочей электронной почты псевдонимы, которые они могут использовать для регистрации.</span><span class="sxs-lookup"><span data-stu-id="62df3-153">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers who have many working email aliases they can use to register.</span></span>

<span data-ttu-id="62df3-154">Как правило, требуется запретить пользователям новых учетных данных для веб-узла до того, как они подтверждения по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="62df3-154">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span> 

<span data-ttu-id="62df3-155">Обновление `ConfigureServices` требовать подтверждения по электронной почте:</span><span class="sxs-lookup"><span data-stu-id="62df3-155">Update `ConfigureServices` to require a confirmed email:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62df3-156">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62df3-156">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62df3-157">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62df3-157">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
<span data-ttu-id="62df3-158">Предыдущая строка запрещает зарегистрированных регистрируются до подтверждения электронной почте.</span><span class="sxs-lookup"><span data-stu-id="62df3-158">The preceding line prevents registered users from being logged in until their email is confirmed.</span></span> <span data-ttu-id="62df3-159">Тем не менее эту строку не запрещает новые пользователи регистрируются после их регистрации.</span><span class="sxs-lookup"><span data-stu-id="62df3-159">However, that line doesn't prevent new users from being logged in after they register.</span></span> <span data-ttu-id="62df3-160">Код по умолчанию осуществляет вход пользователя после их регистрации.</span><span class="sxs-lookup"><span data-stu-id="62df3-160">The default code logs in a user after they register.</span></span> <span data-ttu-id="62df3-161">Как только они выйдите из системы, они будут снова войти в систему до регистрации.</span><span class="sxs-lookup"><span data-stu-id="62df3-161">Once they log out, they won't be able to log in again until they register.</span></span> <span data-ttu-id="62df3-162">Далее в этом учебнике мы изменим, поэтому заново зарегистрирован код пользователя **не** вход.</span><span class="sxs-lookup"><span data-stu-id="62df3-162">Later in the tutorial we'll change the code so newly registered user are **not** logged in.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="62df3-163">Настройка поставщика услуг электронной почты</span><span class="sxs-lookup"><span data-stu-id="62df3-163">Configure email provider</span></span>

<span data-ttu-id="62df3-164">В этом учебнике SendGrid используется для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="62df3-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="62df3-165">Требуется учетной записи SendGrid и ключ для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="62df3-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="62df3-166">Можно использовать другие поставщики электронной почты.</span><span class="sxs-lookup"><span data-stu-id="62df3-166">You can use other email providers.</span></span> <span data-ttu-id="62df3-167">Включает 2.x ASP.NET Core `System.Net.Mail`, позволяющий отправлять электронную почту из приложения.</span><span class="sxs-lookup"><span data-stu-id="62df3-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="62df3-168">Мы рекомендуем использовать SendGrid или другая служба электронной почты для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="62df3-168">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="62df3-169">Для защиты и правильно настроить сложно SMTP.</span><span class="sxs-lookup"><span data-stu-id="62df3-169">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="62df3-170">[Параметры шаблона](xref:fundamentals/configuration/options) используется для доступа к параметрам учетной записи и ключа пользователя.</span><span class="sxs-lookup"><span data-stu-id="62df3-170">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="62df3-171">Дополнительные сведения см. в разделе [конфигурации](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="62df3-171">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="62df3-172">Создание класса для получения ключа защиты электронной почты.</span><span class="sxs-lookup"><span data-stu-id="62df3-172">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="62df3-173">Для этого образца `AuthMessageSenderOptions` класса создается в *Services/AuthMessageSenderOptions.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="62df3-173">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="62df3-174">Задать `SendGridUser` и `SendGridKey` с [секрет диспетчера](../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="62df3-174">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](../app-secrets.md).</span></span> <span data-ttu-id="62df3-175">Пример:</span><span class="sxs-lookup"><span data-stu-id="62df3-175">For example:</span></span>

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="62df3-176">В Windows, диспетчер секрет хранит свои пары ключей и значений в *secrets.json* файл в каталоге %APPDATA%/Microsoft/UserSecrets/ < WebAppName userSecretsId >.</span><span class="sxs-lookup"><span data-stu-id="62df3-176">On Windows, Secret Manager stores your keys/value pairs in a *secrets.json* file in the %APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId> directory.</span></span>

<span data-ttu-id="62df3-177">Содержимое *secrets.json* файла не шифруются.</span><span class="sxs-lookup"><span data-stu-id="62df3-177">The contents of the *secrets.json* file are not encrypted.</span></span> <span data-ttu-id="62df3-178">*Secrets.json* ниже приведен файл ( `SendGridKey` значение был удален.)</span><span class="sxs-lookup"><span data-stu-id="62df3-178">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="62df3-179">Настройка запуска для использования AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="62df3-179">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="62df3-180">Добавить `AuthMessageSenderOptions` в контейнер службы, в конце `ConfigureServices` метод в *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="62df3-180">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62df3-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62df3-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62df3-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62df3-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="62df3-183">Настроить класс AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="62df3-183">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="62df3-184">Этот учебник демонстрирует добавление уведомления по электронной почте через [SendGrid](https://sendgrid.com/), однако вы можете отправлять электронную почту с помощью SMTP и другие механизмы.</span><span class="sxs-lookup"><span data-stu-id="62df3-184">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

* <span data-ttu-id="62df3-185">Установить `SendGrid` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="62df3-185">Install the `SendGrid` NuGet package.</span></span> <span data-ttu-id="62df3-186">Из консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="62df3-186">From the Package Manager Console,  enter the following the following command:</span></span>

  `Install-Package SendGrid`

* <span data-ttu-id="62df3-187">В разделе [начните бесплатно с помощью SendGrid](https://sendgrid.com/free/) для регистрации бесплатной учетной записи SendGrid.</span><span class="sxs-lookup"><span data-stu-id="62df3-187">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="62df3-188">Настройка SendGrid</span><span class="sxs-lookup"><span data-stu-id="62df3-188">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62df3-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62df3-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="62df3-190">Добавьте код в *Services/EmailSender.cs* аналогичный приведенному ниже, чтобы настроить SendGrid:</span><span class="sxs-lookup"><span data-stu-id="62df3-190">Add code in *Services/EmailSender.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62df3-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62df3-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
* <span data-ttu-id="62df3-192">Добавьте код в *Services/MessageServices.cs* аналогичный приведенному ниже, чтобы настроить SendGrid:</span><span class="sxs-lookup"><span data-stu-id="62df3-192">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="62df3-193">Включить учетную запись, пароль и Подтверждение восстановления</span><span class="sxs-lookup"><span data-stu-id="62df3-193">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="62df3-194">Шаблон содержит код для восстановления и подтверждение пароля учетной записи.</span><span class="sxs-lookup"><span data-stu-id="62df3-194">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="62df3-195">Найти `[HttpPost] Register` метод в *AccountController.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="62df3-195">Find the `[HttpPost] Register` method in the  *AccountController.cs* file.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62df3-196">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62df3-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="62df3-197">Запретить пользователям вновь зарегистрированного выполняется автоматический вход в систему, преобразуйте следующую строку:</span><span class="sxs-lookup"><span data-stu-id="62df3-197">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="62df3-198">С измененной выделенной строке показан полный метод.</span><span class="sxs-lookup"><span data-stu-id="62df3-198">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

<span data-ttu-id="62df3-199">Примечание: Предыдущий код завершится ошибкой, если вы реализуете `IEmailSender` и отправки текстового сообщения.</span><span class="sxs-lookup"><span data-stu-id="62df3-199">Note: The previous code will fail if you implement `IEmailSender` and send a plain text email.</span></span> <span data-ttu-id="62df3-200">В разделе [этой проблемы](https://github.com/aspnet/Home/issues/2152) Дополнительные сведения и решение.</span><span class="sxs-lookup"><span data-stu-id="62df3-200">See [this issue](https://github.com/aspnet/Home/issues/2152) for more information and a workaround.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62df3-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62df3-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="62df3-202">Раскомментируйте код, чтобы включить подтверждение учетной записи.</span><span class="sxs-lookup"><span data-stu-id="62df3-202">Uncomment the code to enable account confirmation.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="62df3-203">Примечание: Мы также запрет зарегистрированный пользователю выполняется автоматический вход в систему, преобразуйте следующую строку:</span><span class="sxs-lookup"><span data-stu-id="62df3-203">Note: We're also preventing a newly-registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="62df3-204">Включить восстановление пароля Раскомментировать код в `ForgotPassword` действия в *Controllers/AccountController.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="62df3-204">Enable password recovery by uncommenting the code in the `ForgotPassword` action in the *Controllers/AccountController.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="62df3-205">Раскомментируйте элемента form в *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="62df3-205">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="62df3-206">Может потребоваться удалить `<p> For more information on how to enable reset password ... </p>` элемент, который содержит ссылку на эту статью.</span><span class="sxs-lookup"><span data-stu-id="62df3-206">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element which contains a link to this article.</span></span>

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="62df3-207">Регистрация, подтверждение по электронной почте и сброс пароля</span><span class="sxs-lookup"><span data-stu-id="62df3-207">Register, confirm email, and reset password</span></span>

<span data-ttu-id="62df3-208">Запустите веб-приложения и тестов подтверждение учетной записи и пароль восстановления потока.</span><span class="sxs-lookup"><span data-stu-id="62df3-208">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="62df3-209">Запустите приложение и регистрации нового пользователя</span><span class="sxs-lookup"><span data-stu-id="62df3-209">Run the app and register a new user</span></span>

 ![Веб-приложения, зарегистрируйте учетную запись-представление](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="62df3-211">Проверьте свою почту ссылку для подтверждения учетной записи.</span><span class="sxs-lookup"><span data-stu-id="62df3-211">Check your email for the account confirmation link.</span></span> <span data-ttu-id="62df3-212">В разделе [Отладка электронной почты](#debug) Если вы не получаете сообщение электронной почты.</span><span class="sxs-lookup"><span data-stu-id="62df3-212">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="62df3-213">Щелкните ссылку, чтобы подтвердить ваш адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="62df3-213">Click the link to confirm your email.</span></span>
* <span data-ttu-id="62df3-214">Войдите в систему электронной почты и пароль.</span><span class="sxs-lookup"><span data-stu-id="62df3-214">Log in with your email and password.</span></span>
* <span data-ttu-id="62df3-215">Выйдите из системы.</span><span class="sxs-lookup"><span data-stu-id="62df3-215">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="62df3-216">Страница управления представления</span><span class="sxs-lookup"><span data-stu-id="62df3-216">View the manage page</span></span>

<span data-ttu-id="62df3-217">Выберите имя пользователя в браузере: ![окно браузера с именем пользователя](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="62df3-217">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="62df3-218">Может потребоваться развернуть панель навигации, чтобы увидеть имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="62df3-218">You might need to expand the navbar to see user name.</span></span>

![панель переходов](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62df3-220">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62df3-220">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="62df3-221">Страница управления отображается с **профиль** с выбранной вкладкой.</span><span class="sxs-lookup"><span data-stu-id="62df3-221">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="62df3-222">**Электронной почты** показывает подтвердила типа "флажок", указывающее, сообщения электронной почты.</span><span class="sxs-lookup"><span data-stu-id="62df3-222">The **Email** shows a check box indicating the email has been confirmed.</span></span> 

![страница «Управление»](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62df3-224">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62df3-224">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="62df3-225">Далее в этом учебнике говорится об этой странице.</span><span class="sxs-lookup"><span data-stu-id="62df3-225">We'll talk about this page later in the tutorial.</span></span>
<span data-ttu-id="62df3-226">![страница «Управление»](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="62df3-226">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="62df3-227">Сброс пароля для тестирования</span><span class="sxs-lookup"><span data-stu-id="62df3-227">Test password reset</span></span>

* <span data-ttu-id="62df3-228">Если вы выполнили вход, выберите **выхода**.</span><span class="sxs-lookup"><span data-stu-id="62df3-228">If you're logged in, select **Logout**.</span></span>  
* <span data-ttu-id="62df3-229">Выберите **входа** ссылку и выберите **забыли пароль?** ссылку.</span><span class="sxs-lookup"><span data-stu-id="62df3-229">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="62df3-230">Введите адрес электронной почты, который использовался для регистрации учетной записи.</span><span class="sxs-lookup"><span data-stu-id="62df3-230">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="62df3-231">Будет отправлено сообщение электронной почты со ссылкой для сброса пароля.</span><span class="sxs-lookup"><span data-stu-id="62df3-231">An email with a link to reset your password will be sent.</span></span> <span data-ttu-id="62df3-232">Проверьте свою почту и щелкните ссылку, чтобы сбросить пароль.</span><span class="sxs-lookup"><span data-stu-id="62df3-232">Check your email and click the link to reset your password.</span></span>  <span data-ttu-id="62df3-233">После успешного сброса пароля можно выполнить вход с помощью электронной почты и новый пароль.</span><span class="sxs-lookup"><span data-stu-id="62df3-233">After your password has been successfully reset, you can login with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="62df3-234">Отладка электронной почты</span><span class="sxs-lookup"><span data-stu-id="62df3-234">Debug email</span></span>

<span data-ttu-id="62df3-235">Если не удается получить рабочей электронной почты:</span><span class="sxs-lookup"><span data-stu-id="62df3-235">If you can't get email working:</span></span>

* <span data-ttu-id="62df3-236">Просмотрите [действия электронной почты](https://sendgrid.com/docs/User_Guide/email_activity.html) страницы.</span><span class="sxs-lookup"><span data-stu-id="62df3-236">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="62df3-237">Проверьте папку нежелательной почты.</span><span class="sxs-lookup"><span data-stu-id="62df3-237">Check your spam folder.</span></span>
* <span data-ttu-id="62df3-238">Попробуйте другой псевдоним электронной почты на другой поставщик (Microsoft, Yahoo, Gmail, и т. д.)</span><span class="sxs-lookup"><span data-stu-id="62df3-238">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="62df3-239">Создание [консольного приложения для отправки электронной почты](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="62df3-239">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="62df3-240">Повторите отправку учетным записям другое сообщение.</span><span class="sxs-lookup"><span data-stu-id="62df3-240">Try sending to different email accounts.</span></span>

<span data-ttu-id="62df3-241">**Примечание:** безопасности рекомендуется не использовать секретные данные производства в тестирования и разработки.</span><span class="sxs-lookup"><span data-stu-id="62df3-241">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="62df3-242">При публикации приложения в Azure, секреты SendGrid можно задать как параметры приложения на портале веб-приложения Azure.</span><span class="sxs-lookup"><span data-stu-id="62df3-242">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="62df3-243">Система конфигурации настроен прочитать ключи из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="62df3-243">The configuration system is setup to read keys from environment variables.</span></span>

## <a name="prevent-login-at-registration"></a><span data-ttu-id="62df3-244">Запретить имени входа при регистрации</span><span class="sxs-lookup"><span data-stu-id="62df3-244">Prevent login at registration</span></span>

<span data-ttu-id="62df3-245">С шаблонами текущего пользователя по завершении форме регистрации они вошли (проверку подлинности).</span><span class="sxs-lookup"><span data-stu-id="62df3-245">With the current templates, once a user completes the registration form, they're logged in (authenticated).</span></span> <span data-ttu-id="62df3-246">Как правило, требуется подтверждение перед ее регистрации электронной почте.</span><span class="sxs-lookup"><span data-stu-id="62df3-246">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="62df3-247">В следующем разделе мы изменить код, чтобы требовать новых пользователей имеют подтверждения по электронной почте, прежде чем они вошли.</span><span class="sxs-lookup"><span data-stu-id="62df3-247">In the section below, we will modify the code to require new users have a confirmed email before they're logged in.</span></span> <span data-ttu-id="62df3-248">Обновление `[HttpPost] Login` действия в *AccountController.cs* файл с выделенной следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="62df3-248">Update the `[HttpPost] Login` action in the *AccountController.cs* file with the following highlighted changes.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

<span data-ttu-id="62df3-249">**Примечание:** безопасности рекомендуется не использовать секретные данные производства в тестирования и разработки.</span><span class="sxs-lookup"><span data-stu-id="62df3-249">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="62df3-250">При публикации приложения в Azure, секреты SendGrid можно задать как параметры приложения на портале веб-приложения Azure.</span><span class="sxs-lookup"><span data-stu-id="62df3-250">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="62df3-251">Система конфигурации настроен прочитать ключи из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="62df3-251">The configuration system is setup to read keys from environment variables.</span></span>


## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="62df3-252">Объединение социальных сетей и локальных учетных записей</span><span class="sxs-lookup"><span data-stu-id="62df3-252">Combine social and local login accounts</span></span>

<span data-ttu-id="62df3-253">Примечание: Этот раздел относится только к ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="62df3-253">Note: This section applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="62df3-254">Для ASP.NET Core 2.x см. в разделе [это](https://github.com/aspnet/Docs/issues/3753) проблему.</span><span class="sxs-lookup"><span data-stu-id="62df3-254">For ASP.NET Core 2.x, see [this](https://github.com/aspnet/Docs/issues/3753) issue.</span></span>

<span data-ttu-id="62df3-255">Для выполнения в этом разделе, необходимо сначала включить внешнего поставщика аутентификации.</span><span class="sxs-lookup"><span data-stu-id="62df3-255">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="62df3-256">В разделе [Включение проверки подлинности с использованием Facebook, Google и других внешних поставщиков](social/index.md).</span><span class="sxs-lookup"><span data-stu-id="62df3-256">See [Enabling authentication using Facebook, Google and other external providers](social/index.md).</span></span>

<span data-ttu-id="62df3-257">Можно объединять учетные записи локальных и социальных сетей, щелкнув ссылку по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="62df3-257">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="62df3-258">В следующей последовательности «RickAndMSFT@gmail.com» сначала создается как локальное имя входа; тем не менее, можно сначала создать учетную запись входа на социальных сетей, а затем добавить локальный вход.</span><span class="sxs-lookup"><span data-stu-id="62df3-258">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Веб-приложения: RickAndMSFT@gmail.com пользователь прошел проверку подлинности](accconfirm/_static/rick.png)

<span data-ttu-id="62df3-260">Щелкните **управление** ссылку.</span><span class="sxs-lookup"><span data-stu-id="62df3-260">Click on the **Manage** link.</span></span> <span data-ttu-id="62df3-261">Примечание внешних 0 (социальных имена входа), связанные с этой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="62df3-261">Note the 0 external (social logins) associated with this account.</span></span>

![Управление представления](accconfirm/_static/manage.png)

<span data-ttu-id="62df3-263">Щелкните ссылку на другую службу входа в систему и принимать запросы приложений.</span><span class="sxs-lookup"><span data-stu-id="62df3-263">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="62df3-264">В приведенном ниже рисунке Facebook — внешнего поставщика проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="62df3-264">In the image below, Facebook is the external authentication provider:</span></span>

![Управление листинг Facebook Просмотр внешних имен входа](accconfirm/_static/fb.png)

<span data-ttu-id="62df3-266">Объединены две учетные записи.</span><span class="sxs-lookup"><span data-stu-id="62df3-266">The two accounts have been combined.</span></span> <span data-ttu-id="62df3-267">Можно войти в систему с любой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="62df3-267">You will be able to log on with either account.</span></span> <span data-ttu-id="62df3-268">Может потребоваться пользователям добавлять локальные учетные записи, в случае их социальных вход службы проверки подлинности не работает, или что более вероятно, они потеряли доступ к учетной записи социальных сетей.</span><span class="sxs-lookup"><span data-stu-id="62df3-268">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they've lost access to their social account.</span></span>
