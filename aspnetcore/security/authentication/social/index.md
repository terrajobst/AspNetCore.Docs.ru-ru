---
title: "Включение проверки подлинности с помощью Facebook, Google и других внешних поставщиков"
author: rick-anderson
description: "В этом руководстве демонстрируется построение приложения ASP.NET Core 2.x с использованием OAuth 2.0 с внешними поставщиками проверки подлинности."
keywords: "ASP.NET Core, проверка подлинности, социальные службы, поставщики проверки подлинности, google, facebook, twitter, учетная запись майкрософт"
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: eda7ee17-f38c-462e-8d1d-63f459901cf3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/social/index
ms.openlocfilehash: 9fc0d6c3e9691f8c3fa0d769ac53c3337d822fc5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="enabling-authentication-using-facebook-google-and-other-external-providers"></a><span data-ttu-id="3e825-104">Включение проверки подлинности с помощью Facebook, Google и других внешних поставщиков</span><span class="sxs-lookup"><span data-stu-id="3e825-104">Enabling authentication using Facebook, Google, and other external providers</span></span>

<a name="security-authentication-social-logins"></a>

<span data-ttu-id="3e825-105">Авторы: [Валерий Новицкий](https://github.com/01binary) (Valeriy Novytskyy) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="3e825-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3e825-106">В этом руководстве демонстрируется построение приложения ASP.NET Core 2.x, с помощью которого пользователи могут выполнять вход, используя OAuth 2.0 с учетными данными внешних поставщиков проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3e825-106">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="3e825-107">В следующих разделах рассматриваются такие поставщики, как [Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), и [Microsoft](microsoft-logins.md).</span><span class="sxs-lookup"><span data-stu-id="3e825-107">[Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), and [Microsoft](microsoft-logins.md) providers are covered in the following sections.</span></span> <span data-ttu-id="3e825-108">В сторонних пакетах также доступны другие поставщики, такие как [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) и [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="3e825-108">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Значки социальных сетей для Facebook, Twitter, Google+ и Windows](index/_static/social.png)

<span data-ttu-id="3e825-110">Возможность выполнять вход с использованием существующих учетных данных очень удобна и позволяет передать все задачи, связанные с управлением процессом входа, сторонней организации.</span><span class="sxs-lookup"><span data-stu-id="3e825-110">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="3e825-111">Демонстрацию того, как вход с использованием учетных данных социальных сетей помогает повысить трафик и количество конверсий, см. в примерах для [Facebook](https://www.facebook.com/unsupportedbrowser) и [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="3e825-111">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="3e825-112">Примечание. Представленные здесь пакеты абстрагируют значительную часть задач, связанных с процессом проверки подлинности OAuth, однако для эффективного устранения неполадок необходимо понимать общие сведения об их реализации.</span><span class="sxs-lookup"><span data-stu-id="3e825-112">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="3e825-113">Также вы можете воспользоваться множеством доступных ресурсов, например содержащих [общие сведения о протоколе OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) или [описание основных принципов работы OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span><span class="sxs-lookup"><span data-stu-id="3e825-113">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="3e825-114">Некоторые проблемы можно устранить, ознакомившись с [исходным кодом ASP.NET Core для пакетов поставщиков](https://github.com/aspnet/Security/tree/dev/src).</span><span class="sxs-lookup"><span data-stu-id="3e825-114">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="3e825-115">Создание проекта ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3e825-115">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="3e825-116">В Visual Studio 2017 создайте новый проект, используя начальную страницу или команду меню **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="3e825-116">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="3e825-117">Выберите шаблон **Веб-приложение ASP.NET Core**, который находится в категории **Visual C# > .NET Core**:</span><span class="sxs-lookup"><span data-stu-id="3e825-117">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![Диалоговое окно создания нового проекта](index/_static/new-project.png)

* <span data-ttu-id="3e825-119">Коснитесь пункта **Веб-приложение** и убедитесь, что параметру **Проверка подлинности** присвоено значение **Учетные записи отдельных пользователей**:</span><span class="sxs-lookup"><span data-stu-id="3e825-119">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![Диалоговое окно "Создание веб-приложения"](index/_static/select-project.png)

<span data-ttu-id="3e825-121">Примечание. Это руководство относится к пакету SDK версии ASP.NET Core 2.0, который можно выбрать в верхней части мастера.</span><span class="sxs-lookup"><span data-stu-id="3e825-121">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="3e825-122">Обязательное использование SSL</span><span class="sxs-lookup"><span data-stu-id="3e825-122">Require SSL</span></span>

<span data-ttu-id="3e825-123">Для проверки подлинности по протоколу HTTPS в OAuth 2.0 обязательно использовать протокол SSL.</span><span class="sxs-lookup"><span data-stu-id="3e825-123">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="3e825-124">Примечание. Проекты, созданные с использованием шаблонов **Веб-приложение** или **Веб-API** для ASP.NET Core 2.x, автоматически настраиваются с поддержкой SSL и запускаются с URL-адресом https, если выбран параметр **Учетные записи отдельных пользователей** в диалоговом окне **Изменить способ проверки подлинности** в мастере проекта, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="3e825-124">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="3e825-125">Инструкции по включению обязательного использования протокола SSL на сайте см. в разделе [Принудительное применение протокола SSL в приложении ASP.NET Core](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="3e825-125">Require SSL on your site by following the steps in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="3e825-126">Хранение маркеров безопасности, предоставленных поставщиками входа, с помощью SecretManager</span><span class="sxs-lookup"><span data-stu-id="3e825-126">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="3e825-127">Поставщики входа с использованием учетных данных социальных сетей в процессе регистрации назначают маркеры **идентификатора приложения** и **секретного кода приложения** (точные правила именования зависят от поставщика).</span><span class="sxs-lookup"><span data-stu-id="3e825-127">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process (exact naming varies by provider).</span></span>

<span data-ttu-id="3e825-128">Фактически эти значения определяют *имя пользователя* и *пароль*, которые ваше приложение использует для доступа к API поставщика. Они содержат секретные данные, которые могут быть привязаны к конфигурации приложения с помощью компонента **Secret Manager**, вместо того, чтобы жестко задавать или хранить их непосредственно в файлах конфигурации.</span><span class="sxs-lookup"><span data-stu-id="3e825-128">These values are effectively the *user name* and *password* your application uses to access their API, and constitute the "secrets" that can be linked to your application configuration with the help of **Secret Manager** instead of storing them in configuration files directly or hard-coding them.</span></span>

<span data-ttu-id="3e825-129">Инструкции по сохранению маркеров безопасности, назначаемых приведенными ниже поставщиками входа, см. в разделе [Безопасное хранение секретных данных приложения в процессе разработки в ASP.NET Core](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="3e825-129">Follow the steps in [Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets) topic so that you can store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="3e825-130">Настройка поставщиков входа, используемых приложением</span><span class="sxs-lookup"><span data-stu-id="3e825-130">Setup login providers required by your application</span></span>

<span data-ttu-id="3e825-131">В следующих разделах приводятся инструкции по настройке приложения для работы с соответствующими поставщиками:</span><span class="sxs-lookup"><span data-stu-id="3e825-131">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="3e825-132">Инструкции для [Facebook](facebook-logins.md)</span><span class="sxs-lookup"><span data-stu-id="3e825-132">[Facebook](facebook-logins.md) instructions</span></span>
* <span data-ttu-id="3e825-133">Инструкции для [Twitter](twitter-logins.md)</span><span class="sxs-lookup"><span data-stu-id="3e825-133">[Twitter](twitter-logins.md) instructions</span></span>
* <span data-ttu-id="3e825-134">Инструкции для [Google](google-logins.md)</span><span class="sxs-lookup"><span data-stu-id="3e825-134">[Google](google-logins.md) instructions</span></span>
* <span data-ttu-id="3e825-135">Инструкции для [Майкрософт](microsoft-logins.md)</span><span class="sxs-lookup"><span data-stu-id="3e825-135">[Microsoft](microsoft-logins.md) instructions</span></span>
* <span data-ttu-id="3e825-136">Инструкции для [других поставщиков](other-logins.md)</span><span class="sxs-lookup"><span data-stu-id="3e825-136">[Other provider](other-logins.md) instructions</span></span>

## <a name="optionally-set-password"></a><span data-ttu-id="3e825-137">Необязательная установка пароля</span><span class="sxs-lookup"><span data-stu-id="3e825-137">Optionally set password</span></span>

<span data-ttu-id="3e825-138">При регистрации с использованием внешнего поставщика входа у вас нет пароля, зарегистрированного в приложении.</span><span class="sxs-lookup"><span data-stu-id="3e825-138">When you register with an external login provider, you do not have a password registered with the app.</span></span> <span data-ttu-id="3e825-139">Благодаря этому вам не нужно создавать и запоминать пароль для сайта, однако при этом возникает зависимость от внешнего поставщика входа.</span><span class="sxs-lookup"><span data-stu-id="3e825-139">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="3e825-140">Если внешний поставщик входа недоступен, вы не сможете войти на веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="3e825-140">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="3e825-141">Чтобы создать пароль и войти с использованием адреса электронной почты, который был настроен при входе с использованием внешнего поставщика, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="3e825-141">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="3e825-142">Коснитесь ссылки **Hello <email alias>** в правом верхнем углу, чтобы перейти к представлению **Управление**.</span><span class="sxs-lookup"><span data-stu-id="3e825-142">Tap the **Hello <email alias>** link at the top right corner to navigate to the **Manage** view.</span></span>

![Представление управления веб-приложения](index/_static/pass1a.png)

* <span data-ttu-id="3e825-144">Коснитесь элемента **Создать**</span><span class="sxs-lookup"><span data-stu-id="3e825-144">Tap **Create**</span></span>

![Настройте страницу пароля](index/_static/pass2a.png)

* <span data-ttu-id="3e825-146">Введите допустимый пароль, который будет использоваться для входа с применением этого адреса электронной почты.</span><span class="sxs-lookup"><span data-stu-id="3e825-146">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e825-147">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="3e825-147">Next steps</span></span>

* <span data-ttu-id="3e825-148">В этой статье описывается внешняя проверка подлинности и приводятся предварительные требования для добавления внешних поставщиков входа в приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3e825-148">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="3e825-149">Инструкции по настройке учетных данных для входа, используемых вашим приложением, см. на соответствующих страницах поставщиков.</span><span class="sxs-lookup"><span data-stu-id="3e825-149">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>
