---
title: Подтверждение учетной записи и восстановление пароля в ASP.NET Core
author: rick-anderson
description: Сведения о создании приложения ASP.NET Core с помощью по электронной почте подтверждение и сброс пароля.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 12265903f60ff6d62befc445434db025c244c178
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803276"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="7c07f-103">Подтверждение учетной записи и восстановление пароля в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7c07f-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="7c07f-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)</span><span class="sxs-lookup"><span data-stu-id="7c07f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="7c07f-105">Этом руководстве показано, как создавать приложения ASP.NET Core с помощью по электронной почте подтверждение и сброс пароля.</span><span class="sxs-lookup"><span data-stu-id="7c07f-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="7c07f-106">Это руководство представляет собой **не** начало раздела.</span><span class="sxs-lookup"><span data-stu-id="7c07f-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="7c07f-107">Вы должны быть знакомы с:</span><span class="sxs-lookup"><span data-stu-id="7c07f-107">You should be familiar with:</span></span>

* [<span data-ttu-id="7c07f-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7c07f-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="7c07f-109">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="7c07f-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="7c07f-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="7c07f-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="7c07f-111">См. в разделе [этот PDF-файл](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) для версий ASP.NET Core MVC 1.1 и 2.x.</span><span class="sxs-lookup"><span data-stu-id="7c07f-111">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c07f-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="7c07f-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="7c07f-113">Создайте новый проект ASP.NET Core и .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7c07f-113">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7c07f-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7c07f-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* <span data-ttu-id="7c07f-115">`--auth Individual` Указывает шаблон проекта учетные записи отдельных пользователей.</span><span class="sxs-lookup"><span data-stu-id="7c07f-115">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="7c07f-116">На Windows, добавьте `-uld` параметр.</span><span class="sxs-lookup"><span data-stu-id="7c07f-116">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="7c07f-117">Он указывает, что следует использовать LocalDB вместо SQLite.</span><span class="sxs-lookup"><span data-stu-id="7c07f-117">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="7c07f-118">Запустите `new mvc --help` для получения справки по этой команде.</span><span class="sxs-lookup"><span data-stu-id="7c07f-118">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7c07f-119">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7c07f-119">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7c07f-120">При использовании интерфейса командной строки или SQLite, в окне командной строки выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="7c07f-120">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="7c07f-121">`--auth Individual` Указывает шаблон проекта учетные записи отдельных пользователей.</span><span class="sxs-lookup"><span data-stu-id="7c07f-121">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="7c07f-122">На Windows, добавьте `-uld` параметр.</span><span class="sxs-lookup"><span data-stu-id="7c07f-122">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="7c07f-123">Он указывает, что следует использовать LocalDB вместо SQLite.</span><span class="sxs-lookup"><span data-stu-id="7c07f-123">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="7c07f-124">Запустите `new mvc --help` для получения справки по этой команде.</span><span class="sxs-lookup"><span data-stu-id="7c07f-124">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="7c07f-125">Кроме того можно создать новый проект ASP.NET Core с помощью Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="7c07f-125">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="7c07f-126">В Visual Studio создайте новое **веб-приложение** проекта.</span><span class="sxs-lookup"><span data-stu-id="7c07f-126">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="7c07f-127">Выберите **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="7c07f-127">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="7c07f-128">**.NET core** выбран на следующем рисунке, но вы можете выбрать **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="7c07f-128">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="7c07f-129">Выберите **изменить способ проверки подлинности** и присвоено **учетные записи отдельных пользователей**.</span><span class="sxs-lookup"><span data-stu-id="7c07f-129">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="7c07f-130">Сохраните значение по умолчанию **Store учетных записей пользователей в приложении**.</span><span class="sxs-lookup"><span data-stu-id="7c07f-130">Keep the default **Store user accounts in-app**.</span></span>

![Отображение «Учетные записи отдельных пользователей radio» выбран диалоговое окно нового проекта](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="7c07f-132">Регистрация нового пользователя для тестирования</span><span class="sxs-lookup"><span data-stu-id="7c07f-132">Test new user registration</span></span>

<span data-ttu-id="7c07f-133">Запустите приложение, выберите **зарегистрировать** связать и регистрации пользователя.</span><span class="sxs-lookup"><span data-stu-id="7c07f-133">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="7c07f-134">Следуйте инструкциям для выполнения миграции Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7c07f-134">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="7c07f-135">На этом этапе является только проверка на адрес электронной почты с [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) атрибута.</span><span class="sxs-lookup"><span data-stu-id="7c07f-135">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="7c07f-136">После отправки регистрации, вы вошли в приложение.</span><span class="sxs-lookup"><span data-stu-id="7c07f-136">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="7c07f-137">Далее в этом руководстве код обновляется, поэтому новые пользователи не могут войти, пока не проверен доступ к электронной почте.</span><span class="sxs-lookup"><span data-stu-id="7c07f-137">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="7c07f-138">Представление базы данных удостоверений</span><span class="sxs-lookup"><span data-stu-id="7c07f-138">View the Identity database</span></span>

<span data-ttu-id="7c07f-139">См. в разделе [работа с SQLite в проекте ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) инструкции о том, как просмотреть базу данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="7c07f-139">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="7c07f-140">Для Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="7c07f-140">For Visual Studio:</span></span>

* <span data-ttu-id="7c07f-141">Из **представление** меню, выберите **обозреватель объектов SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="7c07f-141">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="7c07f-142">Перейдите к **(localdb) (SQL Server 13) MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="7c07f-142">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="7c07f-143">Щелкните правой кнопкой мыши **dbo. AspNetUsers** > **просмотра данных**:</span><span class="sxs-lookup"><span data-stu-id="7c07f-143">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Контекстные меню в таблице AspNetUsers в обозревателе объектов SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="7c07f-145">Обратите внимание, таблицы `EmailConfirmed` поле является `False`.</span><span class="sxs-lookup"><span data-stu-id="7c07f-145">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="7c07f-146">Вы можете использовать этот адрес электронной почты снова на следующем шаге, когда приложение отправляет сообщение электронной почты с подтверждением.</span><span class="sxs-lookup"><span data-stu-id="7c07f-146">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="7c07f-147">Щелкните правой кнопкой мыши в строке и выберите **удалить**.</span><span class="sxs-lookup"><span data-stu-id="7c07f-147">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="7c07f-148">Удаление псевдонима электронной почты упрощает в следующих шагах.</span><span class="sxs-lookup"><span data-stu-id="7c07f-148">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="7c07f-149">Требование HTTPS</span><span class="sxs-lookup"><span data-stu-id="7c07f-149">Require HTTPS</span></span>

<span data-ttu-id="7c07f-150">См. в разделе [обязательного использования протокола HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="7c07f-150">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="7c07f-151">Требуется подтверждение по электронной почте</span><span class="sxs-lookup"><span data-stu-id="7c07f-151">Require email confirmation</span></span>

<span data-ttu-id="7c07f-152">Рекомендуется подтвердить адрес электронной почты новой регистрации пользователя.</span><span class="sxs-lookup"><span data-stu-id="7c07f-152">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="7c07f-153">Отправить по электронной почте подтверждение помогает проверить, они не олицетворения кто-то другой (то есть они еще не зарегистрировано другого пользователя по электронной почте).</span><span class="sxs-lookup"><span data-stu-id="7c07f-153">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="7c07f-154">Предположим, что имеется дискуссионный форум и избежать "yli@example.com«из регистрации в качестве»nolivetto@contoso.com«.</span><span class="sxs-lookup"><span data-stu-id="7c07f-154">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="7c07f-155">Без подтверждение по электронной почте "nolivetto@contoso.com" могли получать нежелательные сообщения электронной почты из вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="7c07f-155">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="7c07f-156">Предположим, что пользователь случайно зарегистрирован как "ylo@example.com" и не заметили опечатку «yli».</span><span class="sxs-lookup"><span data-stu-id="7c07f-156">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="7c07f-157">Они не смогут использовать пароль восстановления, так как у приложения нет их правильный адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="7c07f-157">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="7c07f-158">Подтверждение по электронной почте предоставляет ограниченную защиту от программ-роботов.</span><span class="sxs-lookup"><span data-stu-id="7c07f-158">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="7c07f-159">Подтверждение по электронной почте не обеспечивают защиту от пользователей-злоумышленников с многих учетных записей электронной почты.</span><span class="sxs-lookup"><span data-stu-id="7c07f-159">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="7c07f-160">Обычно требуется запретить новым пользователям из учета данных для веб-сайт, прежде чем они получат подтвержден по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="7c07f-160">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="7c07f-161">Обновление `ConfigureServices` требуется подтвержден по электронной почте:</span><span class="sxs-lookup"><span data-stu-id="7c07f-161">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="7c07f-162">`config.SignIn.RequireConfirmedEmail = true;` запрещает вход до подтверждения их по электронной почте зарегистрированным пользователям.</span><span class="sxs-lookup"><span data-stu-id="7c07f-162">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="7c07f-163">Настройка поставщика услуг электронной почты</span><span class="sxs-lookup"><span data-stu-id="7c07f-163">Configure email provider</span></span>

<span data-ttu-id="7c07f-164">В этом руководстве SendGrid используется для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="7c07f-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="7c07f-165">Требуется учетная запись SendGrid и ключ для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="7c07f-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="7c07f-166">Можно использовать другие поставщики электронной почты.</span><span class="sxs-lookup"><span data-stu-id="7c07f-166">You can use other email providers.</span></span> <span data-ttu-id="7c07f-167">ASP.NET Core 2.x включает `System.Net.Mail`, который позволяет отправлять электронную почту из приложения.</span><span class="sxs-lookup"><span data-stu-id="7c07f-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="7c07f-168">Мы рекомендуем использовать SendGrid или другая служба электронной почты для отправки электронной почты.</span><span class="sxs-lookup"><span data-stu-id="7c07f-168">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="7c07f-169">Для защиты и правильно настроить сложно SMTP.</span><span class="sxs-lookup"><span data-stu-id="7c07f-169">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="7c07f-170">[Шаблон параметров](xref:fundamentals/configuration/options) используется для доступа к параметрам и ключ учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="7c07f-170">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="7c07f-171">Дополнительные сведения см. в разделе [конфигурации](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="7c07f-171">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="7c07f-172">Создание класса для извлечения ключа защиты электронной почты.</span><span class="sxs-lookup"><span data-stu-id="7c07f-172">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="7c07f-173">В этом примере `AuthMessageSenderOptions` класс создается в *Services/AuthMessageSenderOptions.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="7c07f-173">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="7c07f-174">Задайте `SendGridUser` и `SendGridKey` с [средство secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7c07f-174">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7c07f-175">Пример:</span><span class="sxs-lookup"><span data-stu-id="7c07f-175">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="7c07f-176">На Windows, Secret Manager хранит пары ключей и значений в *secrets.json* файл `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` каталог.</span><span class="sxs-lookup"><span data-stu-id="7c07f-176">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="7c07f-177">Содержание *secrets.json* файл не зашифрован.</span><span class="sxs-lookup"><span data-stu-id="7c07f-177">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="7c07f-178">*Secrets.json* ниже приведен файл ( `SendGridKey` значение удаляется.)</span><span class="sxs-lookup"><span data-stu-id="7c07f-178">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="7c07f-179">Настройка запуска для использования AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="7c07f-179">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="7c07f-180">Добавить `AuthMessageSenderOptions` в контейнер службы, в конце `ConfigureServices` метод в *Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="7c07f-180">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7c07f-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7c07f-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7c07f-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7c07f-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="7c07f-183">Настроить класс AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="7c07f-183">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="7c07f-184">Этот учебник демонстрирует добавление уведомлений по электронной почте через [SendGrid](https://sendgrid.com/), но вы можете отправлять электронную почту с помощью SMTP и другие механизмы.</span><span class="sxs-lookup"><span data-stu-id="7c07f-184">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="7c07f-185">Установить `SendGrid` пакет NuGet:</span><span class="sxs-lookup"><span data-stu-id="7c07f-185">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="7c07f-186">Из командной строки:</span><span class="sxs-lookup"><span data-stu-id="7c07f-186">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="7c07f-187">В консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="7c07f-187">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="7c07f-188">См. в разделе [приступить к работе с SendGrid бесплатно](https://sendgrid.com/free/) зарегистрироваться для получения бесплатной учетной записи SendGrid.</span><span class="sxs-lookup"><span data-stu-id="7c07f-188">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="7c07f-189">Настройка SendGrid</span><span class="sxs-lookup"><span data-stu-id="7c07f-189">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7c07f-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7c07f-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="7c07f-191">Чтобы настроить SendGrid, добавьте код, аналогичный следующему *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="7c07f-191">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7c07f-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7c07f-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="7c07f-193">Добавьте код в *Services/MessageServices.cs* Настройка SendGrid следующего вида:</span><span class="sxs-lookup"><span data-stu-id="7c07f-193">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="7c07f-194">Включить учетную запись, пароль и Подтверждение восстановления</span><span class="sxs-lookup"><span data-stu-id="7c07f-194">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="7c07f-195">Шаблон содержит код для восстановления и подтверждение пароля учетной записи.</span><span class="sxs-lookup"><span data-stu-id="7c07f-195">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="7c07f-196">Найти `OnPostAsync` метод в *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="7c07f-196">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7c07f-197">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7c07f-197">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="7c07f-198">Запретить пользователям только что зарегистрированное автоматически входит в систему, закомментируйте следующую строку:</span><span class="sxs-lookup"><span data-stu-id="7c07f-198">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="7c07f-199">С помощью измененные строки выделены показан полный метод:</span><span class="sxs-lookup"><span data-stu-id="7c07f-199">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7c07f-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7c07f-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="7c07f-201">Чтобы включить подтверждение учетной записи, раскомментируйте следующий код:</span><span class="sxs-lookup"><span data-stu-id="7c07f-201">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="7c07f-202">**Примечание:** код предотвращает вновь зарегистрированного пользователя автоматически входит в систему, закомментируйте следующую строку:</span><span class="sxs-lookup"><span data-stu-id="7c07f-202">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="7c07f-203">Включить восстановление пароля Раскомментировать код в `ForgotPassword` действие *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="7c07f-203">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="7c07f-204">Раскомментируйте элемент формы в *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7c07f-204">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="7c07f-205">Может потребоваться удалить `<p> For more information on how to enable reset password ... </p>` элемент, который содержится ссылка в этой статье.</span><span class="sxs-lookup"><span data-stu-id="7c07f-205">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="7c07f-206">Регистрация, подтверждение электронной почты и сброс пароля</span><span class="sxs-lookup"><span data-stu-id="7c07f-206">Register, confirm email, and reset password</span></span>

<span data-ttu-id="7c07f-207">Запустите веб-приложение и протестируйте подтверждение учетной записи и процесс восстановления пароля.</span><span class="sxs-lookup"><span data-stu-id="7c07f-207">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="7c07f-208">Запустите приложение и зарегистрируйте нового пользователя</span><span class="sxs-lookup"><span data-stu-id="7c07f-208">Run the app and register a new user</span></span>

  ![Веб-приложения, зарегистрируйте учетную запись-представление](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="7c07f-210">Проверьте свой адрес электронной почты для ссылку для подтверждения учетной записи.</span><span class="sxs-lookup"><span data-stu-id="7c07f-210">Check your email for the account confirmation link.</span></span> <span data-ttu-id="7c07f-211">См. в разделе [отладки электронной почты](#debug) Если вы не получили сообщение электронной почты.</span><span class="sxs-lookup"><span data-stu-id="7c07f-211">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="7c07f-212">Щелкните ссылку, чтобы подтвердить свой адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="7c07f-212">Click the link to confirm your email.</span></span>
* <span data-ttu-id="7c07f-213">Войдите с помощью электронной почты и пароль.</span><span class="sxs-lookup"><span data-stu-id="7c07f-213">Log in with your email and password.</span></span>
* <span data-ttu-id="7c07f-214">Выйдите из системы.</span><span class="sxs-lookup"><span data-stu-id="7c07f-214">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="7c07f-215">Просмотреть страницу управление</span><span class="sxs-lookup"><span data-stu-id="7c07f-215">View the manage page</span></span>

<span data-ttu-id="7c07f-216">Выберите свое имя пользователя в браузере: ![окно браузера с именем пользователя](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="7c07f-216">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="7c07f-217">Может потребоваться развернуть панель навигации, чтобы увидеть имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="7c07f-217">You might need to expand the navbar to see user name.</span></span>

![панель переходов](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7c07f-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7c07f-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7c07f-220">Управление страница отображается с **профиль** выделенной вкладки.</span><span class="sxs-lookup"><span data-stu-id="7c07f-220">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="7c07f-221">**Электронной почты** показано типа "флажок", указывающее, адрес электронной почты будет подтверждена.</span><span class="sxs-lookup"><span data-stu-id="7c07f-221">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![страница «Управление»](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7c07f-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7c07f-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7c07f-224">Это упоминается позже в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="7c07f-224">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="7c07f-225">![страница «Управление»](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="7c07f-225">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="7c07f-226">Сброс пароля теста</span><span class="sxs-lookup"><span data-stu-id="7c07f-226">Test password reset</span></span>

* <span data-ttu-id="7c07f-227">Если вы выполнили вход в, выберите **выхода**.</span><span class="sxs-lookup"><span data-stu-id="7c07f-227">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="7c07f-228">Выберите **вход** ссылку и выберите **забыли пароль?** ссылку.</span><span class="sxs-lookup"><span data-stu-id="7c07f-228">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="7c07f-229">Введите адрес электронной почты, которая использовалась для регистрации учетной записи.</span><span class="sxs-lookup"><span data-stu-id="7c07f-229">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="7c07f-230">Отправляется сообщение электронной почты со ссылкой для сброса пароля.</span><span class="sxs-lookup"><span data-stu-id="7c07f-230">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="7c07f-231">Проверьте свой адрес электронной почты и щелкните ссылку, чтобы сбросить пароль.</span><span class="sxs-lookup"><span data-stu-id="7c07f-231">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="7c07f-232">После успешного сброса пароля вы можете войти с использованием электронной почты и новый пароль.</span><span class="sxs-lookup"><span data-stu-id="7c07f-232">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="7c07f-233">Отладка по электронной почте</span><span class="sxs-lookup"><span data-stu-id="7c07f-233">Debug email</span></span>

<span data-ttu-id="7c07f-234">Если не удается получить рабочий адрес электронной почты:</span><span class="sxs-lookup"><span data-stu-id="7c07f-234">If you can't get email working:</span></span>

* <span data-ttu-id="7c07f-235">Создание [консольное приложение для отправки электронной почты](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="7c07f-235">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="7c07f-236">Просмотрите [действия "сообщения"](https://sendgrid.com/docs/User_Guide/email_activity.html) страницы.</span><span class="sxs-lookup"><span data-stu-id="7c07f-236">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="7c07f-237">Проверьте папку нежелательной почты.</span><span class="sxs-lookup"><span data-stu-id="7c07f-237">Check your spam folder.</span></span>
* <span data-ttu-id="7c07f-238">Попробуйте другой псевдоним электронной почты на другой поставщик электронной почты (Microsoft, Yahoo, Gmail, и т.д.)</span><span class="sxs-lookup"><span data-stu-id="7c07f-238">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="7c07f-239">Попробуйте отправить учетными записями электронной почты.</span><span class="sxs-lookup"><span data-stu-id="7c07f-239">Try sending to different email accounts.</span></span>

<span data-ttu-id="7c07f-240">**Рекомендации по безопасности** — **не** используйте секреты производства в разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="7c07f-240">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="7c07f-241">При публикации приложения в Azure, секреты SendGrid можно задать как параметры приложения на портале веб-приложения Azure.</span><span class="sxs-lookup"><span data-stu-id="7c07f-241">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="7c07f-242">Система конфигурации предназначена для чтения разделов из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="7c07f-242">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="7c07f-243">Объединить учетные записи социальных сетей и локального имени входа</span><span class="sxs-lookup"><span data-stu-id="7c07f-243">Combine social and local login accounts</span></span>

<span data-ttu-id="7c07f-244">Для завершения этого раздела необходимо сначала включить внешний поставщик аутентификации.</span><span class="sxs-lookup"><span data-stu-id="7c07f-244">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="7c07f-245">См. в разделе [Facebook, Google и внешнего поставщика проверки подлинности](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="7c07f-245">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="7c07f-246">Можно объединить учетные записи локальных и социальных сетей, щелкнув ссылку по электронной почте.</span><span class="sxs-lookup"><span data-stu-id="7c07f-246">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="7c07f-247">В следующей последовательности «RickAndMSFT@gmail.com"сначала создается как локальное имя входа; тем не менее, можно сначала создать учетную запись как имя для входа социальных сетей, а затем добавить локальное имя входа.</span><span class="sxs-lookup"><span data-stu-id="7c07f-247">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Веб-приложения: RickAndMSFT@gmail.com пользователь прошел проверку подлинности](accconfirm/_static/rick.png)

<span data-ttu-id="7c07f-249">Щелкните **управление** ссылку.</span><span class="sxs-lookup"><span data-stu-id="7c07f-249">Click on the **Manage** link.</span></span> <span data-ttu-id="7c07f-250">Примечание внешний 0 (имена входа социальных сетей), связанное с этой учетной записи.</span><span class="sxs-lookup"><span data-stu-id="7c07f-250">Note the 0 external (social logins) associated with this account.</span></span>

![Управления представлением](accconfirm/_static/manage.png)

<span data-ttu-id="7c07f-252">Щелкните ссылку на другую службу входа в систему и принимать запросы приложений.</span><span class="sxs-lookup"><span data-stu-id="7c07f-252">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="7c07f-253">На следующем рисунке Facebook, — это внешнего поставщика проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="7c07f-253">In the following image, Facebook is the external authentication provider:</span></span>

![Управление ваш список внешних имен входа Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="7c07f-255">Две учетные записи были объединены.</span><span class="sxs-lookup"><span data-stu-id="7c07f-255">The two accounts have been combined.</span></span> <span data-ttu-id="7c07f-256">Вы сможете выполнить вход с использованием либо учетной записи.</span><span class="sxs-lookup"><span data-stu-id="7c07f-256">You are able to log on with either account.</span></span> <span data-ttu-id="7c07f-257">Вы можете добавить локальные учетные записи, в случае их учетные записи социальных сетей службы проверки подлинности не работает, или более вероятно, они потеряли доступ к своей учетной записи социальной сети пользователей.</span><span class="sxs-lookup"><span data-stu-id="7c07f-257">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="7c07f-258">Включить подтверждение учетной записи, после узла имеет пользователей</span><span class="sxs-lookup"><span data-stu-id="7c07f-258">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="7c07f-259">Включение подтверждение учетной записи на сайте с пользователями блокирует работу всех существующих пользователей.</span><span class="sxs-lookup"><span data-stu-id="7c07f-259">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="7c07f-260">Существующие пользователи заблокированы, так как учетные записи не подтверждены.</span><span class="sxs-lookup"><span data-stu-id="7c07f-260">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="7c07f-261">Чтобы обойти существующие блокировки пользователя, используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="7c07f-261">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="7c07f-262">Обновите базу данных для пометки всех существующих пользователей в качестве подтверждения.</span><span class="sxs-lookup"><span data-stu-id="7c07f-262">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="7c07f-263">Подтвердите существующих пользователей.</span><span class="sxs-lookup"><span data-stu-id="7c07f-263">Confirm exiting users.</span></span> <span data-ttu-id="7c07f-264">Например пакетная Отправка сообщения электронной почты с подтверждением ссылки.</span><span class="sxs-lookup"><span data-stu-id="7c07f-264">For example, batch-send emails with confirmation links.</span></span>
