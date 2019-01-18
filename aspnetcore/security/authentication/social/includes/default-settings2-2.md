---
ms.openlocfilehash: 733a41a76e289f8aed6ec2d246ed720e44808fec
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396354"
---
Вызов [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity) настраивает параметры схемы по умолчанию. [AddAuthentication(String)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_String_) перегружать наборы [DefaultScheme](/dotnet/api/microsoft.aspnetcore.authentication.authenticationoptions.defaultscheme) свойство. [AddAuthentication (действие&lt;AuthenticationOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) перегрузка позволяет настраивать параметры проверки подлинности, которые можно использовать для настройки схемы проверки подлинности по умолчанию для разных целей. Последующие вызовы `AddAuthentication` ранее настроенные переопределения [AuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions) свойства.

[AuthenticationBuilder](/dotnet/api/microsoft.aspnetcore.authentication.authenticationbuilder) методы расширения, которые регистрируют обработчик проверки подлинности может вызываться только один раз на схему проверки подлинности. Перегрузок, которые позволяют настраивать свойства схемы, имя схемы и отображаемое имя.
