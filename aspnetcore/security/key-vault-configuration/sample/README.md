---
ms.openlocfilehash: 209b5c41e17897693962954b1e795bdbb41f9384
ms.sourcegitcommit: 1a7000630e55da90da19b284e1b2f2f13a393d74
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2019
ms.locfileid: "59012738"
---
# <a name="key-vault-configuration-provider-sample-app"></a>Хранилище ключей конфигурации поставщика примера приложения

В этом примере описывается использование поставщика конфигурации хранилища ключей Azure.

Пример выполняется в одном из двух режимов определяется `#define` инструкция в верхней части *Program.cs* файл. Инструкции см. в разделе [директивы препроцессора в образце кода](https://docs.microsoft.com/aspnet/core#preprocessor-directives-in-sample-code):

* `Certificate` &ndash; Демонстрирует использование идентификатора клиента хранилища ключей Azure и X.509 сертификата доступа секреты, хранящиеся в хранилище ключей Azure. Эта версия образца может выполняться из любого места, развернутых в службе приложений Azure или любом узле, может обслуживать приложения ASP.NET Core.
* `Managed` &ndash; Демонстрирует использование Azure [управляемое удостоверение службы](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) для проверки подлинности приложения в хранилище ключей Azure с аутентификацией Azure AD без учетных данных в коде или конфигурации приложения. Идентификатор клиента Azure AD и секрет не являются обязательными для приложения для проверки подлинности с хранилищем ключей Azure. В этом примере должны быть развернуты в службе приложений Azure для изучения scearnio управляемое удостоверение.

Дополнительные сведения см. в разделе [поставщик конфигурации Azure Key Vault](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration).
