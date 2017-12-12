---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: "Добавление модели | Документы Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 13aab58e86829a8d4accd1d304420dcb34ffa472
ms.sourcegitcommit: ec9371e2fbfcb8d62e7e7cae69e7752f3f205385
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2017
---
<a name="adding-a-model"></a>Добавление модели
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

В этом разделе вы добавите некоторые классы для управления фильмов в базе данных. Эти классы будет &quot;модель&quot; частью приложение ASP.NET MVC.

Используемой технологии доступа к данным .NET Framework, известный как [Entity Framework](https://docs.microsoft.com/ef/) для определения и работы с этими классами модели. Поддерживает платформы Entity Framework (часто обозначается как EF), который называется парадигмы разработки *Code First*. Код сначала позволяет создавать объекты модели путем написания простых классов. (Они также известны как классов POCO из &quot;объектов plain old CLR.&quot;) Затем можно установить базу данных создан автоматически из классов, который позволяет очень простой и быстрой разработки рабочего процесса. Если не требуется сначала создать базу данных, по-прежнему можно выполнить этот учебник, чтобы узнать о разработке приложений MVC и EF. После этого можно отследить Tom Fizmakens [формирование шаблонов ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) руководство, в котором описывается первый подход базы данных.

## <a name="adding-model-classes"></a>Добавление классов модели

В **обозревателе решений**, щелкните правой кнопкой мыши *моделей* выберите **добавить**и выберите **класса**.

![](adding-a-model/_static/image1.png)

Введите *класса* имя &quot;фильма&quot;.

Добавьте следующие пять свойства `Movie` класса:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Мы будем использовать `Movie` класс для представления фильмов в базе данных. Каждый экземпляр `Movie` объекта будет соответствовать строки в таблицу базы данных и каждое свойство `Movie` класса сопоставляются со столбцом в таблице.

Примечание: Чтобы использовать System.Data.Entity и связанного класса, необходимо установить [пакет NuGet Entity Framework](https://www.nuget.org/packages/EntityFramework/). Перейдите по ссылке для получения дальнейших инструкций.

В том же файле добавьте следующий `MovieDBContext` класса:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

`MovieDBContext` Класс представляет контекст базы данных Entity Framework фильм, который обрабатывает выборка, хранения и обновления `Movie` класса экземпляров в базе данных. `MovieDBContext` Является производным от `DbContext` базовый класс, предоставляемый платформой Entity Framework.

Чтобы иметь возможность ссылаться на `DbContext` и `DbSet`, необходимо добавить следующие `using` инструкции в верхней части файла:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Это можно сделать вручную, добавив в с помощью инструкции, или можете наведите указатель мыши на красной волнистой линией, нажав кнопку `Show potential fixes` и нажмите кнопку`using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Примечание: Некоторые неиспользуемые `using` инструкций будут удалены. Visual Studio будет отображать неиспользуемые зависимости серым. Можно удалить зависимости unnused навести указатель мыши на серой зависимости, щелкните `Show potential fixes` и нажмите кнопку **удалить неиспользуемые директивы using.**

![](adding-a-model/_static/image3.png)

Наконец, мы добавили модели (M в MVC). В следующем разделе вы ознакомитесь с строку подключения базы данных.

>[!div class="step-by-step"]
[Назад](adding-a-view.md)
[Вперед](creating-a-connection-string.md)
