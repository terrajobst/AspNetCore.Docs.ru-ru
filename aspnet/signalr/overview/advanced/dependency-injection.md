---
uid: signalr/overview/advanced/dependency-injection
title: Внедрение зависимостей в SignalR | Документация Майкрософт
author: bradygaster
description: Версии программного обеспечения используется в этом разделе 4.5 .NET SignalR для Visual Studio 2013 версии 2, предыдущие версии в этом разделе сведения о более ранних версиях...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 54e263e277852d2d478ce5bccd4164254498831a
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836614"
---
<a name="dependency-injection-in-signalr"></a>Внедрение зависимостей в SignalR
====================
по [Майк Уоссон](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Версии программного обеспечения, используемого в этом разделе
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR версии 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Предыдущие версии этого раздела
>
> Сведения о более ранних версий SignalR, см. в разделе [более старых версий SignalR](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
>
> Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы. Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).


Внедрение зависимостей является способ удаления жестких зависимостей между объектами, упрощая для замены зависимости объекта, либо для тестирования (с использованием макетов объектов) или для изменения поведения во время выполнения. Этом руководстве показано, как выполнить внедрения зависимостей для концентраторов SignalR. Также показано, как использовать контейнеры IoC с SignalR. IoC-контейнер — это стандартная структура для внедрения зависимостей.

## <a name="what-is-dependency-injection"></a>Что такое внедрение зависимостей?

Этот раздел можно пропустите, если вы уже знакомы с помощью внедрения зависимостей.

*Внедрение зависимостей* (DI) — это шаблон, где объекты не отвечают за создание собственных зависимостей. Ниже приведен простой пример для внедрения Зависимостей. Предположим, что у вас есть объект, который требуется для регистрации сообщений. Можно определить интерфейс ведения журнала:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

В объекте, можно создать `ILogger` запись сообщений в журнал:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Это работает, но это не рекомендуется. Если вы хотите заменить `FileLogger` с другим `ILogger` реализации, нужно будет только изменить `SomeComponent`. Допустим, что много других объектов используйте `FileLogger`, необходимо изменить все из них. Или если необходимо внести `FileLogger` является одноэлементным множеством, также необходимо внести изменения в приложении.

Лучшим подходом является «внедрение» `ILogger` в объект — например, с помощью аргумента конструктора:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Теперь объект не несет ответственности за выбора `ILogger` для использования. Вы можете swich `ILogger` реализации без изменения объекты, зависящие от нее.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Такая модель называется [внедрение через конструктор](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Другой шаблон — внедрение метод задания, где можно устанавливать для зависимости через метод или свойство.

## <a name="simple-dependency-injection-in-signalr"></a>Внедрение простого зависимостей в SignalR

Рассмотрим приложение чата из учебника [начало работы с SignalR](../getting-started/tutorial-getting-started-with-signalr.md). Ниже показан класс центра из этого приложения:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Предположим, что вам нужно хранить сообщения чата на сервере перед их отправкой. Можно определить интерфейс, который абстрагирует эту функцию и использовать внедрение Зависимостей для вставки интерфейса в `ChatHub` класса.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Единственная проблема состоит, что приложение SignalR непосредственно не создать концентраторы; SignalR создаст их самостоятельно. По умолчанию SignalR ожидает, что для класса концентратора конструктор без параметров. Тем не менее можно легко зарегистрировать функцию для создания экземпляров концентраторов и использовать эту функцию для выполнения внедрения Зависимостей. Зарегистрируйте функцию с помощью метода **GlobalHost.DependencyResolver.Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Теперь SignalR будет вызывать это анонимная функция, каждый раз, когда необходимо создать `ChatHub` экземпляра.

## <a name="ioc-containers"></a>Контейнеры IoC

Предыдущий код годится для простых случаях. Но все равно требовалось написать следующее:

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

В сложных приложениях с много зависимостей может потребоваться написать немало этот код «подключения». Этот код может быть трудно поддерживать, особенно в том случае, если зависимости являются вложенными. Это также трудно модульного теста.

Одним из решений является использование IoC-контейнер. IoC-контейнер — это программный компонент, который отвечает за управление зависимостями. Зарегистрируйте типов в контейнере и затем использовать для создания объектов контейнера. Контейнер автоматически определяет отношение зависимость. Кроме того, множество контейнеров инверсии управления позволяют управлять времени существования объектов и области.

> [!NOTE]
> «IoC» означает «инверсии элемента управления», который является общий шаблон, когда платформа вызывает в код приложения. Контейнер IoC создает объектов, который «инвертирует» обычного потока управления.


## <a name="using-ioc-containers-in-signalr"></a>Использование контейнеров IoC в SignalR

Приложения чата, вероятно, слишком простой использовать преимущества контейнер IoC. Вместо этого давайте взглянем на [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) образца.

Образец StockTicker определяет два основных класса:

- `StockTickerHub`: Класс концентратора, который управляет клиентских подключений.
- `StockTicker`: Единственный экземпляр, который содержит цены акций и периодически обновлять их.

`StockTickerHub` хранит ссылку на `StockTicker` одноэлементным множеством, хотя `StockTicker` хранит ссылку на **IHubConnectionContext** для `StockTickerHub`. Использует этот интерфейс для взаимодействия с `StockTickerHub` экземпляров. (Дополнительные сведения см. в разделе [передача сообщений с сервера с помощью SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)

Можно использовать контейнер IoC для немного клубка эти зависимости. Во-первых давайте упростим `StockTickerHub` и `StockTicker` классы. В следующем коде я закомментирован частей, нам не нужен.

Удалите конструктор без параметров из `StockTickerHub`. Вместо этого всегда используется DI создайте концентратор.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

Для StockTicker удалите одноэлементный экземпляр. Позже мы будем использовать контейнер IoC, управление временем существования StockTicker. Кроме того не открытый конструктор.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

Далее мы можем выполнить рефакторинг кода путем создания интерфейса для `StockTicker`. Мы будем использовать этот интерфейс для отделения `StockTickerHub` из `StockTicker` класса.

Visual Studio делает этот вид рефакторинга просто. Откройте файл StockTicker.cs, щелкните правой кнопкой мыши `StockTicker` объявление класса и выберите **рефакторинг** ... **Извлечение интерфейса**.

![](dependency-injection/_static/image1.png)

В **извлечение интерфейса** диалоговое окно, нажмите кнопку **выбрать все**. Оставьте остальные значения по умолчанию. Нажмите кнопку **ОК**.

![](dependency-injection/_static/image2.png)

Visual Studio создает новый интерфейс с именем `IStockTicker`, а также изменяет `StockTicker` для наследования от `IStockTicker`.

Откройте файл IStockTicker.cs и изменить интерфейс для **открытый**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

В `StockTickerHub` измените два экземпляра `StockTicker` для `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

Создание `IStockTicker` интерфейса не является обязательным, но я хотел показать, как внедрение Зависимостей позволяет сократить взаимозависимости между компонентами приложения.

## <a name="add-the-ninject-library"></a>Добавьте библиотеку Ninject

Существует множество контейнеров инверсии управления открытым исходным кодом для .NET. В этом учебнике я использую [Ninject](http://www.ninject.org/). (Включить других популярных библиотек [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), и [StructureMap ](http://docs.structuremap.net).)

С помощью диспетчера пакетов NuGet для установки [Ninject библиотеки](https://nuget.org/packages/Ninject/3.0.1.10). В Visual Studio из **средства** меню выберите **диспетчер пакетов NuGet** > **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Замените Сопоставитель зависимостей SignalR

Чтобы использовать Ninject в SignalR, создайте класс, производный от **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Этот класс переопределяет **GetService** и **GetServices** методы **DefaultDependencyResolver**. SignalR эти методы вызываются для создания различных объектов во время выполнения, включая экземпляры центра, а также различные службы, используемые внутри SignalR.

- **GetService** метод создает один экземпляр типа. Переопределите этот метод для вызова ядра Ninject **TryGet** метод. Если этот метод возвращает значение null, переключиться на Сопоставитель по умолчанию.
- **GetServices** метод создает коллекцию объектов указанного типа. Переопределите этот метод, чтобы объединить результаты из Ninject с результатами из Сопоставитель по умолчанию.

## <a name="configure-ninject-bindings"></a>Настройка привязок Ninject

Теперь мы будем использовать для объявления привязки типов Ninject.

Откройте класс Startup.cs приложения (либо созданного вручную согласно инструкциям пакета в `readme.txt`, или который был создан путем добавления проверки подлинности в проект). В `Startup.Configuration` метод, создайте контейнер Ninject, который вызывает Ninject *ядра*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Создайте экземпляр сопоставителя наших пользовательскую зависимость:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Создать привязку для `IStockTicker` следующим образом:

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

Этот код говорит две вещи. Во-первых, каждый раз, когда приложение должно `IStockTicker`, ядро следует создать экземпляр `StockTicker`. Во-вторых, `StockTicker` класс должен быть созданный объект в виде одноэлементного объекта. Ninject будет создать один экземпляр объекта и возвращают один и тот же экземпляр для каждого запроса.

Создать привязку для **IHubConnectionContext** следующим образом:

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Этот код creatres анонимную функцию, которая возвращает **IHubConnection**. **WhenInjectedInto** метод указывает Ninject, чтобы использовать эту функцию только в том случае, при создании `IStockTicker` экземпляров. Причина в том, что создает SignalR **IHubConnectionContext** экземпляров на внутреннем уровне и мы не хотим переопределить как SignalR создает их. Эта функция применяется только к нашей `StockTicker` класса.

Передайте в сопоставителе зависимостей в **MapSignalR** метод путем добавления конфигурации концентратора:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Обновите метод Startup.ConfigureSignalR в классе запуска образца, используя новый параметр:

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

Теперь SignalR будет использовать сопоставителя, указанного в **MapSignalR**, вместо Сопоставитель по умолчанию.

Ниже приведен полный код для `Startup.Configuration`.

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

Чтобы запустить приложение StockTicker в Visual Studio, нажмите клавишу F5. В окне браузера перейдите к `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

Приложение имеет ровно ту же функциональность, что перед. (Описание, см. в разделе [передача сообщений с сервера с помощью SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) Мы еще не изменили поведение; просто упростилась код для тестирования, поддержки и развиваться.
