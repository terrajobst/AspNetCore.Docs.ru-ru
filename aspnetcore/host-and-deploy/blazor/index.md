---
title: Размещение и развертывание ASP.NET Core Blazor
author: guardrex
description: Узнайте, как размещать и развертывать приложения Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: d18abbf33c71dca5130bfc6b503b46c1d5bce537
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68913924"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a>Размещение и развертывание ASP.NET Core Blazor

Авторы: [Люк Лэтем](https://github.com/guardrex), [Рэйнер Стропек](https://www.timecockpit.com) и [Дэниэл Рот](https://github.com/danroth27)

## <a name="publish-the-app"></a>Публикация приложения

Приложения публикуются для развертывания в конфигурации выпуска.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. На панели навигации выберите **Сборка** > **Опубликовать {приложение}** .
1. Выберите *целевой объект публикации*. Чтобы опубликовать объект в локальной среде, выберите **папку**.
1. Оставьте расположение по умолчанию в поле **выбора папки** или укажите другое расположение. Нажмите кнопку **Опубликовать**.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Используйте команду [dotnet publish](/dotnet/core/tools/dotnet-publish), чтобы опубликовать приложение с конфигурацией выпуска:

```console
dotnet publish -c Release
```

---

Публикация приложения активирует [восстановление](/dotnet/core/tools/dotnet-restore) зависимостей проекта и выполняет [сборку](/dotnet/core/tools/dotnet-build) проекта, прежде чем создавать ресурсы для развертывания. В ходе процесса построения удаляются неиспользуемые методы и сборки, чтобы уменьшить размер скачиваемого приложения и время загрузки.

Приложения Blazor на стороне клиента публикуются в папке */bin/Release/{целевая_платформа}publish/{имя_сборки}/dist*. Приложения Blazor на стороне сервера публикуются в папке */bin/Release/{целевая_платформа}/publish*.

Ресурсы из папки развертываются на веб-сервере. Развертывание может проводиться вручную или автоматизированно в зависимости от используемых средств разработки.

## <a name="deployment"></a>Развертывание

Инструкции по развертыванию см. в следующих разделах:

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a>Бессерверное размещение Blazor с использованием службы хранилища Azure

Приложения Blazor на стороне клиента можно обслуживать в [службе хранилища Azure](https://azure.microsoft.com/services/storage/) как статическое содержимое непосредственно из контейнера хранилища.

См. дополнительные сведения о том, [как разместить и развернуть приложение ASP.NET Core Blazor на стороне клиента (изолированное развертывание) с помощью службы хранилища Azure](xref:host-and-deploy/blazor/client-side#azure-storage).
