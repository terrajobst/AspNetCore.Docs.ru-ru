---
title: Размещение и развертывание компонентов Razor и Blazor
author: guardrex
description: Размещение и развертывание компонентов Razor и приложений Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/razor-components-blazor/index
ms.openlocfilehash: eeaa27bef584523b77d8b47000b310fe0cac7341
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2019
ms.locfileid: "59070311"
---
# <a name="host-and-deploy-razor-components-and-blazor"></a>Размещение и развертывание компонентов Razor и Blazor

Авторы: [Люк Лэтем](https://github.com/guardrex), [Рэйнер Стропек](https://www.timecockpit.com) и [Дэниэл Рот](https://github.com/danroth27)

## <a name="publish-the-app"></a>Публикация приложения

Приложения публикуются для развертывания в конфигурации выпуска при помощи команды [dotnet publish](/dotnet/core/tools/dotnet-publish). Интегрированная среда разработки (IDE) может обрабатывать выполнение команды `dotnet publish` автоматически с помощью своих встроенных возможностей публикации, поэтому может быть необязательно вручную выполнять команду из командной строки в зависимости от используемых средств разработки.

```console
dotnet publish -c Release
```

`dotnet publish` активирует [восстановление](/dotnet/core/tools/dotnet-restore) зависимостей проекта и выполняет [сборку](/dotnet/core/tools/dotnet-build) проекта, прежде чем создавать ресурсы для развертывания. В ходе процесса построения удаляются неиспользуемые методы и сборки, чтобы уменьшить размер скачиваемого приложения и время загрузки. Развертывание создается в папке */bin/Release/{целевая_платформа}/publish*.

Ресурсы в папке *publish* развертываются на веб-сервере. Развертывание может проводиться вручную или автоматизированно в зависимости от используемых средств разработки.

## <a name="deployment"></a>Развертывание

Инструкции по развертыванию см. в следующих разделах:

* [Компоненты Blazor на стороне клиента](xref:host-and-deploy/razor-components-blazor/blazor)
* [Компоненты ASP.NET Core Razor на стороне сервера](xref:host-and-deploy/razor-components-blazor/razor-components)
