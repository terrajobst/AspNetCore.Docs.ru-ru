---
title: Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core
author: shirhatti
description: Узнайте о поддерживаемых возможностях для отладки приложений ASP.NET Core, выполняемых в службах IIS на Windows Server.
ms.author: riande
ms.custom: mvc
ms.date: 11/26/2018
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 65dbe690a33d82a4edddf315803dc4c656db27a0
ms.sourcegitcommit: e8d80ff566bfe505b43389d7bc4551edb1c0c872
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2018
ms.locfileid: "52549108"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core

Авторы: [Сурабх Ширхатти](https://twitter.com/sshirhatti) (Sourabh Shirhatti) и [Люк Латэм](https://github.com/guardrex) (Luke Latham)

В этой статье описаны поддерживаемые в [Visual Studio](https://www.visualstudio.com/vs/) возможности для отладки приложений ASP.NET Core, выполняющихся в службах IIS в Windows Server. Также здесь приведены пошаговые инструкции по активации этой функциональности и настройке проекта.

## <a name="prerequisites"></a>Предварительные требования

* [Visual Studio для Windows](https://www.microsoft.com/net/download/windows)
* Рабочая нагрузка **ASP.NET и веб-разработка**
* Рабочая нагрузка **Кроссплатформенная разработка .NET Core**
* Сертификат безопасности X.509

## <a name="enable-iis"></a>Активация IIS

1. Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана).
1. Установите флажок **Службы IIS**.

![Показаны компоненты Windows, где флажок "Службы IIS" отображен в виде черного квадрата (не галочки), что означает, что некоторые функции IIS включены](development-time-iis-support/_static/enable_iis.png)

Для установки служб IIS, возможно, потребуется перезагрузить компьютер.

## <a name="configure-iis"></a>Настройка IIS

В службах IIS нужно настроить веб-сайт со следующими характеристиками:

* имя узла, которое соответствует URL-адресу узла для профиля запуска приложения;
* привязка для порта 443 с назначенным сертификатом.

Например, **Имя узла** для добавленного веб-сайта имеет значение localhost (это же значение будет использоваться в профиле запуска далее в этой статье). Порт имеет значение 443 (HTTPS). Нашему веб-сайту назначен **сертификат разработки IIS Express**, но подойдет и любой другой действительный сертификат:

![Окно добавления веб-сайта в IIS с настроенной привязкой для localhost и порта 443, которой назначен сертификат.](development-time-iis-support/_static/add-website-window.png)

Если для установки IIS уже настроен **веб-сайт по умолчанию** с именем узла, которое совпадает с именем узла URL-адреса для профиля запуска приложения, выполните следующие действия:

* Добавьте привязку для порта 443 (HTTPS).
* Назначьте веб-сайту действительный сертификат.

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Включение поддержки служб IIS в Visual Studio во время разработки

1. Запустите установщик Visual Studio.
1. Выберите компонент **поддержки IIS во время разработки**. Этот компонент указан как дополнительный на панели **Сводка** для рабочей нагрузки **ASP.NET и веб-разработки**. С помощью выбранного компонента будет установлен [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). Это собственный модуль IIS, который необходим для запуска приложений ASP.NET Core за IIS в конфигурации обратного прокси-сервера.

![Изменение функций Visual Studio: выбрана вкладка "Рабочие нагрузки". В разделе "Интернет и облако" выбрана панель "ASP.NET и веб-разработка". Флажок поддержки IIS во время разработки, отображающийся в правой части области "Дополнительно" на панели "Сводка".](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Настройка проекта

### <a name="https-redirection"></a>Перенаправление HTTPS

Если это новый проект, установите флажок **Configure for HTTPS** (Настроить для HTTPS) в окне **создания веб-приложения ASP.NET Core**:

![Окно создания веб-приложения ASP.NET Core с установленным флажком настройки для HTTPS.](development-time-iis-support/_static/new-app.png)

Если это существующий проект, используйте ПО промежуточного слоя для перенаправления HTTPS из `Startup.Configure`, вызвав метод расширения [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection):

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a>Профиль запуска служб IIS

Создайте новый профиль запуска, чтобы добавить поддержку IIS во время разработки.

1. В разделе **Профиль** нажмите кнопку **Новый**. Во всплывающем окне присвойте новому профилю имя IIS. Нажмите кнопку **ОК**, чтобы создать проект.
1. В поле **Запуск** выберите из списка значение **IIS**.
1. Установите флажок **Запуск браузера** и укажите URL-адрес конечной точки. Выберите протокол HTTPS. В этом примере используется `https://localhost/WebApplication1`.
1. В разделе **Переменные среды** нажмите кнопку **Добавить**. Введите переменную среды с ключом `ASPNETCORE_ENVIRONMENT` и значением `Development`.
1. В области **Параметры веб-сервера** задайте **URL-адрес приложения**. В этом примере используется `https://localhost/WebApplication1`.
1. Сохраните профиль.

![Окно свойств проекта с выбранной вкладкой "Отладка". В качестве параметров профиля и запуска задано IIS. Функция "Запуск браузера" включена и для нее задан адрес https://localhost/WebApplication1. Этот же адрес указан в поле "URL-адрес приложения" области "Параметры веб-сервера".](development-time-iis-support/_static/project_properties.png)

Вы также можете вручную добавить профиль запуска в файл [launchSettings.json](http://json.schemastore.org/launchsettings) для вашего приложения:

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## <a name="run-the-project"></a>Запуск проекта

В Visual Studio сделайте следующее:

* Убедитесь, что для раскрывающегося списка с конфигурацией сборки построения выбрано значение **Отладка**.
* Выберите для кнопки "Выполнить" профиль **IIS** и нажмите ее запуска приложения:

![Для кнопки "Выполнить" на панели инструментов VS выбран профиль IIS. Для раскрывающегося списка конфигурации сборки выбрано значение "Выпуск".](development-time-iis-support/_static/toolbar.png)

Если вы вошли в Visual Studio без прав администратора, возможно, потребуется перезапуск. Перезапустите Visual Studio при появлении соответствующего запроса.

Если используется сертификат разработки без доверия, возможно, потребуется создать исключение для этого ненадежного сертификата по запросу в браузере.

> [!NOTE]
> Отладка конфигурации сборки выпуска с использованием функции [Только мой код](/visualstudio/debugger/just-my-code) и оптимизации компилятора приводит к ограничению возможностей. Например, точки останова не будут достигнуты.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Размещение ASP.NET Core в Windows со службами IIS](xref:host-and-deploy/iis/index)
* [Введение в модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Принудительное использование HTTPS](xref:security/enforcing-ssl)
