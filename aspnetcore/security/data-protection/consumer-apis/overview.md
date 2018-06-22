---
title: Общие сведения об API-интерфейсы потребителя для ASP.NET Core
author: rick-anderson
description: Получите краткий обзор различных интерфейсов API, доступные в библиотеке защиты данных ASP.NET Core потребителя.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: b0d11d097ee2d448b6781f6fa84445f6400fbc76
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279120"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>Общие сведения об API-интерфейсы потребителя для ASP.NET Core

`IDataProtectionProvider` И `IDataProtector` интерфейсы являются базовые интерфейсы, через которые потребители используют система защиты данных. Они находятся в [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) пакета.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

Интерфейс поставщика представляет в корневом каталоге системы защиты данных. Он не может использоваться непосредственно для защиты или снять защиту данных. Вместо этого пользователь должен получить ссылку на `IDataProtector` путем вызова `IDataProtectionProvider.CreateProtector(purpose)`, где цель — строка, описывающая вариант использования предполагаемого получателя. В разделе [строки цели](xref:security/data-protection/consumer-apis/purpose-strings) гораздо Дополнительные сведения о назначение этого параметра и выборе подходящего значения.

## <a name="idataprotector"></a>IDataProtector

Интерфейс предохранитель возвращается путем вызова `CreateProtector`и его этот интерфейс, который потребители могут использовать для выполнения установки и снятия защиты операций.

Чтобы защитить часть данных, передачи данных `Protect` метод. Базовый интерфейс определяет метод, который преобразует byte [] -> byte [], но есть также перегрузки (предоставляется в виде метода расширения), которая преобразует строку "->" строки. Безопасность, предлагаемую два метода является одинаковым; разработчику необходимо выбрать любой перегрузка является наиболее удобным для их варианта использования. Независимо от выбора перегрузки значения, возвращенного защитить метод теперь защищены (enciphered и подтверждены вскрытия) и приложения можно отправить ненадежный клиент.

Чтобы снять защиту с защищенными фрагмента данных, передайте защищенных данных в `Unprotect` метод. (Существует byte []-перегрузки на основе и на основе строк для удобства разработчиков.) Если защищенные полезных данных, созданных предыдущими вызовами метода `Protect` на этом же `IDataProtector`, `Unprotect` метод вернет исходный незащищенный полезных данных. Если защищенный полезные данные подделки или был создан с помощью другой `IDataProtector`, `Unprotect` метод вызывает исключение CryptographicException.

Понятие таким же или разных `IDataProtector` WITH ties обратно в понятие цели. Если два `IDataProtector` экземпляров формировались с таким же корнем `IDataProtectionProvider` , но через строк в разных целях в вызове `IDataProtectionProvider.CreateProtector`, то все они считаются [предохранители разных](xref:security/data-protection/consumer-apis/purpose-strings), и одно не сможет снять защиту полезные данные, созданные другим.

## <a name="consuming-these-interfaces"></a>Использование этих интерфейсов

Для компонента поддержкой DI предполагаемого использования является, компонент предпринять `IDataProtectionProvider` параметр в конструкторе и что DI система автоматически предоставляет этой службы, при создании экземпляра компонента.

> [!NOTE]
> Некоторые приложения (например, консольные приложения или приложения ASP.NET 4.x) может быть DI поддержкой, нельзя использовать механизм описанных здесь. Для этих сценариев, обратитесь к [не зависимые сценарии DI](xref:security/data-protection/configuration/non-di-scenarios) Дополнительные сведения о получении экземпляром `IDataProtection` поставщика, минуя DI.

В следующем примере показано три понятия:

1. [Добавить в систему защиты данных](xref:security/data-protection/configuration/overview) в контейнер службы

2. С помощью DI для получения экземпляра `IDataProtectionProvider`, и

3. Создание `IDataProtector` из `IDataProtectionProvider` и использовать его для установки и снятия защиты данных.

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Пакет Microsoft.AspNetCore.DataProtection.Abstractions содержит метод расширения `IServiceProvider.GetDataProtector` для удобства разработчиков. Он инкапсулирует единственную операцию обоих получение `IDataProtectionProvider` из поставщика служб и вызова `IDataProtectionProvider.CreateProtector`. Следующий пример демонстрирует его использование.

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Экземпляры `IDataProtectionProvider` и `IDataProtector` потокобезопасны для нескольких клиентов. Он предназначен, когда компонент получает ссылку на `IDataProtector` через вызов `CreateProtector`, он будет использовать эту ссылку для нескольких вызовов `Protect` и `Unprotect`. Вызов `Unprotect` CryptographicException вызывает исключение, если защищенный полезных данных не может быть проверена или расшифровать. Некоторые компоненты нужна возможность пропускать ошибки во время снятия защиты операций; компонент, который считывает файлы cookie проверки подлинности может обработать эту ошибку и обрабатывать запрос, как если бы объекты cookie не на всех, а не ошибкой запроса сразу. В частности, компонентами, которые хотите такого поведения следует перехватывать CryptographicException вместо подавление всех исключений.
