---
title: Получение библиотеки на стороне клиента в ASP.NET Core с LibMan
author: scottaddie
description: Узнайте, как установить ресурсы библиотеки на стороне клиента в проекте ASP.NET Core с помощью диспетчера библиотек (LibMan).
ms.author: scaddie
ms.custom: mvc
ms.date: 08/14/2018
uid: client-side/libman/index
ms.openlocfilehash: a6ff0cc3342cfac74739387aa17046ed5050232f
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312363"
---
# <a name="client-side-library-acquisition-in-aspnet-core-with-libman"></a>Получение библиотеки на стороне клиента в ASP.NET Core с LibMan

Автор: [Скотт Адди](https://twitter.com/Scott_Addie) (Scott Addie)

Диспетчер библиотек (LibMan) — это облегченное средство получения библиотек на стороне клиента. LibMan загружает популярные библиотеки и платформы из файловой системы или из [сети доставки содержимого (CDN)](https://wikipedia.org/wiki/Content_delivery_network). Поддерживаются сети доставки содержимого [CDNJS](https://cdnjs.com/) и [unpkg](https://unpkg.com/#/). Выбранные файлы библиотеки извлекаются и размещаются в соответствующем месте в проекте ASP.NET Core.

## <a name="libman-use-cases"></a>Варианты использования LibMan

LibMan предоставляет следующие преимущества:

* Загружаются только необходимые файлы библиотеки.
* Дополнительные средства, такие как [Node.js](https://nodejs.org), [npm](https://www.npmjs.com) и [WebPack](https://webpack.js.org), не обязательны для получения подмножества файлов в библиотеке.
* Файлы можно разместить в определенном расположении, не прибегая к задачам сборки или копированию файлов вручную.

Дополнительные сведения о преимуществах LibMan смотрите в видео [Разработка современного веб-интерфейса в Visual Studio 2017: сегмент LibMan](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s).

LibMan не является системой управления пакетами. Если вы уже используете диспетчер пакетов, например npm или [yarn](https://yarnpkg.com), используйте его и дальше. LibMan не предназначен для замены этих средств.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:client-side/libman/libman-vs>
* <xref:client-side/libman/libman-cli>
* [Репозиторий LibMan на GitHub](https://github.com/aspnet/LibraryManager)
