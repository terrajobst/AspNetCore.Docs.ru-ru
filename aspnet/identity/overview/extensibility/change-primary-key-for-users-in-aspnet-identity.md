---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Изменение первичного ключа для пользователей в ASP.NET Identity | Документация Майкрософт
author: tfitzmac
description: В Visual Studio 2013 веб-приложения по умолчанию использует строковое значение для ключа для учетных записей пользователей. ASP.NET Identity позволяет изменить тип...
ms.author: aspnetcontent
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 688a0dc09297a63bcf510aae9c25f303a5627df7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808046"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>Изменение первичного ключа для пользователей в ASP.NET Identity
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> В Visual Studio 2013 веб-приложения по умолчанию использует строковое значение для ключа для учетных записей пользователей. ASP.NET Identity позволяет изменить тип ключа в соответствии с требованиями данных. Например можно изменить тип ключа из строки в целое число.
> 
> В этом разделе показано, как начать с веб-приложения по умолчанию и измените ключ учетной записи пользователя в целое число. Можно использовать такие же изменения для реализации ключу любого типа в проекте. Показано, как внести эти изменения в веб-приложения по умолчанию, но вы можете применить аналогичные изменения специальных приложений. Он показывает изменения, необходимые при работе с веб-форм или MVC.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - Visual Studio 2013 с обновлением 2 (или более поздней версии)
> - ASP.NET Identity 2.1 или более поздней версии


Для выполнения шагов в этом руководстве, необходимо иметь Visual Studio 2013 с обновлением 2 (или более поздней версии) и веб-приложения, созданные на основе шаблона веб-приложения ASP.NET. Шаблон, изменения в обновлении 3. В этом разделе показано, как изменить шаблон с обновлением 2 и с обновлением 3.

В этом разделе содержатся следующие подразделы.

