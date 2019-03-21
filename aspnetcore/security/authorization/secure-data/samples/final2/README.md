---
ms.openlocfilehash: 6246247788eefc8f094ec1a7f156cb36715edb53
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210110"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="9f2e3-101">Как построение и запуск образца данных безопасности пользователей</span><span class="sxs-lookup"><span data-stu-id="9f2e3-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="9f2e3-102">Задайте пароль с помощью диспетчера секретов:</span><span class="sxs-lookup"><span data-stu-id="9f2e3-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="9f2e3-103">Обновление базы данных:</span><span class="sxs-lookup"><span data-stu-id="9f2e3-103">Update the database:</span></span>

  `dotnet ef database update`

* <span data-ttu-id="9f2e3-104">Включение протокола HTTPS в проекте</span><span class="sxs-lookup"><span data-stu-id="9f2e3-104">Enable HTTPS in the project</span></span>
