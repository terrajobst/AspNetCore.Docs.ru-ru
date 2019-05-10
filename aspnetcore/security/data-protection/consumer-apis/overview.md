---
title: Обзор потребительских API для ASP.NET Core
author: rick-anderson
description: Получите краткий обзор различных потребителя интерфейсы API, доступные в библиотеке защиты данных ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: b0d11d097ee2d448b6781f6fa84445f6400fbc76
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087532"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>Обзор потребительских API для ASP.NET Core

`IDataProtectionProvider` И `IDataProtector` интерфейсы являются базовые интерфейсы, через которые потребители используют система защиты данных. Они находятся в [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) пакета.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

Интерфейс поставщика представляет корень система защиты данных. Он не может использоваться непосредственно для защиты или снять защиту данных. Вместо этого, потребитель должен получить ссылку на `IDataProtector` путем вызова `IDataProtectionProvider.CreateProtector(purpose)`, где цели — это строка с описанием варианта использования предполагаемого получателя. См. в разделе [строки назначений](xref:security/data-protection/consumer-apis/purpose-strings) для гораздо больше информации о цели этого параметра и как выбрать соответствующее значение.

## <a name="idataprotector"></a>IDataProtector

Интерфейс предохранителя возвращается путем вызова `CreateProtector`и этот интерфейс, который пользователи могут использовать для выполнения установки и снятия защиты операций.

Для защиты данных, передачи данных `Protect` метод. Базовый интерфейс определяет метод, который преобразует byte [] -> byte [], но есть также перегрузки (предоставляется в виде метода расширения), которая преобразует строку "->" (строка. Идентично безопасности за счет двух методов; разработчику следует выбрать, какой перегрузка является наиболее удобным для их использования. Вне зависимости от перегрузки выбрано, значение, возвращенное защитить метод защищенные (зашифрованные и подтверждены незаконного изменения) и приложения можно отправить ненадежный клиент.

Чтобы снять защиту ранее защищено часть данных, передайте защищенных данных в `Unprotect` метод. (Существуют byte []-зависимости и строковых перегрузки для удобства разработчиков.) Если защищенный полезных данных было создано предыдущими вызовами `Protect` на этом же `IDataProtector`, `Unprotect` метод вернет исходный незащищенные полезных данных. Если защищенных полезных данных было изменено или был создан с помощью другой `IDataProtector`, `Unprotect` метод вызывает исключение CryptographicException.

Концепция таким же или разных `IDataProtector` ties обратно в понятие назначения. Если два `IDataProtector` экземпляры, созданные на основе таким же корнем `IDataProtectionProvider` , но с помощью строки различных назначений в вызове `IDataProtectionProvider.CreateProtector`, то они считаются [различных предохранители](xref:security/data-protection/consumer-apis/purpose-strings), и одно не может снять защиту полезные данные, созданные с использованием других.

## <a name="consuming-these-interfaces"></a>Использование этих интерфейсов

Поддерживающие внедрение Зависимостей компонента, предполагаемое использование является, что компонент занять `IDataProtectionProvider` параметр в конструкторе автоматически, и что в системе внедрения Зависимостей этой службы при создании экземпляра компонента.

> [!NOTE]
> Возможно, поддерживающие внедрение Зависимостей, нельзя использовать с механизмом, описанным здесь некоторые приложения (например, консольных приложений или приложений ASP.NET 4.x). См. Эти сценарии [зависимые сценарии не DI](xref:security/data-protection/configuration/non-di-scenarios) Дополнительные сведения о том, как экземпляр `IDataProtection` поставщика, минуя внедрения Зависимостей.

В следующем образце показано три понятия:

1. [Добавить в систему защиты данных](xref:security/data-protection/configuration/overview) в контейнер службы

2. С помощью внедрения Зависимостей для получения экземпляра `IDataProtectionProvider`, и

3. Создание `IDataProtector` из `IDataProtectionProvider` и использовать его для применения и снятия защиты данных.

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Пакет Microsoft.AspNetCore.DataProtection.Abstractions содержит метод расширения `IServiceProvider.GetDataProtector` для удобства разработчиков. Он включает в себя как единственная операция обоих извлечение `IDataProtectionProvider` из поставщика служб и вызова `IDataProtectionProvider.CreateProtector`. В следующем образце показано их использование.

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Экземпляры `IDataProtectionProvider` и `IDataProtector` потокобезопасны для нескольких клиентов. Она предназначена, после того как компонент возвращает ссылку на `IDataProtector` через вызов `CreateProtector`, он будет использовать эту ссылку для нескольких вызовов `Protect` и `Unprotect`. Вызов `Unprotect` вызовет CryptographicException в случае защищенных полезных данных не может проверить или расшифровать. Некоторые компоненты можно пропускать ошибки во время снятия операций; компонент, который считывает файлы cookie проверки подлинности может обрабатывать эту ошибку и обрабатывать запрос, как если бы она имела cookie не вообще, а не отклонят запрос прямо сейчас. Компоненты, которые хотите такого поведения в частности следует перехватывать CryptographicException вместо подавление всех исключений.
