---
title: Параметры проверки подлинности OSS сообщества для ASP.NET Core
author: rick-anderson
description: Ознакомьтесь с возможностями проверки подлинности с открытым исходным кодом ASP.NET Core.
ms.author: riande
ms.date: 02/15/2019
uid: security/authentication/community
ms.openlocfilehash: e25df794bdff8f904382e7a299755ae4c23b892e
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891751"
---
# <a name="community-oss-authentication-options-for-aspnet-core"></a><span data-ttu-id="b9359-103">Параметры проверки подлинности OSS сообщества для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9359-103">Community OSS authentication options for ASP.NET Core</span></span>

<span data-ttu-id="b9359-104">Эта страница содержит параметры проверки подлинности предоставляемых системой сообщества открытым исходным кодом для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b9359-104">This page contains community-provided, open source authentication options for ASP.NET Core.</span></span> <span data-ttu-id="b9359-105">Эта страница периодически обновляется как новые поставщики, становятся доступными.</span><span class="sxs-lookup"><span data-stu-id="b9359-105">This page is periodically updated as new providers become available.</span></span>

## <a name="oss-authentication-providers"></a><span data-ttu-id="b9359-106">Поставщики проверки подлинности OSS</span><span class="sxs-lookup"><span data-stu-id="b9359-106">OSS authentication providers</span></span>

<span data-ttu-id="b9359-107">В списке ниже отсортированы по алфавиту.</span><span class="sxs-lookup"><span data-stu-id="b9359-107">The list below is sorted alphabetically.</span></span>

| <span data-ttu-id="b9359-108">name</span><span class="sxs-lookup"><span data-stu-id="b9359-108">Name</span></span> | <span data-ttu-id="b9359-109">Описание</span><span class="sxs-lookup"><span data-stu-id="b9359-109">Description</span></span> |
| ---- | ----------- |
| [<span data-ttu-id="b9359-110">AspNet.Security.OpenIdConnect.Server (ASOS)</span><span class="sxs-lookup"><span data-stu-id="b9359-110">AspNet.Security.OpenIdConnect.Server (ASOS)</span></span>](https://github.com/aspnet-contrib/AspNet.Security.OpenIdConnect.Server) | <span data-ttu-id="b9359-111">ASOS — это низкого уровня, основанная на протокол OpenID Connect server платформа для ASP.NET Core и OWIN и Katana.</span><span class="sxs-lookup"><span data-stu-id="b9359-111">ASOS is a low-level, protocol-first OpenID Connect server framework for ASP.NET Core and OWIN/Katana.</span></span> |
| [<span data-ttu-id="b9359-112">Cierge</span><span class="sxs-lookup"><span data-stu-id="b9359-112">Cierge</span></span>](https://github.com/pwdless/Cierge) | <span data-ttu-id="b9359-113">Cierge — это сервер, OpenID Connect, который обрабатывает регистрации пользователя, имя входа, профили, управления и входа в социальные сети.</span><span class="sxs-lookup"><span data-stu-id="b9359-113">Cierge is an OpenID Connect server that handles user signup, login, profiles, management, and social logins.</span></span> |
| [<span data-ttu-id="b9359-114">Gluu сервера</span><span class="sxs-lookup"><span data-stu-id="b9359-114">Gluu Server</span></span>](https://gluu.org/) | <span data-ttu-id="b9359-115">Предприятия все будет готово, с открытым кодом для удостоверения, управления доступом (IAM), а единый вход (SSO).</span><span class="sxs-lookup"><span data-stu-id="b9359-115">Enterprise ready, open source software for identity, access management (IAM), and single sign-on (SSO).</span></span> <span data-ttu-id="b9359-116">Дополнительные сведения см. в разделе [документации по продукту Gluu](https://gluu.org/docs/).</span><span class="sxs-lookup"><span data-stu-id="b9359-116">For more information, see the [Gluu Product Documentation](https://gluu.org/docs/).</span></span> |
| [<span data-ttu-id="b9359-117">IdentityServer</span><span class="sxs-lookup"><span data-stu-id="b9359-117">IdentityServer</span></span>](https://identityserver.io/) | <span data-ttu-id="b9359-118">Сервер — это платформа OpenID Connect и OAuth 2.0, для ASP.NET Core, официально сертифицирована Фондом OpenID и под контроль для .NET Foundation.</span><span class="sxs-lookup"><span data-stu-id="b9359-118">IdentityServer is an OpenID Connect and OAuth 2.0 framework for ASP.NET Core, officially certified by the OpenID Foundation and under governance of the .NET Foundation.</span></span> <span data-ttu-id="b9359-119">Дополнительные сведения см. в разделе [приветствует IdentityServer4 (документация)](https://identityserver4.readthedocs.io/en/latest/).</span><span class="sxs-lookup"><span data-stu-id="b9359-119">For more information, see [Welcome to IdentityServer4 (Documentation)](https://identityserver4.readthedocs.io/en/latest/).</span></span> |
| [<span data-ttu-id="b9359-120">OpenIddict</span><span class="sxs-lookup"><span data-stu-id="b9359-120">OpenIddict</span></span>](https://github.com/openiddict/openiddict-core) | <span data-ttu-id="b9359-121">OpenIddict — это простой в использовании сервер OpenID Connect для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b9359-121">OpenIddict is an easy-to-use OpenID Connect server for ASP.NET Core.</span></span> |

<span data-ttu-id="b9359-122">Для добавления поставщика, [редактировать эту страницу](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Faspnet%2FDocs%2Fedit%2Fmaster%2Faspnetcore%2Fsecurity%2Fauthentication%2Fcommunity.md).</span><span class="sxs-lookup"><span data-stu-id="b9359-122">To add a provider, [edit this page](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Faspnet%2FDocs%2Fedit%2Fmaster%2Faspnetcore%2Fsecurity%2Fauthentication%2Fcommunity.md).</span></span>
