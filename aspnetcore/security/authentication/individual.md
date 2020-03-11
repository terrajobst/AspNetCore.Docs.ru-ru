---
title: Статьи на основе проектов ASP.NET Core, созданных с учетными записями отдельных пользователей
author: rick-anderson
description: Ознакомьтесь с статьями, основанными на ASP.NET Core проектах, созданных с учетными записями отдельных пользователей.
ms.author: riande
ms.date: 12/11/2019
uid: security/authentication/individual
ms.openlocfilehash: 7ef0d5eabded61d04fb9fe7be384a663ad7ea5f4
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651958"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="9b75f-103">Статьи на основе проектов ASP.NET Core, созданных с учетными записями отдельных пользователей</span><span class="sxs-lookup"><span data-stu-id="9b75f-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="9b75f-104">ASP.NET Core удостоверение включается в шаблоны проектов Visual Studio с параметром "индивидуальные учетные записи пользователей".</span><span class="sxs-lookup"><span data-stu-id="9b75f-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="9b75f-105">Шаблоны проверки подлинности доступны в .NET Core CLI с `-au Individual`.</span><span class="sxs-lookup"><span data-stu-id="9b75f-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

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

<span data-ttu-id="9b75f-106">См. [эту ошибку GitHub](https://github.com/dotnet/AspNetCore/issues/5833) для проверки подлинности веб-API.</span><span class="sxs-lookup"><span data-stu-id="9b75f-106">See [this GitHub issue](https://github.com/dotnet/AspNetCore/issues/5833) for web API authentication.</span></span>

<a name="no"></a>

## <a name="no-authentication"></a><span data-ttu-id="9b75f-107">Без аутентификации</span><span class="sxs-lookup"><span data-stu-id="9b75f-107">No Authentication</span></span>

<span data-ttu-id="9b75f-108">Проверка подлинности указывается в .NET Core CLI с параметром `-au`.</span><span class="sxs-lookup"><span data-stu-id="9b75f-108">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="9b75f-109">В Visual Studio диалоговое окно **Изменение проверки подлинности** доступно для новых веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="9b75f-109">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="9b75f-110">По умолчанию для новых веб-приложений в Visual Studio **Проверка подлинности не**выполняется.</span><span class="sxs-lookup"><span data-stu-id="9b75f-110">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="9b75f-111">Проекты, созданные без проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="9b75f-111">Projects created with no authentication:</span></span>

* <span data-ttu-id="9b75f-112">Не следует включать веб-страницы и пользовательский интерфейс для входа и выхода.</span><span class="sxs-lookup"><span data-stu-id="9b75f-112">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="9b75f-113">Не содержать код проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="9b75f-113">Don't contain authentication code.</span></span>

<a name="win"></a>

## <a name="windows-authentication"></a><span data-ttu-id="9b75f-114">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="9b75f-114">Windows Authentication</span></span>

<span data-ttu-id="9b75f-115">Для новых веб-приложений в .NET Core CLI указана проверка подлинности Windows с параметром `-au Windows`.</span><span class="sxs-lookup"><span data-stu-id="9b75f-115">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="9b75f-116">В Visual Studio диалоговое окно **Изменение проверки подлинности** предоставляет параметры **проверки подлинности Windows** .</span><span class="sxs-lookup"><span data-stu-id="9b75f-116">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="9b75f-117">Если выбрана проверка подлинности Windows, приложение настроено для использования [модуля IIS проверки подлинности Windows](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="9b75f-117">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="9b75f-118">Проверка подлинности Windows предназначена для веб-сайтов интрасети.</span><span class="sxs-lookup"><span data-stu-id="9b75f-118">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="dotnet-new-webapp-authentication-options"></a><span data-ttu-id="9b75f-119">Параметры проверки подлинности DotNet New webapp</span><span class="sxs-lookup"><span data-stu-id="9b75f-119">dotnet new webapp authentication options</span></span>

<span data-ttu-id="9b75f-120">В следующей таблице приведены параметры проверки подлинности, доступные для новых веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="9b75f-120">The following table shows the authentication options available for new web apps:</span></span>

| <span data-ttu-id="9b75f-121">Параметр</span><span class="sxs-lookup"><span data-stu-id="9b75f-121">Option</span></span> | <span data-ttu-id="9b75f-122">Тип проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="9b75f-122">Type of authentication</span></span> | <span data-ttu-id="9b75f-123">Ссылка для получения дополнительных сведений</span><span class="sxs-lookup"><span data-stu-id="9b75f-123">Link for more information</span></span> |
 | ----------------- | ------------ | ---------- |
| <span data-ttu-id="9b75f-124">None</span><span class="sxs-lookup"><span data-stu-id="9b75f-124">None</span></span>            |  <span data-ttu-id="9b75f-125">без аутентификации;</span><span class="sxs-lookup"><span data-stu-id="9b75f-125">No authentication</span></span> | | 
| <span data-ttu-id="9b75f-126">Индивидуальное лицо</span><span class="sxs-lookup"><span data-stu-id="9b75f-126">Individual</span></span>      |  <span data-ttu-id="9b75f-127">Индивидуальная проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="9b75f-127">Individual authentication</span></span> | <xref:security/authentication/identity>
| <span data-ttu-id="9b75f-128">IndividualB2C</span><span class="sxs-lookup"><span data-stu-id="9b75f-128">IndividualB2C</span></span>   |  <span data-ttu-id="9b75f-129">Индивидуальная проверка подлинности, размещенная в облаке, с Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="9b75f-129">Cloud-hosted individual authentication with Azure AD B2C</span></span> | [<span data-ttu-id="9b75f-130">Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="9b75f-130">Azure AD B2C</span></span>](/azure/active-directory-b2c/) |
| <span data-ttu-id="9b75f-131">синглеорг</span><span class="sxs-lookup"><span data-stu-id="9b75f-131">SingleOrg</span></span>       |  <span data-ttu-id="9b75f-132">Аутентификация в Организации для одного клиента</span><span class="sxs-lookup"><span data-stu-id="9b75f-132">Organizational authentication for a single tenant</span></span> | [<span data-ttu-id="9b75f-133">Azure AD</span><span class="sxs-lookup"><span data-stu-id="9b75f-133">Azure AD</span></span>](/azure/active-directory/develop/quickstart-v2-aspnet-core-webapp) |
| <span data-ttu-id="9b75f-134">мултиорг</span><span class="sxs-lookup"><span data-stu-id="9b75f-134">MultiOrg</span></span>        |  <span data-ttu-id="9b75f-135">Проверка подлинности организации для нескольких клиентов</span><span class="sxs-lookup"><span data-stu-id="9b75f-135">Organizational authentication for multiple tenants</span></span> | [<span data-ttu-id="9b75f-136">Azure AD</span><span class="sxs-lookup"><span data-stu-id="9b75f-136">Azure AD</span></span>](/azure/active-directory/develop/quickstart-v2-aspnet-core-webapp) |
| <span data-ttu-id="9b75f-137">Windows</span><span class="sxs-lookup"><span data-stu-id="9b75f-137">Windows</span></span>         |  <span data-ttu-id="9b75f-138">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="9b75f-138">Windows authentication</span></span> | [<span data-ttu-id="9b75f-139">Проверка подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="9b75f-139">Windows Authentication</span></span>](xref:security/authentication/windowsauth)

## <a name="visual-studio-new-webapp-authentication-options"></a><span data-ttu-id="9b75f-140">Параметры проверки подлинности New webapp в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b75f-140">Visual Studio new webapp authentication options</span></span>

<span data-ttu-id="9b75f-141">В следующей таблице показаны параметры проверки подлинности, доступные при создании нового веб-приложения с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9b75f-141">The following table shows the authentication options available when creating a new web app with Visual Studio:</span></span>

| <span data-ttu-id="9b75f-142">Параметр</span><span class="sxs-lookup"><span data-stu-id="9b75f-142">Option</span></span> | <span data-ttu-id="9b75f-143">Тип проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="9b75f-143">Type of authentication</span></span> | <span data-ttu-id="9b75f-144">Ссылка для получения дополнительных сведений</span><span class="sxs-lookup"><span data-stu-id="9b75f-144">Link for more information</span></span> |
 | ----------------- | ------------ | ---------- |
| <span data-ttu-id="9b75f-145">None</span><span class="sxs-lookup"><span data-stu-id="9b75f-145">None</span></span>            |  <span data-ttu-id="9b75f-146">без аутентификации;</span><span class="sxs-lookup"><span data-stu-id="9b75f-146">No authentication</span></span> | | 
| <span data-ttu-id="9b75f-147">Индивидуальные учетные записи пользователей и хранение учетных записей пользователей в приложении</span><span class="sxs-lookup"><span data-stu-id="9b75f-147">Individual User Accounts / Store user accounts in-app</span></span> |  <span data-ttu-id="9b75f-148">Индивидуальная проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="9b75f-148">Individual authentication</span></span> | <xref:security/authentication/identity> |
| <span data-ttu-id="9b75f-149">Учетные записи отдельных пользователей и подключение к существующему хранилищу пользователей в облаке</span><span class="sxs-lookup"><span data-stu-id="9b75f-149">Individual User Accounts / Connect to an existing user store in the cloud</span></span> |  <span data-ttu-id="9b75f-150">Индивидуальная проверка подлинности, размещенная в облаке, с Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="9b75f-150">Cloud-hosted individual authentication with Azure AD B2C</span></span> | [<span data-ttu-id="9b75f-151">Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="9b75f-151">Azure AD B2C</span></span>](/azure/active-directory-b2c/) |
| <span data-ttu-id="9b75f-152">Рабочее или учебное облако/Единая Организация</span><span class="sxs-lookup"><span data-stu-id="9b75f-152">Work or School Cloud / Single Org</span></span>  |  <span data-ttu-id="9b75f-153">Аутентификация в Организации для одного клиента</span><span class="sxs-lookup"><span data-stu-id="9b75f-153">Organizational authentication for a single tenant</span></span> | [<span data-ttu-id="9b75f-154">Azure AD</span><span class="sxs-lookup"><span data-stu-id="9b75f-154">Azure AD</span></span>](/azure/active-directory/develop/quickstart-v2-aspnet-core-webapp) |
| <span data-ttu-id="9b75f-155">Рабочее или учебное облако/несколько Организации</span><span class="sxs-lookup"><span data-stu-id="9b75f-155">Work or School Cloud / Multiple Org</span></span> |  <span data-ttu-id="9b75f-156">Проверка подлинности организации для нескольких клиентов</span><span class="sxs-lookup"><span data-stu-id="9b75f-156">Organizational authentication for multiple tenants</span></span> | [<span data-ttu-id="9b75f-157">Azure AD</span><span class="sxs-lookup"><span data-stu-id="9b75f-157">Azure AD</span></span>](/azure/active-directory/develop/quickstart-v2-aspnet-core-webapp) |
| <span data-ttu-id="9b75f-158">Windows</span><span class="sxs-lookup"><span data-stu-id="9b75f-158">Windows</span></span>         |  <span data-ttu-id="9b75f-159">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="9b75f-159">Windows authentication</span></span> | [<span data-ttu-id="9b75f-160">Проверка подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="9b75f-160">Windows Authentication</span></span>](xref:security/authentication/windowsauth)

## <a name="additional-resources"></a><span data-ttu-id="9b75f-161">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9b75f-161">Additional resources</span></span>

<span data-ttu-id="9b75f-162">В следующих статьях показано, как использовать код, созданный в шаблонах ASP.NET Core, использующих отдельные учетные записи пользователей.</span><span class="sxs-lookup"><span data-stu-id="9b75f-162">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="9b75f-163">Подтверждение учетной записи и восстановление пароля в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b75f-163">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="9b75f-164">Создание приложения ASP.NET Core с данными пользователя, защищенными с помощью авторизации</span><span class="sxs-lookup"><span data-stu-id="9b75f-164">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
