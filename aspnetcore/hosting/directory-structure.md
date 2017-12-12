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
ms.openlocfilehash: 60797bff85a44dd10caad4aabc109ee12dedfe61
ms.sourcegitcommit: 7efdc4b6025ad70c15c26bf7451c3c0411123794
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2017
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>Структура каталогов опубликованные приложения ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

В ASP.NET Core, каталоге приложения *публикации*, состоит из файлов приложения, файлы конфигурации, статические активы, пакетов и для среды выполнения (автономные приложения). Это как предыдущие версии платформы ASP.NET, где все приложение находится в корневом каталоге веб ту же структуру каталогов.

| Тип приложения | Структура каталогов |
| --- | --- |
| Зависящие от Framework развертывания | <ul><li>Публикация\*<ul><li>журналы\* (если они включены в publishOptions)</li><li>Refs\*</li><li>среды выполнения\*</li><li>Представления\* (если они включены в publishOptions)</li><li>wwwroot\* (если они включены в publishOptions)</li><li>DLL-файлы</li><li>MyApp.deps.JSON</li><li>Имя MyApp.dll</li><li>MyApp.PDB</li><li>MyApp. PrecompiledViews.dll (если предварительная компиляция представлений Razor)</li><li>MyApp. PrecompiledViews.pdb (если предварительная компиляция представлений Razor)</li><li>MyApp.runtimeconfig.JSON</li><li>файл Web.config (если они включены в publishOptions)</li></ul></li></ul> |
| Автономное развертывание | <ul><li>Публикация\*<ul><li>журналы\* (если они включены в publishOptions)</li><li>Refs\*</li><li>Представления\* (если они включены в publishOptions)</li><li>wwwroot\* (если они включены в publishOptions)</li><li>DLL-файлы</li><li>MyApp.deps.JSON</li><li>MyApp.exe</li><li>MyApp.PDB</li><li>MyApp. PrecompiledViews.dll (если предварительная компиляция представлений Razor)</li><li>MyApp. PrecompiledViews.pdb (если предварительная компиляция представлений Razor)</li><li>MyApp.runtimeconfig.JSON</li><li>файл Web.config (если они включены в publishOptions)</li></ul></li></ul> |
\*Указывает каталог

Содержимое *публикации* представляет каталог *содержимого корневой путь*, который также называется *пути к базовой папке приложения*, развертывания. Любое имя *публикации* каталога в развертывании расположения служит физический путь на сервере для размещенного приложения. *Wwwroot* каталог, при его наличии, содержит только статические активы. *Журналы* каталог может быть включено в развертывании путем создания его в проект и добавление `<Target>` элемент, показанный ниже, для вашей *.csproj* файл или создав физически в каталог сервер.

```xml
<Target Name="CreateLogsFolder" AfterTargets="Publish">
  <MakeDir Directories="$(PublishDir)Logs" 
           Condition="!Exists('$(PublishDir)Logs')" />
  <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                    Lines="Generated file" 
                    Overwrite="True" 
                    Condition="!Exists('$(PublishDir)Logs\.log')" />
</Target>
```

`<MakeDir>` Создает пустой элемент *журналы* папки в опубликованной выходных данных. Элемент использует `PublishDir` свойства, чтобы определить целевое расположение для создания папки. Несколько способов развертывания, таких как веб-развертывания, пропустите пустые папки во время развертывания. `<WriteLinesToFile>` Элемент создает файл в *журналы* папку, которая гарантирует развертывания папки на сервере. Обратите внимание, что создание папки может завершиться ошибкой, если рабочий процесс не имеет доступа на запись в папку назначения.

Каталог развертывания требует разрешений на чтение и выполнение, пока *журналы* directory требуются разрешения на чтение и запись. Дополнительные каталоги, в который будет записан активы требуются разрешения на чтение и запись.
