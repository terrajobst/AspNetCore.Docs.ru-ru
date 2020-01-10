---
title: Статьи на основе проектов ASP.NET Core, созданных с учетными записями отдельных пользователей
author: rick-anderson
description: Ознакомьтесь с статьями, основанными на ASP.NET Core проектах, созданных с учетными записями отдельных пользователей.
ms.author: riande
ms.date: 12/11/2019
uid: security/authentication/individual
ms.openlocfilehash: 7ef0d5eabded61d04fb9fe7be384a663ad7ea5f4
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828715"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="b2f63-103">Статьи на основе проектов ASP.NET Core, созданных с учетными записями отдельных пользователей</span><span class="sxs-lookup"><span data-stu-id="b2f63-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="b2f63-104">ASP.NET Core удостоверение включается в шаблоны проектов Visual Studio с параметром "индивидуальные учетные записи пользователей".</span><span class="sxs-lookup"><span data-stu-id="b2f63-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="b2f63-105">Шаблоны проверки подлинности доступны в .NET Core CLI с `-au Individual`.</span><span class="sxs-lookup"><span data-stu-id="b2f63-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```dotnetcli
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```dotnetcli
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="b2f63-106">См. [эту ошибку GitHub](https://github.com/dotnet/AspNetCore/issues/5833) для проверки подлинности веб-API.</span><span class="sxs-lookup"><span data-stu-id="b2f63-106">See [this GitHub issue](https://github.com/dotnet/AspNetCore/issues/5833) for web API authentication.</span></span>

<a name="no"></a>

## <a name="no-authentication"></a><span data-ttu-id="b2f63-107">Без проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="b2f63-107">No Authentication</span></span>

<span data-ttu-id="b2f63-108">Проверка подлинности указывается в .NET Core CLI с параметром `-au`.</span><span class="sxs-lookup"><span data-stu-id="b2f63-108">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="b2f63-109">В Visual Studio диалоговое окно **Изменение проверки подлинности** доступно для новых веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="b2f63-109">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="b2f63-110">По умолчанию для новых веб-приложений в Visual Studio **Проверка подлинности не**выполняется.</span><span class="sxs-lookup"><span data-stu-id="b2f63-110">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="b2f63-111">Проекты, созданные без проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="b2f63-111">Projects created with no authentication:</span></span>

* <span data-ttu-id="b2f63-112">Не следует включать веб-страницы и пользовательский интерфейс для входа и выхода.</span><span class="sxs-lookup"><span data-stu-id="b2f63-112">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="b2f63-113">Не содержать код проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="b2f63-113">Don't contain authentication code.</span></span>

<a name="win"></a>

## <a name="windows-authentication"></a><span data-ttu-id="b2f63-114">Аутентификация Windows</span><span class="sxs-lookup"><span data-stu-id="b2f63-114">Windows Authentication</span></span>

<span data-ttu-id="b2f63-115">Для новых веб-приложений в .NET Core CLI указана проверка подлинности Windows с параметром `-au Windows`.</span><span class="sxs-lookup"><span data-stu-id="b2f63-115">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="b2f63-116">В Visual Studio диалоговое окно **Изменение проверки подлинности** предоставляет параметры **проверки подлинности Windows** .</span><span class="sxs-lookup"><span data-stu-id="b2f63-116">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="b2f63-117">Если выбрана проверка подлинности Windows, приложение настроено для использования [модуля IIS проверки подлинности Windows](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="b2f63-117">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="b2f63-118">Проверка подлинности Windows предназначена для веб-сайтов интрасети.</span><span class="sxs-lookup"><span data-stu-id="b2f63-118">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="dotnet-new-webapp-authentication-options"></a><span data-ttu-id="b2f63-119">Параметры проверки подлинности DotNet New webapp</span><span class="sxs-lookup"><span data-stu-id="b2f63-119">dotnet new webapp authentication options</span></span>

<span data-ttu-id="b2f63-120">В следующей таблице приведены параметры проверки подлинности, доступные для новых веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="b2f63-120">The following table shows the authentication options available for new web apps:</span></span>

| <span data-ttu-id="b2f63-121">Параметр</span><span class="sxs-lookup"><span data-stu-id="b2f63-121">Option</span></span> | <span data-ttu-id="b2f63-122">Тип проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="b2f63-122">Type of authentication</span></span> | <span data-ttu-id="b2f63-123">Ссылка на дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="b2f63-123">Link for more information</span></span> |
 | ----------------- | ------------ | ---------- |
| <span data-ttu-id="b2f63-124">Нет</span><span class="sxs-lookup"><span data-stu-id="b2f63-124">None</span></span>            |  <span data-ttu-id="b2f63-125">Нет проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="b2f63-125">No authentication</span></span> | | 
| <span data-ttu-id="b2f63-126">Индивидуальное лицо</span><span class="sxs-lookup"><span data-stu-id="b2f63-126">Individual</span></span>      |  <span data-ttu-id="b2f63-127">Индивидуальная проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="b2f63-127">Individual authentication</span></span> | <xref:security/authentication/identity>
| <span data-ttu-id="b2f63-128">IndividualB2C</span><span class="sxs-lookup"><span data-stu-id="b2f63-128">IndividualB2C</span></span>   |  <span data-ttu-id="b2f63-129">Индивидуальная проверка подлинности, размещенная в облаке, с Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="b2f63-129">Cloud-hosted individual authentication with Azure AD B2C</span></span> | [<span data-ttu-id="b2f63-130">Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="b2f63-130">Azure AD B2C</span></span>](/azure/active-directory-b2c/) |
| <span data-ttu-id="b2f63-131">синглеорг</span><span class="sxs-lookup"><span data-stu-id="b2f63-131">SingleOrg</span></span>       |  <span data-ttu-id="b2f63-132">Аутентификация в Организации для одного клиента</span><span class="sxs-lookup"><span data-stu-id="b2f63-132">Organizational authentication for a single tenant</span></span> | [<span data-ttu-id="b2f63-133">Azure AD</span><span class="sxs-lookup"><span data-stu-id="b2f63-133">Azure AD</span></span>](/azure/active-directory/develop/quickstart-v2-aspnet-core-webapp) |
| <span data-ttu-id="b2f63-134">мултиорг</span><span class="sxs-lookup"><span data-stu-id="b2f63-134">MultiOrg</span></span>        |  <span data-ttu-id="b2f63-135">Проверка подлинности организации для нескольких клиентов</span><span class="sxs-lookup"><span data-stu-id="b2f63-135">Organizational authentication for multiple tenants</span></span> | [<span data-ttu-id="b2f63-136">Azure AD</span><span class="sxs-lookup"><span data-stu-id="b2f63-136">Azure AD</span></span>](/azure/active-directory/develop/quickstart-v2-aspnet-core-webapp) |
| <span data-ttu-id="b2f63-137">Портал</span><span class="sxs-lookup"><span data-stu-id="b2f63-137">Windows</span></span>         |  <span data-ttu-id="b2f63-138">Аутентификация Windows</span><span class="sxs-lookup"><span data-stu-id="b2f63-138">Windows authentication</span></span> | [<span data-ttu-id="b2f63-139">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="b2f63-139">Windows Authentication</span></span>](xref:security/authentication/windowsauth)

## <a name="visual-studio-new-webapp-authentication-options"></a><span data-ttu-id="b2f63-140">Параметры проверки подлинности New webapp в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2f63-140">Visual Studio new webapp authentication options</span></span>

<span data-ttu-id="b2f63-141">В следующей таблице показаны параметры проверки подлинности, доступные при создании нового веб-приложения с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b2f63-141">The following table shows the authentication options available when creating a new web app with Visual Studio:</span></span>

| <span data-ttu-id="b2f63-142">Параметр</span><span class="sxs-lookup"><span data-stu-id="b2f63-142">Option</span></span> | <span data-ttu-id="b2f63-143">Тип проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="b2f63-143">Type of authentication</span></span> | <span data-ttu-id="b2f63-144">Ссылка на дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="b2f63-144">Link for more information</span></span> |
 | ----------------- | ------------ | ---------- |
| <span data-ttu-id="b2f63-145">Нет</span><span class="sxs-lookup"><span data-stu-id="b2f63-145">None</span></span>            |  <span data-ttu-id="b2f63-146">Нет проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="b2f63-146">No authentication</span></span> | | 
| <span data-ttu-id="b2f63-147">Индивидуальные учетные записи пользователей и хранение учетных записей пользователей в приложении</span><span class="sxs-lookup"><span data-stu-id="b2f63-147">Individual User Accounts / Store user accounts in-app</span></span> |  <span data-ttu-id="b2f63-148">Индивидуальная проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="b2f63-148">Individual authentication</span></span> | <xref:security/authentication/identity> |
| <span data-ttu-id="b2f63-149">Учетные записи отдельных пользователей и подключение к существующему хранилищу пользователей в облаке</span><span class="sxs-lookup"><span data-stu-id="b2f63-149">Individual User Accounts / Connect to an existing user store in the cloud</span></span> |  <span data-ttu-id="b2f63-150">Индивидуальная проверка подлинности, размещенная в облаке, с Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="b2f63-150">Cloud-hosted individual authentication with Azure AD B2C</span></span> | [<span data-ttu-id="b2f63-151">Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="b2f63-151">Azure AD B2C</span></span>](/azure/active-directory-b2c/) |
| <span data-ttu-id="b2f63-152">Рабочее или учебное облако/Единая Организация</span><span class="sxs-lookup"><span data-stu-id="b2f63-152">Work or School Cloud / Single Org</span></span>  |  <span data-ttu-id="b2f63-153">Аутентификация в Организации для одного клиента</span><span class="sxs-lookup"><span data-stu-id="b2f63-153">Organizational authentication for a single tenant</span></span> | [<span data-ttu-id="b2f63-154">Azure AD</span><span class="sxs-lookup"><span data-stu-id="b2f63-154">Azure AD</span></span>](/azure/active-directory/develop/quickstart-v2-aspnet-core-webapp) |
| <span data-ttu-id="b2f63-155">Рабочее или учебное облако/несколько Организации</span><span class="sxs-lookup"><span data-stu-id="b2f63-155">Work or School Cloud / Multiple Org</span></span> |  <span data-ttu-id="b2f63-156">Проверка подлинности организации для нескольких клиентов</span><span class="sxs-lookup"><span data-stu-id="b2f63-156">Organizational authentication for multiple tenants</span></span> | [<span data-ttu-id="b2f63-157">Azure AD</span><span class="sxs-lookup"><span data-stu-id="b2f63-157">Azure AD</span></span>](/azure/active-directory/develop/quickstart-v2-aspnet-core-webapp) |
| <span data-ttu-id="b2f63-158">Портал</span><span class="sxs-lookup"><span data-stu-id="b2f63-158">Windows</span></span>         |  <span data-ttu-id="b2f63-159">Аутентификация Windows</span><span class="sxs-lookup"><span data-stu-id="b2f63-159">Windows authentication</span></span> | [<span data-ttu-id="b2f63-160">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="b2f63-160">Windows Authentication</span></span>](xref:security/authentication/windowsauth)

## <a name="additional-resources"></a><span data-ttu-id="b2f63-161">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b2f63-161">Additional resources</span></span>

<span data-ttu-id="b2f63-162">В следующих статьях показано, как использовать код, созданный в шаблонах ASP.NET Core, использующих отдельные учетные записи пользователей.</span><span class="sxs-lookup"><span data-stu-id="b2f63-162">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="b2f63-163">Подтверждение учетной записи и восстановление пароля в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2f63-163">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="b2f63-164">Создание приложения ASP.NET Core с данными пользователя, защищенными с помощью авторизации</span><span class="sxs-lookup"><span data-stu-id="b2f63-164">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
