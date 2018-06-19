---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Модульное тестирование приложений SignalR | Документы Microsoft
author: pfletcher
description: В этой статье описывается использование возможностей модульное тестирование SignalR 2.0.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: cff866716cb1179e02b930f33cb0f8c33d4a6cf0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870847"
---
<a name="unit-testing-signalr-applications"></a>Тестирование приложений SignalR единицы
====================
по [Патрик Флетчера](https://github.com/pfletcher)

> Этой статье описывается использование возможностей модульное тестирование SignalR 2. 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Версии программного обеспечения, используемого в этом разделе
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR версии 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
> 
> Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы. Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Модульное тестирование приложений SignalR

Функции модульного теста в SignalR 2 можно использовать для создания модульных тестов для приложения SignalR. Включает SignalR 2 [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) интерфейс, который можно использовать для создания макетов объекта для имитации для тестирования методов концентратора.

В этом разделе вы добавите модульных тестов для приложения, созданного в [учебник по началу работы](../getting-started/tutorial-getting-started-with-signalr.md) с помощью [XUnit.net](https://github.com/xunit/xunit) и [заказа](https://github.com/Moq/moq4).

XUnit.net будет использоваться для управления теста; Заказа будет использоваться для создания [макета](http://en.wikipedia.org/wiki/Mock_object) объект для тестирования. При желании; можно использовать другие платформы имитации [NSubstitute](http://nsubstitute.github.io/) также будет хорошим выбором. Этот учебник посвящен Настройка макетов объекта двумя способами: во-первых, с помощью `dynamic` объекта (впервые появилась в .NET Framework 4), во-вторых, с помощью интерфейса.

### <a name="contents"></a>Описание

Этот учебник содержит следующие разделы.

- [Модульное тестирование с помощью динамической](#dynamic)
- [Модульное тестирование по типу](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Модульное тестирование с помощью динамической

В этом разделе вы добавите модульный тест для приложения, созданного в [учебник по началу работы](../getting-started/tutorial-getting-started-with-signalr.md) с помощью динамического объекта.

1. Установка [XUnit Runner расширения](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) для Visual Studio 2013.
2. Либо выполните [учебник по началу работы](../getting-started/tutorial-getting-started-with-signalr.md), или загрузить завершенное приложение из [коллекции кода MSDN](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Если вы используете версию загрузки приложения Приступая к работе, откройте **консоль диспетчера пакетов** и нажмите кнопку **восстановить** для добавления в проект пакета SignalR.

    ![Восстановление пакетов](unit-testing-signalr-applications/_static/image1.png)
4. Добавьте проект в решение для модульного теста. Щелкните правой кнопкой мыши решение в **обозревателе решений** и выберите **добавить**, **новый проект...** . В разделе **C#** выберите **Windows** узла. Выберите **библиотека классов**. Имя для нового проекта **TestLibrary** и нажмите кнопку **ОК**.

    ![Создание библиотеки тестов](unit-testing-signalr-applications/_static/image2.png)
5. Добавьте ссылку в тестовом проекте библиотеки в проект SignalRChat. Щелкните правой кнопкой мыши **TestLibrary** проект и выберите **добавить**, **ссылку...** . Выберите **проекты** узле **решения** узла, а также проверьте **SignalRChat**. Нажмите кнопку **ОК**.

    ![Добавить ссылку на проект](unit-testing-signalr-applications/_static/image3.png)
6. Добавление пакетов, SignalR, заказа и XUnit **TestLibrary** проекта. В **консоль диспетчера пакетов**, задайте **проект по умолчанию** раскрывающийся список для **TestLibrary**. В окне консоли, выполните следующие команды:

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Установить пакеты](unit-testing-signalr-applications/_static/image4.png)
7. Создайте файл теста. Щелкните правой кнопкой мыши **TestLibrary** проекта и нажмите кнопку **добавить...** , **Класса**. Имя для нового класса **Tests.cs**.
8. Замените содержимое Tests.cs следующий код.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    В приведенном выше коде тестовый клиент создается с помощью `Mock` объекта из [заказа](https://github.com/Moq/moq4) библиотеки типа [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (в версии 2.1 SignalR, назначьте `dynamic` для типа параметр). `IHubCallerConnectionContext` Интерфейс является прокси-объект, с помощью которого можно вызывать методы на стороне клиента. `broadcastMessage` Функция затем определено для макетов клиента таким образом, он может быть вызван `ChatHub` класса. Затем вызывается обработчик тестов `Send` метод `ChatHub` класс, который в свою очередь вызывает макеты `broadcastMessage` функции.
9. Постройте решение, нажав клавишу **F6**.
10. Выполните модульный тест. В Visual Studio, выберите **тест**, **Windows**, **Test Explorer**. Щелкните правой кнопкой мыши в окне обозревателя тестов **HubsAreMockableViaDynamic** и выберите **выполнение выбранных тестов**.

    ![Обозреватель тестов](unit-testing-signalr-applications/_static/image5.png)
11. Убедитесь, что тест был пройден, установив на нижней панели в окне обозревателя тестов. Окно покажет, что тест был пройден.

    ![Тест пройден](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Модульное тестирование по типу

В этом разделе вы добавите теста для приложения, созданные в [учебник по началу работы](../getting-started/tutorial-getting-started-with-signalr.md) с помощью интерфейс, который содержит метод для тестирования.

1. Выполните шаги 1 – 7 [модульного тестирования с динамической](#dynamic) учебника выше.
2. Замените содержимое Tests.cs следующий код.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    В приведенном выше коде интерфейс создается определение сигнатура `broadcastMessage` метода, для которого обработчик тестов будет создание макетов клиента. Макеты клиента создается с помощью `Mock` объекта типа [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (в версии 2.1 SignalR, назначьте `dynamic` для параметра типа.) `IHubCallerConnectionContext` Интерфейс является прокси-объект, с помощью которого можно вызывать методы на стороне клиента.

    Тест, затем создает экземпляр `ChatHub`, а затем создает имитационную версию объекта `broadcastMessage` метод, который в свою очередь, вызывается путем вызова метода `Send` метод на концентраторе.
3. Постройте решение, нажав клавишу **F6**.
4. Выполните модульный тест. В Visual Studio, выберите **тест**, **Windows**, **Test Explorer**. Щелкните правой кнопкой мыши в окне обозревателя тестов **HubsAreMockableViaDynamic** и выберите **выполнение выбранных тестов**.

    ![Обозреватель тестов](unit-testing-signalr-applications/_static/image7.png)
5. Убедитесь, что тест был пройден, установив на нижней панели в окне обозревателя тестов. Окно покажет, что тест был пройден.

    ![Тест пройден](unit-testing-signalr-applications/_static/image8.png)
