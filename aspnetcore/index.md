---
title: "Введение в ASP.NET Core"
author: rick-anderson
description: "Общие сведения о работе с ASP.NET Core."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: a075c63fddb9e28a1da37d4ef6647808a0dcb583
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-aspnet-core"></a>Введение в ASP.NET Core

Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27), [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT) и [Шон Луттин (Shaun Luttin)](https://twitter.com/dicshaunary)

ASP.NET Core является кроссплатформенной, высокопроизводительной средой с [открытым исходным кодом](https://github.com/aspnet/home) для создания современных облачных приложений, подключенных к Интернету. ASP.NET Core позволяет выполнять следующие задачи:

* Создавать веб-приложения и службы, приложения [IoT](https://www.microsoft.com/en-us/internet-of-things/) и серверные части для мобильных приложений.
* Использовать избранные средства разработки в Windows, macOS и Linux.
* Выполнять развертывания в облаке и локальной среде.
* Работать в [.NET Core или .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>Зачем использовать ASP.NET Core?

Миллионы разработчиков использовали и продолжают использовать ASP.NET для создания веб-приложений. ASP.NET Core является модификацией ASP.NET с внесенными архитектурными изменениями, формирующими более рациональную и модульную платформу.

ASP.NET Core предоставляет следующие преимущества:

* Единое решение для создания пользовательского веб-интерфейса и веб-API.
* Интеграция [современных клиентских платформ](xref:client-side/index) и рабочих процессов разработки.
* Облачная [система конфигурации](xref:fundamentals/configuration/index) на основе среды.
* Встроенное [введение зависимостей](xref:fundamentals/dependency-injection).
* Упрощенный, высокопроизводительный и модульный конвейер HTTP-запросов.
* Возможность размещения в IIS или в собственном процессе.
* Возможность запуска на платформе [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), которая поддерживает параллельное управление версиями приложения.
* Инструментарий, упрощающий процесс современной веб-разработки.
* Возможность сборки и запуска в ОС Windows, macOS и Linux.
* Открытый исходный код и ориентация на сообщество.

ASP.NET Core поставляется полностью в виде пакетов [NuGet](https://www.nuget.org/). Это позволяет оптимизировать приложения для включения только необходимых пакетов NuGet. За счет небольшого размера контактной зоны приложения доступны такие преимущества, как более высокий уровень безопасности, минимальное обслуживание и улучшенная производительность.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Создание веб-API и пользовательского веб-интерфейса с помощью ASP.NET Core MVC

ASP.NET Core MVC предоставляет функциональные возможности, упрощающие создание [веб-API](xref:tutorials/index#building-web-apis) и [веб-приложений](xref:tutorials/index#building-web-applications).

* [Шаблон Model-View-Controller (MVC)](xref:mvc/overview) помогает сделать веб-API и веб-приложения [тестируемыми](testing/index.md).
* [Страницы Razor](xref:mvc/razor-pages/index) (новинка в версии 2.0) — это основанная на страницах модель программирования, которая упрощает и повышает эффективность создания пользовательского веб-интерфейса.
* [Синтаксис Razor](xref:mvc/views/razor) предоставляет эффективный язык для [страниц Razor](xref:mvc/razor-pages/index) и [представлений MVC](xref:mvc/views/overview).
* [Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) позволяют серверному коду участвовать в создании и отображении HTML-элементов в файлах Razor.
* Благодаря встроенной поддержке [нескольких форматов данных и согласованию содержимого](mvc/models/formatting.md) веб-API становятся доступными для множества клиентов, включая браузеры и мобильные устройства.
* [Привязка модели](xref:mvc/models/model-binding) автоматически сопоставляет данные из HTTP-запросы с параметрами метода действия.
* [Проверка модели](xref:mvc/models/validation) автоматически выполняет проверку на стороне сервера и клиента.

## <a name="client-side-development"></a>Клиентская разработка

ASP.NET Core легко интегрируется с широким спектром клиентских платформ, включая [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout) и [Bootstrap](xref:client-side/bootstrap). Дополнительные сведения см. в разделе о [клиентской разработке](client-side/index.md).

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в следующих ресурсах:

* [Учебники по ASP.NET Core](xref:tutorials/index)
* [Основы ASP.NET Core](xref:fundamentals/index)
* [В еженедельном выпуске ASP.NET community standup](https://live.asp.net/) рассматривается ход работы и планы команды, а также публикуются новые блоги и стороннее программное обеспечение.
