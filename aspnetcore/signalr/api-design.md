---
title: Рекомендации по проектированию SignalR API
author: anurse
description: Дополнительные сведения о разработке SignalR API-интерфейсы для обеспечения совместимости между версиями приложения.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/api-design
ms.openlocfilehash: 3f17bf055b793e8fc91fbcc15f668928ca261f77
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897811"
---
# <a name="signalr-api-design-considerations"></a>Рекомендации по проектированию SignalR API

По [Andrew Stanton медсестра](https://twitter.com/anurse)

Эта статья содержит рекомендации по созданию API на основе SignalR.

## <a name="use-custom-object-parameters-to-ensure-backwards-compatibility"></a>Использование параметров пользовательского объекта для обеспечения обратной совместимости

Добавление параметров в метод концентратора SignalR (на клиенте или сервере) является *критическое изменение*. Это означает, что старые клиенты и серверы ошибки возникают при попытке вызвать метод без параметров, соответствующее числу. Однако добавление свойств к параметру пользовательского объекта является **не** является критическим изменением. Это можно использовать для разработки совместимые интерфейсы API, которые устойчивы к изменениям на стороне клиента или сервера.

Например рассмотрим API серверной части следующим образом:

[!code-csharp[ParameterBasedOldVersion](api-design/sample/Samples.cs?name=ParameterBasedOldVersion)]

Клиент JavaScript вызывает этот метод с помощью `invoke` следующим образом:

[!code-typescript[CallWithOneParameter](api-design/sample/Samples.ts?name=CallWithOneParameter)]

Если вы позже добавить второй параметр метода сервера, старые клиенты не предоставляют значение этого параметра. Пример:

[!code-csharp[ParameterBasedNewVersion](api-design/sample/Samples.cs?name=ParameterBasedNewVersion)]

Когда старый клиент пытается вызвать этот метод, он получит такую ошибку:

```
Microsoft.AspNetCore.SignalR.HubException: Failed to invoke 'GetTotalLength' due to an error on the server.
```

На сервере вы увидите примерно следующее сообщение журнала:

```
System.IO.InvalidDataException: Invocation provides 1 argument(s) but target expects 2.
```

Старый клиент отправил только один параметр, но более новые API сервера требуется два параметра. Использование пользовательских объектов в качестве параметров обеспечивает большую гибкость. Давайте перепроектировать исходного API-интерфейса для пользовательского объекта:

[!code-csharp[ObjectBasedOldVersion](api-design/sample/Samples.cs?name=ObjectBasedOldVersion)]

Теперь клиент использует объект для вызова метода:

[!code-typescript[CallWithObject](api-design/sample/Samples.ts?name=CallWithObject)]

Вместо добавления параметра, добавьте свойство, чтобы `TotalLengthRequest` объекта:

[!code-csharp[ObjectBasedNewVersion](api-design/sample/Samples.cs?name=ObjectBasedNewVersion&highlight=4,9-13)]

Когда старый клиент отправляет один параметр, лишние `Param2` остается свойство `null`. Вы можете обнаружить сообщение, отправленное более старого клиента путем проверки `Param2` для `null` и применить значение по умолчанию. Новый клиент может отправлять оба параметра.

[!code-typescript[CallWithObjectNew](api-design/sample/Samples.ts?name=CallWithObjectNew)]

Тот же метод работает для методов, определенные на клиенте. Пользовательский объект можно отправлять на стороне сервера:

[!code-csharp[ClientSideObjectBasedOld](api-design/sample/Samples.cs?name=ClientSideObjectBasedOld)]

На стороне клиента, доступ к `Message` свойство, а не с помощью параметра:

[!code-typescript[OnWithObjectOld](api-design/sample/Samples.ts?name=OnWithObjectOld)]

Если позднее вы решите добавить отправителя сообщения в полезных данных, добавьте свойство к объекту:

[!code-csharp[ClientSideObjectBasedNew](api-design/sample/Samples.cs?name=ClientSideObjectBasedNew&highlight=5)]

Старые клиенты не будут ожидать `Sender` значение, поэтому они не будет обрабатывать его. Новый клиент сможет принять его, обновив для чтения свойство:

[!code-typescript[OnWithObjectNew](api-design/sample/Samples.ts?name=OnWithObjectNew&highlight=2-5)]

В этом случае новый клиент также устойчива к старый сервер, который не предоставляет `Sender` значение. Поскольку старый сервер не предоставляют `Sender` значение, клиент проверяет, существует ли он перед доступом к нему.
