---
title: "Приступая к работе с ASP.NET Core и Entity Framework 6"
author: tdykstra
description: "В этой статье показано, как использовать Entity Framework 6 в приложении ASP.NET Core."
keywords: EF ASP.NET Core, Entity Framework 6
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.assetid: 016cc836-4c43-45a4-b9a7-9efaf53350df
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: e186568e27c067e29985b8a286e26b87c3186ac4
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a>Приступая к работе с ASP.NET Core и Entity Framework 6

По [Paweł Grudzień](https://github.com/pgrudzien12), [Pontifex Дэмьен](https://github.com/DamienPontifex), и [Tom Dykstra](https://github.com/tdykstra)

В этой статье показано, как использовать Entity Framework 6 в приложении ASP.NET Core.

## <a name="overview"></a>Обзор

Для использования платформы Entity Framework 6, проекта должно выполнять компиляцию для .NET Framework, как Entity Framework 6 не поддерживает .NET Core. Если вам требуются такие функции кросс платформенных необходимо будет обновить до [Entity Framework Core](https://docs.efproject.net).

Для использования платформы Entity Framework 6 в приложении ASP.NET Core рекомендуется поместить EF6 контекста и классы моделей в библиотеке классов проекта, предназначенного полной платформы. Добавьте ссылку на библиотеку классов из проекта ASP.NET Core. См. в образце [решения Visual Studio с проектами EF6 и ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).

Контекст EF6 нельзя поместить в проекте ASP.NET Core, потому что .NET Core проектов не поддерживают все функциональные возможности, EF6 команды, такие как *Enable-Migrations* требуется.

Независимо от типа проекта найдите контекстом EF6 средства EF6 командной строки работают с EF6 контекстом. Например `Scaffold-DbContext` доступна только в Entity Framework Core. Если необходимо выполнить реконструирование базы данных в модель EF6, см. раздел [Code First для существующей базы данных](https://msdn.microsoft.com/jj200620).

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a>Справочник по полной версии .NET framework и EF6 в проекте ASP.NET Core

Проект ASP.NET Core должен ссылаться на .NET framework и EF6. Например *.csproj* файл проекта ASP.NET Core будет выглядеть примерно следующим образом (показаны только значимые части файла).

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

При создании нового проекта используйте **веб-приложения ASP.NET Core (.NET Framework)** шаблона.

## <a name="handle-connection-strings"></a>Дескриптор строки подключения

EF6 средства командной строки, которые будут использоваться в проекте библиотеки классов EF6 требуется конструктор по умолчанию, поэтому можно создать экземпляр контекста. Но, возможно, потребуется указать строку подключения для использования в проекте ASP.NET Core, в этом случае в контексте конструктор должен иметь параметр, который позволяет передавать в строке подключения. Ниже приведен пример.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

Поскольку EF6 контекст не имеет конструктора без параметров, EF6 проект должен предоставить реализацию [IDbContextFactory](https://msdn.microsoft.com/library/hh506876). Средства командной строки EF6 находит и использовать эту реализацию, поэтому можно создать экземпляр контекста. Ниже приведен пример.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

В этом образце кода `IDbContextFactory` реализация передает в строку подключения, жестко. Это строка подключения, который будет использовать средства командной строки. Может потребоваться реализовать стратегию, чтобы гарантировать, что библиотека классов используется та же строка подключения, которая использует вызывающему приложению. Например может получить значение из переменной среды в обоих проектах.

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a>Настройка внедрение зависимостей в проекте ASP.NET Core

В проекте Core *файла Startup.cs* файл, настроить контекст EF6 для внедрения зависимости (DI) в `ConfigureServices`. Объекты контекста EF следует задавать для времени жизни для каждого запроса.

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

Затем можно получить экземпляр контекста в контроллеры с помощью DI. Код похожа на то, что можно написать для контекста EF:

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a>Образец приложения

Рабочий пример приложения см. в разделе [образец решения Visual Studio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) , прилагаемый к данной статье.

В этом примере можно создать с нуля в Visual Studio выполните следующие действия:

* Создайте решение.

* **Добавление нового проекта > Web > веб-приложения ASP.NET Core (.NET Framework)**

* **Добавить новый проект > Windows классического > Библиотека (.NET Framework) классов**

* В **консоль диспетчера пакетов** (PMC) для обоих проектов, выполните команду `Install-Package Entityframework`.

* В проекте библиотеки классов, создать классы модели данных, класс контекста и реализация `IDbContextFactory`.

* В PMC проект библиотеки классов, выполните команды `Enable-Migrations` и `Add-Migration Initial`. Если проект ASP.NET Core установлен как автозагружаемый проект, добавить `-StartupProjectName EF6` для этих команд.

* В проекте Core добавьте ссылку на проект в проект библиотеки классов.

* В проекте ядра в *файла Startup.cs*, Регистрация контекста для DI.

* В проекте ядра в *appsettings.json*, добавьте строку подключения.

* Добавьте в проект Core, контроллера и представлений, чтобы убедиться, что могут читать и записывать данные. (Обратите внимание, что ASP.NET Core MVC функции формирования шаблонов не будет работать с EF6 контекст ссылки из библиотеки классов.)

## <a name="summary"></a>Сводка

В этой статье был дан основные рекомендации по использованию платформы Entity Framework 6 в приложении ASP.NET Core.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Entity Framework — конфигурация на основе кода](https://msdn.microsoft.com/data/jj680699.aspx)
