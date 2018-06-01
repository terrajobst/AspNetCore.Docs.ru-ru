---
title: Поддержка защиты стабилизации (GDPR) общие данные в ASP.NET Core
author: rick-anderson
description: Показано, как получить доступ к точек расширения GDPR в ASP.NET Core веб-приложения.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/gdpr
ms.openlocfilehash: 92a7000f4f8e4c2097065cb530fe106ef0e98545
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/31/2018
ms.locfileid: "34688631"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>Поддержка Европа общие данные защиты стабилизации (GDPR) в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

ASP.NET Core предоставляет API-интерфейсы и шаблоны с помощью требованиям [Европа общие данные защиты стабилизации (GDPR)](https://www.eugdpr.org/) требования:

* Шаблоны проектов включают точки расширения и кратких разметку, можно заменить конфиденциальности и политики использования файлов cookie.
* Функция согласия куки-файл позволяет запрашивать (и отслеживания) согласие от пользователей для сохранения личных сведений. Если пользователь не дал свое согласие на сбор данных и приложение устанавливается с [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) для `true`, необязательные файлы cookie не будут отправляться в браузере.
* Файлы cookie могут быть помечены как важные. Основные файлы cookie отправляются в браузере даже в том случае, если пользователь не согласился и трассировка отключена.
* [Файлы cookie TempData и сеанса](#tempdata) не работают при отключении отслеживания.
* [Управление удостоверения](#pd) странице приводятся ссылки для загрузки и удаление данных пользователей.

[Пример приложения](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) позволяет проверить большинство точек расширения GDPR и добавлен в список шаблонов ASP.NET Core 2.1 API-интерфейсы. В разделе [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) файла для тестирования инструкции.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>Поддержка GDPR ASP.NET Core в шаблоне созданного кода

Страниц Razor и MVC проектов, созданных с использованием шаблонов проекта поддерживает следующие GDPR:

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) и [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) задаются в `Startup`.
* *_CookieConsentPartial.cshtml* [частичного представления](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).
* *Pages/Privacy.cshtml* или *Home/Privacy.cshtml* представление предоставляет страницы для подробного описания политики конфиденциальности вашего веб-узла. *_CookieConsentPartial.cshtml* файл создает ссылку на страницу о конфиденциальности.
* Для приложений, созданных с помощью отдельных учетных записей пользователей, страница управления ссылки для загрузки и удалить [персональных данных пользователя](#pd).

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions и UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) инициализируются в `Startup` класса `ConfigureServices` метод:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) вызывается в `Startup` класса `Configure` метод:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>Частичное представление _CookieConsentPartial.cshtml

*_CookieConsentPartial.cshtml* частичного представления:

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

Это частично:

* Возвращает состояние отслеживания для пользователя. Если приложение настроено требуется согласие пользователя пользователь должен дать согласие, прежде чем можно будет отслеживать файлы cookie. Устранена ли требуется согласие, chrome согласия cookie поверх панели навигации, созданные в *Pages/Shared/_Layout.cshtml* файла.
* Предоставляет HTML `<p>` элемент для суммирования конфиденциальность и файлы cookie с помощью политики.
* Ссылка на *Pages/Privacy.cshtml* где подробно описывается политика конфиденциальности веб-узла.

## <a name="essential-cookies"></a>Основные файлы cookie

Если разрешения не были указаны, в браузере отправляются только файлы cookie, помеченные как важные. В следующем коде выполняется essential куки-файл:

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a>TempData поставщика сеанса состояния файлы cookie и не нужны

[Tempdata поставщика](xref:fundamentals/app-state#tempdata) куки-файл не является необходимым. При отключении отслеживания Tempdata поставщика не работает. Чтобы включить поставщик Tempdata при отключении отслеживания, отметки куки-файла TempData ключевым в `ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

[Состояние сеанса](xref:fundamentals/app-state) файлы cookie не нужны. При отключении отслеживания состояния сеанса не работает.

<a name="pd"></a>

## <a name="personal-data"></a>Персональные данные

Приложения ASP.NET Core, созданные с помощью отдельных учетных записей пользователей включить код для загрузки и удалять личные данные.

Выберите имя пользователя, а затем выберите **личных данных**:

![Страница «Управление» персональные данные](gdpr/_static/pd.png)

Примечания.

* Для создания `Account/Manage` кода см. в разделе [удостоверение формирования шаблонов](xref:security/authentication/scaffold-identity).
* Удалите и загрузить только влияние учетных данных по умолчанию. Создать пользовательские данные приложения необходимо расширить до удаления загрузку пользовательские данные. Проблема GitHub [как добавлять или удалять пользовательские данные для удостоверения](https://github.com/aspnet/Docs/issues/6226) отслеживает предложенный статьи о создании пользовательских или удаление или загрузке пользовательские данные. Если хотелось бы в разделе, приоритету, оставьте Палец вверх реакции на проблему.
* Сохранить токены для пользователя, которые хранятся в таблице базы данных удостоверений `AspNetUserTokens` удаляются при удалении пользователя через каскадных поведение удаления из-за [внешний ключ](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).

## <a name="encryption-at-rest"></a>Шифрование неактивных

Шифрование неактивных разрешить некоторые базы данных и механизмы хранения. Шифрование при хранении:

* Автоматически шифрует сохраненные данные.
* Шифрует без конфигурации, программирования и другие типы работ, для программного обеспечения, которое обращается к данным.
* Является простейшим и наиболее безопасным вариантом.
* Позволяет управлять ключей и шифрования базы данных.

Пример:

* Microsoft SQL и Azure SQL предоставляет [прозрачное шифрование данных](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).
* [SQL Azure шифрует базы данных по умолчанию](https://azure.microsoft.com/en-us/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [По умолчанию зашифрован Azure BLOB-объектов, файлы, таблицы и очереди хранилища](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).

Для баз данных, которые не предоставляют встроенные шифрование неактивных можно использовать шифрование диска для обеспечения такой же уровень защиты. Пример:

* [BitLocker для windows server](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux:
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Microsoft.com/GDPR](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
