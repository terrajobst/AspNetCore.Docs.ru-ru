---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: "Изменение первичного ключа для пользователей в ASP.NET Identity | Документы Microsoft"
author: tfitzmac
description: "В Visual Studio 2013 веб-приложения по умолчанию использует строковое значение для ключа для учетных записей пользователей. ASP.NET Identity позволяет изменить тип..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>Изменение первичного ключа для пользователей в ASP.NET Identity
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> В Visual Studio 2013 веб-приложения по умолчанию использует строковое значение для ключа для учетных записей пользователей. ASP.NET Identity позволяет изменить тип ключа для соответствия требованиям данных. Например можно изменить тип ключа из строки в целое число.
> 
> В этом разделе показано, как со веб-приложения по умолчанию и изменить на целочисленный ключ учетной записи пользователя. Те же изменения можно использовать для реализации любого типа ключа в своем проекте. Показано, как внести эти изменения в веб-приложения по умолчанию, но можно применить аналогичные изменения настраиваемого приложения. Он показывает изменения, необходимые при работе с MVC или веб-форм.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - Visual Studio 2013 с обновлением 2 (или более поздней версии)
> - ASP.NET Identity 2.1 или более поздней версии


Для выполнения шагов в этом учебнике, необходимо иметь Visual Studio 2013 с обновлением 2 (или более поздней версии) и веб-приложения, созданные на основе шаблона веб-приложения ASP.NET. Шаблон изменения в обновлении 3. В этом разделе показано, как изменить шаблон в обновлении 2 и обновлением 3.

В этом разделе содержатся следующие подразделы.

