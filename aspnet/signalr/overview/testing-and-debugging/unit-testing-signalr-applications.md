---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Модульное тестирование приложений SignalR | Документация Майкрософт
author: pfletcher
description: В этой статье описывается использование функций Unit Testing SignalR 2.0.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: ba8f5d4577403fe9765641d7ee5d88bde045680a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910828"
---
<a name="unit-testing-signalr-applications"></a>Модульное тестирование приложений SignalR
====================
по [Патрик Флетчера](https://github.com/pfletcher)

> В этой статье описывается использование функции Unit Testing SignalR 2.
>
> ## <a name="software-versions-used-in-this-topic"></a>Версии программного обеспечения, используемого в этом разделе
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR версии 2
>
>
>
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
>
> Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы. Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Модульное тестирование приложений SignalR

Функции модульных тестов в SignalR 2 можно использовать для создания модульных тестов для приложения SignalR. SignalR 2 включает [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) интерфейс, который может использоваться для создания объекта макеты для имитации свои методы концентратора для тестирования.

В этом разделе вы добавите модульных тестов для приложения, созданного в [учебнике Приступая к работе](../getting-started/tutorial-getting-started-with-signalr.md) с помощью [XUnit.net](https://github.com/xunit/xunit) и [Moq](https://github.com/Moq/moq4).

XUnit.net будет использоваться для контроля теста; MOQ будет использоваться для создания [макетирование](http://en.wikipedia.org/wiki/Mock_object) объект для тестирования. Можно использовать другие платформы макетирования, при необходимости; [NSubstitute](http://nsubstitute.github.io/) также хорошо подходит. Этом руководстве показано, как настроить макета объекта двумя способами: во-первых, с помощью `dynamic` объекта (появился в .NET Framework 4), во-вторых, с помощью интерфейса.

### <a name="contents"></a>Описание

Этот учебник содержит следующие разделы.

- [Модульное тестирование с помощью динамической](#dynamic)
- [Модульное тестирование по типу](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Модульное тестирование с помощью динамической

В этом разделе вы добавите модульный тест для приложения, созданного в [учебнике Приступая к работе](../getting-started/tutorial-getting-started-with-signalr.md) с помощью динамического объекта.

1. Установка [расширения средство запуска XUnit](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) для Visual Studio 2013.
2. Либо выполните [учебнике Приступая к работе](../getting-started/tutorial-getting-started-with-signalr.md), или загрузить готовое приложение из [коллекции кода MSDN](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Если вы используете версию загрузки, Приступая к работе приложения, откройте **консоль диспетчера пакетов** и нажмите кнопку **восстановить** Добавление SignalR пакета в проект.

    ![Восстановление пакетов](unit-testing-signalr-applications/_static/image1.png)
4. Добавление проекта к решению для модульного теста. Щелкните правой кнопкой мыши решение в **обозревателе решений** и выберите **добавить**, **новый проект...** . В разделе **C#** выберите **Windows** узла. Выберите **библиотека классов**. Назовите новый проект **TestLibrary** и нажмите кнопку **ОК**.

    ![Создание тестов библиотеки](unit-testing-signalr-applications/_static/image2.png)
5. Добавьте ссылку в тестовом проекте библиотеки в проект SignalRChat. Щелкните правой кнопкой мыши **TestLibrary** проекта и выберите **добавить**, **ссылку...** . Выберите **проекты** узле **решение** узел и проверьте **SignalRChat**. Нажмите кнопку **ОК**.

    ![Добавить ссылку на проект](unit-testing-signalr-applications/_static/image3.png)
6. Добавьте пакеты SignalR, Moq и XUnit для **TestLibrary** проекта. В **консоль диспетчера пакетов**, задайте **проект по умолчанию** раскрывающийся список, чтобы **TestLibrary**. Выполните следующие команды в окне консоли:

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Установка пакетов](unit-testing-signalr-applications/_static/image4.png)
7. Создайте тестовый файл. Щелкните правой кнопкой мыши **TestLibrary** проекта и нажмите кнопку **добавить...** , **Класс**. Назовите новый класс **Tests.cs**.
8. Замените содержимое Tests.cs следующим кодом.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    В приведенном выше коде тестовый клиент создается с помощью `Mock` объекта из [Moq](https://github.com/Moq/moq4) библиотеки типа [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (в SignalR 2.1, назначьте `dynamic` для типа параметр.) `IHubCallerConnectionContext` Интерфейс является прокси-объект, с помощью которого можно вызывать методы на клиент. `broadcastMessage` Функция затем определена для макетов клиента, чтобы к нему можно обращаться `ChatHub` класса. Затем вызывается обработчик тестов `Send` метод `ChatHub` класс, который в свою очередь вызывает макеты `broadcastMessage` функции.
9. Постройте решение, нажав клавишу **F6**.
10. Выполните модульный тест. В Visual Studio выберите **теста**, **Windows**, **обозреватель тестов**. Щелкните правой кнопкой мыши в окне обозревателя тестов **HubsAreMockableViaDynamic** и выберите **запуск выбранных тестов**.

    ![Обозреватель тестов](unit-testing-signalr-applications/_static/image5.png)
11. Убедитесь, что тест был пройден, проверив нижней области в окне обозревателя тестов. В окне отображаются, что тест был пройден.

    ![Тест пройден](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Модульное тестирование по типу

В этом разделе вы добавите теста для приложения, созданного в [учебнике Приступая к работе](../getting-started/tutorial-getting-started-with-signalr.md) с помощью интерфейса, который содержит метод, чтобы протестировать.

1. Выполните шаги 1 – 7 в [модульное тестирование с помощью динамической](#dynamic) выше руководства.
2. Замените содержимое Tests.cs следующим кодом.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    В приведенном выше коде интерфейс создается определение подпись `broadcastMessage` метод, для которого обработчик тестов будет создан клиент макетов. Макеты клиент создается с помощью `Mock` объект типа [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (в SignalR 2.1, назначьте `dynamic` для параметра типа.) `IHubCallerConnectionContext` Интерфейс является прокси-объект, с помощью которого можно вызывать методы на клиент.

    Тест, затем создает экземпляр класса `ChatHub`, а затем создает имитационную версию объекта `broadcastMessage` метод, который в свою очередь, вызывается путем вызова метода `Send` метод на концентраторе.
3. Постройте решение, нажав клавишу **F6**.
4. Выполните модульный тест. В Visual Studio выберите **теста**, **Windows**, **обозреватель тестов**. Щелкните правой кнопкой мыши в окне обозревателя тестов **HubsAreMockableViaDynamic** и выберите **запуск выбранных тестов**.

    ![Обозреватель тестов](unit-testing-signalr-applications/_static/image7.png)
5. Убедитесь, что тест был пройден, проверив нижней области в окне обозревателя тестов. В окне отображаются, что тест был пройден.

    ![Тест пройден](unit-testing-signalr-applications/_static/image8.png)
