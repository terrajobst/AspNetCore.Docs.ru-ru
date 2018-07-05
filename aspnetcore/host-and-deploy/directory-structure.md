---
title: Структура каталогов ASP.NET Core
author: guardrex
description: Сведения о структуре каталогов опубликованных приложений ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 8e2693397f826d0e9a36ff52aa1d1d623b31043d
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960831"
---
# <a name="aspnet-core-directory-structure"></a>Структура каталогов ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

В ASP.NET Core каталог опубликованных приложений *publish* содержит файлы приложений, файлы конфигурации, статические ресурсы, пакеты и среды выполнения (для [автономных развертываний](/dotnet/core/deploying/#self-contained-deployments-scd)).


| Тип приложения | Структура каталогов |
| -------- | ------------------- |
| [Развертывание, зависящее от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>publish&dagger;<ul><li>Logs&dagger; (необязательно, если не требуется для получения журналов stdout)</li><li>Views&dagger; (в приложениях MVC, если представления не компилируются заранее)</li><li>Pages&dagger; (в приложениях MVC или Razor Pages, если страницы не компилируются заранее)</li><li>wwwroot&dagger;</li><li>*\.DLL-файлы</li><li>\<имя_сборки>.deps.json</li><li>\<имя_сборки>.dll</li><li>\<имя_сборки>.pdb</li><li>\<имя_сборки>.PrecompiledViews.dll</li><li>\<имя_сборки>.PrecompiledViews.pdb</li><li>\<имя_сборки>.runtimeconfig.json</li><li>web.config (в развертываниях IIS)</li></ul></li></ul> |
| [Автономное развертывание](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>publish&dagger;<ul><li>Logs&dagger; (необязательно, если не требуется для получения журналов stdout)</li><li>refs&dagger;</li><li>Views&dagger; (в приложениях MVC, если представления не компилируются заранее)</li><li>Pages&dagger; (в приложениях MVC или Razor Pages, если страницы не компилируются заранее)</li><li>wwwroot&dagger;</li><li>\*DLL-файлы</li><li>\<имя_сборки>.deps.json</li><li>\<имя_сборки>.exe</li><li>\<имя_сборки>.pdb</li><li>\<имя_сборки>.PrecompiledViews.dll</li><li>\<имя_сборки>.PrecompiledViews.pdb</li><li>\<имя_сборки>.runtimeconfig.json</li><li>web.config (в развертываниях IIS)</li></ul></li></ul> |

&dagger;Обозначает каталог

Каталог *публикации* представляет *корневой путь содержимого* для развертывания, который также называется *путь к базовой папке приложения*. Независимо от того, какое имя присвоено каталогу *публикации*, развернутого на сервере приложения, именно это расположение обозначает физический путь к размещенному приложению на этом сервере.

Каталог *wwwroot*, если таковой имеется, содержит только статические активы.

Каталог *Logs* для журналов stdout можно создать для развертывания одним из следующих двух методов:

* Добавьте в файл проекта элемент `<Target>` следующего содержания:

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

   Элемент `<MakeDir>` создает пустую папку *Logs* в публикуемых выходных данных. Этот элемент использует свойство `PublishDir`, чтобы определить целевое расположение для создания папки. Несколько методов развертывания, например веб-развертывание, пропускают пустые папки во время развертывания. Элемент `<WriteLinesToFile>` создает файл в папке *Logs*, чтобы гарантировать ее развертывание на целевом сервере. Обратите внимание, что создание папки может завершиться ошибкой, если рабочий процесс не имеет прав на запись в целевую папку.

* Самостоятельно создайте физическую папку *Logs* на сервере в каталоге развертывания.

Для каталога развертывания нужны права на чтение и выполнение. Для каталога *Logs* нужны права на чтение и запись. Для дополнительных каталогов, в которые записываются файлы, нужны права на чтение и запись.
