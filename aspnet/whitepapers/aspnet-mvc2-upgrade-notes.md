---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Обновление приложения ASP.NET MVC 1.0 для ASP.NET MVC 2 | Документы Microsoft
author: rick-anderson
description: В этом документе описываются оба способа обновления и вручную с помощью мастера приложения ASP.NET MVC 1.0 для ASP.NET MVC 2. В этом документе также доступна для d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: d1227f0738b2d2a396fed942f32ae5e75579596e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530063"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Обновление приложения ASP.NET MVC 1.0 для ASP.NET MVC 2
====================
> В этом документе описываются оба способа обновления и вручную с помощью мастера приложения ASP.NET MVC 1.0 для ASP.NET MVC 2. В этом документе также доступен для [загрузки](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>Вступление

ASP.NET MVC 2 можно установить параллельно с ASP.NET MVC 1.0 на том же сервере. Это предоставляет разработчикам гибкость в выборе при обновлении приложения ASP.NET MVC 1.0 для ASP.NET MVC 2.

Visual Studio 2010 содержит мастер, обновления существующих проектов ASP.NET MVC 1.0, созданные с помощью Visual Studio 2008 в ASP.NET MVC 2. Мастер обновления инициируется Открытие проекта ASP.NET MVC 1.0 в Visual Studio 2010.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Мастер для ASP.NET MVC 1.0 для Visual Studio 2008 SP1 обновления

Чтобы обновить приложение ASP.NET MVC 1.0 для ASP.NET MVC 2 в Visual Studio 2008 SP1, используйте (неподдерживаемые) MvcAppConverter приложение. Это приложение можно загрузить со следующего URL:

[https://go.Microsoft.com/fwlink/?LinkId=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Вручную обновление проекта ASP.NET MVC 1.0

Чтобы вручную обновить существующее приложение ASP.NET MVC 1.0 до версии 2, выполните следующие действия:

1. Создайте резервную копию существующего проекта.
2. В текстовом редакторе откройте файл проекта (файл с расширением .csproj или .vbproj) и найдите элемент ProjectTypeGuid. Значением этого элемента замените GUID {603c0e0b-db56-11dc-be95-000d561079b0} с {F85E285D-A4E0-4152-9332-AB1D724D3325}. Когда вы закончите, значение этого элемента должен быть следующим: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. В корневой папке веб-приложений измените файл Web.config. Поиск System.Web.Mvc, версия = 1.0.0.0 и замените все экземпляры System.Web.Mvc, версия = 2.0.0.0.
4. Повторите предыдущий шаг для файла Web.config, расположенный в папке «представления».
5. Откройте проект с помощью Visual Studio и в **обозревателе решений**, разверните **ссылки** узла. Удалите ссылку на System.Web.Mvc (которая указывает на сборку версии 1.0). Добавьте ссылку на System.Web.Mvc (v2.0.0.0).
6. Добавьте следующий элемент bindingRedirect в файл Web.config в корневой папке приложения в разделе конфигурации:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Создайте новое пустое приложение ASP.NET MVC 2. Скопируйте файлы из папки сценариев нового приложения в папку «Скрипты» существующего приложения.
8. Обновление существующего applicationâ€™ s CSS-файл в файле Site.css определений стилей CSS.
9. Скомпилируйте приложение и запустите его. Если возникнут ошибки, см. раздел критические изменения [новые возможности в ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) страницы.
