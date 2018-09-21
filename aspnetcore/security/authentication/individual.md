---
title: Статьи на основе проектов ASP.NET Core, созданных с помощью отдельных учетных записей пользователей
author: rick-anderson
description: Ознакомьтесь со статьями, на основе проектов ASP.NET Core, созданных с помощью отдельных учетных записей пользователей.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: ac843342ffc73632fbf9f6359c6c1a5878dcef0d
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523068"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="e1ad3-103">Статьи на основе проектов ASP.NET Core, созданных с помощью отдельных учетных записей пользователей</span><span class="sxs-lookup"><span data-stu-id="e1ad3-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="e1ad3-104">Удостоверение ASP.NET Core включается в шаблоны проектов в Visual Studio с параметром «Учетные записи отдельных пользователей».</span><span class="sxs-lookup"><span data-stu-id="e1ad3-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="e1ad3-105">Шаблоны проверки подлинности доступны в .NET Core CLI с помощью `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="e1ad3-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<a name="no"></a>
## <a name="no-authentication"></a><span data-ttu-id="e1ad3-106">Без проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="e1ad3-106">No Authentication</span></span>

<span data-ttu-id="e1ad3-107">Указана проверка подлинности в .NET Core CLI с `-au` параметр.</span><span class="sxs-lookup"><span data-stu-id="e1ad3-107">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="e1ad3-108">В Visual Studio **изменить способ проверки подлинности** диалоговое окно доступно для новых веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="e1ad3-108">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="e1ad3-109">Значение по умолчанию для новых веб-приложений в Visual Studio — **без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="e1ad3-109">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="e1ad3-110">Проекты, созданные без проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="e1ad3-110">Projects created with no authentication:</span></span>

* <span data-ttu-id="e1ad3-111">Не содержат веб-страницы и пользовательский Интерфейс для входа систему и выхода.</span><span class="sxs-lookup"><span data-stu-id="e1ad3-111">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="e1ad3-112">Не содержат код проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="e1ad3-112">Don't contain authentication code.</span></span>

<a name="win"></a>
## <a name="windows-authentication"></a><span data-ttu-id="e1ad3-113">Проверка подлинности Windows</span><span class="sxs-lookup"><span data-stu-id="e1ad3-113">Windows Authentication</span></span>

<span data-ttu-id="e1ad3-114">Для новых веб-приложений в .NET Core CLI с задана проверка подлинности Windows `-au Windows` параметр.</span><span class="sxs-lookup"><span data-stu-id="e1ad3-114">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="e1ad3-115">В Visual Studio **изменить способ проверки подлинности** диалоговое окно предоставляет **проверки подлинности Windows** параметры.</span><span class="sxs-lookup"><span data-stu-id="e1ad3-115">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="e1ad3-116">Если выбран метод проверки подлинности Windows, приложение настроено для использования [модуль IIS для Windows](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="e1ad3-116">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="e1ad3-117">Проверка подлинности Windows предназначен для веб-сайтов интрасети.</span><span class="sxs-lookup"><span data-stu-id="e1ad3-117">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e1ad3-118">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e1ad3-118">Additional resources</span></span>

<span data-ttu-id="e1ad3-119">В следующих статьях описывается, как использовать код, созданный в шаблонах ASP.NET Core, использующих отдельных учетных записей пользователей:</span><span class="sxs-lookup"><span data-stu-id="e1ad3-119">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="e1ad3-120">Двухфакторная проверка подлинности с помощью SMS</span><span class="sxs-lookup"><span data-stu-id="e1ad3-120">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="e1ad3-121">Подтверждение учетной записи и восстановление пароля в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e1ad3-121">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="e1ad3-122">Создание приложения ASP.NET Core с помощью данных пользователя с помощью авторизации</span><span class="sxs-lookup"><span data-stu-id="e1ad3-122">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
