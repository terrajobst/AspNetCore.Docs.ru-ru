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
ms.openlocfilehash: ee8dccac3ba990b110758042192779426b01fc53
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2017
---
# <a name="ephemeral-data-protection-providers"></a><span data-ttu-id="169bd-103">Поставщики защиты временных данных</span><span class="sxs-lookup"><span data-stu-id="169bd-103">Ephemeral data protection providers</span></span>

<a name="data-protection-implementation-key-storage-ephemeral"></a>

<span data-ttu-id="169bd-104">Существуют сценарии, если приложению необходима throwaway IDataProtectionProvider.</span><span class="sxs-lookup"><span data-stu-id="169bd-104">There are scenarios where an application needs a throwaway IDataProtectionProvider.</span></span> <span data-ttu-id="169bd-105">Например, разработчик может просто Поэкспериментировав в одноразовые консольное приложение или само приложение является временным (скрипт создается или проект модульного теста).</span><span class="sxs-lookup"><span data-stu-id="169bd-105">For example, the developer might just be experimenting in a one-off console application, or the application itself is transient (it's scripted or a unit test project).</span></span> <span data-ttu-id="169bd-106">Для поддержки следующих сценариев пакета Microsoft.AspNetCore.DataProtection включает тип EphemeralDataProtectionProvider.</span><span class="sxs-lookup"><span data-stu-id="169bd-106">To support these scenarios the package Microsoft.AspNetCore.DataProtection includes a type EphemeralDataProtectionProvider.</span></span> <span data-ttu-id="169bd-107">Этот тип обеспечивает базовую реализацию IDataProtectionProvider которого ключа репозитория удерживается исключительно в памяти и не записывается в резервное хранилище.</span><span class="sxs-lookup"><span data-stu-id="169bd-107">This type provides a basic implementation of IDataProtectionProvider whose key repository is held solely in-memory and isn't written out to any backing store.</span></span>

<span data-ttu-id="169bd-108">Каждый экземпляр EphemeralDataProtectionProvider использует свой собственный уникальный главного ключа.</span><span class="sxs-lookup"><span data-stu-id="169bd-108">Each instance of EphemeralDataProtectionProvider uses its own unique master key.</span></span> <span data-ttu-id="169bd-109">Таким образом, если с корнем EphemeralDataProtectionProvider IDataProtector создает защищенные полезных данных, полезных данных можно только быть незащищенными, эквивалентный IDataProtector (соответствует заданному [цели](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) цепочки) корневому каталогу, в то же Экземпляр EphemeralDataProtectionProvider.</span><span class="sxs-lookup"><span data-stu-id="169bd-109">Therefore, if an IDataProtector rooted at an EphemeralDataProtectionProvider generates a protected payload, that payload can only be unprotected by an equivalent IDataProtector (given the same [purpose](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) chain) rooted at the same EphemeralDataProtectionProvider instance.</span></span>

<span data-ttu-id="169bd-110">В следующем образце показано создание экземпляра EphemeralDataProtectionProvider и использовать его для установки и снятия защиты данных.</span><span class="sxs-lookup"><span data-stu-id="169bd-110">The following sample demonstrates instantiating an EphemeralDataProtectionProvider and using it to protect and unprotect data.</span></span>

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
