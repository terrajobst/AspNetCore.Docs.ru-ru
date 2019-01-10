---
title: Структура каталогов ASP.NET Core
author: guardrex
description: Сведения о структуре каталогов опубликованных приложений ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 12/11/2018
uid: host-and-deploy/directory-structure
ms.openlocfilehash: ee0bebb8b5c688f8471d6420d1641b87ac271f6c
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284569"
---
# <a name="aspnet-core-directory-structure"></a>Структура каталогов ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Каталог *публикации* содержит развертываемые ресурсы приложения, созданные командной [dotnet publish](/dotnet/core/tools/dotnet-publish). Каталог содержит следующее.

* Файлы приложения.
* Файлы конфигурации.
* Статические ресурсы.
* Пакеты
* Среда выполнения ([только автономное развертывание](/dotnet/core/deploying/#self-contained-deployments-scd))

| Тип приложения | Структура каталогов |
| -------- | ------------------- |
| [Развертывание, зависящее от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>publish&dagger;<ul><li>Logs&dagger; (необязательно, если не требуется для получения журналов stdout)</li><li>Views&dagger; (в приложениях MVC, если представления не компилируются заранее)</li><li>Pages&dagger; (в приложениях MVC или Razor Pages, если страницы не компилируются заранее)</li><li>wwwroot&dagger;</li><li>*\.DLL-файлы</li><li>{имя_сборки}.deps.json</li><li>{имя_сборки}.dll</li><li>{имя_сборки}.pdb</li><li>{имя_сборки}.Views.dll</li><li>{имя_сборки}.Views.pdb</li><li>{имя_сборки}.runtimeconfig.json</li><li>web.config (в развертываниях IIS)</li></ul></li></ul> |
| [Автономное развертывание](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>publish&dagger;<ul><li>Logs&dagger; (необязательно, если не требуется для получения журналов stdout)</li><li>Views&dagger; (в приложениях MVC, если представления не компилируются заранее)</li><li>Pages&dagger; (в приложениях MVC или Razor Pages, если страницы не компилируются заранее)</li><li>wwwroot&dagger;</li><li>\*DLL-файлы</li><li>{имя_сборки}.deps.json</li><li>{имя_сборки}.dll</li><li>{имя_сборки}.exe</li><li>{имя_сборки}.pdb</li><li>{имя_сборки}.Views.dll</li><li>{имя_сборки}.Views.pdb</li><li>{имя_сборки}.runtimeconfig.json</li><li>web.config (в развертываниях IIS)</li></ul></li></ul> |

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

   Элемент `<MakeDir>` создает пустую папку *Logs* в публикуемых выходных данных. Этот элемент использует свойство `PublishDir`, чтобы определить целевое расположение для создания папки. Несколько методов развертывания, например веб-развертывание, пропускают пустые папки во время развертывания. Элемент `<WriteLinesToFile>` создает файл в папке *Logs*, чтобы гарантировать ее развертывание на целевом сервере. Создание папки с помощью этого подхода завершается ошибкой, если рабочий процесс не имеет прав на запись в целевую папку.

* Самостоятельно создайте физическую папку *Logs* на сервере в каталоге развертывания.

Для каталога развертывания нужны права на чтение и выполнение. Для каталога *Logs* нужны права на чтение и запись. Для дополнительных каталогов, в которые записываются файлы, нужны права на чтение и запись.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* [Развертывание приложений .NET Core](/dotnet/core/deploying/)
* [Целевые платформы](/dotnet/standard/frameworks)
* [Каталог относительных идентификаторов в .NET Core](/dotnet/core/rid-catalog)
