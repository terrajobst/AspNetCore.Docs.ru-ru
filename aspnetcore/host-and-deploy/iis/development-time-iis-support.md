---
title: Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core
author: shirhatti
description: Обнаружение поддержки для отладки приложений ASP.NET Core при запуске за IIS на сервере Windows.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Поддержка служб IIS во время разработки в Visual Studio для ASP.NET Core

По [Sourabh Shirhatti](https://twitter.com/sshirhatti) и [Latham Люк](https://github.com/guardrex)

В этой статье описывается [Visual Studio](https://www.visualstudio.com/vs/) поддержка отладки приложений ASP.NET Core, работающие за IIS в Windows Server. В этом разделе описывается включение этой функции и настройка проекта.

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a>Активация IIS

1. Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана).
1. Выберите **Internet Information Services** флажок.

![Отображение компонентов Windows Internet Information Services флажков как черный квадрат (не в виде галочки), указывающее на то, что некоторые функции IIS включены](development-time-iis-support/_static/enable_iis.png)

Для установки служб IIS может потребоваться перезагрузка компьютера.

## <a name="configure-iis"></a>Настройка IIS

Службы IIS необходимо иметь следующие настройки веб-сайта.

* Имя узла для имени узла, соответствующий URL-адрес профиля запуска приложения.
* Привязка для порта 443, назначенный сертификатом.

Например **имя узла** для добавленных веб-сайт имеет значение «localhost» (профиль запуска будет также использовать «localhost» далее в этом разделе). Порт имеет значение «443» (HTTPS). **IIS Express разработки сертификат** назначается веб-сайт, но любой действительный сертификат works:

![Добавьте окно веб-сайта в IIS, отображение привязка, указанная для localhost на порте 443 с назначенного сертификата.](development-time-iis-support/_static/add-website-window.png)

Если установка IIS уже имеет **веб-сайт по умолчанию** с именем узла, который соответствует имя узла URL-адрес профиля запуска приложения:

* Добавьте привязку порта для порта 443 (HTTPS).
* Назначьте действительный сертификат для веб-сайта.

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Включить поддержку IIS во время разработки в Visual Studio

1. Запустите установщик Visual Studio.
1. Выберите **времени разработки, IIS поддерживает** компонента. Компонент указан как необязательные в **Сводка** панели для **ASP.NET и веб-разработки** рабочей нагрузки. Устанавливает компонент [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), который является собственный модуль IIS, необходимые для запуска приложения под IIS ASP.NET Core в конфигурации обратного прокси-сервера.

![Изменение функций Visual Studio: выбрана вкладка "Рабочие нагрузки". В разделе "Интернет и облако" выбрана панель "ASP.NET и веб-разработка". В правой области необязательно панель сводки нет типа "флажок" для времени разработки, поддерживаемых IIS.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Настройка проекта

### <a name="https-redirection"></a>Перенаправление на HTTPS

Для нового проекта, установите флажок для **настроить для использования протокола HTTPS** в **нового веб-приложения ASP.NET Core** окна:

![Новое окно веб-приложения ASP.NET Core, настройка для HTTPS флажок установлен.](development-time-iis-support/_static/new-app.png)

В существующий проект, с помощью по промежуточного слоя перенаправления HTTPS в `Startup.Configure` путем вызова [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) метода расширения:

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

Создание профиля запуска для добавления поддержки IIS во время разработки:

1. Для **профиль**выберите **New** кнопки. Имя профиля «IIS» во всплывающем окне. Выберите **ОК** для создания профиля.
1. Для **запуска** выберите **IIS** из списка.
1. Установите флажок для **браузер** и укажите URL-адрес конечной точки. Использование протокола HTTPS. В этом примере используется `https://localhost/WebApplication1`.
1. В **переменных среды** выберите **добавить** кнопки. Укажите переменную среды с ключом `ASPNETCORE_ENVIRONMENT` и значение `Development`.
1. В **параметров веб-сервера** задайте **URL-адрес приложения**. В этом примере используется `https://localhost/WebApplication1`.
1. Сохранение профиля.

![Окно свойств проекта с выбранной вкладкой "Отладка". В качестве параметров профиля и запуска задано IIS. Запуск браузера включена с адресом https://localhost/WebApplication1. Также предоставляется тот же адрес в поле URL-адрес приложения в области параметров веб-сервера.](development-time-iis-support/_static/project_properties.png)

Можно также вручную добавить профиль запуска для [launchSettings.json](http://json.schemastore.org/launchsettings) файл в приложении:

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

В пользовательском Интерфейсе VS присвоено кнопку Выполнить **IIS** профиль и нажмите кнопку, чтобы запустить приложение:

![На панели инструментов VS, равным «IIS» профиль запуска.](development-time-iis-support/_static/toolbar.png)

Visual Studio могут потребовать перезапуска, если не запущена с правами администратора. Перезапустите Visual Studio при появлении соответствующего запроса.

Если используется сертификат ненадежных разработки, может потребоваться обозревателя позволяет создавать исключения для ненадежный сертификат.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Размещение ASP.NET Core в Windows со службами IIS](xref:host-and-deploy/iis/index)
* [Введение в модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Принудительное использование HTTPS](xref:security/enforcing-ssl)
