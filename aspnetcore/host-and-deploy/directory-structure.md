---
title: "Структура каталогов ASP.NET Core"
author: guardrex
description: "См. структуру каталогов опубликованных приложений ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 27f0f40aea1c55315642d7d6f9b9d7be3e111cb4
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2018
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>Структура каталогов опубликованные приложения ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

В ASP.NET Core, каталоге приложения *публикации*, состоит из файлов приложения, файлы конфигурации, статические активы, пакетов и для среды выполнения (автономные приложения).

| Тип приложения                       | Структура каталогов |
| ------------------------------ | ------------------- |
| Зависящие от Framework развертывания | <ul><li>Публикация\*<ul><li>журналы\* (если они включены в publishOptions)</li><li>refs\*</li><li>среды выполнения\*</li><li>Представления\* (если они включены в publishOptions)</li><li>wwwroot\* (если они включены в publishOptions)</li><li>DLL-файлы</li><li>myapp.deps.json</li><li>Имя MyApp.dll</li><li>MyApp.PDB</li><li>MyApp. PrecompiledViews.dll (если предварительная компиляция представлений Razor)</li><li>MyApp. PrecompiledViews.pdb (если предварительная компиляция представлений Razor)</li><li>myapp.runtimeconfig.json</li><li>файл Web.config (если они включены в publishOptions)</li></ul></li></ul> |
| Автономное развертывание      | <ul><li>Публикация\*<ul><li>журналы\* (если они включены в publishOptions)</li><li>refs\*</li><li>Представления\* (если они включены в publishOptions)</li><li>wwwroot\* (если они включены в publishOptions)</li><li>DLL-файлы</li><li>myapp.deps.json</li><li>myapp.exe</li><li>MyApp.PDB</li><li>MyApp. PrecompiledViews.dll (если предварительная компиляция представлений Razor)</li><li>MyApp. PrecompiledViews.pdb (если предварительная компиляция представлений Razor)</li><li>myapp.runtimeconfig.json</li><li>файл Web.config (если они включены в publishOptions)</li></ul></li></ul> |
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