- [Измените тип ключа в класс пользовательского удостоверения](#userclass)
- [Добавьте настраиваемые классы удостоверений, использующие тип ключа](#customclass)
- [Изменение контекста класса и диспетчера пользователей для использования данного типа ключа](#context)
- [Изменение конфигурации запуска для использования данного типа ключа](#startup)
- [Для MVC с обновлением 2 измените AccountController передаваемый тип ключа](#mvcupdate2)
- [Для MVC с обновлением 3 измените AccountController и ManageController для передачи типа ключа](#mvcupdate3)
- [Для веб-форм с обновлением 2 измените учетную запись страницы для передачи типа ключа](#webformsupdate2)
- [Для веб-форм с обновлением 3 измените учетную запись страницы для передачи типа ключа](#webformsupdate3)
- [Запуск приложения](#run)
- [Другие ресурсы](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Измените тип ключа в класс пользовательского удостоверения

В проекте, созданные на основе шаблона веб-приложение ASP.NET укажите, что класс ApplicationUser использует целое число для ключа для учетных записей пользователей. В IdentityModels.cs, изменить наследование из IdentityUser с типом класса ApplicationUser **int** TKey универсального параметра. Можно также передать имена трех настраиваемый класс, который еще не реализован.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Тип ключа был изменен, но, по умолчанию остальной части приложения предполагает, что ключ является строкой. Необходимо явно указать тип ключа в программный код, предполагающий строка.

В **ApplicationUser** измените **GenerateUserIdentityAsync** метод для включения целое число, как показано в выделенный ниже код. Это изменение не требуется для проектов веб-форм с помощью шаблона с обновлением 3.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Добавьте настраиваемые классы удостоверений, использующие тип ключа

Другие классы удостоверений, например IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, по-прежнему настраиваются для использования ключа строки. Создание новых версий этих классов, указать целое число для ключа. Необходимо предоставить много кода реализации в этих классах, главным образом только при установке int как ключ.

Добавьте следующие классы в файле IdentityModels.cs.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Изменение контекста класса и диспетчера пользователей для использования данного типа ключа

В IdentityModels.cs, измените определение **ApplicationDbContext** класс для использования нового настроить классы и **int** для ключа, как показано в выделенном коде.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

Данный параметр ThrowIfV1Schema больше не является допустимым в конструкторе. Измените конструктор, поэтому он не передает значение ThrowIfV1Schema.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

Откройте IdentityConfig.cs и измените **ApplicationUserManger** класс для использования нового пользователя хранилища класс для сохранения данных и **int** для ключа.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

В шаблоне с обновлением 3 необходимо изменить класс ApplicationSignInManager.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Изменение конфигурации запуска для использования данного типа ключа

В Startup.Auth.cs замените код OnValidateIdentity, как показано ниже. Обратите внимание на то, что определение getUserIdCallback, анализирует строковое значение в целое число.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Если проект не распознает универсальную реализацию **GetUserId** метод, может потребоваться обновить пакет NuGet удостоверения ASP.NET до версии 2.1

Классы инфраструктуры, используемые для идентификации ASP.NET внесено много изменений. При попытке компиляции проекта, можно заметить большое число ошибок. К счастью оставшихся ошибок все похожи. Класс идентификации ожидает, что целое число для ключа, но контроллера (или веб-формы) передает значение строки. В каждом случае необходимо преобразовать в строку и целое число путем вызова **GetUserId&lt;int&gt;**. Можно работать через список ошибок из компиляции или ниже изменения.

Оставшиеся изменения зависят от типа проекта, вы создаете и какие обновления установлены в Visual Studio. Вы можете перейти непосредственно на соответствующий раздел по ссылке

- [Для MVC с обновлением 2 измените AccountController передаваемый тип ключа](#mvcupdate2)
- [Для MVC с обновлением 3 измените AccountController и ManageController для передачи типа ключа](#mvcupdate3)
- [Для веб-форм с обновлением 2 измените учетную запись страницы для передачи типа ключа](#webformsupdate2)
- [Для веб-форм с обновлением 3 измените учетную запись страницы для передачи типа ключа](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Для MVC с обновлением 2 измените AccountController передаваемый тип ключа

Откройте файл AccountController.cs. Вам нужно изменить следующие методы.

**ConfirmEmail** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**Отмените связь** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Manage(ManageUserViewModel)** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

**LinkLoginCallback** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

**RemoveAccountList** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

**HasPassword** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

Теперь вы можете [запустите приложение](#run) и зарегистрируйте нового пользователя.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Для MVC с обновлением 3 измените AccountController и ManageController для передачи типа ключа

Откройте файл AccountController.cs. Вам нужно изменить следующий метод.

**ConfirmEmail** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

**SendCode** метод

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

Откройте файл ManageController.cs. Вам нужно изменить следующие методы.

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

Теперь вы можете [запустите приложение](#run) и зарегистрируйте нового пользователя.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Для веб-форм с обновлением 2 измените учетную запись страницы для передачи типа ключа

Для веб-форм с обновлением 2 необходимо изменить на следующих страницах.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

Теперь вы можете [запустите приложение](#run) и зарегистрируйте нового пользователя.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Для веб-форм с обновлением 3 измените учетную запись страницы для передачи типа ключа

Для веб-форм с обновлением 3 необходимо изменить на следующих страницах.

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

Вы завершили все изменения, необходимые для шаблона веб-приложения по умолчанию. Запустите приложение и зарегистрируйте нового пользователя. После регистрации пользователя можно заметить, что таблица AspNetUsers содержит идентификатор столбца, должно быть целым числом.

![новый первичный ключ](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Если вы ранее создали ASP.NET Identity таблицы с другой первичный ключ, необходимо внести некоторые дополнительные изменения. Если это возможно просто удалите существующую базу данных. База данных будет восстановлено с правильный подход, при запуске веб-приложения и добавить нового пользователя. Если удаление не поддерживается, запуск code first migrations для изменения таблиц. Тем не менее на новый первичный ключ целое число не будет быть установлен как свойство ИДЕНТИФИКАТОРОВ SQL в базе данных. Столбец идентификаторов необходимо вручную задать в качестве УДОСТОВЕРЕНИЯ.

<a id="other"></a>
## <a name="other-resources"></a>Другие источники

- [Обзор пользовательских поставщиков хранилищ для ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Миграция существующего веб-сайта из членства SQL в ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Перенос данных универсального поставщика членства и профилей пользователей в ASP.NET Identity](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Пример приложения](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) измененные первичный ключ
