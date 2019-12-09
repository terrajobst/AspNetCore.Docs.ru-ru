---
title: Размещение и развертывание серверного приложения Blazor с помощью ASP.NET Core
author: guardrex
description: Сведения о размещении и развертывании серверного приложения Blazor с помощью ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/server
ms.openlocfilehash: b688d000f26c9b230d9fdee8423b3194145fe1aa
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317301"
---
# <a name="host-and-deploy-opno-locblazor-server"></a>Размещение и развертывание серверного приложения Blazor

Авторы: [Люк Лэтем](https://github.com/guardrex), [Рэйнер Стропек](https://www.timecockpit.com) и [Дэниэл Рот](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Значения конфигурации узла

[Серверные приложения Blazor](xref:blazor/hosting-models#blazor-server) могут принимать [значения конфигурации универсального узла](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Развертывание

Если используется [модель размещения на сервере Blazor](xref:blazor/hosting-models#blazor-server), Blazor выполняется на сервере из приложения ASP.NET Core. Обновление элементов пользовательского интерфейса, обработка событий и вызовы JavaScript обрабатываются через подключение [SignalR](xref:signalr/introduction).

Необходим веб-сервер, позволяющий размещать приложения ASP.NET Core. Visual Studio содержит шаблон проекта **Серверное приложение Blazor** (шаблон `blazorserverside` при использовании команды [dotnet new](/dotnet/core/tools/dotnet-new)).

## <a name="scalability"></a>Масштабируемость

Планируйте развертывание так, чтобы доступная инфраструктура максимально эффективно использовалась серверным приложением Blazor. Вопросы, связанные с масштабируемостью серверных приложений Blazor, рассматриваются в следующих статьях:

* [Статья со сведениями о серверных приложениях Blazor](xref:blazor/hosting-models#blazor-server)
* <xref:security/blazor/server>

### <a name="deployment-server"></a>Сервер развертывания

В контексте масштабируемости отдельного сервера (вертикальное масштабирование) доступная приложению память с большой вероятностью будет первым ресурсом, расходуемым приложением при изменении потребностей пользователей. Объем доступной памяти на сервере влияет на:

* число активных каналов, которые может поддерживать сервер;
* задержку пользовательского интерфейса в клиенте.

Рекомендации по созданию безопасных и масштабируемых серверных приложений Blazor см. здесь: <xref:security/blazor/server>.

Каждый канал использует около 250 КБ памяти для минималистичного приложения в стиле *Hello World*. Размер канала зависит от кода приложения и требований к обслуживанию состояний для каждого компонента. Мы рекомендуем определять требования к ресурсам во время разработки приложения и инфраструктуры, но при планировании цели развертывания можно использовать следующие базовые показатели: если вы предполагаете, что приложение будет одновременно поддерживать 5000 пользователей, выделите для приложения по меньшей мере 1,3 ГБ серверной памяти (или около 273 КБ на пользователя).

### <a name="opno-locsignalr-configuration"></a>Конфигурация SignalR

Серверные приложения Blazor используют службу SignalR в ASP.NET Core для обмена данными с браузером. [Условия размещения и масштабирования SignalR](xref:signalr/publish-to-azure-web-app) применяются и к серверным приложениям Blazor.

Blazor лучше всего работает с WebSocket в качестве транспортного механизма SignalR благодаря низкой задержке, надежности и [безопасности](xref:signalr/security). Продолжительный опрос используется службой SignalR, если подключения WebSocket недоступны или если приложение явно настроено на применение продолжительного опроса. При развертывании Службы приложений Azure настройте приложение на использование WebSocket в параметрах службы на портале Azure. Подробные сведения о настройке приложения для Службы приложений Azure см. в статье с [рекомендациями по публикации приложения SignalR](xref:signalr/publish-to-azure-web-app).

#### <a name="azure-opno-locsignalr-service"></a>Служба Azure SignalR

Мы рекомендуем использовать для серверных приложений Blazor [службу Azure SignalR](/azure/azure-signalr). Она позволяет вертикально масштабировать серверные приложения Blazor для одновременного использования большого числа подключений SignalR. Кроме того, глобальный охват службы SignalR и ее высокопроизводительные центры обработки данных обеспечивают низкую задержку благодаря доступности в разных регионах. Чтобы настроить приложение (и при необходимости подготовить к работе) для службы Azure SignalR, сделайте следующее:

1. Включите в службе поддержку *прикрепленных сеансов*, когда клиенты [перенаправляются обратно на тот же сервер при предварительной отрисовке](xref:blazor/hosting-models#reconnection-to-the-same-server). Установите параметр или значение конфигурации `ServerStickyMode` равным `Required`. Как правило, приложение создает конфигурацию, используя **один** из следующих подходов:

   * `Startup.ConfigureServices`.
  
     ```csharp
     services.AddSignalR().AddAzureSignalR(options =>
     {
         options.ServerStickyMode = 
             Microsoft.Azure.SignalR.ServerStickyMode.Required;
     });
     ```

   * Конфигурация (воспользуйтесь **одним** из перечисленных ниже подходов):
  
     * *appsettings.json*:

       ```json
       "Azure:SignalR:ServerStickyMode": "Required"
       ```

     * раздел **Конфигурация** > **Параметры приложения** для службы приложений на портале Azure (**Имя**: `Azure:SignalR:ServerStickyMode`, **Значение**: `Required`).

1. Создайте профиль публикации приложений Azure Apps в Visual Studio для серверного приложения Blazor.
1. Добавьте в профиль зависимость **службы Azure SignalR** . Если подписка Azure не имеет уже существующего экземпляра службы Azure SignalR для назначения приложению, выберите **Создайте новый экземпляр службы Azure SignalR** для предоставления нового экземпляра службы.
1. Публикация приложения в Azure.

#### <a name="iis"></a>IIS

При использовании служб IIS прикрепленные сеансы включаются с помощью маршрутизации запросов приложений. Дополнительные сведения см. в статье [Балансировка нагрузки HTTP с помощью маршрутизации запросов приложений](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).

#### <a name="kubernetes"></a>Kubernetes

Создайте определение входящего трафика со следующими [заметками Kubernetes для прикрепленных сеансов](https://kubernetes.github.io/ingress-nginx/examples/affinity/cookie/):

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: <ingress-name>
  annotations:
    nginx.ingress.kubernetes.io/affinity: "cookie"
    nginx.ingress.kubernetes.io/session-cookie-name: "affinity"
    nginx.ingress.kubernetes.io/session-cookie-expires: "14400"
    nginx.ingress.kubernetes.io/session-cookie-max-age: "14400"
```

### <a name="measure-network-latency"></a>Измерение задержки сети

Для оценки задержки сети можно использовать [средства взаимодействия JavaScript](xref:blazor/javascript-interop), как показано в следующем примере:

```cshtml
@inject IJSRuntime JS

@if (latency is null)
{
    <span>Calculating...</span>
}
else
{
    <span>@(latency.Value.TotalMilliseconds)ms</span>
}

@code
{
    private DateTime startTime;
    private TimeSpan? latency;

    protected override async Task OnInitializedAsync()
    {
        startTime = DateTime.UtcNow;
        var _ = await JS.InvokeAsync<string>("toString");
        latency = DateTime.UtcNow - startTime;
    }
}
```

Для удобной работы с пользовательским интерфейсом мы рекомендуем обеспечить стабильную задержку не более 250 мс.
