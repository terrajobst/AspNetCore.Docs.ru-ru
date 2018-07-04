---
uid: mvc/overview/getting-started/mvc-learning-sequence
title: MVC рекомендуемые учебники и статьи | Документация Майкрософт
author: Rick-Anderson
description: Эта страница содержит ссылки на учебники по ASP.NET MVC и предлагаемые последовательность, которая будет выполнять их.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 8513a57a-2d45-4d6b-881c-15a01c5cbb1c
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/mvc-learning-sequence
msc.type: authoredcontent
ms.openlocfilehash: fd972549fd4c1f46f5bdeafbb466baab7a21e9df
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363993"
---
<a name="mvc-recommended-tutorials-and-articles"></a>Рекомендуемые учебники и статьи MVC
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

<a id="pwd"></a>
## <a name="getting-started"></a>Начало работы

- [Приступая к работе с ASP.NET MVC 5](introduction/getting-started.md) This 11 частей хорошо подходит для запуска.
- [Pluralsight ASP.NET MVC 5 Fundamentals](https://pluralsight.com/training/Player?author=scott-allen&amp;name=aspdotnet-mvc5-fundamentals-m1-introduction&amp;mode=live&amp;clip=0&amp;course=aspdotnet-mvc5-fundamentals) (видеокурс)
- [Введение в ASP.NET MVC](https://www.microsoftvirtualacademy.com/training-courses/introduction-to-asp-net-mvc) Джон Гэллоуэй, Кристофер Харрисон
- [Жизненный цикл приложения ASP.NET MVC 5](lifecycle-of-an-aspnet-mvc-5-application.md) PDF-документ, что диаграммы жизненного цикла приложения ASP.NET MVC 5.

<a id="con"></a>
## <a name="working-with-data"></a>Работа с данными

- [Начало работы с EF 6 Code First с помощью MVC 5](getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) том Дайкстра великолепную серии углубляется в EF.

<a id="wj"></a>
## <a name="security"></a>Безопасность

- [Создание приложения ASP.NET MVC с проверкой подлинности и база данных SQL и развертывание в Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) этот популярный руководстве описывается создание простого приложения и добавление членства и ролей.
- [Создание приложения ASP.NET MVC 5 с Facebook, Twitter, LinkedIn и Google OAuth2 Sign-on](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) этом руководстве показано, как создавать веб-приложения ASP.NET MVC 5, которая позволяет пользователям входить в систему с помощью OAuth 2.0 с учетными данными из внешней проверки подлинности Поставщик, например Facebook, Twitter, LinkedIn, Microsoft или Google.
- [Создание безопасного веб-приложения ASP.NET MVC 5 со входом, по электронной почте подтверждение и сброс пароля](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) сначала в серии, посвященной удостоверений, включает в себя код, чтобы [повторно отправить ссылку для подтверждения](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md#rsend).
- [Приложения ASP.NET MVC 5 с помощью SMS и электронной почты двухфакторной проверки подлинности](../security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) второй на серии удостоверений.
- [Рекомендации по развертыванию паролей и других конфиденциальных данных в ASP.NET и службу приложений Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)
- [Двухфакторная проверка подлинности с помощью SMS и электронной почты с ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) `isPersistent` и файл cookie безопасности кода, для которых пользователю необходимо иметь учетную запись проверенные электронной почты, прежде чем они могут войти на, как SignInManager проверяет наличие 2FA требование и многое другое.
- [Подтверждение учетной записи и восстановление пароля в ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) содержит сведения о удостоверение не найдено в [создать безопасное веб-приложение ASP.NET MVC 5 со входом, по электронной почте подтверждение и сброс пароля](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) : их let Пользователи сбрасывать забытый пароль.

<a id="da"></a>
## <a name="azure"></a>Azure

- [Создание веб-приложение ASP.NET в Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/) короткие и простые руководства, развертываемых в Azure.
- [Создание приложения ASP.NET MVC с проверкой подлинности и база данных SQL и развертывание в Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)

<a id="perf"></a>
## <a name="performance-and-debugging"></a>Производительности и отладки

- [Профилирование и отладка приложения ASP.NET MVC с помощью Glimpse](../performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse.md)
