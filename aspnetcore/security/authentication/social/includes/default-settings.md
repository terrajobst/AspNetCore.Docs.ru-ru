---
ms.openlocfilehash: a7be92adffc06ac0f25b84ea15d1a8c4896f4f9d
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/18/2019
ms.locfileid: "54397089"
---
<!--Don't update this for 2.2, use the 2.2 version --> Вызов [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity) настраивает параметры схемы по умолчанию. [AddAuthentication(String)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_String_) перегружать наборы [DefaultScheme](/dotnet/api/microsoft.aspnetcore.authentication.authenticationoptions.defaultscheme) свойство. [AddAuthentication (действие&lt;AuthenticationOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) перегрузка позволяет настраивать параметры проверки подлинности, которые можно использовать для настройки схемы проверки подлинности по умолчанию для разных целей. Последующие вызовы `AddAuthentication` ранее настроенные переопределения [AuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions) свойства.

[AuthenticationBuilder](/dotnet/api/microsoft.aspnetcore.authentication.authenticationbuilder) методы расширения, которые регистрируют обработчик проверки подлинности может вызываться только один раз на схему проверки подлинности. Перегрузок, которые позволяют настраивать свойства схемы, имя схемы и отображаемое имя.