- [Измените тип ключа в пользовательском классе удостоверений](#userclass)
- [Добавить пользовательские классы удостоверений, использующие тип ключа](#customclass)
- [Изменение контекста класса и диспетчера пользователей для использования данного типа ключа](#context)
- [Изменить конфигурацию при запуске для использования данного типа ключа](#startup)
- [Измените AccountController для передачи ключа типа MVC с обновлением 2](#mvcupdate2)
- [Для MVC с обновлением 3 измените AccountController и ManageController для передачи ключа типа](#mvcupdate3)
- [Для веб-формы с обновлением 2 измените учетную запись страницы для передачи ключа типа](#webformsupdate2)
- [Для веб-формы с обновлением 3 измените учетную запись страницы для передачи ключа типа](#webformsupdate3)
- [Запуск приложения](#run)
- [Другие ресурсы](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Измените тип ключа в пользовательском классе удостоверений

В проекте, созданные на основе шаблона веб-приложения ASP.NET укажите, что класс ApplicationUser использует целое число для ключа для учетных записей пользователей. В IdentityModels.cs, измените класс ApplicationUser наследование от IdentityUser, который имеет тип **int** универсального параметра TKey. Можно также передать имена трех настраиваемого класса, который еще не реализован.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Тип ключа был изменен, но по умолчанию остальной части приложения по-прежнему принимает ключ является строка. Необходимо явно указать тип ключа в программный код, предполагающий строка.

В **ApplicationUser** класса, измените **GenerateUserIdentityAsync** метод, чтобы включить int, как показано в приведенном ниже выделенного кода. Это изменение не требуется для проектов веб-форм с помощью шаблона обновления 3.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Добавить пользовательские классы удостоверений, использующие тип ключа

И другие классы удостоверений, например IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, по-прежнему настраиваются для использования ключа строки. Создание новых версий этих классов, укажите целочисленное значение для ключа. Необходимо предоставить объема кода реализации в этих классах, в основном так же установке int как ключ.

Добавьте следующие классы в файл IdentityModels.cs.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Изменение контекста класса и диспетчера пользователей для использования данного типа ключа

В IdentityModels.cs, измените определение **ApplicationDbContext** класс, чтобы использовать новый настроить классы и **int** для ключа, как показано в выделенный код.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

Параметр ThrowIfV1Schema больше не является допустимым в конструкторе. Измените конструктор, чтобы он не передает значение ThrowIfV1Schema.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

Откройте IdentityConfig.cs и измените **ApplicationUserManger** класс для использования нового пользователя хранилища класса для сохранения данных и **int** для ключа.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

В шаблоне с обновлением 3 необходимо изменить класс ApplicationSignInManager.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Изменить конфигурацию при запуске для использования данного типа ключа

В Startup.Auth.cs замените код OnValidateIdentity, как показано ниже. Обратите внимание, что определение getUserIdCallback анализирует строковое значение в целое число.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Если проект не распознает Универсальная реализация **GetUserId** метод, может потребоваться обновление до версии 2.1 пакета ASP.NET Identity NuGet

Много изменений, внесенные в классы инфраструктуры, используемые в ASP.NET Identity. Если компиляция проекта, вы заметите, что большое число ошибок. К счастью оставшихся ошибок всех похожи. Класс идентификатора ожидает целое число для ключа, но контроллера (или веб-формы) передается строковое значение. В каждом случае необходимо преобразовать в строку и целое число путем вызова **GetUserId&lt;int&gt;**. Можно работать через список ошибок из компиляции или выполните ниже изменения.

Оставшиеся изменения зависят от типа проекта, вы создаете и какие обновления установки в Visual Studio. Можно перейти непосредственно к соответствующий раздел по ссылке

- [Измените AccountController для передачи ключа типа MVC с обновлением 2](#mvcupdate2)
- [Для MVC с обновлением 3 измените AccountController и ManageController для передачи ключа типа](#mvcupdate3)
- [Для веб-формы с обновлением 2 измените учетную запись страницы для передачи ключа типа](#webformsupdate2)
- [Для веб-формы с обновлением 3 измените учетную запись страницы для передачи ключа типа](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Измените AccountController для передачи ключа типа MVC с обновлением 2

Откройте файл AccountController.cs. Необходимо изменить следующие методы.

**ConfirmEmail** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**Отмените регистрацию** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Manage(ManageUserViewModel)** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

**LinkLoginCallback** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

**RemoveAccountList** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

**HasPassword** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

Теперь вы можете [запустите приложение](#run) и регистрации нового пользователя.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Для MVC с обновлением 3 измените AccountController и ManageController для передачи ключа типа

Откройте файл AccountController.cs. Необходимо изменить следующий метод.

**ConfirmEmail** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

**SendCode** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

Откройте файл ManageController.cs. Необходимо изменить следующие методы.

**Индекс** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

**RemoveLogin** методы

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

**AddPhoneNumber** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

**EnableTwoFactorAuthentication** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

**DisableTwoFactorAuthentication** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

**VerifyPhoneNumber** методы

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

**RemovePhoneNumber** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

**ChangePassword** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

**SetPassword** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

**ManageLogins** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

**LinkLoginCallback** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

**HasPassword** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

**HasPhoneNumber** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

Теперь вы можете [запустите приложение](#run) и регистрации нового пользователя.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Для веб-формы с обновлением 2 измените учетную запись страницы для передачи ключа типа

Для веб-формы с обновлением 2 необходимо изменить на следующих страницах.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

Теперь вы можете [запустите приложение](#run) и регистрации нового пользователя.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Для веб-формы с обновлением 3 измените учетную запись страницы для передачи ключа типа

Для веб-формы с обновлением 3 необходимо изменить на следующих страницах.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>Запуск приложения

Вы завершили все необходимые изменения в шаблон веб-приложения по умолчанию. Запустите приложение и регистрации нового пользователя. После регистрации пользователя, можно заметить, что таблица AspNetUsers содержит идентификатор столбца, который является целым числом.

![новый первичный ключ](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Если созданный ранее ASP.NET Identity таблицы с другой первичный ключ, необходимо внести некоторые дополнительные изменения. Если это возможно удалите существующую базу данных. База данных будет восстановлено с правильный подход, при запуске веб-приложения и добавить нового пользователя. Если удаление не поддерживается, выполнения миграции code first для изменения таблиц. Тем не менее новый первичный ключ целое число будет не установлен как свойство ИДЕНТИФИКАТОРОВ SQL в базе данных. Столбец идентификаторов необходимо вручную задать как УДОСТОВЕРЕНИЕ.

<a id="other"></a>
## <a name="other-resources"></a>Другие источники

- [Обзор поставщиков пользовательского хранилища для ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Миграция существующего веб-сайта из SQL членства в ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Перенос данных универсальной поставщик членства и профили пользователей для ASP.NET Identity](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Образец приложения](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) с измененной первичного ключа
