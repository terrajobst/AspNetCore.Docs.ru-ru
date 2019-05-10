---
title: Статьи на основе проектов ASP.NET Core, созданных с помощью отдельных учетных записей пользователей
author: rick-anderson
description: Ознакомьтесь со статьями, на основе проектов ASP.NET Core, созданных с помощью отдельных учетных записей пользователей.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: f9c1be16386da935382275815bb5fd5c72894b1c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892531"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="5de39-103">Статьи на основе проектов ASP.NET Core, созданных с помощью отдельных учетных записей пользователей</span><span class="sxs-lookup"><span data-stu-id="5de39-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="5de39-104">Удостоверение ASP.NET Core включается в шаблоны проектов в Visual Studio с параметром «Учетные записи отдельных пользователей».</span><span class="sxs-lookup"><span data-stu-id="5de39-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="5de39-105">Шаблоны проверки подлинности доступны в .NET Core CLI с помощью `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="5de39-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="5de39-106">См. в разделе [проблема GitHub](https://github.com/aspnet/AspNetCore/issues/5833) для проверки подлинности веб-API.</span><span class="sxs-lookup"><span data-stu-id="5de39-106">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5833) for web API authentication.</span></span>

<a name="no"></a>

## <a name="no-authentication"></a><span data-ttu-id="5de39-107">Без проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="5de39-107">No Authentication</span></span>

<span data-ttu-id="5de39-108">Указана проверка подлинности в .NET Core CLI с `-au` параметр.</span><span class="sxs-lookup"><span data-stu-id="5de39-108">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="5de39-109">В Visual Studio **изменить способ проверки подлинности** диалоговое окно доступно для новых веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="5de39-109">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="5de39-110">Значение по умолчанию для новых веб-приложений в Visual Studio — **без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="5de39-110">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="5de39-111">Проекты, созданные без проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="5de39-111">Projects created with no authentication:</span></span>

* <span data-ttu-id="5de39-112">Не содержат веб-страницы и пользовательский Интерфейс для входа систему и выхода.</span><span class="sxs-lookup"><span data-stu-id="5de39-112">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="5de39-113">Не содержат код проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="5de39-113">Don't contain authentication code.</span></span>

<a name="win"></a>

## <a name="windows-authentication"></a><span data-ttu-id="5de39-114">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="5de39-114">Windows Authentication</span></span>

<span data-ttu-id="5de39-115">Для новых веб-приложений в .NET Core CLI с задана проверка подлинности Windows `-au Windows` параметр.</span><span class="sxs-lookup"><span data-stu-id="5de39-115">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="5de39-116">В Visual Studio **изменить способ проверки подлинности** диалоговое окно предоставляет **проверки подлинности Windows** параметры.</span><span class="sxs-lookup"><span data-stu-id="5de39-116">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="5de39-117">Если выбран метод проверки подлинности Windows, приложение настроено для использования [модуль IIS для Windows](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="5de39-117">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="5de39-118">Проверка подлинности Windows предназначен для веб-сайтов интрасети.</span><span class="sxs-lookup"><span data-stu-id="5de39-118">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5de39-119">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5de39-119">Additional resources</span></span>

<span data-ttu-id="5de39-120">В следующих статьях описывается, как использовать код, созданный в шаблонах ASP.NET Core, использующих отдельных учетных записей пользователей:</span><span class="sxs-lookup"><span data-stu-id="5de39-120">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="5de39-121">Двухфакторная проверка подлинности с помощью SMS</span><span class="sxs-lookup"><span data-stu-id="5de39-121">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="5de39-122">Подтверждение учетной записи и восстановление пароля в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5de39-122">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="5de39-123">Создание приложения ASP.NET Core с помощью данных пользователя с помощью авторизации</span><span class="sxs-lookup"><span data-stu-id="5de39-123">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
