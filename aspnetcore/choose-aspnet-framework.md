---
title: Выбор между ASP.NET и ASP.NET Core
author: rick-anderson
description: Как выбрать между ASP.NET и ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 05/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 0c6924d40b7327d2032a0278c56a0b4fa41d15a1
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2018
ms.locfileid: "34094439"
---
# <a name="choose-between-aspnet-and-aspnet-core"></a>Выбор между ASP.NET и ASP.NET Core

Независимо от того, какое веб-приложение вы создаете, ASP.NET предлагает вам решение: от корпоративных веб-приложений, предназначенных для Windows Server, до небольших микрослужб, предназначенных для контейнеров Linux, и все остальное.

## <a name="aspnet-core"></a>ASP.NET Core

ASP.NET Core — это кроссплатформенная среда с открытым кодом для создания современных облачных веб-приложений в Windows, macOS или Linux.

## <a name="aspnet"></a>ASP.NET

ASP.NET — это развитая платформа, предоставляющая все необходимые службы для создания серверных веб-приложений корпоративного класса в Windows.

## <a name="framework-selection"></a>Выбор платформы

Просмотрите следующую таблицу и выберите подходящую платформу.

| ASP.NET Core | ASP.NET |
|---|---|
|Предназначена для Windows, macOS или Linux|Предназначена для Windows|
|[Razor Pages](xref:mvc/razor-pages/index) — рекомендуемый метод создания пользовательского веб-интерфейса в ASP.NET Core 2.x. См. также [MVC](xref:mvc/overview), [веб-API](xref:tutorials/first-web-api), и [SignalR](xref:signalr/introduction).|Использование [веб-форм](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [веб-API](/aspnet/web-api/), [веб-перехватчиков](/aspnet/webhooks/) или [веб-страниц](/aspnet/web-pages)|
|Несколько версий для одного компьютера|Одна версия для одного компьютера|
|Разработка в Visual Studio, [Visual Studio для Mac](https://www.visualstudio.com/vs/visual-studio-mac/) или [Visual Studio Code](https://code.visualstudio.com/) с использованием C# или F#|Разработка с Visual Studio с использованием C#, VB и F#|
|Более высокая производительность, чем в ASP.NET|Хорошая производительность|
|[Выбор среды выполнения .NET Framework или .NET Core](/dotnet/articles/standard/choosing-core-framework-server)|Использование среды выполнения .NET Framework|

## <a name="aspnet-core-scenarios"></a>Сценарии ASP.NET Core

* [Razor Pages](xref:mvc/razor-pages/index) — рекомендуемый метод создания пользовательского веб-интерфейса в ASP.NET Core 2.x.
* [Веб-сайты](xref:tutorials/first-mvc-app/index)
* [API-интерфейсы](xref:tutorials/first-web-api)
* [Режим реального времени](xref:signalr/index)

## <a name="aspnet-scenarios"></a>Сценарии ASP.NET

* [Веб-сайты](/aspnet/mvc)
* [API-интерфейсы](/aspnet/web-api)
* [Режим реального времени](/aspnet/signalr)

## <a name="resources"></a>Ресурсы

* [Введение в ASP.NET](/aspnet/overview)
* [Введение в ASP.NET Core](xref:index)
