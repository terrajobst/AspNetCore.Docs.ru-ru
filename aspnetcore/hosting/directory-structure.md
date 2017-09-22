---
title: "Структура каталогов ASP.NET Core"
author: guardrex
description: "В списке каталогов опубликованных приложений ASP.NET Core."
keywords: "ASP.NET Core, структура каталогов"
ms.author: riande
manager: wpickett
ms.date: 03/15/2017
ms.topic: article
ms.assetid: e55eb131-d42e-4bf6-b130-fd626082243c
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/directory-structure
ms.openlocfilehash: 9ec635b596520e3d19f4040758dd59c1cbca97f4
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>Структура каталогов опубликованные приложения ASP.NET Core

По [Latham Люк](https://github.com/GuardRex)

В ASP.NET Core, каталоге приложения *публикации*, состоит из файлов приложения, файлы конфигурации, статические активы, пакетов и для среды выполнения (автономные приложения). Это как предыдущие версии платформы ASP.NET, где все приложение находится в корневом каталоге веб ту же структуру каталогов.

| Тип приложения | Структура каталогов |
| --- | --- |
| Зависящие от Framework развертывания | <ul><li>Публикация\*<ul><li>журналы\* (если они включены в publishOptions)</li><li>Refs\*</li><li>среды выполнения\*</li><li>Представления\* (если они включены в publishOptions)</li><li>wwwroot\* (если они включены в publishOptions)</li><li>DLL-файлы</li><li>MyApp.deps.JSON</li><li>Имя MyApp.dll</li><li>MyApp.PDB</li><li>MyApp. PrecompiledViews.dll (если предварительная компиляция представлений Razor)</li><li>MyApp. PrecompiledViews.pdb (если предварительная компиляция представлений Razor)</li><li>MyApp.runtimeconfig.JSON</li><li>файл Web.config (если они включены в publishOptions)</li></ul></li></ul> |
| Автономное развертывание | <ul><li>Публикация\*<ul><li>журналы\* (если они включены в publishOptions)</li><li>Refs\*</li><li>Представления\* (если они включены в publishOptions)</li><li>wwwroot\* (если они включены в publishOptions)</li><li>DLL-файлы</li><li>MyApp.deps.JSON</li><li>MyApp.exe</li><li>MyApp.PDB</li><li>MyApp. PrecompiledViews.dll (если предварительная компиляция представлений Razor)</li><li>MyApp. PrecompiledViews.pdb (если предварительная компиляция представлений Razor)</li><li>MyApp.runtimeconfig.JSON</li><li>файл Web.config (если они включены в publishOptions)</li></ul></li></ul> |
\*Указывает каталог

Содержимое *публикации* представляет каталог *содержимого корневой путь*, который также называется *пути к базовой папке приложения*, развертывания. Любое имя *публикации* каталога в развертывании расположения служит физический путь на сервере для размещенного приложения. *Wwwroot* каталог, при его наличии, содержит только статические активы. *Журналы* каталог может быть включено в развертывании путем создания его в проект и добавление `<Target>` элемент, показанный ниже, для вашей *.csproj* файл или создав физически в каталог сервер.

```xml
<Target Name="CreateLogsFolder" AfterTargets="AfterPublish">
  <MakeDir Directories="$(PublishDir)logs" Condition="!Exists('$(PublishDir)logs')" />
  <MakeDir Directories="$(PublishUrl)Logs" Condition="!Exists('$(PublishUrl)Logs')" />
</Target>
```

Первый `<MakeDir>` элемент, который использует `PublishDir` , используется объектами .NET Core CLI для определения расположения целевой для операции публикации. Второй `<MakeDir>` элемент, который использует `PublishUrl` свойство, используется средой Visual Studio для определения целевого расположения. Visual Studio использует `PublishUrl` свойство для совместимости с проектами .NET Core.

Каталог развертывания требует разрешений на чтение и выполнение, пока *журналы* directory требуются разрешения на чтение и запись. Дополнительные каталоги, в который будет записан активы требуются разрешения на чтение и запись.
