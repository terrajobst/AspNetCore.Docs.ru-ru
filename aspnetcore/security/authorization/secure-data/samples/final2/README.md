---
ms.openlocfilehash: 6246247788eefc8f094ec1a7f156cb36715edb53
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210110"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a>Как построение и запуск образца данных безопасности пользователей

* Задайте пароль с помощью диспетчера секретов:

  `dotnet user-secrets set SeedUserPW <pw>`

* Обновление базы данных:

  `dotnet ef database update`

* Включение протокола HTTPS в проекте
