---
title: Структура каталогов ASP.NET Core
author: guardrex
description: Дополнительные сведения о структуре каталогов опубликованные приложения ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: ac9b777bcc7f4a8634161fc1347a4d0fdc3b4784
ms.sourcegitcommit: 7c8fd9b7445cd77eb7f7d774bfd120c26f3b5d84
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/19/2018
---
# <a name="aspnet-core-directory-structure"></a>Структура каталогов ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

В ASP.NET Core, каталоге опубликованное приложение *публикации*, состоит из файлов приложения, файлы конфигурации, статические активы, пакеты и среды выполнения (для [самодостаточным развертываний](/dotnet/core/deploying/#self-contained-deployments-scd)).


| Тип приложения | Структура каталогов |
| -------- | ------------------- |
| [Зависящие от Framework развертывания](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>Публикация&dagger;<ul><li>журналы&dagger; (необязательно, если не требуется для получения журналов stdout)</li><li>Представления&dagger; (приложения MVC; при представления не)</li><li>Страницы&dagger; (MVC или страниц Razor приложения; Если не являются предварительно скомпилированные страницы)</li><li>wwwroot&dagger;</li><li>*\.DLL-файлы</li><li>\<имя сборки >. deps.json</li><li>\<имя сборки > .dll</li><li>\<имя сборки > PDB-файл</li><li>\<имя сборки >. PrecompiledViews.dll</li><li>\<имя сборки >. PrecompiledViews.pdb</li><li>\<имя сборки >. runtimeconfig.json</li><li>файл Web.config (развертывание IIS)</li></ul></li></ul> |
| [Автономное развертывание](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>Публикация&dagger;<ul><li>журналы&dagger; (необязательно, если не требуется для получения журналов stdout)</li><li>Refs&dagger;</li><li>Представления&dagger; (приложения MVC; при представления не)</li><li>Страницы&dagger; (MVC или страниц Razor приложения; Если не являются предварительно скомпилированные страницы)</li><li>wwwroot&dagger;</li><li>\*DLL-файлы</li><li>\<имя сборки >. deps.json</li><li>\<имя сборки > .exe</li><li>\<имя сборки > PDB-файл</li><li>\<имя сборки >. PrecompiledViews.dll</li><li>\<имя сборки >. PrecompiledViews.pdb</li><li>\<имя сборки >. runtimeconfig.json</li><li>файл Web.config (развертывание IIS)</li></ul></li></ul> |

&dagger;Указывает каталог

*Публикации* представляет каталог *содержимого корневой путь*, который также называется *пути к базовой папке приложения*, развертывания. Любое имя *публикации* каталога развернутого приложения на сервере, его расположение служит физический путь на сервере для размещенного приложения.

*Wwwroot* каталог, при его наличии, содержит только статические активы.

Stdout *журналы* удается создать каталог развертывания, с помощью одного из следующих двух способов:

* Добавьте следующие `<Target>` элемент в файл проекта:

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

* Создание физически *журналы* каталог на сервере в развертывании.

Каталог развертывания требует разрешения на чтение и выполнение. *Журналы* directory требуются разрешения на чтение и запись. Дополнительные каталоги, в который записываются файлы требуются разрешения на чтение и запись.
