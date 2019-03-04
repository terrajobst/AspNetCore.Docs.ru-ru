---
title: Совместимая версия для ASP.NET Core MVC
author: rick-anderson
description: Сведения о том, как класс Startup в ASP.NET Core настраивает службы и конвейер запросов приложения.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2019
uid: mvc/compatibility-version
ms.openlocfilehash: b360da105799a1dccb1902e167e50e78864b76a9
ms.sourcegitcommit: 0945078a09c372f17e9b003758ed87e99c2449f4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2019
ms.locfileid: "56647932"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a>Совместимая версия для ASP.NET Core MVC

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Метод <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> позволяет приложению принимать или отклонять потенциально критические изменения в поведении, появившиеся в ASP.NET Core MVC 2.1 или более поздних версий. Эти потенциально критические изменения обычно затрагивают поведение подсистем MVC и способы вызова **кода** средой выполнения. Если принять эти изменения, вы сможете использовать актуальное поведение, которое сохранится в ASP.NET Core.

Следующий код задает режим совместимости в ASP.NET Core 2.2:

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

Мы рекомендуем протестировать приложение с помощью последней версии (`CompatibilityVersion.Version_2_2`). Мы полагаем, что в большинстве приложений использование последней версии не приведет к критически важным изменениям в поведении.

Приложения, вызывающие `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`, защищены от потенциальных критически важных изменений в поведении, появившихся в MVC ASP.NET 2.1 и более поздних версий 2.x. Особенности защиты:

* Не применяется ко всем изменениям в версии 2.1 и более поздних версиях. Она направлена только на потенциально критические изменения в поведении среды выполнения ASP.NET Core в подсистеме MVC.
* Не распространяется на следующую основную версию.

По умолчанию для приложений ASP.NET Core 2.1 и более поздних версий 2.x, которые **не** вызывают `SetCompatibilityVersion`, предусмотрена совместимость с 2.0. Отсутствие вызова `SetCompatibilityVersion` равноценно вызову `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.

Следующий код задает режим совместимости в ASP.NET Core 2.2, за исключением следующего поведения:

* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowCombiningAuthorizeFilters>
* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.InputFormatterExceptionPolicy>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

Если в приложении возникают критические изменения в поведении, использование параметров совместимости имеет следующие преимущества:

* Позволяет использовать последний выпуск и отказаться от конкретных критических изменений.
* Дает вам время обновить приложение, чтобы оно работало с последними изменениями.

В документации по <xref:Microsoft.AspNetCore.Mvc.MvcOptions> подробно объясняется, что изменилось и почему изменения выгодны для большинства пользователей.

Позднее мы выпустим [версию ASP.NET Core 3.0](https://github.com/aspnet/Home/wiki/Roadmap). Старое поведение, поддерживаемое параметрами совместимости, не сохранится в версии 3.0. Мы думаем, что это полезные изменения почти для всех пользователей. Большинство приложений сможет воспользоваться этими изменениями уже сейчас, а другие успеют обновиться.
