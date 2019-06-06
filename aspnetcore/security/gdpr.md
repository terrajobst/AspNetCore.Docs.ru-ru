---
title: Поддержка регламент по защите данных (GDPR) в ASP.NET Core
author: rick-anderson
description: Узнайте, как получить доступ к точкам расширения GDPR в веб-приложения ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/05/2019
uid: security/gdpr
ms.openlocfilehash: 967f3246836c93a1af56f7109edb056220606b58
ms.sourcegitcommit: c716ea9155a6b404c1f3d3d34e2388454cd276d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2019
ms.locfileid: "66716346"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>Поддержка общих данных защиты стабилизации (GDPR) ЕС в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

ASP.NET Core предоставляет интерфейсы API и шаблоны для выполнения некоторых [общие данные защиты стабилизации (GDPR) ЕС](https://www.eugdpr.org/) требования:

* Шаблоны проектов включены, точки расширения и для которых созданы заглушки разметка, которую можно заменить конфиденциальности и политике использования файлов cookie.
* Файл cookie согласия позволяет запрашивать (и отслеживать) согласие от пользователей для хранения личных сведений. Если пользователь еще не означает согласие на сбор данных и у приложения есть [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) присвоено `true`, необязательные файлы cookie не отправляются в браузер.
* Файлы cookie могут помечаться как важные. Важные файлы cookie отправляются в браузер, даже в том случае, если пользователь еще не свое согласие, а также отслеживание отключено.
* [Файлы cookie сеанса и TempData](#tempdata) не могут использоваться при отключении отслеживания.
* [Управление удостоверений](#pd) странице содержится ссылка для загрузки и удаления данных пользователя.

[Пример приложения](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) позволяет проверить большинство точек расширения GDPR и добавлены интерфейсы API, шаблоны ASP.NET Core 2.1. См. в разделе [ReadMe](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) файла для тестирования инструкции.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>Поддержка ASP.NET Core GDPR в код, созданный шаблон

Razor Pages и MVC следующую поддержку GDPR включают проекты, созданные с помощью шаблонов проекта:

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) и [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) задаются в `Startup` класса.
* *\_CookieConsentPartial.cshtml* [частичное представление](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper). **Accept** кнопка включается в этот файл. Когда пользователь щелкает **Accept** кнопку, предоставление согласия для хранения файлов cookie предоставляется.
* *Pages/Privacy.cshtml* страницы или *Views/Home/Privacy.cshtml* представление будет содержать подробные сведения о политике конфиденциальности веб сайта страницы. *\_CookieConsentPartial.cshtml* файл создает ссылку на страницу о конфиденциальности.
* Для приложений, созданных с помощью отдельных учетных записей пользователей, управление ссылками на странице загрузки и удалить [персональные данные пользователя](#pd).

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions и UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) инициализируются в `Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) вызывается в `Startup.Configure`:

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>\_CookieConsentPartial.cshtml частичного представления

*\_CookieConsentPartial.cshtml* частичного представления:

[!code-html[](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

Это частично:

* Получает состояние отслеживания для пользователя. Если приложению требуется согласие, пользователь должен дать согласие, прежде чем можно отслеживать файлы cookie. Если требуется согласие, на панели согласия файл cookie является фиксированным в верхней части панели навигации, созданные  *\_Layout.cshtml* файла.
* Предоставляет HTML `<p>` элемент таким образом, конфиденциальности и файлах cookie с помощью политики.
* Ссылка на страницу конфиденциальность или представление, где подробно описаны политика конфиденциальности веб сайта.

## <a name="essential-cookies"></a>Важные файлы cookie

Если согласие для хранения файлов cookie не указано, только файлы cookie, помеченные как важные, отправляются в браузер. Следующий код делает файл cookie, необходимо:

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a>TempData поставщика и состояния файлов cookie сеанса не имеют существенного значения

[Поставщик TempData](xref:fundamentals/app-state#tempdata) важного файла cookie. Если отслеживание отключено, поставщик TempData недоступно. Чтобы включить поставщик TempData, при отключении отслеживания, пометить файл cookie TempData ключевым в `Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

[Состояние сеанса](xref:fundamentals/app-state) файлы cookie не нужны. Состояние сеанса недоступно при отключении отслеживания. В следующем коде выполняется важные файлы cookie сеанса:

[!code-csharp[](gdpr/sample/RP/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a>Персональные данные

Приложений ASP.NET Core, созданных с помощью отдельных учетных записей пользователей включать код для загрузки и удаление персональных данных.

Выберите имя пользователя, а затем выберите **персональных данных**:

![Страница «Управление» персональных данных](gdpr/_static/pd.png)

Примечания.

* Для создания `Account/Manage` кода, см. в разделе [удостоверений каркаса](xref:security/authentication/scaffold-identity).
* **Удалить** и **загрузить** ссылки воздействуют только на данные удостоверений по умолчанию. Приложения, которые создают пользовательские данные необходимо расширить до удаления или скачивания пользовательские данные. Дополнительные сведения см. в разделе [Add, скачивание и удаление пользовательских данных для идентификации](xref:security/authentication/add-user-data).
* Сохранить маркеры для пользователя, которые хранятся в таблице базы данных удостоверений `AspNetUserTokens` удаляются при удалении пользователя с помощью каскадных поведение удаления из-за [внешний ключ](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).
* [Внешнего поставщика проверки подлинности](xref:security/authentication/social/index), такие как Facebook и Google, не доступна, прежде чем принят политике файлов cookie.

## <a name="encryption-at-rest"></a>Шифрование при хранении

Некоторые механизмы хранения и баз данных позволяют шифрование в неактивном состоянии. Шифрование при хранении:

* Автоматически шифрует сохраненные данные.
* Шифрует без конфигурации, программирование и другую работу программного обеспечения, которое обращается к данным.
* — Самый простой и безопасный вариант.
* Позволяет базе данных для управления ключами и шифрования.

Пример:

* Microsoft SQL и Azure SQL предоставляют [прозрачное шифрование данных](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).
* [SQL Azure шифрует базы данных по умолчанию](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [Шифрование Azure большие двоичные объекты, файлы, таблицы и очереди хранилища по умолчанию](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).

Для баз данных, которые не предоставляют встроенные шифрование в неактивном состоянии можно использовать шифрование дисков для обеспечения ту же защиту. Пример:

* [BitLocker для Windows Server](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux:
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Microsoft.com/GDPR](https://www.microsoft.com/trustcenter/Privacy/GDPR)
