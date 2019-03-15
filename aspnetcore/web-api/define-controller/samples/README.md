---
ms.openlocfilehash: 07abb12af390c0f2a50e98fc5e53545b6635f968
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665513"
---
# <a name="aspnet-core-web-api-controller-sample"></a>Пример контроллера веб-API ASP.NET Core

Этот пример приложения состоит из следующих проектов:

- **WebApiSample.Api.22**: проект ASP.NET Core 2.2, предназначенный для .NET Core 2.2.
- **WebApiSample.Api.21**: проект ASP.NET Core 2.1, предназначенный для .NET Core 2.1.
- **WebApiSample.Api.Pre21**: проект ASP.NET Core 2.0, предназначенный для .NET Core 2.0.
- **WebApiSample.DataAccess**: библиотека классов .NET Standard 2.0, которая служит в качестве уровня доступа к данным для двух проектов веб-API.

В этом примере показаны различные способы создания контроллера веб-API:

- [Создание класса, производного от ControllerBase](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [Аннотирование класса атрибутом ApiControllerAttribute](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
