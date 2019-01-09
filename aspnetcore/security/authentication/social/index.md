---
title: Проверка подлинности Facebook, Google и внешних поставщиков в ASP.NET Core
author: rick-anderson
description: В этом руководстве демонстрируется построение приложения ASP.NET Core 2.x с использованием OAuth 2.0 с внешними поставщиками проверки подлинности.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/social/index
ms.openlocfilehash: 063d452fb6ab91b712ade7f7b7ed99823dbdc657
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098822"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="1ac38-103">Проверка подлинности Facebook, Google и внешних поставщиков в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ac38-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="1ac38-104">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="1ac38-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1ac38-105">В этом руководстве демонстрируется построение приложения ASP.NET Core 2.x, с помощью которого пользователи могут выполнять вход, используя OAuth 2.0 с учетными данными внешних поставщиков проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="1ac38-105">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="1ac38-106">В следующих разделах рассматриваются такие поставщики, как [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), и [Microsoft](xref:security/authentication/microsoft-logins).</span><span class="sxs-lookup"><span data-stu-id="1ac38-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="1ac38-107">В сторонних пакетах также доступны другие поставщики, такие как [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) и [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="1ac38-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Значки социальных сетей для Facebook, Twitter, Google+ и Windows](index/_static/social.png)

<span data-ttu-id="1ac38-109">Возможность выполнять вход с использованием существующих учетных данных очень удобна и позволяет передать все задачи, связанные с управлением процессом входа, сторонней организации.</span><span class="sxs-lookup"><span data-stu-id="1ac38-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="1ac38-110">Демонстрацию того, как вход с использованием учетных данных социальных сетей помогает повысить трафик и количество конверсий, см. в примерах для [Facebook](https://www.facebook.com/unsupportedbrowser) и [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="1ac38-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="1ac38-111">Создание проекта ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ac38-111">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="1ac38-112">В Visual Studio 2017 создайте проект на начальной странице или выбрав **Файл** > **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="1ac38-112">In Visual Studio 2017, create a new project from the Start Page, or via **File** > **New** > **Project**.</span></span>

* <span data-ttu-id="1ac38-113">Выберите шаблон **Веб-приложение ASP.NET Core** из категории **Visual C#** > **.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="1ac38-113">Select the **ASP.NET Core Web Application** template available in the **Visual C#** > **.NET Core** category:</span></span>

![Диалоговое окно создания нового проекта](index/_static/new-project.png)

* <span data-ttu-id="1ac38-115">Коснитесь пункта **Веб-приложение** и убедитесь, что параметру **Проверка подлинности** присвоено значение **Учетные записи отдельных пользователей**:</span><span class="sxs-lookup"><span data-stu-id="1ac38-115">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![Диалоговое окно "Создание веб-приложения"](index/_static/select-project.png)

<span data-ttu-id="1ac38-117">Примечание. Этот учебник относится к версии пакета SDK для ASP.NET Core 2.0, которую можно выбрать в верхней части мастера.</span><span class="sxs-lookup"><span data-stu-id="1ac38-117">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="1ac38-118">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="1ac38-118">Apply migrations</span></span>

* <span data-ttu-id="1ac38-119">Запустите приложение и щелкните ссылку для **входа**.</span><span class="sxs-lookup"><span data-stu-id="1ac38-119">Run the app and select the **Log in** link.</span></span>
* <span data-ttu-id="1ac38-120">Выберите ссылку **Регистрация нового пользователя**.</span><span class="sxs-lookup"><span data-stu-id="1ac38-120">Select the **Register as a new user** link.</span></span>
* <span data-ttu-id="1ac38-121">Введите адрес электронной почты и пароль для новой учетной записи, а затем щелкните **Зарегистрироваться**.</span><span class="sxs-lookup"><span data-stu-id="1ac38-121">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="1ac38-122">Следуйте инструкциям по применению миграции.</span><span class="sxs-lookup"><span data-stu-id="1ac38-122">Follow the instructions to apply migrations.</span></span>

## <a name="require-https"></a><span data-ttu-id="1ac38-123">Требование к использованию протокола HTTPS</span><span class="sxs-lookup"><span data-stu-id="1ac38-123">Require HTTPS</span></span>

<span data-ttu-id="1ac38-124">Для проверки подлинности по протоколу HTTPS в OAuth 2.0 нужно обязательно использовать протокол SSL/TLS.</span><span class="sxs-lookup"><span data-stu-id="1ac38-124">OAuth 2.0 requires the use of SSL/TLS for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="1ac38-125">Проекты, создаваемые по шаблонам проектов **Веб-приложение** или **Веб-API** в ASP.NET Core 2.1 и более поздних версиях, автоматически активируют протокол HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1ac38-125">Projects created using the **Web Application** or **Web API** project templates with ASP.NET Core 2.1 or later are automatically configured to enable HTTPS.</span></span> <span data-ttu-id="1ac38-126">При выборе параметра **Индивидуальные учетные записи пользователей** в диалоговом окне **Изменение способа проверки подлинности** мастера проектов приложение запускается с безопасной конечной точкой по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="1ac38-126">The app launches with a secure default endpoint if the **Individual User Accounts** option is selected in the **Change Authentication dialog** of the project wizard.</span></span>

<span data-ttu-id="1ac38-127">Для получения дополнительной информации см. <xref:security/enforcing-ssl>.</span><span class="sxs-lookup"><span data-stu-id="1ac38-127">For more information, see <xref:security/enforcing-ssl>.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="1ac38-128">Хранение маркеров безопасности, предоставленных поставщиками входа, с помощью SecretManager</span><span class="sxs-lookup"><span data-stu-id="1ac38-128">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="1ac38-129">Поставщики входа социальных сетей назначают маркеры **идентификатора приложения** и **секрета приложения** в процессе регистрации.</span><span class="sxs-lookup"><span data-stu-id="1ac38-129">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="1ac38-130">Точные имена маркеров зависят от поставщика.</span><span class="sxs-lookup"><span data-stu-id="1ac38-130">The exact token names vary by provider.</span></span> <span data-ttu-id="1ac38-131">Эти маркеры соответствуют учетным данным, которые используются приложением для доступа к API.</span><span class="sxs-lookup"><span data-stu-id="1ac38-131">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="1ac38-132">Маркеры предоставляют "секреты", которые можно подключить к конфигурации приложения с помощью [Secret Manager](xref:security/app-secrets#secret-manager) (Диспетчера секретов).</span><span class="sxs-lookup"><span data-stu-id="1ac38-132">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="1ac38-133">Secret Manager — более безопасная альтернатива хранению маркеров в файле конфигурации, например в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1ac38-133">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1ac38-134">Secret Manager предназначен только для разработки.</span><span class="sxs-lookup"><span data-stu-id="1ac38-134">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="1ac38-135">Для хранения и защиты секретов Azure в ходе тестирования и непосредственной работы используйте [Поставщик конфигурации Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="1ac38-135">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="1ac38-136">В разделе [Безопасное хранение секретов приложений во время разработки в ASP.NET Core](xref:security/app-secrets) описано, как хранить маркеры, назначаемые приведенными ниже поставщиками входа.</span><span class="sxs-lookup"><span data-stu-id="1ac38-136">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="1ac38-137">Настройка поставщиков входа, используемых приложением</span><span class="sxs-lookup"><span data-stu-id="1ac38-137">Setup login providers required by your application</span></span>

<span data-ttu-id="1ac38-138">В следующих разделах приводятся инструкции по настройке приложения для работы с соответствующими поставщиками:</span><span class="sxs-lookup"><span data-stu-id="1ac38-138">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="1ac38-139">Инструкции для [Facebook](xref:security/authentication/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="1ac38-139">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="1ac38-140">Инструкции для [Twitter](xref:security/authentication/twitter-logins)</span><span class="sxs-lookup"><span data-stu-id="1ac38-140">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="1ac38-141">Инструкции для [Google](xref:security/authentication/google-logins)</span><span class="sxs-lookup"><span data-stu-id="1ac38-141">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="1ac38-142">Инструкции для [Майкрософт](xref:security/authentication/microsoft-logins)</span><span class="sxs-lookup"><span data-stu-id="1ac38-142">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="1ac38-143">Инструкции для [других поставщиков](xref:security/authentication/otherlogins)</span><span class="sxs-lookup"><span data-stu-id="1ac38-143">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="1ac38-144">Необязательная установка пароля</span><span class="sxs-lookup"><span data-stu-id="1ac38-144">Optionally set password</span></span>

<span data-ttu-id="1ac38-145">При регистрации с использованием внешнего поставщика входа у вас нет пароля, зарегистрированного в приложении.</span><span class="sxs-lookup"><span data-stu-id="1ac38-145">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="1ac38-146">Благодаря этому вам не нужно создавать и запоминать пароль для сайта, однако при этом возникает зависимость от внешнего поставщика входа.</span><span class="sxs-lookup"><span data-stu-id="1ac38-146">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="1ac38-147">Если внешний поставщик входа недоступен, вы не сможете войти на веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="1ac38-147">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="1ac38-148">Чтобы создать пароль и войти с использованием адреса электронной почты, который был настроен при входе с использованием внешнего поставщика, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="1ac38-148">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="1ac38-149">Коснитесь ссылки **Hello &lt;псевдоним электронной почты&gt;** в правом верхнем углу, чтобы перейти к представлению **Управление**.</span><span class="sxs-lookup"><span data-stu-id="1ac38-149">Tap the **Hello &lt;email alias&gt;** link at the top right corner to navigate to the **Manage** view.</span></span>

![Представление управления веб-приложения](index/_static/pass1a.png)

* <span data-ttu-id="1ac38-151">Коснитесь элемента **Создать**</span><span class="sxs-lookup"><span data-stu-id="1ac38-151">Tap **Create**</span></span>

![Настройте страницу пароля](index/_static/pass2a.png)

* <span data-ttu-id="1ac38-153">Введите допустимый пароль, который будет использоваться для входа с применением этого адреса электронной почты.</span><span class="sxs-lookup"><span data-stu-id="1ac38-153">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1ac38-154">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="1ac38-154">Next steps</span></span>

* <span data-ttu-id="1ac38-155">В этой статье описывается внешняя проверка подлинности и приводятся предварительные требования для добавления внешних поставщиков входа в приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1ac38-155">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="1ac38-156">Инструкции по настройке учетных данных для входа, используемых вашим приложением, см. на соответствующих страницах поставщиков.</span><span class="sxs-lookup"><span data-stu-id="1ac38-156">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="1ac38-157">Вы можете сохранить дополнительные данные о пользователях и их маркерах доступа и обновления.</span><span class="sxs-lookup"><span data-stu-id="1ac38-157">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="1ac38-158">Для получения дополнительной информации см. <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="1ac38-158">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
