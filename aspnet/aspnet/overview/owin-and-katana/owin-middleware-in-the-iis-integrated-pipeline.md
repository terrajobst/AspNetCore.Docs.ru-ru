---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: По промежуточного слоя OWIN в службах IIS интегрирован конвейера | Документация Майкрософт
author: Praburaj
description: В этой статье показано, как для запуска компонентов по промежуточного слоя OWIN (OMCs) в интегрированном конвейере служб IIS и работает как для установки события конвейера OMC на. Вы должны...
ms.author: riande
ms.date: 11/07/2013
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 56bd145688e1ab0a70710cc80cb8f5fa10ee8968
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829065"
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>По промежуточного слоя OWIN в интегрированном конвейере служб IIS
====================
по [Praburaj Thiagarajan](https://github.com/Praburaj), [Рик Андерсон](https://github.com/Rick-Anderson)

> В этой статье показано, как для запуска компонентов по промежуточного слоя OWIN (OMCs) в интегрированном конвейере служб IIS и работает как для установки события конвейера OMC на. Необходимо ознакомиться с [Обзор проекта Katana](an-overview-of-project-katana.md) и [определение класса запуска OWIN](owin-startup-class-detection.md) перед чтением этого руководства. Это руководство было написано с Рик Андерсон ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Крис Росс Praburaj Thiagarajan и Говард Дайеркинг ( [ @howard \_Дайеркинг](https://twitter.com/howard_dierking) ).


Несмотря на то что [OWIN](an-overview-of-project-katana.md) компоненты по промежуточного слоя (OMCs) в первую очередь предназначены для запуска в конвейере независимой от сервера, можно запустить в интегрированном конвейере служб IIS также OMC (**является классический режим *не* поддерживается**). OMC можно сделать для работы в интегрированном конвейере служб IIS, установив следующий пакет из консоли диспетчера пакетов (PMC):

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Это означает, что все платформы приложений, даже те, которые еще не запускаться вне IIS и System.Web, можно использовать существующие компоненты по промежуточного слоя OWIN. 

> [!NOTE]
> Все `Microsoft.Owin.Security.*` пакеты доставки с помощью новой системы удостоверений в Visual Studio 2013 (например: файлы cookie, учетной записи Майкрософт, Google, Facebook, Twitter, [маркера носителя](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, сервер авторизации, JWT, Azure Active каталог, службы федерации Active directory) обрабатывать как OMCs и может использоваться в сценарии как локальную среду, так и размещенных в IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Как выполняется по промежуточного слоя OWIN в интегрированном конвейере служб IIS

Для консольных приложений OWIN, использующем конвейера приложения [конфигурации запуска](owin-startup-class-detection.md) задается порядок добавления компонентов с помощью `IAppBuilder.Use` метод. То есть в конвейер OWIN в [Katana](an-overview-of-project-katana.md) среда выполнения обработает OMCs в порядке, они были зарегистрированы с помощью `IAppBuilder.Use`. В интегрированном конвейере служб IIS конвейера запросов состоит из [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) подписка на набор предварительно определенных событий конвейера, такие как [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)и т. д.

Сравнив OMC, [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) в мире ASP.NET OMC должно быть зарегистрировано для конвейера правильный предварительно определенные события. Например, модуль HttpModule `MyModule` будет вызван при поступлении запроса [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) этапом работы конвейера:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Выполнился OMC участвовать в этот порядок выполнения же, основанное на событиях [Katana](an-overview-of-project-katana.md) кода среды выполнения просматривает [конфигурации запуска](owin-startup-class-detection.md) и подписывается, каждый из компонентов по промежуточного слоя для событие интегрированного конвейера. Например следующий код OMC и регистрации позволяет см. в разделе регистрации событий по умолчанию компонентов по промежуточного слоя. (Более подробные инструкции по созданию класса запуска OWIN, см. в разделе [определение класса запуска OWIN](owin-startup-class-detection.md).)

1. Создайте приложение в пустой веб-проект и назовите его **owin2**.
2. Из консоли диспетчера пакетов (PMC), выполните следующую команду: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Добавить `OWIN Startup Class` и назовите его `Startup`. Замените сгенерированный код следующим (изменения выделены):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Нажмите клавишу F5 для запуска приложения.

Конфигурации запуска настраивает конвейер с по промежуточного слоя для трех компонентов, первые два отображения диагностической информации и реагирование на события последним (и также отображения диагностической информации). `PrintCurrentIntegratedPipelineStage` Метод отображает интегрированного конвейера событий, вызывается это по промежуточного слоя, а также сообщение. В окнах вывода отображаются следующие сведения:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Среда выполнения Katana сопоставить каждый из компонентов по промежуточного слоя OWIN для [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) по умолчанию, который соответствует событию конвейера IIS [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Маркеры рабочей области

Вы можете пометить OMCs выполнение на определенных этапах конвейера при помощи `IAppBuilder UseStageMarker()` метода расширения. Для запуска набора компонентов по промежуточного слоя во время определенного этапа, Вставить метку этапа сразу после последнего компонента — это набор во время регистрации. Существуют правила, на каком этапе конвейера можно выполнить по промежуточного слоя и компоненты заказа необходимо запустить (правила мы вернемся позже в этом руководстве). Добавить `UseStageMarker` метод `Configuration` кода, как показано ниже:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

`app.UseStageMarker(PipelineStage.Authenticate)` Вызов настраивает все компоненты по промежуточного слоя для зарегистрированного ранее (в данном случае наши два компонента диагностики) для запуска на стадии конвейера проверки подлинности. Последний компонент по промежуточного слоя (который отображает диагностики и отвечает на запросы) будет выполняться на `ResolveCache` рабочей области ( [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) событий).

Нажмите клавишу F5 для запуска приложения. В окне вывода отобразится следующее:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Правила этапа маркера

Компоненты по промежуточного слоя Owin (OMC) можно настроить для выполнения на следующие события этап конвейера OWIN:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. По умолчанию OMCs выполняются с последнего события (`PreHandlerExecute`). Вот почему наш первый пример кода отображается «PreExecuteRequestHandler».
2. Можно использовать `app.UseStageMarker` метод, чтобы зарегистрировать OMC для более ранних версиях выполнения на любом этапе конвейера OWIN перечисленные в `PipelineStage` перечисления.
3. Конвейер OWIN и конвейер IIS упорядочен, поэтому вызовы `app.UseStageMarker` должно быть в порядке. Не удается задать обработчик событий на событие, которое предшествует последнее событие, зарегистрированное для `app.UseStageMarker`. Например *после* вызова:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   вызовы `app.UseStageMarker` передачи `Authenticate` или `PostAuthenticate` не будут выполняться, а не исключение. OMCs, выполните в последней рабочей области, который по умолчанию является `PreHandlerExecute`. Чтобы сделать их для выполнения ранее используются маркеры рабочей области. При указании маркеры рабочей области по порядку, мы округление к более ранней маркеру. Другими словами Добавление метку этапа, — говорит «Запуск не позднее, чем этап X». OMC его выполнения в самая ранняя метка этапа, добавленные после их в конвейер OWIN.
4. Самый ранний этап вызовы `app.UseStageMarker` wins. Например, если Смена местами `app.UseStageMarker` вызовы из предыдущего примера:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   Для отображения в окне вывода: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   Работают в OMCs `AuthenticateRequest` этап, так как последний OMC зарегистрирована `Authenticate` событий и `Authenticate` событий предшествует всех остальных событий.
