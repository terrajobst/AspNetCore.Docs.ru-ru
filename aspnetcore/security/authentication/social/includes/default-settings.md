---
ms.openlocfilehash: 2fe12027e7a5233cf01e6c412f7ee536d479facd
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665785"
---
<!--Don't update this for 2.2, use the 2.2 version --> Вызов [AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity) настраивает параметры схемы по умолчанию. [AddAuthentication(String)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_String_) перегружать наборы [DefaultScheme](/dotnet/api/microsoft.aspnetcore.authentication.authenticationoptions.defaultscheme) свойство. [AddAuthentication (действие&lt;AuthenticationOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) перегрузка позволяет настраивать параметры проверки подлинности, которые можно использовать для настройки схемы проверки подлинности по умолчанию для разных целей. Последующие вызовы `AddAuthentication` ранее настроенные переопределения [AuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions) свойства.

[AuthenticationBuilder](/dotnet/api/microsoft.aspnetcore.authentication.authenticationbuilder) методы расширения, которые регистрируют обработчик проверки подлинности может вызываться только один раз на схему проверки подлинности. Перегрузок, которые позволяют настраивать свойства схемы, имя схемы и отображаемое имя.
