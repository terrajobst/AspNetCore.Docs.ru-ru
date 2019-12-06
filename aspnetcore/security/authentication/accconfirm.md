---
title: Подтверждение учетной записи и восстановление пароля в ASP.NET Core
author: rick-anderson
description: Узнайте, как создать приложение ASP.NET Core с подтверждением электронной почты и сбросом пароля.
ms.author: riande
ms.date: 03/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: a4ecc2d91fb72915703dfaa146260f0c1360bded
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880771"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Подтверждение учетной записи и восстановление пароля в ASP.NET Core

[Рик Андерсон (](https://twitter.com/RickAndMSFT), [Понант](https://github.com/Ponant)и [Джо аудетте](https://twitter.com/joeaudette)

В этом руководстве показано, как создать приложение ASP.NET Core с подтверждением электронной почты и сбросом пароля. Это руководство **не** является началом статьи. Вы должны быть знакомы с:

* [ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start)
* [Authentication](xref:security/authentication/identity)
* [Entity Framework Core](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

::: moniker range="<= aspnetcore-2.0"

См. [этот PDF-файл](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) для ASP.NET Core версии 1,1.

::: moniker-end

::: moniker range="> aspnetcore-2.2"

## <a name="prerequisites"></a>Необходимые компоненты

[Пакет SDK для .NET Core 3.0 или более поздней версии](https://dotnet.microsoft.com/download/dotnet-core/3.0)

## <a name="create-and-test-a-web-app-with-authentication"></a>Создание и тестирование веб-приложения с проверкой подлинности

Выполните следующие команды, чтобы создать веб-приложение с проверкой подлинности.

```dotnetcli
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet run
```

Запустите приложение, выберите ссылку **Register** и зарегистрируйте пользователя. После регистрации вы будете перенаправлены на страницу `/Identity/Account/RegisterConfirmation`, содержащую ссылку для имитации подтверждения по электронной почте:

* Выберите ссылку `Click here to confirm your account`.
* Выберите ссылку для **входа** и выполните вход с теми же учетными данными.
* Выберите ссылку `Hello YourEmail@provider.com!`, которая перенаправит вас на страницу `/Identity/Account/Manage/PersonalData`.
* Выберите вкладку **личные данные** слева, а затем щелкните **Удалить**.

### <a name="configure-an-email-provider"></a>Настройка поставщика электронной почты

В этом руководстве [SendGrid](https://sendgrid.com) используется для отправки электронной почты. Для отправки электронной почты требуется учетная запись SendGrid и ключ. Вы можете использовать другие поставщики электронной почты. Для отправки электронной почты рекомендуется использовать SendGrid или другую почтовую службу. Протокол SMTP трудно защитить и настроить правильно.

Создайте класс для выборки ключа защищенной электронной почты. Для этого примера создайте *Services/аусмессажесендероптионс. CS*:

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a>Настройка секретов пользователя SendGrid

Задайте `SendGridUser` и `SendGridKey` с помощью [средства Secret-Manager](xref:security/app-secrets). Например:

```dotnetcli
dotnet user-secrets set SendGridUser RickAndMSFT
dotnet user-secrets set SendGridKey <key>

Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

В Windows Диспетчер секретов сохраняет пары "ключ-значение" в файле *секреты. JSON* в каталоге `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`.

Содержимое файла *секреты. JSON* не шифруется. В следующей разметке показан файл *секреты. JSON* . Значение `SendGridKey` было удалено.

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

Дополнительные сведения см. в разделе [шаблон](xref:fundamentals/configuration/options) и [Конфигурация](xref:fundamentals/configuration/index)параметров.

### <a name="install-sendgrid"></a>Установка SendGrid

В этом руководстве показано, как добавлять уведомления по электронной почте через [SendGrid](https://sendgrid.com/), но можно отправлять сообщения электронной почты с помощью SMTP и других механизмов.

Установите `SendGrid` пакет NuGet:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

В консоли диспетчера пакетов введите следующую команду:

```powershell
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

В консоли введите следующую команду:

```dotnetcli
dotnet add package SendGrid
```

---

Ознакомьтесь [со статьей начало работы с SendGrid](https://sendgrid.com/free/) , чтобы зарегистрироваться для получения бесплатной учетной записи SendGrid.

### <a name="implement-iemailsender"></a>Реализация Иемаилсендер

Чтобы реализовать `IEmailSender`, создайте *службы/емаилсендер. CS* с кодом, аналогичным следующему:

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a>Настройка запуска для поддержки электронной почты

Добавьте следующий код в метод `ConfigureServices` в файле *Startup.CS* :

* Добавьте `EmailSender` в качестве временной службы.
* Зарегистрируйте экземпляр конфигурации `AuthMessageSenderOptions`.

[!code-csharp[](accconfirm/sample/WebPWrecover30/Startup.cs?name=snippet1&highlight=11-15)]

## <a name="register-confirm-email-and-reset-password"></a>Регистрация, подтверждение электронной почты и сброс пароля

Запустите веб-приложение и протестируйте поток подтверждения учетной записи и восстановления пароля.

* Запустите приложение и зарегистрируйте нового пользователя
* Проверьте свою электронную почту на наличие ссылки для подтверждения учетной записи. Если вы не получаете электронное письмо, см. раздел [Отладка электронной почты](#debug) .
* Щелкните ссылку, чтобы подтвердить свою электронную почту.
* Выполните вход, используя свой адрес электронной почты и пароль.
* Выполните выход.

### <a name="test-password-reset"></a>Проверка сброса пароля

* Если вы вошли в этот компьютер, выберите **выход**.
* Щелкните ссылку **войти** и выберите ссылку **забыли пароль?** .
* Введите адрес электронной почты, использованный для регистрации учетной записи.
* Отправляется сообщение электронной почты со ссылкой для сброса пароля. Проверьте свой адрес электронной почты и щелкните ссылку, чтобы сбросить пароль. После успешного сброса пароля можно выполнить вход с помощью электронной почты и нового пароля.

## <a name="change-email-and-activity-timeout"></a>Изменить время ожидания для электронной почты и активности

Время ожидания бездействия по умолчанию составляет 14 дней. Следующий код задает время ожидания простоя в 5 дней:

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a>Изменить все продолжительность существования маркеров защиты данных

Следующий код изменяет все маркеры защиты данных в течение времени ожидания до 3 часов:

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAllTokens.cs?name=snippet1&highlight=11-12)]

Встроенные маркеры пользователей удостоверений (см. [AspNetCore/src/Identity/Extensions. Core/src/токеноптионс. CS](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) имеют [время ожидания в один день](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).

### <a name="change-the-email-token-lifespan"></a>Изменение срока существования маркера электронной почты

Срок существования токена [пользователя](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) по умолчанию — [один день](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs). В этом разделе показано, как изменить срок существования маркера электронной почты.

Добавьте настраиваемую [датапротектортокенпровидер\<тусер >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) и <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:

[!code-csharp[](accconfirm/sample/WebPWrecover30/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

Добавьте настраиваемый поставщик в контейнер службы:

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupEmail.cs?name=snippet1&highlight=10-16)]

### <a name="resend-email-confirmation"></a>Подтверждение повторной отправки электронной почты

Также см. [эту проблему в GitHub](https://github.com/aspnet/AspNetCore/issues/5410).

<a name="debug"></a>

### <a name="debug-email"></a>Отладка электронной почты

Если вы не можете работать с электронной почтой:

* Установите точку останова в `EmailSender.Execute` для проверки вызова `SendGridClient.SendEmailAsync`.
* Создание [консольного приложения для отправки электронной почты](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) с помощью аналогичного кода для `EmailSender.Execute`.
* Ознакомьтесь со страницей [действие по электронной почте](https://sendgrid.com/docs/User_Guide/email_activity.html) .
* Проверьте папку спама.
* Попробуйте использовать другой псевдоним электронной почты в другом поставщике электронной почты (Microsoft, Yahoo, Gmail и т. д.).
* Попробуйте отправить их в другую учетную запись электронной почты.

В целях **безопасности** рекомендуется **не** использовать рабочие секреты в тестировании и разработке. Если приложение публикуется в Azure, задайте секреты SendGrid как параметры приложения на портале веб-приложения Azure. Система конфигурации предназначена для чтения разделов из переменных среды.

## <a name="combine-social-and-local-login-accounts"></a>Объединение социальных и местных учетных записей входа

Для выполнения этого раздела необходимо сначала включить внешний поставщик проверки подлинности. См. статью [Проверка подлинности в Facebook, Google и внешнем поставщике](xref:security/authentication/social/index).

Вы можете объединить локальные и социальные учетные записи, щелкнув ссылку на электронную почту. В следующей последовательности «RickAndMSFT@gmail.com» сначала создается как локальное имя входа. Тем не менее можно сначала создать учетную запись в качестве имени для входа в социальных сетях, а затем добавить локальное имя входа.

![Веб-приложение: RickAndMSFT@gmail.com пользователь прошел проверку подлинности](accconfirm/_static/rick.png)

Щелкните ссылку **Управление** . Обратите внимание на 0 внешних (социальных) имен, связанных с этой учетной записью.

![Управление представлением](accconfirm/_static/manage.png)

Щелкните ссылку на другую службу входа и примите запросы приложения. На следующем рисунке Facebook является внешним поставщиком проверки подлинности:

![Управление внешними именами входа Просмотр списка Facebook](accconfirm/_static/fb.png)

Две учетные записи были объединены. Вы можете войти с помощью любой учетной записи. Пользователям может потребоваться добавить локальные учетные записи на случай, если служба проверки подлинности в социальных сетях не работает, или более вероятно, что они потеряют доступ к учетной записи социальных сетей.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Включить подтверждение учетной записи после того, как у сайта есть пользователи

Включение подтверждения учетной записи на сайте с пользователями блокирует всех существующих пользователей. Существующие пользователи заблокированы, так как их учетные записи не подтверждены. Чтобы обойти существующую блокировку пользователей, используйте один из следующих подходов.

* Обновите базу данных, чтобы пометить все существующие пользователи как подтвержденные.
* Подтвердите существующих пользователей. Например, пакетная отправка сообщений электронной почты с ссылками для подтверждения.

::: moniker-end

::: moniker range="> aspnetcore-2.0 < aspnetcore-3.0"

## <a name="prerequisites"></a>Необходимые компоненты

[Пакет SDK для .NET Core 2,2 или более поздней версии](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a>Создание веб-приложения и удостоверение шаблона

Выполните следующие команды, чтобы создать веб-приложение с проверкой подлинности.

```dotnetcli
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a>Тестирование регистрации нового пользователя

Запустите приложение, выберите ссылку **Register** и зарегистрируйте пользователя. На этом этапе единственной проверкой адреса электронной почты является атрибут [`[EmailAddress]`](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) . После отправки регистрации вы вошли в приложение. Далее в этом руководстве код обновляется, поэтому новые пользователи не смогут войти в систему, пока не будет проверена электронная почта.

[!INCLUDE[](~/includes/view-identity-db.md)]

Обратите внимание, что поле `EmailConfirmed` таблицы `False`.

Вы можете снова использовать это сообщение на следующем шаге, когда приложение отправит сообщение электронной почты с подтверждением. Щелкните строку правой кнопкой мыши и выберите команду **Удалить**. Удаление псевдонима электронной почты упрощает следующие шаги.

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a>Требовать подтверждение по электронной почте

Рекомендуется подтвердить сообщение электронной почты о новой регистрации пользователя. Подтверждение по электронной почте помогает проверить, что они не олицетворяют другого пользователя (т. е. они не зарегистрированы в сообщении электронной почты другого пользователя). Предположим, у вас есть дискуссионный форум, и вы хотите предотвратить регистрацию "yli@example.com" как "nolivetto@contoso.com". Без подтверждения по электронной почте "nolivetto@contoso.com" может получить от вашего приложения нежелательное сообщение электронной почты. Предположим, что пользователь случайно зарегистрировался как "ylo@example.com" и не заметил опечатку "или". Они не смогут использовать восстановление пароля, так как у приложения нет нужной электронной почты. Подтверждение по электронной почте обеспечивает ограниченную защиту от программы-роботы. Подтверждение по электронной почте не обеспечивает защиту от вредоносных пользователей со многими учетными записями электронной почты.

Обычно требуется запретить новым пользователям отправлять данные на ваш веб-сайт, прежде чем они будут иметь подтвержденное сообщение электронной почты.

Обновите `Startup.ConfigureServices`, чтобы требовать подтверждения по электронной почте:

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

`config.SignIn.RequireConfirmedEmail = true;` запрещает зарегистрированным пользователям входить в систему, пока не будет подтверждено их электронная почта.

### <a name="configure-email-provider"></a>Настройка поставщика электронной почты

В этом руководстве [SendGrid](https://sendgrid.com) используется для отправки электронной почты. Для отправки электронной почты требуется учетная запись SendGrid и ключ. Вы можете использовать другие поставщики электронной почты. ASP.NET Core 2. x включает `System.Net.Mail`, что позволяет отправлять электронную почту из приложения. Для отправки электронной почты рекомендуется использовать SendGrid или другую почтовую службу. Протокол SMTP трудно защитить и настроить правильно.

Создайте класс для выборки ключа защищенной электронной почты. Для этого примера создайте *Services/аусмессажесендероптионс. CS*:

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a>Настройка секретов пользователя SendGrid

Задайте `SendGridUser` и `SendGridKey` с помощью [средства Secret-Manager](xref:security/app-secrets). Например:

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

В Windows Диспетчер секретов сохраняет пары "ключ-значение" в файле *секреты. JSON* в каталоге `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`.

Содержимое файла *секреты. JSON* не шифруется. В следующей разметке показан файл *секреты. JSON* . Значение `SendGridKey` было удалено.

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

Дополнительные сведения см. в разделе [шаблон](xref:fundamentals/configuration/options) и [Конфигурация](xref:fundamentals/configuration/index)параметров.

### <a name="install-sendgrid"></a>Установка SendGrid

В этом руководстве показано, как добавлять уведомления по электронной почте через [SendGrid](https://sendgrid.com/), но можно отправлять сообщения электронной почты с помощью SMTP и других механизмов.

Установите `SendGrid` пакет NuGet:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

В консоли диспетчера пакетов введите следующую команду:

```powershell
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

В консоли введите следующую команду:

```dotnetcli
dotnet add package SendGrid
```

---

Ознакомьтесь [со статьей начало работы с SendGrid](https://sendgrid.com/free/) , чтобы зарегистрироваться для получения бесплатной учетной записи SendGrid.

### <a name="implement-iemailsender"></a>Реализация Иемаилсендер

Чтобы реализовать `IEmailSender`, создайте *службы/емаилсендер. CS* с кодом, аналогичным следующему:

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a>Настройка запуска для поддержки электронной почты

Добавьте следующий код в метод `ConfigureServices` в файле *Startup.CS* :

* Добавьте `EmailSender` в качестве временной службы.
* Зарегистрируйте экземпляр конфигурации `AuthMessageSenderOptions`.

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a>Включение подтверждения учетной записи и восстановление пароля

Шаблон содержит код для подтверждения учетной записи и восстановления пароля. Найдите метод `OnPostAsync` в *области/удостоверение/страницы/учетная запись/Register. cshtml. CS*.

Запретите автоматический вход новых зарегистрированных пользователей с помощью комментария к следующей строке:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

Показан полный метод с выделенной измененной строкой:

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a>Регистрация, подтверждение электронной почты и сброс пароля

Запустите веб-приложение и протестируйте поток подтверждения учетной записи и восстановления пароля.

* Запустите приложение и зарегистрируйте нового пользователя
* Проверьте свою электронную почту на наличие ссылки для подтверждения учетной записи. Если вы не получаете электронное письмо, см. раздел [Отладка электронной почты](#debug) .
* Щелкните ссылку, чтобы подтвердить свою электронную почту.
* Выполните вход, используя свой адрес электронной почты и пароль.
* Выполните выход.

### <a name="view-the-manage-page"></a>Просмотр страницы "Управление"

Выберите имя пользователя в браузере: ![окно браузера с именем пользователя](accconfirm/_static/un.png)

Страница Управление отображается с выбранной вкладкой **профиль** . В **сообщении электронной почты** отображается флажок, указывающий, что сообщение электронной почты подтверждено.

### <a name="test-password-reset"></a>Проверка сброса пароля

* Если вы вошли в этот компьютер, выберите **выход**.
* Щелкните ссылку **войти** и выберите ссылку **забыли пароль?** .
* Введите адрес электронной почты, использованный для регистрации учетной записи.
* Отправляется сообщение электронной почты со ссылкой для сброса пароля. Проверьте свой адрес электронной почты и щелкните ссылку, чтобы сбросить пароль. После успешного сброса пароля можно выполнить вход с помощью электронной почты и нового пароля.

## <a name="change-email-and-activity-timeout"></a>Изменить время ожидания для электронной почты и активности

Время ожидания бездействия по умолчанию составляет 14 дней. Следующий код задает время ожидания простоя в 5 дней:

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a>Изменить все продолжительность существования маркеров защиты данных

Следующий код изменяет все маркеры защиты данных в течение времени ожидания до 3 часов:

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

Встроенные маркеры пользователей удостоверений (см. [AspNetCore/src/Identity/Extensions. Core/src/токеноптионс. CS](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) имеют [время ожидания в один день](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).

### <a name="change-the-email-token-lifespan"></a>Изменение срока существования маркера электронной почты

Срок существования токена [пользователя](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) по умолчанию — [один день](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs). В этом разделе показано, как изменить срок существования маркера электронной почты.

Добавьте настраиваемую [датапротектортокенпровидер\<тусер >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) и <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

Добавьте настраиваемый поставщик в контейнер службы:

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a>Подтверждение повторной отправки электронной почты

Также см. [эту проблему в GitHub](https://github.com/aspnet/AspNetCore/issues/5410).

<a name="debug"></a>

### <a name="debug-email"></a>Отладка электронной почты

Если вы не можете работать с электронной почтой:

* Установите точку останова в `EmailSender.Execute` для проверки вызова `SendGridClient.SendEmailAsync`.
* Создание [консольного приложения для отправки электронной почты](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) с помощью аналогичного кода для `EmailSender.Execute`.
* Ознакомьтесь со страницей [действие по электронной почте](https://sendgrid.com/docs/User_Guide/email_activity.html) .
* Проверьте папку спама.
* Попробуйте использовать другой псевдоним электронной почты в другом поставщике электронной почты (Microsoft, Yahoo, Gmail и т. д.).
* Попробуйте отправить их в другую учетную запись электронной почты.

В целях **безопасности** рекомендуется **не** использовать рабочие секреты в тестировании и разработке. При публикации приложения в Azure можно задать секреты SendGrid как параметры приложения на портале веб-приложения Azure. Система конфигурации предназначена для чтения разделов из переменных среды.

## <a name="combine-social-and-local-login-accounts"></a>Объединение социальных и местных учетных записей входа

Для выполнения этого раздела необходимо сначала включить внешний поставщик проверки подлинности. См. статью [Проверка подлинности в Facebook, Google и внешнем поставщике](xref:security/authentication/social/index).

Вы можете объединить локальные и социальные учетные записи, щелкнув ссылку на электронную почту. В следующей последовательности «RickAndMSFT@gmail.com» сначала создается как локальное имя входа. Тем не менее можно сначала создать учетную запись в качестве имени для входа в социальных сетях, а затем добавить локальное имя входа.

![Веб-приложение: RickAndMSFT@gmail.com пользователь прошел проверку подлинности](accconfirm/_static/rick.png)

Щелкните ссылку **Управление** . Обратите внимание на 0 внешних (социальных) имен, связанных с этой учетной записью.

![Управление представлением](accconfirm/_static/manage.png)

Щелкните ссылку на другую службу входа и примите запросы приложения. На следующем рисунке Facebook является внешним поставщиком проверки подлинности:

![Управление внешними именами входа Просмотр списка Facebook](accconfirm/_static/fb.png)

Две учетные записи были объединены. Вы можете войти с помощью любой учетной записи. Пользователям может потребоваться добавить локальные учетные записи на случай, если служба проверки подлинности в социальных сетях не работает, или более вероятно, что они потеряют доступ к учетной записи социальных сетей.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Включить подтверждение учетной записи после того, как у сайта есть пользователи

Включение подтверждения учетной записи на сайте с пользователями блокирует всех существующих пользователей. Существующие пользователи заблокированы, так как их учетные записи не подтверждены. Чтобы обойти существующую блокировку пользователей, используйте один из следующих подходов.

* Обновите базу данных, чтобы пометить все существующие пользователи как подтвержденные.
* Подтвердите существующих пользователей. Например, пакетная отправка сообщений электронной почты с ссылками для подтверждения.

::: moniker-end
