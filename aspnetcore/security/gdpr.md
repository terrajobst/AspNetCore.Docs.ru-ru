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
ms.openlocfilehash: dc1724e8a78c25d3697d14ad784ce853737681f2
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/24/2018
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="52759-103">Поддержка Европа общие данные защиты стабилизации (GDPR) в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="52759-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="52759-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="52759-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="52759-105">ASP.NET Core предоставляет API-интерфейсы и шаблоны с помощью требованиям [UE общие данные защиты стабилизации (GDPR)](https://www.eugdpr.org/) требования:</span><span class="sxs-lookup"><span data-stu-id="52759-105">ASP.NET Core provides APIs and templates to help meet some of the [UE General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="52759-106">Шаблоны проектов включают точки расширения и кратких разметку, можно заменить конфиденциальности и политики использования файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="52759-106">The project templates include extension points and stubbed markup you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="52759-107">Функция согласия куки-файл позволяет запрашивать (и отслеживания) согласие от пользователей для сохранения личных сведений.</span><span class="sxs-lookup"><span data-stu-id="52759-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="52759-108">Если пользователь не дал свое согласие на сбор данных и приложение устанавливается с [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) для `true`, необязательные файлы cookie не будут отправляться в браузере.</span><span class="sxs-lookup"><span data-stu-id="52759-108">If a user has not consented to data collection and the app is set with [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) to `true`, non-essential cookies will not be sent to the browser.</span></span>
* <span data-ttu-id="52759-109">Файлы cookie могут быть помечены как важные.</span><span class="sxs-lookup"><span data-stu-id="52759-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="52759-110">Основные файлы cookie отправляются в браузере даже в том случае, если пользователь не согласился и трассировка отключена.</span><span class="sxs-lookup"><span data-stu-id="52759-110">Essential cookies are sent to the browser even when the user has not consented and tracking is disabled.</span></span>
* <span data-ttu-id="52759-111">[Файлы cookie TempData и сеанса](#tempdata) не работают при отключении отслеживания.</span><span class="sxs-lookup"><span data-stu-id="52759-111">[TempData and Session cookies](#tempdata) are not functional when tracking is disabled.</span></span>
* <span data-ttu-id="52759-112">[Управление удостоверения](#pd) странице приводятся ссылки для загрузки и удаление данных пользователей.</span><span class="sxs-lookup"><span data-stu-id="52759-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="52759-113">[Пример приложения](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) позволяет проверить большинство точек расширения GDPR и добавлен в список шаблонов ASP.NET Core 2.1 API-интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="52759-113">The [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="52759-114">В разделе [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) файла для тестирования инструкции.</span><span class="sxs-lookup"><span data-stu-id="52759-114">See the [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="52759-115">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="52759-115">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="52759-116">Поддержка GDPR ASP.NET Core в шаблоне созданного кода</span><span class="sxs-lookup"><span data-stu-id="52759-116">ASP.NET Core GDPR support in template generated code</span></span>

<span data-ttu-id="52759-117">Страниц Razor и MVC проектов, созданных с использованием шаблонов проекта поддерживает следующие GDPR:</span><span class="sxs-lookup"><span data-stu-id="52759-117">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="52759-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) и [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) задаются в `Startup`.</span><span class="sxs-lookup"><span data-stu-id="52759-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) are set in `Startup`.</span></span>
* <span data-ttu-id="52759-119">*_CookieConsentPartial.cshtml* [частичного представления](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="52759-119">The *_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span>
* <span data-ttu-id="52759-120">*Pages/Privacy.cshtml* или *Home/rivacy.cshtml* представление предоставляет страницы для подробного описания политики конфиденциальности вашего веб-узла.</span><span class="sxs-lookup"><span data-stu-id="52759-120">The *Pages/Privacy.cshtml*  or *Home/rivacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="52759-121">*_CookieConsentPartial.cshtml* файл создает ссылку на страницу о конфиденциальности.</span><span class="sxs-lookup"><span data-stu-id="52759-121">The *_CookieConsentPartial.cshtml* file generates a link to the privacy page.</span></span>
* <span data-ttu-id="52759-122">Для приложений, созданных с помощью отдельных учетных записей пользователей, страница управления ссылки для загрузки и удалить [персональных данных пользователя](#pd).</span><span class="sxs-lookup"><span data-stu-id="52759-122">For applications created with individual user accounts, the manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="52759-123">CookiePolicyOptions и UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="52759-123">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="52759-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) инициализируются в `Startup` класса `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="52759-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) are initialized in the `Startup` class `ConfigureServices` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="52759-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) вызывается в `Startup` класса `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="52759-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) is called in the `Startup` class `Configure` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="52759-126">Частичное представление _CookieConsentPartial.cshtml</span><span class="sxs-lookup"><span data-stu-id="52759-126">_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="52759-127">*_CookieConsentPartial.cshtml* частичного представления:</span><span class="sxs-lookup"><span data-stu-id="52759-127">The *_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="52759-128">Это частично:</span><span class="sxs-lookup"><span data-stu-id="52759-128">This partial:</span></span>

* <span data-ttu-id="52759-129">Возвращает состояние отслеживания для пользователя.</span><span class="sxs-lookup"><span data-stu-id="52759-129">Gets the state of tracking for the user.</span></span> <span data-ttu-id="52759-130">Если приложение настроено требуется согласие пользователя пользователь должен дать согласие, прежде чем можно будет отслеживать файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="52759-130">If the application is configured to require consent the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="52759-131">Устранена ли требуется согласие, chrome согласия cookie поверх панели навигации, созданные в *Pages/Shared/_Layout.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="52759-131">If consent is required, the cookie consent chrome is fixed on top of the navigation bar created in the *Pages/Shared/_Layout.cshtml* file.</span></span>
* <span data-ttu-id="52759-132">Предоставляет HTML `<p>` элемент для суммирования конфиденциальность и файлы cookie с помощью политики.</span><span class="sxs-lookup"><span data-stu-id="52759-132">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="52759-133">Ссылка на *Pages/Privacy.cshtml* где подробно описывается политика конфиденциальности веб-узла.</span><span class="sxs-lookup"><span data-stu-id="52759-133">Provides a link to *Pages/Privacy.cshtml* where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="52759-134">Основные файлы cookie</span><span class="sxs-lookup"><span data-stu-id="52759-134">Essential cookies</span></span>

<span data-ttu-id="52759-135">Если разрешения не были указаны, в браузере отправляются только файлы cookie, помеченные как важные.</span><span class="sxs-lookup"><span data-stu-id="52759-135">If consent has not been given, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="52759-136">В следующем коде выполняется essential куки-файл:</span><span class="sxs-lookup"><span data-stu-id="52759-136">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a><span data-ttu-id="52759-137">TempData поставщика сеанса состояния файлы cookie и не нужны</span><span class="sxs-lookup"><span data-stu-id="52759-137">Tempdata provider and session state cookies are not essential</span></span>

<span data-ttu-id="52759-138">[Tempdata поставщика](xref:fundamentals/app-state#tempdata) куки-файл не является необходимым.</span><span class="sxs-lookup"><span data-stu-id="52759-138">The [Tempdata provider](xref:fundamentals/app-state#tempdata) cookie is not essential.</span></span> <span data-ttu-id="52759-139">При отключении отслеживания Tempdata поставщика не работает.</span><span class="sxs-lookup"><span data-stu-id="52759-139">If tracking is disabled, the Tempdata provider is not functional.</span></span> <span data-ttu-id="52759-140">Чтобы включить поставщик Tempdata при отключении отслеживания, отметки куки-файла TempData ключевым в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="52759-140">To enable the Tempdata provider when tracking is disabled, mark the TempData cookie as essential in `ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="52759-141">[Состояние сеанса](xref:fundamentals/app-state) файлы cookie не нужны.</span><span class="sxs-lookup"><span data-stu-id="52759-141">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="52759-142">При отключении отслеживания состояния сеанса не работает.</span><span class="sxs-lookup"><span data-stu-id="52759-142">Session state is not functional when tracking is disabled.</span></span>

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="52759-143">Персональные данные</span><span class="sxs-lookup"><span data-stu-id="52759-143">Personal data</span></span>

<span data-ttu-id="52759-144">Приложения ASP.NET Core, созданные с помощью отдельных учетных записей пользователей включить код для загрузки и удалять личные данные.</span><span class="sxs-lookup"><span data-stu-id="52759-144">ASP.NET Core applications created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="52759-145">Выберите имя пользователя, а затем выберите **личных данных**:</span><span class="sxs-lookup"><span data-stu-id="52759-145">Select the user name and then select **Personal data**:</span></span>

![Страница «Управление» персональные данные](gdpr/_static/pd.png)

<span data-ttu-id="52759-147">Примечания.</span><span class="sxs-lookup"><span data-stu-id="52759-147">Notes:</span></span>

* <span data-ttu-id="52759-148">Для создания `Account/Manage` кода см. в разделе [удостоверение формирования шаблонов](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="52759-148">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="52759-149">Удалите и загрузить только влияние учетных данных по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="52759-149">Delete and download only impact the default identity data.</span></span> <span data-ttu-id="52759-150">Создать пользовательские данные приложения необходимо расширить до удаления загрузку пользовательские данные.</span><span class="sxs-lookup"><span data-stu-id="52759-150">Apps the create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="52759-151">Проблема GitHub [как добавлять или удалять пользовательские данные для удостоверения](https://github.com/aspnet/Docs/issues/6226) отслеживает предложенный статьи о создании пользовательских или удаление или загрузке пользовательские данные.</span><span class="sxs-lookup"><span data-stu-id="52759-151">GitHub issue [How to add/delete custom user data to Identity](https://github.com/aspnet/Docs/issues/6226) tracks a proposed article on creating custom/deleting/downloading custom user data.</span></span> <span data-ttu-id="52759-152">Если хотелось бы в разделе, приоритету, оставьте Палец вверх реакции на проблему.</span><span class="sxs-lookup"><span data-stu-id="52759-152">If you'd like to see that topic prioritized, leave a thumbs up reaction in the issue.</span></span>
* <span data-ttu-id="52759-153">Сохранить токены для пользователя, которые хранятся в таблице базы данных удостоверений `AspNetUserTokens` удаляются при удалении пользователя через каскадных поведение удаления из-за [внешний ключ](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="52759-153">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="52759-154">Шифрование неактивных</span><span class="sxs-lookup"><span data-stu-id="52759-154">Encryption at rest</span></span>

<span data-ttu-id="52759-155">Шифрование неактивных разрешить некоторые базы данных и механизмы хранения.</span><span class="sxs-lookup"><span data-stu-id="52759-155">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="52759-156">Шифрование при хранении:</span><span class="sxs-lookup"><span data-stu-id="52759-156">Encryption at rest:</span></span>

* <span data-ttu-id="52759-157">Автоматически шифрует сохраненные данные.</span><span class="sxs-lookup"><span data-stu-id="52759-157">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="52759-158">Шифрует без конфигурации, программирования и другие типы работ, для программного обеспечения, которое обращается к данным.</span><span class="sxs-lookup"><span data-stu-id="52759-158">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="52759-159">Является простейшим и наиболее безопасным вариантом.</span><span class="sxs-lookup"><span data-stu-id="52759-159">Is the easiest and safest option.</span></span>
* <span data-ttu-id="52759-160">Позволяет управлять ключей и шифрования базы данных.</span><span class="sxs-lookup"><span data-stu-id="52759-160">Lets the database manage keys and encryption.</span></span>

<span data-ttu-id="52759-161">Пример:</span><span class="sxs-lookup"><span data-stu-id="52759-161">For example:</span></span>

* <span data-ttu-id="52759-162">Microsoft SQL и Azure SQL предоставляет [прозрачное шифрование данных](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).</span><span class="sxs-lookup"><span data-stu-id="52759-162">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).</span></span>
* [<span data-ttu-id="52759-163">SQL Azure шифрует базы данных по умолчанию</span><span class="sxs-lookup"><span data-stu-id="52759-163">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/en-us/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="52759-164">[По умолчанию зашифрован Azure BLOB-объектов, файлы, таблицы и очереди хранилища](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="52759-164">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="52759-165">Для баз данных, которые не предоставляют встроенные шифрование неактивных можно использовать шифрование диска для обеспечения такой же уровень защиты.</span><span class="sxs-lookup"><span data-stu-id="52759-165">For databases that don't provide built-in encryption at rest you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="52759-166">Пример:</span><span class="sxs-lookup"><span data-stu-id="52759-166">For example:</span></span>

* [<span data-ttu-id="52759-167">BitLocker для windows server</span><span class="sxs-lookup"><span data-stu-id="52759-167">Bitlocker for windows server</span></span>](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="52759-168">Linux:</span><span class="sxs-lookup"><span data-stu-id="52759-168">Linux:</span></span>
  * [<span data-ttu-id="52759-169">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="52759-169">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="52759-170">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="52759-170">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="52759-171">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="52759-171">Additional Resources</span></span>

* [<span data-ttu-id="52759-172">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="52759-172">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
