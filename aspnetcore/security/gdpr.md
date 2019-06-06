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
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="59674-103">Поддержка общих данных защиты стабилизации (GDPR) ЕС в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="59674-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="59674-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="59674-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="59674-105">ASP.NET Core предоставляет интерфейсы API и шаблоны для выполнения некоторых [общие данные защиты стабилизации (GDPR) ЕС](https://www.eugdpr.org/) требования:</span><span class="sxs-lookup"><span data-stu-id="59674-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="59674-106">Шаблоны проектов включены, точки расширения и для которых созданы заглушки разметка, которую можно заменить конфиденциальности и политике использования файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="59674-106">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="59674-107">Файл cookie согласия позволяет запрашивать (и отслеживать) согласие от пользователей для хранения личных сведений.</span><span class="sxs-lookup"><span data-stu-id="59674-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="59674-108">Если пользователь еще не означает согласие на сбор данных и у приложения есть [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) присвоено `true`, необязательные файлы cookie не отправляются в браузер.</span><span class="sxs-lookup"><span data-stu-id="59674-108">If a user hasn't consented to data collection and the app has [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) set to `true`, non-essential cookies aren't sent to the browser.</span></span>
* <span data-ttu-id="59674-109">Файлы cookie могут помечаться как важные.</span><span class="sxs-lookup"><span data-stu-id="59674-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="59674-110">Важные файлы cookie отправляются в браузер, даже в том случае, если пользователь еще не свое согласие, а также отслеживание отключено.</span><span class="sxs-lookup"><span data-stu-id="59674-110">Essential cookies are sent to the browser even when the user hasn't consented and tracking is disabled.</span></span>
* <span data-ttu-id="59674-111">[Файлы cookie сеанса и TempData](#tempdata) не могут использоваться при отключении отслеживания.</span><span class="sxs-lookup"><span data-stu-id="59674-111">[TempData and Session cookies](#tempdata) aren't functional when tracking is disabled.</span></span>
* <span data-ttu-id="59674-112">[Управление удостоверений](#pd) странице содержится ссылка для загрузки и удаления данных пользователя.</span><span class="sxs-lookup"><span data-stu-id="59674-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="59674-113">[Пример приложения](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) позволяет проверить большинство точек расширения GDPR и добавлены интерфейсы API, шаблоны ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="59674-113">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) allows you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="59674-114">См. в разделе [ReadMe](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) файла для тестирования инструкции.</span><span class="sxs-lookup"><span data-stu-id="59674-114">See the [ReadMe](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="59674-115">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="59674-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="59674-116">Поддержка ASP.NET Core GDPR в код, созданный шаблон</span><span class="sxs-lookup"><span data-stu-id="59674-116">ASP.NET Core GDPR support in template-generated code</span></span>

<span data-ttu-id="59674-117">Razor Pages и MVC следующую поддержку GDPR включают проекты, созданные с помощью шаблонов проекта:</span><span class="sxs-lookup"><span data-stu-id="59674-117">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="59674-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) и [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) задаются в `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="59674-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) are set in the `Startup` class.</span></span>
* <span data-ttu-id="59674-119">*\_CookieConsentPartial.cshtml* [частичное представление](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="59674-119">The *\_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="59674-120">**Accept** кнопка включается в этот файл.</span><span class="sxs-lookup"><span data-stu-id="59674-120">An **Accept** button is included in this file.</span></span> <span data-ttu-id="59674-121">Когда пользователь щелкает **Accept** кнопку, предоставление согласия для хранения файлов cookie предоставляется.</span><span class="sxs-lookup"><span data-stu-id="59674-121">When the user clicks the **Accept** button, consent to store cookies is provided.</span></span>
* <span data-ttu-id="59674-122">*Pages/Privacy.cshtml* страницы или *Views/Home/Privacy.cshtml* представление будет содержать подробные сведения о политике конфиденциальности веб сайта страницы.</span><span class="sxs-lookup"><span data-stu-id="59674-122">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="59674-123">*\_CookieConsentPartial.cshtml* файл создает ссылку на страницу о конфиденциальности.</span><span class="sxs-lookup"><span data-stu-id="59674-123">The *\_CookieConsentPartial.cshtml* file generates a link to the Privacy page.</span></span>
* <span data-ttu-id="59674-124">Для приложений, созданных с помощью отдельных учетных записей пользователей, управление ссылками на странице загрузки и удалить [персональные данные пользователя](#pd).</span><span class="sxs-lookup"><span data-stu-id="59674-124">For apps created with individual user accounts, the Manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="59674-125">CookiePolicyOptions и UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="59674-125">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="59674-126">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) инициализируются в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="59674-126">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) are initialized in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="59674-127">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) вызывается в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="59674-127">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) is called in `Startup.Configure`:</span></span>

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="59674-128">\_CookieConsentPartial.cshtml частичного представления</span><span class="sxs-lookup"><span data-stu-id="59674-128">\_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="59674-129">*\_CookieConsentPartial.cshtml* частичного представления:</span><span class="sxs-lookup"><span data-stu-id="59674-129">The *\_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="59674-130">Это частично:</span><span class="sxs-lookup"><span data-stu-id="59674-130">This partial:</span></span>

* <span data-ttu-id="59674-131">Получает состояние отслеживания для пользователя.</span><span class="sxs-lookup"><span data-stu-id="59674-131">Obtains the state of tracking for the user.</span></span> <span data-ttu-id="59674-132">Если приложению требуется согласие, пользователь должен дать согласие, прежде чем можно отслеживать файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="59674-132">If the app is configured to require consent, the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="59674-133">Если требуется согласие, на панели согласия файл cookie является фиксированным в верхней части панели навигации, созданные  *\_Layout.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="59674-133">If consent is required, the cookie consent panel is fixed at top of the navigation bar created by the *\_Layout.cshtml* file.</span></span>
* <span data-ttu-id="59674-134">Предоставляет HTML `<p>` элемент таким образом, конфиденциальности и файлах cookie с помощью политики.</span><span class="sxs-lookup"><span data-stu-id="59674-134">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="59674-135">Ссылка на страницу конфиденциальность или представление, где подробно описаны политика конфиденциальности веб сайта.</span><span class="sxs-lookup"><span data-stu-id="59674-135">Provides a link to Privacy page or view where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="59674-136">Важные файлы cookie</span><span class="sxs-lookup"><span data-stu-id="59674-136">Essential cookies</span></span>

<span data-ttu-id="59674-137">Если согласие для хранения файлов cookie не указано, только файлы cookie, помеченные как важные, отправляются в браузер.</span><span class="sxs-lookup"><span data-stu-id="59674-137">If consent to store cookies hasn't been provided, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="59674-138">Следующий код делает файл cookie, необходимо:</span><span class="sxs-lookup"><span data-stu-id="59674-138">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a><span data-ttu-id="59674-139">TempData поставщика и состояния файлов cookie сеанса не имеют существенного значения</span><span class="sxs-lookup"><span data-stu-id="59674-139">TempData provider and session state cookies aren't essential</span></span>

<span data-ttu-id="59674-140">[Поставщик TempData](xref:fundamentals/app-state#tempdata) важного файла cookie.</span><span class="sxs-lookup"><span data-stu-id="59674-140">The [TempData provider](xref:fundamentals/app-state#tempdata) cookie isn't essential.</span></span> <span data-ttu-id="59674-141">Если отслеживание отключено, поставщик TempData недоступно.</span><span class="sxs-lookup"><span data-stu-id="59674-141">If tracking is disabled, the TempData provider isn't functional.</span></span> <span data-ttu-id="59674-142">Чтобы включить поставщик TempData, при отключении отслеживания, пометить файл cookie TempData ключевым в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="59674-142">To enable the TempData provider when tracking is disabled, mark the TempData cookie as essential in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="59674-143">[Состояние сеанса](xref:fundamentals/app-state) файлы cookie не нужны.</span><span class="sxs-lookup"><span data-stu-id="59674-143">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="59674-144">Состояние сеанса недоступно при отключении отслеживания.</span><span class="sxs-lookup"><span data-stu-id="59674-144">Session state isn't functional when tracking is disabled.</span></span> <span data-ttu-id="59674-145">В следующем коде выполняется важные файлы cookie сеанса:</span><span class="sxs-lookup"><span data-stu-id="59674-145">The following code makes session cookies essential:</span></span>

[!code-csharp[](gdpr/sample/RP/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="59674-146">Персональные данные</span><span class="sxs-lookup"><span data-stu-id="59674-146">Personal data</span></span>

<span data-ttu-id="59674-147">Приложений ASP.NET Core, созданных с помощью отдельных учетных записей пользователей включать код для загрузки и удаление персональных данных.</span><span class="sxs-lookup"><span data-stu-id="59674-147">ASP.NET Core apps created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="59674-148">Выберите имя пользователя, а затем выберите **персональных данных**:</span><span class="sxs-lookup"><span data-stu-id="59674-148">Select the user name and then select **Personal data**:</span></span>

![Страница «Управление» персональных данных](gdpr/_static/pd.png)

<span data-ttu-id="59674-150">Примечания.</span><span class="sxs-lookup"><span data-stu-id="59674-150">Notes:</span></span>

* <span data-ttu-id="59674-151">Для создания `Account/Manage` кода, см. в разделе [удостоверений каркаса](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="59674-151">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="59674-152">**Удалить** и **загрузить** ссылки воздействуют только на данные удостоверений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="59674-152">The **Delete** and **Download** links only act on the default identity data.</span></span> <span data-ttu-id="59674-153">Приложения, которые создают пользовательские данные необходимо расширить до удаления или скачивания пользовательские данные.</span><span class="sxs-lookup"><span data-stu-id="59674-153">Apps that create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="59674-154">Дополнительные сведения см. в разделе [Add, скачивание и удаление пользовательских данных для идентификации](xref:security/authentication/add-user-data).</span><span class="sxs-lookup"><span data-stu-id="59674-154">For more information, see [Add, download, and delete custom user data to Identity](xref:security/authentication/add-user-data).</span></span>
* <span data-ttu-id="59674-155">Сохранить маркеры для пользователя, которые хранятся в таблице базы данных удостоверений `AspNetUserTokens` удаляются при удалении пользователя с помощью каскадных поведение удаления из-за [внешний ключ](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="59674-155">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span></span>
* <span data-ttu-id="59674-156">[Внешнего поставщика проверки подлинности](xref:security/authentication/social/index), такие как Facebook и Google, не доступна, прежде чем принят политике файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="59674-156">[External provider authentication](xref:security/authentication/social/index), such as Facebook and Google, isn't available before the cookie policy is accepted.</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="59674-157">Шифрование при хранении</span><span class="sxs-lookup"><span data-stu-id="59674-157">Encryption at rest</span></span>

<span data-ttu-id="59674-158">Некоторые механизмы хранения и баз данных позволяют шифрование в неактивном состоянии.</span><span class="sxs-lookup"><span data-stu-id="59674-158">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="59674-159">Шифрование при хранении:</span><span class="sxs-lookup"><span data-stu-id="59674-159">Encryption at rest:</span></span>

* <span data-ttu-id="59674-160">Автоматически шифрует сохраненные данные.</span><span class="sxs-lookup"><span data-stu-id="59674-160">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="59674-161">Шифрует без конфигурации, программирование и другую работу программного обеспечения, которое обращается к данным.</span><span class="sxs-lookup"><span data-stu-id="59674-161">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="59674-162">— Самый простой и безопасный вариант.</span><span class="sxs-lookup"><span data-stu-id="59674-162">Is the easiest and safest option.</span></span>
* <span data-ttu-id="59674-163">Позволяет базе данных для управления ключами и шифрования.</span><span class="sxs-lookup"><span data-stu-id="59674-163">Allows the database to manage keys and encryption.</span></span>

<span data-ttu-id="59674-164">Пример:</span><span class="sxs-lookup"><span data-stu-id="59674-164">For example:</span></span>

* <span data-ttu-id="59674-165">Microsoft SQL и Azure SQL предоставляют [прозрачное шифрование данных](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span><span class="sxs-lookup"><span data-stu-id="59674-165">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span></span>
* [<span data-ttu-id="59674-166">SQL Azure шифрует базы данных по умолчанию</span><span class="sxs-lookup"><span data-stu-id="59674-166">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="59674-167">[Шифрование Azure большие двоичные объекты, файлы, таблицы и очереди хранилища по умолчанию](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="59674-167">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="59674-168">Для баз данных, которые не предоставляют встроенные шифрование в неактивном состоянии можно использовать шифрование дисков для обеспечения ту же защиту.</span><span class="sxs-lookup"><span data-stu-id="59674-168">For databases that don't provide built-in encryption at rest, you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="59674-169">Пример:</span><span class="sxs-lookup"><span data-stu-id="59674-169">For example:</span></span>

* [<span data-ttu-id="59674-170">BitLocker для Windows Server</span><span class="sxs-lookup"><span data-stu-id="59674-170">BitLocker for Windows Server</span></span>](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="59674-171">Linux:</span><span class="sxs-lookup"><span data-stu-id="59674-171">Linux:</span></span>
  * [<span data-ttu-id="59674-172">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="59674-172">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="59674-173">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="59674-173">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="59674-174">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="59674-174">Additional resources</span></span>

* [<span data-ttu-id="59674-175">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="59674-175">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/trustcenter/Privacy/GDPR)
