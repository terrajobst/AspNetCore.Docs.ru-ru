---
title: Подтверждение учетной записи и восстановление пароля в ASP.NET Core
author: rick-anderson
description: Сведения о создании приложения ASP.NET Core с помощью по электронной почте подтверждение и сброс пароля.
ms.author: riande
ms.date: 3/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 3bfc2ce46cfbc2ee308940f9e04eb2ffeec09073
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265501"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Подтверждение учетной записи и восстановление пароля в ASP.NET Core

::: moniker range="<= aspnetcore-2.0"

См. в разделе [этот PDF-файл](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) для ASP.NET Core 1.1 и версия 2.1.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), и [Joe Audette](https://twitter.com/joeaudette)

Этом руководстве описывается создание приложения ASP.NET Core с помощью по электронной почте подтверждение и сброс пароля. Это руководство представляет собой **не** начало раздела. Вы должны быть знакомы с:

* [ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start)
* [Authentication](xref:security/authentication/identity)
* [Entity Framework Core](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a>Предварительные требования

[Пакет SDK для .NET core 2.2 или более поздней версии](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a>Создание веб-приложения и сформировать шаблон удостоверений

Выполните следующие команды для создания веб-приложения с помощью проверки подлинности.

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a>Регистрация нового пользователя для тестирования

Запустите приложение, выберите **зарегистрировать** связать и регистрации пользователя. На этом этапе является только проверка на адрес электронной почты с [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) атрибута. После отправки регистрации, вы вошли в приложение. Далее в этом руководстве код обновляется, поэтому новые пользователи не могут войти до окончания проверки электронной почты.

[!INCLUDE[](~/includes/view-identity-db.md)]

Обратите внимание, таблицы `EmailConfirmed` поле является `False`.

Вы можете использовать этот адрес электронной почты снова на следующем шаге, когда приложение отправляет сообщение электронной почты с подтверждением. Щелкните правой кнопкой мыши в строке и выберите **удалить**. Удаление псевдонима электронной почты упрощает в следующих шагах.

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a>Требуется подтверждение по электронной почте

Рекомендуется подтвердить адрес электронной почты новой регистрации пользователя. Отправить по электронной почте подтверждение помогает проверить, они не олицетворения кто-то другой (то есть они еще не зарегистрировано другого пользователя по электронной почте). Предположим, что имеется дискуссионный форум и избежать "yli@example.com«из регистрации в качестве»nolivetto@contoso.com«. Без подтверждение по электронной почте "nolivetto@contoso.com" могли получать нежелательные сообщения электронной почты из вашего приложения. Предположим, что пользователь случайно зарегистрирован как "ylo@example.com" и не заметили опечатку «yli». Они не смогут использовать пароль восстановления, так как у приложения нет их правильный адрес электронной почты. Подтверждение по электронной почте предоставляет ограниченную защиту от программ-роботов. Подтверждение по электронной почте не обеспечивают защиту от пользователей-злоумышленников с многих учетных записей электронной почты.

Обычно требуется запретить новым пользователям из учета данных для веб-сайт, прежде чем они получат подтвержден по электронной почте.

Обновление `Startup.ConfigureServices` требуется подтвержден по электронной почте:

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

`config.SignIn.RequireConfirmedEmail = true;` запрещает вход до подтверждения их по электронной почте зарегистрированным пользователям.

### <a name="configure-email-provider"></a>Настройка поставщика услуг электронной почты

В этом руководстве [SendGrid](https://sendgrid.com) используется для отправки электронной почты. Требуется учетная запись SendGrid и ключ для отправки электронной почты. Можно использовать другие поставщики электронной почты. ASP.NET Core 2.x включает `System.Net.Mail`, который позволяет отправлять электронную почту из приложения. Мы рекомендуем использовать SendGrid или другая служба электронной почты для отправки электронной почты. Для защиты и правильно настроить сложно SMTP.

Создание класса для извлечения ключа защиты электронной почты. Для этого примера создайте *Services/AuthMessageSenderOptions.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a>Настройка SendGrid секреты пользователя

Задайте `SendGridUser` и `SendGridKey` с [средство secret manager](xref:security/app-secrets). Пример:

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

На Windows, Secret Manager хранит пары ключей и значений в *secrets.json* файл `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` каталог.

Содержание *secrets.json* файл не зашифрован. Следующая разметка показывает *secrets.json* файла. `SendGridKey` Значение удаляется.

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

Дополнительные сведения см. в разделе [шаблон параметров](xref:fundamentals/configuration/options) и [конфигурации](xref:fundamentals/configuration/index).

### <a name="install-sendgrid"></a>Установка SendGrid

Этот учебник демонстрирует добавление уведомлений по электронной почте через [SendGrid](https://sendgrid.com/), но вы можете отправлять электронную почту с помощью SMTP и другие механизмы.

Установить `SendGrid` пакет NuGet:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

В консоли диспетчера пакетов введите следующую команду:

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

В консоли введите следующую команду:

```cli
dotnet add package SendGrid
```

------

См. в разделе [приступить к работе с SendGrid бесплатно](https://sendgrid.com/free/) зарегистрироваться для получения бесплатной учетной записи SendGrid.

### <a name="implement-iemailsender"></a>Реализовать IEmailSender

Для реализации `IEmailSender`, создание *Services/EmailSender.cs* с кодом, аналогичную следующей:

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a>Настройка запуска для поддержки по электронной почте

Добавьте следующий код, чтобы `ConfigureServices` метод в *Startup.cs* файла:

* Добавление `EmailSender` как временной ошибкой службы.
* Зарегистрировать `AuthMessageSenderOptions` конфигурации экземпляра.

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a>Включить учетную запись, пароль и Подтверждение восстановления

Шаблон содержит код для восстановления и подтверждение пароля учетной записи. Найти `OnPostAsync` метод в *Areas/Identity/Pages/Account/Register.cshtml.cs*.

Запретить новым зарегистрированным пользователям, чтобы автоматически входить, закомментируйте следующую строку:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

С помощью измененные строки выделены показан полный метод:

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a>Регистрация, подтверждение электронной почты и сброс пароля

Запустите веб-приложение и протестируйте подтверждение учетной записи и процесс восстановления пароля.

* Запустите приложение и зарегистрируйте нового пользователя
* Проверьте свой адрес электронной почты для ссылку для подтверждения учетной записи. См. в разделе [отладки электронной почты](#debug) Если вы не получили сообщение электронной почты.
* Щелкните ссылку, чтобы подтвердить свой адрес электронной почты.
* Войдите, используя свой адрес электронной почты и пароль.
* Выйдите из системы.

### <a name="view-the-manage-page"></a>Просмотреть страницу управление

Выберите свое имя пользователя в браузере: ![окно браузера с именем пользователя](accconfirm/_static/un.png)

Управление страница отображается с **профиль** выделенной вкладки. **Электронной почты** показано типа "флажок", указывающее, адрес электронной почты будет подтверждена.

### <a name="test-password-reset"></a>Сброс пароля теста

* Если вы уже вошли в, выберите **выхода**.
* Выберите **вход** ссылку и выберите **забыли пароль?** ссылку.
* Введите адрес электронной почты, которая использовалась для регистрации учетной записи.
* Отправляется сообщение электронной почты со ссылкой для сброса пароля. Проверьте свой адрес электронной почты и щелкните ссылку, чтобы сбросить пароль. После успешного сброса пароля вы можете войти с использованием электронной почты и новый пароль.

## <a name="change-email-and-activity-timeout"></a>Изменить время ожидания сообщения электронной почты и действия

Время ожидания бездействия по умолчанию — 14 дней. Следующий код задает время ожидания в бездействии до 5 дней:

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a>Изменить все самозаверяющиеся маркеров защиты данных

Следующий код изменяет все ожидания маркеры защиты данных на 3 часа:

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

Встроенные в маркерах удостоверения пользователя (см. в разделе [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) имеют [одного дня время ожидания](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).

### <a name="change-the-email-token-lifespan"></a>Изменить время существования токена электронной почты

По умолчанию время существования токена доступа из [маркеров идентификации пользователя](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) — [один день](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs). В этом разделе показано, как изменить время существования токена электронной почты.

Добавить пользовательское [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) и <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

Добавьте пользовательский поставщик в контейнер службы:

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a>Повторно отправить подтверждение по электронной почте

См. в разделе [проблема GitHub](https://github.com/aspnet/AspNetCore/issues/5410).

<a name="debug"></a>

### <a name="debug-email"></a>Отладка по электронной почте

Если не удается получить рабочий адрес электронной почты:

* Установите точку останова в `EmailSender.Execute` для проверки `SendGridClient.SendEmailAsync` вызывается.
* Создание [консольное приложение для отправки электронной почты](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) аналогичный код, используя `EmailSender.Execute`.
* Просмотрите [действия "сообщения"](https://sendgrid.com/docs/User_Guide/email_activity.html) страницы.
* Проверьте папку нежелательной почты.
* Попробуйте другой псевдоним электронной почты на другой поставщик электронной почты (Microsoft, Yahoo, Gmail, и т.д.)
* Попробуйте отправить учетными записями электронной почты.

**Рекомендации по безопасности** — **не** используйте секреты производства в разработки и тестирования. При публикации приложения в Azure, секреты SendGrid можно задать как параметры приложения на портале веб-приложения Azure. Система конфигурации предназначена для чтения разделов из переменных среды.

## <a name="combine-social-and-local-login-accounts"></a>Объединить учетные записи социальных сетей и локального имени входа

Для завершения этого раздела необходимо сначала включить внешний поставщик аутентификации. См. в разделе [Facebook, Google и внешнего поставщика проверки подлинности](xref:security/authentication/social/index).

Можно объединить учетные записи локальных и социальных сетей, щелкнув ссылку по электронной почте. В следующей последовательности «RickAndMSFT@gmail.com"сначала создается как локальное имя входа; тем не менее, можно сначала создать учетную запись как имя для входа социальных сетей, а затем добавить локальное имя входа.

![Веб-приложения: RickAndMSFT@gmail.com пользователь прошел проверку подлинности](accconfirm/_static/rick.png)

Щелкните **управление** ссылку. Примечание внешний 0 (имена входа социальных сетей), связанное с этой учетной записи.

![Управления представлением](accconfirm/_static/manage.png)

Щелкните ссылку на другую службу входа в систему и принимать запросы приложений. На следующем рисунке Facebook, — это внешнего поставщика проверки подлинности:

![Управление ваш список внешних имен входа Facebook](accconfirm/_static/fb.png)

Две учетные записи были объединены. Вы сможете выполнить вход с использованием либо учетной записи. Вы можете добавить локальные учетные записи, в случае их учетные записи социальных сетей службы проверки подлинности не работает, или более вероятно, они потеряли доступ к своей учетной записи социальной сети пользователей.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Включить подтверждение учетной записи, после узла имеет пользователей

Включение подтверждение учетной записи на сайте с пользователями блокирует работу всех существующих пользователей. Существующие пользователи заблокированы, так как учетные записи не подтверждены. Чтобы обойти существующие блокировки пользователя, используйте один из следующих подходов:

* Обновите базу данных для пометки всех существующих пользователей в качестве подтверждения.
* Подтвердите существующих пользователей. Например пакетная Отправка сообщения электронной почты с подтверждением ссылки.

::: moniker-end
