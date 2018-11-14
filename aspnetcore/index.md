---
title: Введение в ASP.NET Core
author: rick-anderson
description: Введение в ASP.NET Core — кроссплатформенную высокопроизводительную платформу с открытым исходным кодом для создания современных облачных интернет-приложений.
ms.author: riande
ms.custom: mvc
ms.date: 11/10/2018
uid: index
ms.openlocfilehash: 1699acc0086dfd50c573afc239bc8f37eb9e7af9
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2018
ms.locfileid: "51569992"
---
# <a name="introduction-to-aspnet-core"></a>Введение в ASP.NET Core

Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27), [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT) и [Шон Луттин (Shaun Luttin)](https://twitter.com/dicshaunary)

ASP.NET Core является кроссплатформенной, высокопроизводительной средой с [открытым исходным кодом](https://github.com/aspnet/home) для создания современных облачных приложений, подключенных к Интернету. ASP.NET Core позволяет выполнять следующие задачи:

* Создавать веб-приложения и службы, приложения [IoT](https://www.microsoft.com/internet-of-things/) и серверные части для мобильных приложений.
* Использовать избранные средства разработки в Windows, macOS и Linux.
* Выполнять развертывания в облаке или локальной среде.
* Работать в [.NET Core или .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>Зачем использовать ASP.NET Core?

Миллионы разработчиков использовали и продолжают использовать [ASP.NET 4.x](/aspnet/overview) для создания веб-приложений. ASP.NET Core — это модификация ASP.NET 4.x с архитектурными изменениями, формирующими более рациональную и более модульную платформу.

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Создание веб-API и пользовательского веб-интерфейса с помощью ASP.NET Core MVC

ASP.NET Core MVC предоставляет функции, которые позволяют создавать [веб-интерфейсы API](xref:tutorials/first-web-api) и [веб-приложения](xref:tutorials/razor-pages/index).

* [Шаблон Model-View-Controller (MVC)](xref:mvc/overview) помогает сделать веб-API и веб-приложения тестируемыми.
* [Страницы Razor](xref:razor-pages/index) (новый компонент в ASP.NET Core 2.0) — это основанная на страницах модель программирования, которая упрощает создание пользовательского веб-интерфейса и повышает его эффективность.
* [Разметка Razor](xref:mvc/views/razor) предоставляет эффективный синтаксис для [страниц Razor](xref:razor-pages/index) и [представлений MVC](xref:mvc/views/overview).
* [Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) позволяют серверному коду участвовать в создании и отображении HTML-элементов в файлах Razor.
* Благодаря встроенной поддержке [нескольких форматов данных и согласованию содержимого](xref:web-api/advanced/formatting) веб-API становятся доступными для множества клиентов, включая браузеры и мобильные устройства.
* [Привязка модели](xref:mvc/models/model-binding) автоматически сопоставляет данные из HTTP-запросов с параметрами методов действия.
* [Проверка модели](xref:mvc/models/validation) автоматически выполняется на стороне сервера и клиента.

## <a name="client-side-development"></a>Клиентская разработка

ASP.NET Core легко интегрируется с распространенными клиентскими платформами и библиотеками, в том числе [Angular](xref:spa/angular), [React](xref:spa/react) и [Bootstrap](https://getbootstrap.com/). Дополнительные сведения см. в разделе о [клиентской разработке](xref:client-side/index).

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a>ASP.NET Core для платформы .NET Framework

Приложения ASP.NET Core 2.x могут выполняться в .NET Core или .NET Framework. Приложения ASP.NET Core, предназначенные для .NET Framework, не являются кроссплатформенными &mdash; они выполняются только в Windows. Как правило, ASP.NET Core 2.x состоит из библиотек [.NET Standard](/dotnet/standard/net-standard). Приложения, написанные для .NET Standard 2.0 запускаются везде, где есть поддержка .NET Standard 2.0.

ASP.NET Core 2.x поддерживается в версиях .NET Framework, совместимых с .NET Standard 2.0:

* .NET Framework 4.7.1 и более поздних версий (рекомендуется).
* .NET Framework 4.6.1 и более поздних версий.

ASP.NET Core версии 3.0 и более поздних будут выполняться только в .NET Core. Дополнительные сведения об этом изменении см. в разделе [Первое знакомство с предстоящими изменениями в ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).

При использовании .NET Core существуют некоторые преимущества, и их число увеличивается с каждым выпуском. Преимущества .NET Core по сравнению с .NET Framework включают:

* Кроссплатформенность. Выполняется на macOS, Linux и Windows.
* Повышение производительности
* Управление параллельными версиями
* Новые интерфейсы API
* Открытый исходный код

Мы прилагаем максимум усилий, чтобы устранить различия API между .NET Framework и .NET Core. Благодаря [пакету обеспечения совместимости Windows](/dotnet/core/porting/windows-compat-pack) в .NET Core доступны тысячи API-интерфейсов, созданных только для Windows. Эти API-интерфейсы не были доступны в .NET Core 1.x.

## <a name="how-to-download-a-sample"></a>Загрузка примера

Многие статьи и учебники содержат ссылки на примеры кода.

1. [Загрузите ZIP-файл репозитория ASP.NET](https://codeload.github.com/aspnet/Docs/zip/master).
1. Распакуйте файл *Docs-master.zip*.
1. Перейдите в папку примера по URL-адресу, указанному в примере.

Для демонстрации нескольких сценариев в примерах приложений используются инструкции C# `#define` и `#if-#else/#elif-#endif`, выборочно компилирующие и запускающие разные фрагменты примеров кода. В примерах, где применяется этот подход, задайте в начале файлов C# инструкцию `#define` для символа, связанного со сценарием, который нужно запустить. Возможно, для запуска сценария в примере потребуется задать символ в начале нескольких файлов.

Например, в следующем списке символов `#define` видно, что доступно четыре сценария (один сценарий на символ). В текущем примере конфигурации запускается сценарий `TemplateCode`:

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

Чтобы запустить в примере сценарий `ExpandDefault`, задайте символ `ExpandDefault` и оставьте остальные символы раскомментированными:

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

Дополнительные сведения об использовании [директив препроцессора C#](/dotnet/csharp/language-reference/preprocessor-directives/) для выборочной компиляции фрагментов кода см. в разделах [#define (Справочник по C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) и [#if (Справочник по C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в следующих ресурсах:

* [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Основы ASP.NET Core](xref:fundamentals/index)
* [В еженедельном выпуске ASP.NET Community Standup](https://live.asp.net/) рассматривается ход работы и планы команды. Помимо этого, публикуются новые блоги и стороннее программное обеспечение.
