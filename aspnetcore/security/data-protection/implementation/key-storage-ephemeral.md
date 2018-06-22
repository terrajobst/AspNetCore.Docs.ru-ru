---
title: Защита от поставщиков временных данных в ASP.NET Core
author: rick-anderson
description: Узнайте сведения о реализации поставщиков ASP.NET Core временных данных защиты.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: e4b0014ab3bdbf90b91383e8a33102f94faa8153
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279471"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a>Защита от поставщиков временных данных в ASP.NET Core

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Существуют сценарии, если приложению необходима throwaway `IDataProtectionProvider`. Например, разработчик может просто Поэкспериментировав в одноразовые консольное приложение или само приложение является временным (скрипт создается или проект модульного теста). Для поддержки следующих сценариев [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) пакет включает тип `EphemeralDataProtectionProvider`. Этот тип предоставляет базовую реализацию `IDataProtectionProvider` которого ключа репозитория удерживается исключительно в памяти и не записывается в резервное хранилище.

Каждый экземпляр `EphemeralDataProtectionProvider` использует свой собственный уникальный главного ключа. Таким образом Если `IDataProtector` с корнем `EphemeralDataProtectionProvider` приводит к возникновению ошибки защищенных полезных данных, без защиты полезных данных можно только с эквивалентный `IDataProtector` (соответствует заданному [цели](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) цепочки) корневыми в то же самое `EphemeralDataProtectionProvider` экземпляр.

В следующем примере показано создание экземпляра `EphemeralDataProtectionProvider` и использовать его для установки и снятия защиты данных.

```csharp
using System;
using Microsoft.AspNetCore.DataProtection;

public class Program
{
    public static void Main(string[] args)
    {
        const string purpose = "Ephemeral.App.v1";

        // create an ephemeral provider and demonstrate that it can round-trip a payload
        var provider = new EphemeralDataProtectionProvider();
        var protector = provider.CreateProtector(purpose);
        Console.Write("Enter input: ");
        string input = Console.ReadLine();

        // protect the payload
        string protectedPayload = protector.Protect(input);
        Console.WriteLine($"Protect returned: {protectedPayload}");

        // unprotect the payload
        string unprotectedPayload = protector.Unprotect(protectedPayload);
        Console.WriteLine($"Unprotect returned: {unprotectedPayload}");

        // if I create a new ephemeral provider, it won't be able to unprotect existing
        // payloads, even if I specify the same purpose
        provider = new EphemeralDataProtectionProvider();
        protector = provider.CreateProtector(purpose);
        unprotectedPayload = protector.Unprotect(protectedPayload); // THROWS
    }
}

/*
* SAMPLE OUTPUT
*
* Enter input: Hello!
* Protect returned: CfDJ8AAAAAAAAAAAAAAAAAAAAA...uGoxWLjGKtm1SkNACQ
* Unprotect returned: Hello!
* << throws CryptographicException >>
*/
```
