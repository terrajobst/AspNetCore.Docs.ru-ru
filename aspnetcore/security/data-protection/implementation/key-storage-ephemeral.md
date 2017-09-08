---
title: "Поставщики защиты временных данных"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: af6ea1d0-0d9d-41df-a870-5dda24978e2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 6b564f082eefad993ac938407e0f3b657d83fb52
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="ephemeral-data-protection-providers"></a>Поставщики защиты временных данных

<a name=data-protection-implementation-key-storage-ephemeral></a>

Существуют сценарии, если приложению необходима throwaway IDataProtectionProvider. Например, разработчик может просто Поэкспериментировав в одноразовые консольное приложение или само приложение является временным (скрипт создается или проект модульного теста). Для поддержки следующих сценариев пакета Microsoft.AspNetCore.DataProtection включает тип EphemeralDataProtectionProvider. Этот тип обеспечивает базовую реализацию IDataProtectionProvider которого ключа репозитория удерживается исключительно в памяти и не записывается в резервное хранилище.

Каждый экземпляр EphemeralDataProtectionProvider использует свой собственный уникальный главного ключа. Таким образом, если с корнем EphemeralDataProtectionProvider IDataProtector создает защищенные полезных данных, полезных данных можно только быть незащищенными, эквивалентный IDataProtector (соответствует заданному [цели](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) цепочки) корневому каталогу, в то же Экземпляр EphemeralDataProtectionProvider.

В следующем образце показано создание экземпляра EphemeralDataProtectionProvider и использовать его для установки и снятия защиты данных.

```none
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
