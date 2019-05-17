---
title: Проверка подлинности Facebook, Google и внешних поставщиков в ASP.NET Core
author: rick-anderson
description: В этом руководстве демонстрируется построение приложения ASP.NET Core 2.x с использованием OAuth 2.0 с внешними поставщиками проверки подлинности.
ms.author: riande
ms.custom: mvc
ms.date: 4/19/2019
uid: security/authentication/social/index
ms.openlocfilehash: e2d68ac93bdcfa2fc015e8447ea38626787cdb02
ms.sourcegitcommit: a3926eae3f687013027a2828830c12a89add701f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2019
ms.locfileid: "65451042"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="bafc1-103">Проверка подлинности Facebook, Google и внешних поставщиков в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bafc1-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="bafc1-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="bafc1-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bafc1-105">В этом руководстве показано создание приложения ASP.NET Core 2.2, позволяющего пользователям выполнять вход с помощью OAuth 2.0 и учетных данных от внешних поставщиков проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="bafc1-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="bafc1-106">В следующих разделах рассматриваются такие поставщики, как [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), и [Microsoft](xref:security/authentication/microsoft-logins).</span><span class="sxs-lookup"><span data-stu-id="bafc1-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="bafc1-107">В сторонних пакетах также доступны другие поставщики, такие как [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) и [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="bafc1-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Значки социальных сетей для Facebook, Twitter, Google+ и Windows](index/_static/social.png)

<span data-ttu-id="bafc1-109">Позволяет пользователям входить в систему с имеющимися учетными данными:</span><span class="sxs-lookup"><span data-stu-id="bafc1-109">Enabling users to sign in with their existing credentials:</span></span>
* <span data-ttu-id="bafc1-110">Удобно для пользователей.</span><span class="sxs-lookup"><span data-stu-id="bafc1-110">Is convenient for the users.</span></span>
* <span data-ttu-id="bafc1-111">Многие задачи, связанные с управлением процессом входа, передаются сторонним производителям.</span><span class="sxs-lookup"><span data-stu-id="bafc1-111">Shifts many of the complexities of managing the sign-in process onto a third party.</span></span> 

