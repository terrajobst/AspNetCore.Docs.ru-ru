---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: По промежуточного слоя OWIN в IIS интегрированном конвейере | Документы Microsoft
author: Praburaj
description: В этой статье показано, как выполнять компонентов по промежуточного слоя OWIN (OMCs) в интегрированном конвейере служб IIS и работает как задать события конвейера OMC на. Вы должны...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2013
ms.topic: article
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 5df70c80084a32c5f61ac9288c8cdbfaaa47f124
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871497"
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>По промежуточного слоя OWIN в интегрированном конвейере служб IIS
====================
по [Praburaj Thiagarajan](https://github.com/Praburaj), [Рик Андерсон](https://github.com/Rick-Anderson)

> В этой статье показано, как выполнять компонентов по промежуточного слоя OWIN (OMCs) в интегрированном конвейере служб IIS и работает как задать события конвейера OMC на. Изучите [Обзор проекта Katana](an-overview-of-project-katana.md) и [определение класса запуска OWIN](owin-startup-class-detection.md) перед чтением этого учебника. Это руководство было написано с Рик Андерсон ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Крис Ross Praburaj Thiagarajan и Говард Дайеркинг ( [ @howard \_Дайеркинг](https://twitter.com/howard_dierking) ).


Несмотря на то что [OWIN](an-overview-of-project-katana.md) компонентов по промежуточного слоя (OMCs) в первую очередь предназначены для запуска в конвейере независимой от сервера, возможно, для работы в интегрированном конвейере служб IIS также OMC (**классический режим *не* поддерживается**). OMC можно сделать для работы в интегрированном конвейере служб IIS, установив следующий пакет из консоли диспетчера пакетов (PMC).

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Это означает, что все платформы приложений, даже те, которые еще не может запустить вне IIS и System.Web, можно воспользоваться преимуществами существующих компонентов по промежуточного слоя OWIN. 

> [!NOTE]
> Все `Microsoft.Owin.Security.*` пакетов доставки в новой системе удостоверений в Visual Studio 2013 (например: файлы cookie, учетной записи Майкрософт, Google, Facebook, Twitter, [маркеров носителей](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, сервер авторизации, JWT, Azure Active каталог служб федерации Active directory) создан как OMCs и может использоваться в сценариях резидентной и размещенных в IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Как выполняется по промежуточного слоя OWIN в интегрированном конвейере служб IIS

Для консольных приложений OWIN, созданные в конвейер приложения [начальной конфигурации](owin-startup-class-detection.md) задается порядок добавления компонентов с помощью `IAppBuilder.Use` метод. То есть в конвейер OWIN в [Katana](an-overview-of-project-katana.md) среда выполнения будет обрабатывать OMCs в порядке, они были зарегистрированы с помощью `IAppBuilder.Use`. В интегрированном конвейере служб IIS конвейер запросов состоит из [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) подписка на набор предварительно определенных событий конвейера, таких как [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)и т. д.

Сравнив OMC, [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) в мире ASP.NET OMC должны быть зарегистрированы правильно конвейера предварительно определенных событий. Например, HttpModule `MyModule` вызывается при поступлении запроса [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) этап конвейера:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Чтобы OMC участвовать в этот порядок выполнения одинаковую, основанной на событиях [Katana](an-overview-of-project-katana.md) кода среды выполнения просматривает [начальной конфигурации](owin-startup-class-detection.md) и подписывается каждого из компонентов по промежуточного слоя для событие интегрированного конвейера. Например следующий код OMC и регистрация дает возможность просмотра регистрации событий по умолчанию для компонентов по промежуточного слоя. (См. более подробные инструкции по созданию класс запуска OWIN, [определение класса запуска OWIN](owin-startup-class-detection.md).)

1. Создайте пустой веб-узел проекта приложения и назовите его **owin2**.
2. Из пакета Диспетчер консоли (PMC), выполните следующую команду: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Добавить `OWIN Startup Class` и назовите его `Startup`. Замените созданный код (изменения выделены) следующие:  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Нажмите клавишу F5 для запуска приложения.

Конфигурация запуска настраивает конвейер с компонентов три по промежуточного слоя, первые два отображения диагностической информации и реагирование на события последнее из них (и также вывода диагностических сведений). `PrintCurrentIntegratedPipelineStage` Метод выводит события интегрированного конвейера, вызванную этого по промежуточного слоя и сообщение. Вывод windows выводится следующее сообщение:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Среда выполнения Katana сопоставить каждый из компонентов по промежуточного слоя OWIN для [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) по умолчанию, который соответствует событию конвейера IIS [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Маркеры рабочей области

Вы можете пометить OMCs выполнение на определенных стадиях конвейера при помощи `IAppBuilder UseStageMarker()` метода расширения. Для определенного этапе необходимо запустить набор компонентов по промежуточного слоя, вставьте метку этапа сразу после последнего компонента — это набор во время регистрации. Правила, на каком этапе конвейера можно выполнить по промежуточного слоя и порядка компонентов необходимо запустить (Далее в этом учебнике рассматриваются правила). Добавить `UseStageMarker` метод `Configuration` код, как показано ниже:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

`app.UseStageMarker(PipelineStage.Authenticate)` Вызов настраивает все компоненты (в данном случае наших двух компонентов диагностики) ранее зарегистрированного по промежуточного слоя для выполнения на стадии конвейера проверки подлинности. Последний компонент по промежуточного слоя (который отображает диагностики и отвечает на запросы) будут выполняться на `ResolveCache` рабочей области ( [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) событий).

Нажмите клавишу F5 для запуска приложения. В окне вывода содержит следующую информацию:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Правила этапа маркера

Компоненты по промежуточного слоя Owin (OMC) можно настроить на запуск в следующие события этап конвейера OWIN:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. По умолчанию OMCs выполняются на последнего события (`PreHandlerExecute`). Вот почему наши первый пример кода отображается «PreExecuteRequestHandler».
2. Можно использовать `app.UseStageMarker` метода для регистрации OMC чтобы раньше, на любом этапе конвейера OWIN перечисленные в `PipelineStage` перечисления.
3. Конвейер OWIN и конвейер IIS упорядочен, поэтому вызовы `app.UseStageMarker` должны располагаться в порядке. Невозможно задать обработчик событий к событию, предшествует зарегистрирована для последнего события `app.UseStageMarker`. Например *после* вызов:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   вызовы `app.UseStageMarker` передачи `Authenticate` или `PostAuthenticate` не удается обработать, а не исключение. Выполняются на последнюю этапе, который по умолчанию является OMCs `PreHandlerExecute`. Чтобы сделать их для выполнения ранее используются маркеры рабочей области. При указании маркеры этапа по порядку, мы округление до более ранней маркера. Другими словами Добавление метку этапа говорится «Запуск не позднее чем этап X». OMC его выполнения в ранняя метка этапа добавлен после их в конвейер OWIN.
4. Самым ранним этапом вызовы `app.UseStageMarker` wins. Например, если поменять местами `app.UseStageMarker` вызовы из предыдущего примера:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   Для отображения в окне вывода: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   Работать в OMCs `AuthenticateRequest` этап, так как последний OMC зарегистрирован с `Authenticate` события и `Authenticate` событие предшествует все события.
