---
uid: signalr/overview/advanced/dependency-injection
title: "Внедрение зависимостей в SignalR | Документы Microsoft"
author: MikeWasson
description: "Версии программного обеспечения используется в этом разделе Visual Studio 2013 .NET 4.5 SignalR предыдущие версии версии 2 в этом разделе сведения о предыдущих версиях..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-signalr"></a>Внедрение зависимостей в SignalR
====================
по [Mike Wasson](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Версии программного обеспечения, используемого в этом разделе
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR версии 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Предыдущие версии этого раздела
> 
> Сведения о более ранних версий SignalR см. в разделе [более ранних версий SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
> 
> Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы. Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).


Внедрение зависимостей способ для удаления жестких зависимостей между объектами, упрощая процесс для замены зависимости объекта, либо для тестирования (с помощью макетов объектов) или для изменения поведения во время выполнения. Этого учебника показано, как выполнить внедрения зависимостей для концентраторов SignalR. Также показано, как использовать контейнеры IoC с SignalR. Контейнер IoC — это стандартная структура для внедрения зависимости.

## <a name="what-is-dependency-injection"></a>Что такое внедрения зависимости

Этот раздел можно пропустите, если вы уже знакомы с внедрения зависимостей.

*Внедрение зависимостей* (DI) — это шаблон, где объекты, не отвечают за создание собственных зависимостей. Ниже приведен простой пример, чтобы стимулировать DI. Предположим, что имеется объект, который требуется для записи сообщений. Можно определить интерфейс ведения журнала:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Объект, можно создать `ILogger` сообщений в журнале:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Это работает, но это не рекомендуется. Если вы хотите заменить `FileLogger` с другим `ILogger` реализации, необходимо будет изменить `SomeComponent`. Supposing, много других объекты, используйте `FileLogger`, необходимо будет изменить их все. Или если необходимо внести `FileLogger` является одноэлементным множеством, вам также потребуется внести изменения в приложении.

Лучшим подходом является «вставка» `ILogger` в объекте — например, с помощью аргумента конструктора:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Теперь объект не является обязательной при выборе которой `ILogger` для использования. Вы можете swich `ILogger` реализации без изменения объектов, зависящих от нее.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Эта модель называется [внедрение конструктора](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Другой подход заключается внедрения setter задаются зависимостей через свойство или метод задания.

## <a name="simple-dependency-injection-in-signalr"></a>Внедрение зависимостей простой в SignalR

Рассмотрим приложение чата из учебника [Приступая к работе с SignalR](../getting-started/tutorial-getting-started-with-signalr.md). Вот классу hub из этого приложения.

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Предположим, что вы хотите хранить сообщения чата на сервере перед их отправкой. Может Определите интерфейс, который абстрагирует эту функцию и использовать для вставки в интерфейс DI `ChatHub` класса.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Единственная проблема состоит в том, что приложение SignalR не создаются непосредственно концентраторов; SignalR создает их. По умолчанию SignalR ожидает концентратора класса должен быть конструктор без параметров. Тем не менее вы можно легко зарегистрировать функцию для создания экземпляров концентратора и использовать эту функцию для выполнения DI. Зарегистрируйте эту функцию, вызвав **GlobalHost.DependencyResolver.Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Теперь SignalR будет вызывать это анонимная функция, каждый раз, когда он должен создать `ChatHub` экземпляра.

## <a name="ioc-containers"></a>Контейнеры IoC

Предыдущий код хорошо работает для простых случаях. Но по-прежнему должны были написать это:

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

В сложных приложениях с много зависимостей может потребоваться запись большого объема кода это «привязки». Этот код может быть трудно поддерживать, особенно в том случае, если зависимости являются вложенными. Также будет сложно модульного теста.

Единственное решение — использовать контейнер IoC. Контейнер IoC — это программный компонент, который отвечает за управление зависимостями. Зарегистрировать типы с контейнером и затем использовать для создания объектов контейнера. Контейнер автоматически решает, отношений зависимостей. Многие контейнеры IoC также позволяют управлять вещей, как время существования объектов и области.

> [!NOTE]
> «IoC» означает «инверсии элемента управления», который является общий шаблон, когда платформа вызывает в код приложения. Контейнер IoC создает объекты, который «инвертирует «обычного потока управления.


## <a name="using-ioc-containers-in-signalr"></a>Использование контейнеров IoC в SignalR

Приложения разговора, возможно, слишком простой для использования преимуществ контейнер IoC. Вместо этого, давайте взглянем на [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) образца.

Образец StockTicker определяет два основных класса:

- `StockTickerHub`: Концентратора класс, который управляет клиентских подключений.
- `StockTicker`: Одноэлементным. он содержит акций и периодически обновлять их.

`StockTickerHub`хранит ссылку на `StockTicker` одноэлементным, хотя `StockTicker` хранит ссылку на **IHubConnectionContext** для `StockTickerHub`. Использует этот интерфейс для взаимодействия с `StockTickerHub` экземпляров. (Дополнительные сведения см. в разделе [Server широковещательных пакетов с помощью ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)

Можно использовать контейнер IoC для немного untangle эти зависимости. Во-первых давайте упростить `StockTickerHub` и `StockTicker` классы. В следующем коде я закомментированы частей нам не нужен.

Удалите конструктор без параметров из `StockTickerHub`. Вместо этого мы всегда будем использовать DI для создания концентратора.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

Для StockTicker удалите одноэлементный экземпляр. Более поздней версии мы будем использовать для управления временем существования StockTicker контейнер IoC. Кроме того сделайте открытый конструктор.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

Далее мы можем выполнить рефакторинг кода путем создания интерфейса для `StockTicker`. Мы будем использовать этот интерфейс для отделения `StockTickerHub` из `StockTicker` класса.

Visual Studio делает этот тип рефакторинга просто. Откройте файл StockTicker.cs, щелкните правой кнопкой мыши `StockTicker` объявления класса и выберите **рефакторинг** ... **Извлечение интерфейса**.

![](dependency-injection/_static/image1.png)

В **извлечение интерфейса** диалоговое окно, нажмите кнопку **выделить все**. Оставьте другие значения по умолчанию. Нажмите кнопку **ОК**.

![](dependency-injection/_static/image2.png)

Visual Studio создает новый интерфейс с именем `IStockTicker`, а также изменяет `StockTicker` для наследования от `IStockTicker`.

Откройте файл IStockTicker.cs и изменить интерфейс для **открытый**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

В `StockTickerHub` класса, измените два экземпляра `StockTicker` для `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

Создание `IStockTicker` интерфейса не является обязательным, но нужно показать, как DI может помочь сократить взаимозависимости между компонентами приложения.

## <a name="add-the-ninject-library"></a>Добавьте библиотеку Ninject

Существует много контейнеры IoC открытым исходным кодом для .NET. В этом учебнике, я буду использовать [Ninject](http://www.ninject.org/). (Другие популярные библиотеки включают в себя [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), и [StructureMap ](http://docs.structuremap.net).)

Используйте диспетчер пакетов NuGet для установки [Ninject библиотеки](https://nuget.org/packages/Ninject/3.0.1.10). В Visual Studio из **средства** меню выберите **диспетчер пакетов библиотеки** | **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Замените Сопоставитель зависимостей SignalR

Чтобы использовать Ninject в SignalR, создайте класс, производный от **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Этот класс переопределяет **GetService** и **GetServices** методы **DefaultDependencyResolver**. SignalR вызывает эти методы для создания различных объектов во время выполнения, включая экземпляры центра, а также различные службы, используются внутренними механизмами SignalR.

- **GetService** метод создает один экземпляр типа. Переопределите этот метод для вызова ядра Ninject **TryGet** метод. Если этот метод возвращает значение null, вернутся к Сопоставитель по умолчанию.
- **GetServices** метод создает коллекцию объектов определенного типа. Переопределите этот метод, чтобы объединить результаты из Ninject с результатами из Сопоставитель по умолчанию.

## <a name="configure-ninject-bindings"></a>Настройте привязки Ninject

Теперь мы будем использовать Ninject для объявления типа привязки.

Откройте класс приложения в файле Startup.cs (либо созданным вручную согласно инструкциям пакета `readme.txt`, или который был создан путем добавления в проект проверки подлинности). В `Startup.Configuration` метод, создать контейнер Ninject, который вызывает Ninject *ядра*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Создайте экземпляр распознавателя нашей пользовательскую зависимость:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Создайте привязку для `IStockTicker` следующим образом:

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

Этот код означает следующее. Во-первых, каждый раз, когда приложение должно `IStockTicker`, ядро должен создать экземпляр `StockTicker`. Во-вторых, `StockTicker` класс должен быть созданный как одноэлементный объект. Ninject создать один экземпляр объекта и возвращают один и тот же экземпляр для каждого запроса.

Создайте привязку для **IHubConnectionContext** следующим образом:

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Этот код creatres анонимную функцию, которая возвращает **IHubConnection**. **WhenInjectedInto** метод указывает Ninject, чтобы использовать эту функцию только при создании `IStockTicker` экземпляров. Причина заключается в том, что создает SignalR **IHubConnectionContext** экземпляров внутренне, и мы не хотим переопределить как SignalR создает их. Эта функция применяется только к нашей `StockTicker` класса.

Передайте Сопоставитель зависимостей в **MapSignalR** метод путем добавления конфигурацию концентратора:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Добавьте в метод Startup.ConfigureSignalR в классе при запуске образца новый параметр:

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

Теперь использует сопоставителя, указанного в SignalR **MapSignalR**, вместо Сопоставитель по умолчанию.

Ниже приведен полный код для `Startup.Configuration`.

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

Чтобы запустить приложение StockTicker в Visual Studio, нажмите клавишу F5. В окне браузера перейдите к `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

Приложение имеет точно ту же функциональность, что перед. (Описание см. в разделе [Server широковещательных пакетов с помощью ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) Мы не было изменено поведение. только что внесли код удобнее для тестирования, обслуживания и развития.