<span data-ttu-id="bafc1-112">Демонстрацию того, как вход с использованием учетных данных социальных сетей помогает повысить трафик и количество конверсий, см. в примерах для [Facebook](https://www.facebook.com/unsupportedbrowser) и [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="bafc1-112">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="bafc1-113">Создание проекта ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bafc1-113">Create a New ASP.NET Core Project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bafc1-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bafc1-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bafc1-115">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="bafc1-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="bafc1-116">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bafc1-116">Create a new ASP.NET Core Web Application.</span></span>
* <span data-ttu-id="bafc1-117">Выберите в раскрывающемся списке **ASP.NET Core 2.2**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="bafc1-117">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>
* <span data-ttu-id="bafc1-118">Выберите **Изменить проверку подлинности** и задайте способ **Учетные записи отдельных пользователей**.</span><span class="sxs-lookup"><span data-stu-id="bafc1-118">Select **Change Authentication** and set authentication to **Individual User Accounts**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bafc1-119">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="bafc1-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="bafc1-120">Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="bafc1-120">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="bafc1-121">Смените каталог (`cd`) на папку, в которой будет содержаться проект.</span><span class="sxs-lookup"><span data-stu-id="bafc1-121">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="bafc1-122">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="bafc1-122">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o WebApp1 -au Individual -uld
  code -r WebApp1
  ```

  * <span data-ttu-id="bafc1-123">Команда `dotnet new` создает новый проект Razor Pages в папке *WebApp1*.</span><span class="sxs-lookup"><span data-stu-id="bafc1-123">The `dotnet new` command creates a new Razor Pages project in the *WebApp1* folder.</span></span>
  * <span data-ttu-id="bafc1-124">`-uld` использует LocalDB вместо SQLite.</span><span class="sxs-lookup"><span data-stu-id="bafc1-124">`-uld` uses LocalDB instead of SQLite.</span></span> <span data-ttu-id="bafc1-125">Не указывайте `-uld`, чтобы использовать SQLite.</span><span class="sxs-lookup"><span data-stu-id="bafc1-125">Omit `-uld` to use SQLite.</span></span>
  * <span data-ttu-id="bafc1-126">`-au Individual` создает код для отдельной проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="bafc1-126">`-au Individual` creates the code for Individual authentication.</span></span>
  * <span data-ttu-id="bafc1-127">Команда `code` открывает папку *WebApp1* в новом экземпляре Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="bafc1-127">The `code` command opens the *WebApp1* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="bafc1-128">Появится диалоговое окно с предупреждением **В WebApp1 отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**</span><span class="sxs-lookup"><span data-stu-id="bafc1-128">A dialog box appears with **Required assets to build and debug are missing from 'WebApp1'. Add them?**</span></span>

* <span data-ttu-id="bafc1-129">Выберите ответ **Да**.</span><span class="sxs-lookup"><span data-stu-id="bafc1-129">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bafc1-130">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="bafc1-130">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="bafc1-131">В терминале выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="bafc1-131">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o WebApp1 -au Individual
```

<span data-ttu-id="bafc1-132">Указанные выше команды используют [интерфейс командной строки .NET Core](/dotnet/core/tools/dotnet) для создания проекта Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="bafc1-132">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="bafc1-133">Открытие проекта</span><span class="sxs-lookup"><span data-stu-id="bafc1-133">Open the project</span></span>

<span data-ttu-id="bafc1-134">В Visual Studio откройте меню **Файл > Открыть** и выберите файл *WebApp1.csproj*.</span><span class="sxs-lookup"><span data-stu-id="bafc1-134">From Visual Studio, select **File > Open**, and then select the *WebApp1.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="apply-migrations"></a><span data-ttu-id="bafc1-135">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="bafc1-135">Apply migrations</span></span>

* <span data-ttu-id="bafc1-136">Запустите приложение и щелкните ссылку **Регистрация**.</span><span class="sxs-lookup"><span data-stu-id="bafc1-136">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="bafc1-137">Введите адрес электронной почты и пароль для новой учетной записи, а затем щелкните **Зарегистрироваться**.</span><span class="sxs-lookup"><span data-stu-id="bafc1-137">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="bafc1-138">Следуйте инструкциям по применению миграции.</span><span class="sxs-lookup"><span data-stu-id="bafc1-138">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="bafc1-139">Хранение маркеров безопасности, предоставленных поставщиками входа, с помощью SecretManager</span><span class="sxs-lookup"><span data-stu-id="bafc1-139">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="bafc1-140">Поставщики входа социальных сетей назначают маркеры **идентификатора приложения** и **секрета приложения** в процессе регистрации.</span><span class="sxs-lookup"><span data-stu-id="bafc1-140">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="bafc1-141">Точные имена маркеров зависят от поставщика.</span><span class="sxs-lookup"><span data-stu-id="bafc1-141">The exact token names vary by provider.</span></span> <span data-ttu-id="bafc1-142">Эти маркеры соответствуют учетным данным, которые используются приложением для доступа к API.</span><span class="sxs-lookup"><span data-stu-id="bafc1-142">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="bafc1-143">Маркеры предоставляют "секреты", которые можно подключить к конфигурации приложения с помощью [Secret Manager](xref:security/app-secrets#secret-manager) (Диспетчера секретов).</span><span class="sxs-lookup"><span data-stu-id="bafc1-143">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="bafc1-144">Secret Manager — более безопасная альтернатива хранению маркеров в файле конфигурации, например в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bafc1-144">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bafc1-145">Secret Manager предназначен только для разработки.</span><span class="sxs-lookup"><span data-stu-id="bafc1-145">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="bafc1-146">Для хранения и защиты секретов Azure в ходе тестирования и непосредственной работы используйте [Поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="bafc1-146">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="bafc1-147">В разделе [Безопасное хранение секретов приложений во время разработки в ASP.NET Core](xref:security/app-secrets) описано, как хранить маркеры, назначаемые приведенными ниже поставщиками входа.</span><span class="sxs-lookup"><span data-stu-id="bafc1-147">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="bafc1-148">Настройка поставщиков входа, используемых приложением</span><span class="sxs-lookup"><span data-stu-id="bafc1-148">Setup login providers required by your application</span></span>

<span data-ttu-id="bafc1-149">В следующих разделах приводятся инструкции по настройке приложения для работы с соответствующими поставщиками:</span><span class="sxs-lookup"><span data-stu-id="bafc1-149">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="bafc1-150">Инструкции для [Facebook](xref:security/authentication/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="bafc1-150">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="bafc1-151">Инструкции для [Twitter](xref:security/authentication/twitter-logins)</span><span class="sxs-lookup"><span data-stu-id="bafc1-151">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="bafc1-152">Инструкции для [Google](xref:security/authentication/google-logins)</span><span class="sxs-lookup"><span data-stu-id="bafc1-152">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="bafc1-153">Инструкции для [Майкрософт](xref:security/authentication/microsoft-logins)</span><span class="sxs-lookup"><span data-stu-id="bafc1-153">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="bafc1-154">Инструкции для [других поставщиков](xref:security/authentication/otherlogins)</span><span class="sxs-lookup"><span data-stu-id="bafc1-154">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="bafc1-155">Необязательная установка пароля</span><span class="sxs-lookup"><span data-stu-id="bafc1-155">Optionally set password</span></span>

<span data-ttu-id="bafc1-156">При регистрации с использованием внешнего поставщика входа у вас нет пароля, зарегистрированного в приложении.</span><span class="sxs-lookup"><span data-stu-id="bafc1-156">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="bafc1-157">Благодаря этому вам не нужно создавать и запоминать пароль для сайта, однако при этом возникает зависимость от внешнего поставщика входа.</span><span class="sxs-lookup"><span data-stu-id="bafc1-157">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="bafc1-158">Если внешний поставщик входа недоступен, вы не сможете войти на веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="bafc1-158">If the external login provider is unavailable, you won't be able to sign in to the web site.</span></span>

<span data-ttu-id="bafc1-159">Чтобы создать пароль и войти с использованием адреса электронной почты, который был настроен при входе с использованием внешнего поставщика, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="bafc1-159">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="bafc1-160">Щелкните ссылку **Здравствуйте, &lt;псевдоним электронной почты&gt;** в правом верхнем углу, чтобы перейти к представлению **Управление**.</span><span class="sxs-lookup"><span data-stu-id="bafc1-160">Select the **Hello &lt;email alias&gt;** link at the top-right corner to navigate to the **Manage** view.</span></span>

![Представление управления веб-приложения](index/_static/pass1a.png)

* <span data-ttu-id="bafc1-162">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="bafc1-162">Select **Create**</span></span>

![Настройте страницу пароля](index/_static/pass2a.png)

* <span data-ttu-id="bafc1-164">Введите допустимый пароль, который будет использоваться для входа с применением этого адреса электронной почты.</span><span class="sxs-lookup"><span data-stu-id="bafc1-164">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bafc1-165">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="bafc1-165">Next steps</span></span>

* <span data-ttu-id="bafc1-166">В этой статье описывается внешняя проверка подлинности и приводятся предварительные требования для добавления внешних поставщиков входа в приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bafc1-166">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="bafc1-167">Инструкции по настройке учетных данных для входа, используемых вашим приложением, см. на соответствующих страницах поставщиков.</span><span class="sxs-lookup"><span data-stu-id="bafc1-167">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="bafc1-168">Вы можете сохранить дополнительные данные о пользователях и их маркерах доступа и обновления.</span><span class="sxs-lookup"><span data-stu-id="bafc1-168">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="bafc1-169">Для получения дополнительной информации см. <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="bafc1-169">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
