---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Обновление приложения ASP.NET MVC 1.0 до ASP.NET MVC 2 | Документация Майкрософт
author: rick-anderson
description: В этом документе описывается, как обновление вручную и с помощью мастера приложения ASP.NET MVC 1.0 до ASP.NET MVC 2. В этом документе также доступна для d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 354dbab3057068eb13b16c9aa31f66c72371ce7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381920"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Обновление приложения ASP.NET MVC 1.0 до ASP.NET MVC 2
====================
> В этом документе описывается, как обновление вручную и с помощью мастера приложения ASP.NET MVC 1.0 до ASP.NET MVC 2. В этом документе также доступна для [загрузки](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>Вступление

ASP.NET MVC 2 может устанавливаться параллельно с ASP.NET MVC 1.0 на одном сервере. Это обеспечивает гибкость разработчикам приложений Выбор места для обновления приложения ASP.NET MVC 1.0 до ASP.NET MVC 2.

Visual Studio 2010 включает в себя Мастер этого обновления существующих проектов ASP.NET MVC 1.0, созданные с помощью Visual Studio 2008 и ASP.NET MVC 2. Мастер обновления инициируется Открытие проекта ASP.NET MVC 1.0 в Visual Studio 2010.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Обновление мастера для ASP.NET MVC 1.0 для Visual Studio 2008 с пакетом обновления 1

Для обновления приложения ASP.NET MVC 1.0 до ASP.NET MVC 2 в Visual Studio 2008 SP1, используйте (неподдерживаемые) MvcAppConverter приложение. Это приложение можно загрузить со следующего URL:

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Вручную обновление проекта ASP.NET MVC 1.0

Чтобы вручную обновить существующее приложение ASP.NET MVC 1.0 до версии 2, выполните следующие действия:

1. Создайте резервную копию существующего проекта.
2. В текстовом редакторе откройте файл проекта (файл с расширением CSPROJ или VBPROJ) и найдите элемент ProjectTypeGuid. Как значение этого элемента замените GUID {603c0e0b-db56-11dc-be95-000d561079b0} с {F85E285D-A4E0-4152-9332-AB1D724D3325}. Когда вы закончите, значение этого элемента должно быть следующим: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. В корневой папке веб-приложения измените файл Web.config. Поиск System.Web.Mvc, версия = 1.0.0.0 и замените все экземпляры System.Web.Mvc, версия = 2.0.0.0.
4. Повторите предыдущий шаг для файла Web.config, расположенный в папке Views.
5. Откройте проект с помощью Visual Studio и в **обозревателе решений**, разверните **ссылки** узла. Удалите ссылку на System.Web.Mvc (которая указывает на сборку версии 1.0). Добавьте ссылку на System.Web.Mvc (v2.0.0.0).
6. Добавьте следующий элемент bindingRedirect в файл Web.config в корневой папке приложения в разделе "Конфигурация":   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Создайте новое приложение ASP.NET MVC 2 пустой. Скопируйте файлы из папки скриптов нового приложения в папку «Скрипты» существующего приложения.
8. Обновление существующего приложения €™ s CSS-файл с помощью определения стилей CSS в файле Site.css.
9. Скомпилируйте приложение и запустите его. Если возникли ошибки, обратитесь к разделу критические изменения [новые возможности в ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) страницы.
