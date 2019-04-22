---
title: Размещение и развертывание Blazor
author: guardrex
description: Узнайте, как размещать и развертывать приложения Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: a7739e2b240d7fd6c85e68105892c802228ebeb5
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614590"
---
# <a name="host-and-deploy-blazor"></a>Размещение и развертывание Blazor

Авторы: [Люк Лэтем](https://github.com/guardrex), [Рэйнер Стропек](https://www.timecockpit.com) и [Дэниэл Рот](https://github.com/danroth27)

## <a name="publish-the-app"></a>Публикация приложения

Приложения публикуются для развертывания в конфигурации выпуска при помощи команды [dotnet publish](/dotnet/core/tools/dotnet-publish). Интегрированная среда разработки (IDE) может обрабатывать выполнение команды `dotnet publish` автоматически с помощью своих встроенных возможностей публикации, поэтому может быть необязательно вручную выполнять команду из командной строки в зависимости от используемых средств разработки.

```console
dotnet publish -c Release
```

`dotnet publish` активирует [восстановление](/dotnet/core/tools/dotnet-restore) зависимостей проекта и проводит [сборку](/dotnet/core/tools/dotnet-build) проекта, прежде чем создавать ресурсы для развертывания. В ходе процесса построения удаляются неиспользуемые методы и сборки, чтобы уменьшить размер скачиваемого приложения и время загрузки. Развертывание создается в папке */bin/Release/{целевая_платформа}/publish*.

Ресурсы в папке *publish* развертываются на веб-сервере. Развертывание может проводиться вручную или автоматизированно в зависимости от используемых средств разработки.

## <a name="deployment"></a>Развертывание

Инструкции по развертыванию см. в следующих разделах:

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
