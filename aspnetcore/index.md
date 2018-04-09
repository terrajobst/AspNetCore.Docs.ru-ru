---
title: Введение в ASP.NET Core
author: rick-anderson
description: Введение в ASP.NET Core — кроссплатформенную высокопроизводительную платформу с открытым исходным кодом для создания современных облачных интернет-приложений.
manager: wpickett
ms.author: riande
ms.date: 02/28/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: index
ms.openlocfilehash: 63ea2aaf7b502ee08fc2f5268d17ed459adaee73
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2018
---
# <a name="introduction-to-aspnet-core"></a>Введение в ASP.NET Core

Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27), [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT) и [Шон Луттин (Shaun Luttin)](https://twitter.com/dicshaunary)

ASP.NET Core является кроссплатформенной, высокопроизводительной средой с [открытым исходным кодом](https://github.com/aspnet/home) для создания современных облачных приложений, подключенных к Интернету. ASP.NET Core позволяет выполнять следующие задачи:

* Создавать веб-приложения и службы, приложения [IoT](https://www.microsoft.com/internet-of-things/) и серверные части для мобильных приложений.
* Использовать избранные средства разработки в Windows, macOS и Linux.
* Выполнять развертывания в облаке или локальной среде.
* Работать в [.NET Core или .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>Зачем использовать ASP.NET Core?

Миллионы разработчиков использовали и продолжают использовать [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) для создания веб-приложений. ASP.NET Core — это модификация ASP.NET 4.x с архитектурными изменениями, формирующими более рациональную и более модульную платформу.

ASP.NET Core предоставляет следующие преимущества:

* Единое решение для создания пользовательского веб-интерфейса и веб-API.
* Интеграция [современных клиентских платформ](xref:client-side/index) и рабочих процессов разработки.
* Облачная [система конфигурации](xref:fundamentals/configuration/index) на основе среды.
* Встроенное [введение зависимостей](xref:fundamentals/dependency-injection).
* Упрощенный [высокопроизводительный](https://github.com/aspnet/benchmarks) модульный конвейер HTTP-запросов.
* Возможность размещения в [IIS](xref:host-and-deploy/iis/index), [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), [Docker](xref:host-and-deploy/docker/index) или в собственном процессе.
* Параллельное управление версиями приложения, ориентированное на [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
* Инструментарий, упрощающий процесс современной веб-разработки.
* Возможность сборки и запуска в ОС Windows, macOS и Linux.
* Открытый исходный код и [ориентация на сообщество](https://live.asp.net/).

ASP.NET Core поставляется полностью в виде пакетов [NuGet](https://www.nuget.org/). Использование пакетов NuGet позволяет оптимизировать приложения для включения только необходимых зависимостей. На самом деле для приложений ASP.NET Core 2.x, ориентированных на .NET Core, требуется только [один пакет NuGet](xref:fundamentals/metapackage). За счет небольшого размера контактной зоны приложения доступны такие преимущества, как более высокий уровень безопасности, минимальное обслуживание и улучшенная производительность.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Создание веб-API и пользовательского веб-интерфейса с помощью ASP.NET Core MVC

ASP.NET Core MVC предоставляет функции, которые позволяют создавать [веб-интерфейсы API](xref:tutorials/index#build-web-apis) и [веб-приложения](xref:tutorials/index#build-web-apps).

* [Шаблон Model-View-Controller (MVC)](xref:mvc/overview) помогает сделать веб-API и веб-приложения [тестируемыми](testing/index.md).
* [Страницы Razor](xref:mvc/razor-pages/index) (новый компонент в ASP.NET Core 2.0) — это основанная на страницах модель программирования, которая упрощает создание пользовательского веб-интерфейса и повышает его эффективность.
* [Разметка Razor](xref:mvc/views/razor) предоставляет эффективный синтаксис для [страниц Razor](xref:mvc/razor-pages/index) и [представлений MVC](xref:mvc/views/overview).
* [Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) позволяют серверному коду участвовать в создании и отображении HTML-элементов в файлах Razor.
* Благодаря встроенной поддержке [нескольких форматов данных и согласованию содержимого](xref:web-api/advanced/formatting) веб-API становятся доступными для множества клиентов, включая браузеры и мобильные устройства.
* [Привязка модели](xref:mvc/models/model-binding) автоматически сопоставляет данные из HTTP-запросов с параметрами методов действия.
* [Проверка модели](xref:mvc/models/validation) автоматически выполняется на стороне сервера и клиента.

## <a name="client-side-development"></a>Клиентская разработка

ASP.NET Core легко интегрируется с распространенными клиентскими платформами и библиотеками, в том числе [Angular](xref:spa/angular), [React](xref:spa/react) и [Bootstrap](xref:client-side/bootstrap). Дополнительные сведения см. в разделе о [клиентской разработке](xref:client-side/index).

## <a name="aspnet-core-targeting-net-framework"></a>ASP.NET Core для платформы .NET Framework

Приложения ASP.NET Core могут выполняться в .NET Core или .NET Framework. Приложения ASP.NET Core, предназначенные для .NET Framework, не являются кроссплатформенными &mdash; они выполняются только в Windows. Отказ от поддержки .NET Framework в ASP.NET Core не планируется. Как правило, ASP.NET Core состоит из библиотек [.NET Standard](/dotnet/standard/net-standard). Приложения, написанные для .NET Standard 2.0 запускаются везде, где есть поддержка .NET Standard 2.0.

При использовании .NET Core существуют некоторые преимущества, и их число увеличивается с каждым выпуском. Преимущества .NET Core по сравнению с .NET Framework включают:

* Кроссплатформенность. Выполняется на macOS, Linux и Windows.
* Повышение производительности
* Управление параллельными версиями
* Новые интерфейсы API
* Открыть исходный код

Мы прилагаем максимум усилий, чтобы нивелировать разрыв в API между .NET Framework и .NET Core. Благодаря [пакету обеспечения совместимости Windows](/dotnet/core/porting/windows-compat-pack) в .NET Core доступны тысячи API-интерфейсов, созданных только для Windows. Эти API-интерфейсы не были доступны в .NET Core 1.x.

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в следующих ресурсах:

* [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Учебники по ASP.NET Core](xref:tutorials/index)
* [Основы ASP.NET Core](xref:fundamentals/index)
* [В еженедельном выпуске ASP.NET Community Standup](https://live.asp.net/) рассматривается ход работы и планы команды. Помимо этого, публикуются новые блоги и стороннее программное обеспечение.
