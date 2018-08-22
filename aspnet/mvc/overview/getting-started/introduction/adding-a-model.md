---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Добавление модели | Документация Майкрософт
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7326cefd52feb63fc2f602210ed560cdf28706a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835423"
---
<a name="adding-a-model"></a>Добавление модели
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

В этом разделе вы добавите некоторые классы для управления фильмами в базе данных. Эти классы будут представлять &quot;модели&quot; входит приложение ASP.NET MVC.

Вы используете .NET Framework технологий доступа к данным, известный как [Entity Framework](https://docs.microsoft.com/ef/) определить и работать с этими классами модели. Поддерживает Entity Framework (часто обозначается как EF), называется парадигмы разработки *Code First*. Во-первых, код позволяет создавать объекты модели путем написания простых классов. (Они также называются классами POCO из &quot;обычные старые объекты CLR.&quot;) Затем можно создавать базы данных, созданной в режиме реального времени из классов, который позволяет очень простой и быстрой разработки рабочего процесса. Вы должны сначала создать базу данных, можно по-прежнему выполнить для этого учебника, чтобы узнать о разработке приложений MVC и EF. После этого можно отследить Tom Fizmakens [формирование шаблонов ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) руководство, охватывающее первый подход базы данных.

## <a name="adding-model-classes"></a>Добавление классов модели

В **обозревателе решений**, щелкните правой кнопкой мыши *моделей* папку, выберите **добавить**, а затем выберите **класс**.

![](adding-a-model/_static/image1.png)

Введите *класс* имя &quot;фильма&quot;.

Добавьте следующие пять свойства `Movie` класса:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Мы будем использовать `Movie` класс для представления фильмов в базе данных. Каждый экземпляр `Movie` объекта будет соответствовать ряду в таблице базы данных, а каждое свойство `Movie` класс сопоставляется со столбцом в таблице.

Примечание: Чтобы использовать System.Data.Entity и связанный класс, необходимо установить [пакет NuGet Entity Framework](https://www.nuget.org/packages/EntityFramework/). Перейдите по ссылке для получения дальнейших инструкций.

В этом же файле добавьте следующий `MovieDBContext` класса:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

`MovieDBContext` Класс представляет контекст базы данных movie Entity Framework, который обрабатывает получение, хранения и обновления `Movie` экземпляров в базе данных класса. `MovieDBContext` Является производным от `DbContext` базовый класс, предоставляемый платформой Entity Framework.

Чтобы иметь возможность ссылаться на `DbContext` и `DbSet`, необходимо добавить следующие `using` инструкция в верхней части файла:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Это можно сделать вручную, добавив в с помощью инструкции, или вы можете наведите указатель мыши красной волнистой линией, нажав кнопку `Show potential fixes` и нажмите кнопку `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Примечание: Некоторые неиспользуемые `using` инструкций будут удалены. Visual Studio будет отображать неиспользуемые зависимости серым. Вы можете удалить неиспользуемые зависимости навести указатель мыши на серую зависимости, нажав кнопку `Show potential fixes` и нажмите кнопку **удалить неиспользуемые директивы using.**

![](adding-a-model/_static/image3.png)

Наконец, мы добавили модели (M в MVC). В следующем разделе вы будете работать со строкой подключения базы данных.

> [!div class="step-by-step"]
> [Назад](adding-a-view.md)
> [Вперед](creating-a-connection-string.md)
