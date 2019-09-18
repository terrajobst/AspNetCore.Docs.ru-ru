---
title: Статьи на основе проектов ASP.NET Core, созданных с учетными записями отдельных пользователей
author: rick-anderson
description: Ознакомьтесь с статьями, основанными на ASP.NET Core проектах, созданных с учетными записями отдельных пользователей.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: cf548417268a8587787471b9ed91c0ed109fbee9
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080706"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="85629-103">Статьи на основе проектов ASP.NET Core, созданных с учетными записями отдельных пользователей</span><span class="sxs-lookup"><span data-stu-id="85629-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="85629-104">ASP.NET Core удостоверение включается в шаблоны проектов Visual Studio с параметром "индивидуальные учетные записи пользователей".</span><span class="sxs-lookup"><span data-stu-id="85629-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="85629-105">Шаблоны проверки подлинности доступны в .NET Core CLI `-au Individual`с помощью:</span><span class="sxs-lookup"><span data-stu-id="85629-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

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

<span data-ttu-id="85629-106">См. [эту ошибку GitHub](https://github.com/aspnet/AspNetCore/issues/5833) для проверки подлинности веб-API.</span><span class="sxs-lookup"><span data-stu-id="85629-106">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5833) for web API authentication.</span></span>

<a name="no"></a>

## <a name="no-authentication"></a><span data-ttu-id="85629-107">Без проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="85629-107">No Authentication</span></span>

<span data-ttu-id="85629-108">Проверка подлинности указывается в .NET Core CLI с `-au` параметром.</span><span class="sxs-lookup"><span data-stu-id="85629-108">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="85629-109">В Visual Studio диалоговое окно **Изменение проверки подлинности** доступно для новых веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="85629-109">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="85629-110">По умолчанию для новых веб-приложений в Visual Studio **Проверка подлинности не**выполняется.</span><span class="sxs-lookup"><span data-stu-id="85629-110">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="85629-111">Проекты, созданные без проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="85629-111">Projects created with no authentication:</span></span>

* <span data-ttu-id="85629-112">Не следует включать веб-страницы и пользовательский интерфейс для входа и выхода.</span><span class="sxs-lookup"><span data-stu-id="85629-112">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="85629-113">Не содержать код проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="85629-113">Don't contain authentication code.</span></span>

<a name="win"></a>

## <a name="windows-authentication"></a><span data-ttu-id="85629-114">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="85629-114">Windows Authentication</span></span>

<span data-ttu-id="85629-115">Проверка подлинности Windows указана для новых веб-приложений в `-au Windows` .NET Core CLI с параметром.</span><span class="sxs-lookup"><span data-stu-id="85629-115">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="85629-116">В Visual Studio диалоговое окно **Изменение проверки подлинности** предоставляет параметры **проверки подлинности Windows** .</span><span class="sxs-lookup"><span data-stu-id="85629-116">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="85629-117">Если выбрана проверка подлинности Windows, приложение настроено для использования [модуля IIS проверки подлинности Windows](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="85629-117">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="85629-118">Проверка подлинности Windows предназначена для веб-сайтов интрасети.</span><span class="sxs-lookup"><span data-stu-id="85629-118">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="85629-119">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="85629-119">Additional resources</span></span>

<span data-ttu-id="85629-120">В следующих статьях показано, как использовать код, созданный в шаблонах ASP.NET Core, использующих отдельные учетные записи пользователей.</span><span class="sxs-lookup"><span data-stu-id="85629-120">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="85629-121">Двухфакторная проверка подлинности с помощью SMS</span><span class="sxs-lookup"><span data-stu-id="85629-121">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="85629-122">Подтверждение учетной записи и восстановление пароля в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="85629-122">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="85629-123">Создание приложения ASP.NET Core с данными пользователя, защищенными с помощью авторизации</span><span class="sxs-lookup"><span data-stu-id="85629-123">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
