---
uid: config-builder
title: Построители конфигурации для ASP.NET
author: rick-anderson
description: Узнайте, как получить данные конфигурации из источников, отличных от web.config значения из внешних источников.
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: d5a3916c3df9778d14be80342bafbc3456a69a03
ms.sourcegitcommit: f2d14a7518d6ee51aca9333818ac1276e7b5ecef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2018
ms.locfileid: "50134539"
---
# <a name="configuration-builders-for-aspnet"></a><span data-ttu-id="b1af7-103">Построители конфигурации для ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b1af7-103">Configuration builders for ASP.NET</span></span>

<span data-ttu-id="b1af7-104">По [Molloy Стивен](https://github.com/StephenMolloy) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b1af7-104">By [Stephen Molloy](https://github.com/StephenMolloy) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b1af7-105">Построители конфигурации предоставляют механизм современных и гибкой разработки для приложений ASP.NET получить значения конфигурации из внешних источников.</span><span class="sxs-lookup"><span data-stu-id="b1af7-105">Configuration builders provide a modern and agile mechanism for ASP.NET apps to get configuration values from external sources.</span></span>

<span data-ttu-id="b1af7-106">Построители конфигурации:</span><span class="sxs-lookup"><span data-stu-id="b1af7-106">Configuration builders:</span></span>

* <span data-ttu-id="b1af7-107">Становятся доступными в .NET Framework 4.7.1 и более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="b1af7-107">Are available in .NET Framework 4.7.1 and later.</span></span>
* <span data-ttu-id="b1af7-108">Предоставляют гибкий механизм для чтения значения конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b1af7-108">Provide a flexible mechanism for reading configuration values.</span></span>
* <span data-ttu-id="b1af7-109">Устранить некоторые из основных потребностей приложений, когда они перемещаются в контейнер и облачную среду с фокусом ввода.</span><span class="sxs-lookup"><span data-stu-id="b1af7-109">Address some of the basic needs of apps as they move into a container and cloud focused environment.</span></span>
* <span data-ttu-id="b1af7-110">Можно использовать для улучшения защиты данных конфигурации путем рисования из источников, которые ранее недоступны (например, Azure Key Vault и переменные среды) в систему конфигурации .NET.</span><span class="sxs-lookup"><span data-stu-id="b1af7-110">Can be used to improve protection of configuration data by drawing from sources previously unavailable (for example, Azure Key Vault and environment variables) in the .NET configuration system.</span></span>

## <a name="keyvalue-configuration-builders"></a><span data-ttu-id="b1af7-111">Ключ/значение построителей конфигурации</span><span class="sxs-lookup"><span data-stu-id="b1af7-111">Key/value configuration builders</span></span>

<span data-ttu-id="b1af7-112">Распространенный сценарий, который может обрабатываться с помощью построителей конфигурации — предоставить механизм замены базовый ключ значение для разделов конфигурации, которые соответствуют шаблону ключ/значение.</span><span class="sxs-lookup"><span data-stu-id="b1af7-112">A common scenario that can be handled by configuration builders is to provide a basic key/value replacement mechanism for configuration sections that follow a key/value pattern.</span></span> <span data-ttu-id="b1af7-113">.NET Framework концепцию ConfigurationBuilders не ограничивается разделы конфигурации или шаблоны.</span><span class="sxs-lookup"><span data-stu-id="b1af7-113">The .NET Framework concept of ConfigurationBuilders is not limited to specific configuration sections or patterns.</span></span> <span data-ttu-id="b1af7-114">Тем не менее многие из построителей конфигурации в `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders)), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders) работать внутри шаблона ключ/значение.</span><span class="sxs-lookup"><span data-stu-id="b1af7-114">However, many of the configuration builders in `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders)), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders) work within the key/value pattern.</span></span>

## <a name="keyvalue-configuration-builders-settings"></a><span data-ttu-id="b1af7-115">Параметры построители конфигурации ключ значение</span><span class="sxs-lookup"><span data-stu-id="b1af7-115">Key/value configuration builders settings</span></span>

<span data-ttu-id="b1af7-116">Следующие параметры применяются к все построители конфигурации ключ/значение в `Microsoft.Configuration.ConfigurationBuilders`.</span><span class="sxs-lookup"><span data-stu-id="b1af7-116">The following settings apply to all key/value configuration builders in `Microsoft.Configuration.ConfigurationBuilders`.</span></span>

### <a name="mode"></a><span data-ttu-id="b1af7-117">Режим</span><span class="sxs-lookup"><span data-stu-id="b1af7-117">Mode</span></span>

<span data-ttu-id="b1af7-118">Построители конфигурации использование внешнего источника данных ключ/значение для заполнения элементов выбранный ключ значение конфигурации системы.</span><span class="sxs-lookup"><span data-stu-id="b1af7-118">The configuration builders use an external source of key/value information to populate selected key/value elements of the configuration system.</span></span> <span data-ttu-id="b1af7-119">В частности `<appSettings/>` и `<connectionStrings/>` разделы имеют специальную обработку из построителей конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b1af7-119">Specifically, the `<appSettings/>` and `<connectionStrings/>` sections receive special treatment from the configuration builders.</span></span> <span data-ttu-id="b1af7-120">Построители работать в трех режимах:</span><span class="sxs-lookup"><span data-stu-id="b1af7-120">The builders work in three modes:</span></span>

* <span data-ttu-id="b1af7-121">`Strict` — Режим по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b1af7-121">`Strict` - The default mode.</span></span> <span data-ttu-id="b1af7-122">В этом режиме построитель конфигурации работает только с хорошо известные ключ значение ориентированное и разделы конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b1af7-122">In this mode, the configuration builder only operates on well-known key/value-centric configuration sections.</span></span> <span data-ttu-id="b1af7-123">`Strict` режим перечисляет каждый ключ раздела.</span><span class="sxs-lookup"><span data-stu-id="b1af7-123">`Strict` mode enumerates each key in the section.</span></span> <span data-ttu-id="b1af7-124">Если найден соответствующий ключ из внешнего источника:</span><span class="sxs-lookup"><span data-stu-id="b1af7-124">If a matching key is found in the external source:</span></span>

   * <span data-ttu-id="b1af7-125">Построители конфигурации замените значение в результате раздел конфигурации со значением из внешнего источника.</span><span class="sxs-lookup"><span data-stu-id="b1af7-125">The configuration builders replace the value in the resulting configuration section with the value from the external source.</span></span>
* <span data-ttu-id="b1af7-126">`Greedy` — В этом режиме тесно связано с `Strict` режим.</span><span class="sxs-lookup"><span data-stu-id="b1af7-126">`Greedy` - This mode is closely related to `Strict` mode.</span></span> <span data-ttu-id="b1af7-127">Вместо того чтобы ограничения ключей, которые уже существуют в исходной конфигурации:</span><span class="sxs-lookup"><span data-stu-id="b1af7-127">Rather than being limited to keys that already exist in the original configuration:</span></span>

  * <span data-ttu-id="b1af7-128">Построители конфигурации добавляет все пары "ключ/значение" из внешнего источника, в результате раздел конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b1af7-128">The configuration builders adds all key/value pairs from the external source into the resulting configuration section.</span></span>

* <span data-ttu-id="b1af7-129">`Expand` -Обрабатывает необработанный код XML, прежде чем оно разбивается на объект раздела конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b1af7-129">`Expand` - Operates on the raw XML before it's parsed into a configuration section object.</span></span> <span data-ttu-id="b1af7-130">Он может рассматриваться как расширение слова токенов в строке.</span><span class="sxs-lookup"><span data-stu-id="b1af7-130">It can be thought of as an expansion of tokens in a string.</span></span> <span data-ttu-id="b1af7-131">Любую часть необработанные XML-строку, которая соответствует шаблону `${token}` является кандидатом на расширение токена.</span><span class="sxs-lookup"><span data-stu-id="b1af7-131">Any part of the raw XML string that matches the pattern `${token}` is a candidate for token expansion.</span></span> <span data-ttu-id="b1af7-132">При обнаружении без соответствующего значения из внешнего источника маркера не изменяется.</span><span class="sxs-lookup"><span data-stu-id="b1af7-132">If no corresponding value is found in the external source, then the token is not changed.</span></span> <span data-ttu-id="b1af7-133">Построители в этом режиме не ограничиваются `<appSettings/>` и `<connectionStrings/>` разделы.</span><span class="sxs-lookup"><span data-stu-id="b1af7-133">Builders in this mode are not limited to the `<appSettings/>` and `<connectionStrings/>` sections.</span></span>

<span data-ttu-id="b1af7-134">Следующая разметка из *web.config* позволяет [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) в `Strict` режиме:</span><span class="sxs-lookup"><span data-stu-id="b1af7-134">The following markup from *web.config* enables the [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` mode:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

<span data-ttu-id="b1af7-135">В следующем примере кода операций чтения `<appSettings/>` и `<connectionStrings/>` показан в предыдущем *web.config* файла:</span><span class="sxs-lookup"><span data-stu-id="b1af7-135">The following code reads the `<appSettings/>` and `<connectionStrings/>` shown in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

<span data-ttu-id="b1af7-136">Приведенный выше код будет присвоено значения свойств:</span><span class="sxs-lookup"><span data-stu-id="b1af7-136">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="b1af7-137">Значения в *web.config* файла, если ключи не заданы в переменных среды.</span><span class="sxs-lookup"><span data-stu-id="b1af7-137">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="b1af7-138">Значения среды переменной, если задать.</span><span class="sxs-lookup"><span data-stu-id="b1af7-138">The values of the environment variable, if set.</span></span>

<span data-ttu-id="b1af7-139">Например `ServiceID` будет содержать:</span><span class="sxs-lookup"><span data-stu-id="b1af7-139">For example, `ServiceID` will contain:</span></span>

* <span data-ttu-id="b1af7-140">«Value ServiceID из файла web.config», если переменная среды `ServiceID` не задано.</span><span class="sxs-lookup"><span data-stu-id="b1af7-140">"ServiceID value from web.config", if the environment variable `ServiceID` is not set.</span></span>
* <span data-ttu-id="b1af7-141">Значение `ServiceID` переменной, среды, если задать.</span><span class="sxs-lookup"><span data-stu-id="b1af7-141">The value of the `ServiceID` environment variable, if set.</span></span>

<span data-ttu-id="b1af7-142">На следующем рисунке показана `<appSettings/>` ключей/значений из предыдущей *web.config* набор файлов в редакторе среды:</span><span class="sxs-lookup"><span data-stu-id="b1af7-142">The following image shows the `<appSettings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![редактор среды](config-builder/static/env.png)

<span data-ttu-id="b1af7-144">Примечание: Может потребоваться закрыть и перезапустить Visual Studio, чтобы увидеть изменения в переменных среды.</span><span class="sxs-lookup"><span data-stu-id="b1af7-144">Note: You might need to exit and restart Visual Studio to see changes in environment variables.</span></span>

### <a name="prefix-handling"></a><span data-ttu-id="b1af7-145">Обработка префикс</span><span class="sxs-lookup"><span data-stu-id="b1af7-145">Prefix handling</span></span>

<span data-ttu-id="b1af7-146">Префиксы ключа можно упростить ключи параметров, так как:</span><span class="sxs-lookup"><span data-stu-id="b1af7-146">Key prefixes can simplify setting keys because:</span></span>

* <span data-ttu-id="b1af7-147">Конфигурации платформы .NET Framework — сложный и вложенных.</span><span class="sxs-lookup"><span data-stu-id="b1af7-147">The .NET Framework configuration is complex and nested.</span></span>
* <span data-ttu-id="b1af7-148">Источники внешних ключей и значений обычно представляют собой базовый и плоский по своей природе.</span><span class="sxs-lookup"><span data-stu-id="b1af7-148">External key/value sources are commonly basic and flat by nature.</span></span> <span data-ttu-id="b1af7-149">Например переменные среды не являются вложенными.</span><span class="sxs-lookup"><span data-stu-id="b1af7-149">For example, environment variables are not nested.</span></span>

<span data-ttu-id="b1af7-150">Использовать любой из следующих методов для вставки оба `<appSettings/>` и `<connectionStrings/>` в конфигурацию через переменные среды:</span><span class="sxs-lookup"><span data-stu-id="b1af7-150">Use any of the following approaches to inject both `<appSettings/>` and `<connectionStrings/>` into the configuration via environment variables:</span></span>

* <span data-ttu-id="b1af7-151">С помощью `EnvironmentConfigBuilder` в используемом по умолчанию `Strict` режим и соответствующие имена ключей в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b1af7-151">With the `EnvironmentConfigBuilder` in the default `Strict` mode and the appropriate key names in the configuration file.</span></span> <span data-ttu-id="b1af7-152">Предыдущий код и разметку принимает этот подход.</span><span class="sxs-lookup"><span data-stu-id="b1af7-152">The preceding code and markup takes this approach.</span></span> <span data-ttu-id="b1af7-153">При таком подходе вы можете **не** одинаковые имена ключей в обоих `<appSettings/>` и `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="b1af7-153">Using this approach you can **not** have identically named keys in both `<appSettings/>` and `<connectionStrings/>`.</span></span>
* <span data-ttu-id="b1af7-154">Используйте два `EnvironmentConfigBuilder`s в `Greedy` режиме с помощью различных префиксов и `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="b1af7-154">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes and `stripPrefix`.</span></span> <span data-ttu-id="b1af7-155">В этом случае приложение может считывать `<appSettings/>` и `<connectionStrings/>` без необходимости обновления файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b1af7-155">With this approach, the app can read `<appSettings/>` and `<connectionStrings/>` without needing to update the configuration file.</span></span> <span data-ttu-id="b1af7-156">Следующий раздел, [stripPrefix](#stripprefix), показано, как это сделать.</span><span class="sxs-lookup"><span data-stu-id="b1af7-156">The next section,  [stripPrefix](#stripprefix),  shows how to do this.</span></span>
* <span data-ttu-id="b1af7-157">Используйте два `EnvironmentConfigBuilder`s в `Greedy` режиме с помощью различных префиксов.</span><span class="sxs-lookup"><span data-stu-id="b1af7-157">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes.</span></span> <span data-ttu-id="b1af7-158">При таком подходе не может иметь повторяющиеся имена ключей, так как имена ключей должны отличаться по префиксу.</span><span class="sxs-lookup"><span data-stu-id="b1af7-158">With this approach you can't have duplicate key names as key names must differ by prefix.</span></span>  <span data-ttu-id="b1af7-159">Пример:</span><span class="sxs-lookup"><span data-stu-id="b1af7-159">For example:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

<span data-ttu-id="b1af7-160">С помощью предыдущего исправления можно использовать один и тот же источник неструктурированного ключ значение для заполнения конфигурации в двух разных разделах.</span><span class="sxs-lookup"><span data-stu-id="b1af7-160">With the preceding markup, the same flat key/value source can be used to populate configuration for two different sections.</span></span>

<span data-ttu-id="b1af7-161">На следующем рисунке показана `<appSettings/>` и `<connectionStrings/>` ключей/значений из предыдущей *web.config* набор файлов в редакторе среды:</span><span class="sxs-lookup"><span data-stu-id="b1af7-161">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![редактор среды](config-builder/static/prefix.png)

<span data-ttu-id="b1af7-163">В следующем примере кода операций чтения `<appSettings/>` и `<connectionStrings/>` ключи/значения, содержащиеся в предыдущем *web.config* файла:</span><span class="sxs-lookup"><span data-stu-id="b1af7-163">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

<span data-ttu-id="b1af7-164">Приведенный выше код будет присвоено значения свойств:</span><span class="sxs-lookup"><span data-stu-id="b1af7-164">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="b1af7-165">Значения в *web.config* файла, если ключи не заданы в переменных среды.</span><span class="sxs-lookup"><span data-stu-id="b1af7-165">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="b1af7-166">Значения среды переменной, если задать.</span><span class="sxs-lookup"><span data-stu-id="b1af7-166">The values of the environment variable, if set.</span></span>

<span data-ttu-id="b1af7-167">Например, с помощью предыдущего *web.config* файл, заданы ключи/значения на предыдущем рисунке редактор среды и предыдущий код, следующие значения:</span><span class="sxs-lookup"><span data-stu-id="b1af7-167">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="b1af7-168">Ключ</span><span class="sxs-lookup"><span data-stu-id="b1af7-168">Key</span></span>              | <span data-ttu-id="b1af7-169">Значение</span><span class="sxs-lookup"><span data-stu-id="b1af7-169">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="b1af7-170">AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="b1af7-170">AppSetting_ServiceID</span></span>           | <span data-ttu-id="b1af7-171">AppSetting_ServiceID из переменных среды</span><span class="sxs-lookup"><span data-stu-id="b1af7-171">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="b1af7-172">AppSetting_default</span><span class="sxs-lookup"><span data-stu-id="b1af7-172">AppSetting_default</span></span>            | <span data-ttu-id="b1af7-173">Значение AppSetting_default из env</span><span class="sxs-lookup"><span data-stu-id="b1af7-173">AppSetting_default value from env</span></span> |
|       <span data-ttu-id="b1af7-174">ConnStr_default</span><span class="sxs-lookup"><span data-stu-id="b1af7-174">ConnStr_default</span></span>         | <span data-ttu-id="b1af7-175">Val ConnStr_default из env</span><span class="sxs-lookup"><span data-stu-id="b1af7-175">ConnStr_default val from env</span></span>|

### <a name="stripprefix"></a><span data-ttu-id="b1af7-176">stripPrefix</span><span class="sxs-lookup"><span data-stu-id="b1af7-176">stripPrefix</span></span>

<span data-ttu-id="b1af7-177">`stripPrefix`: логическое значение, по умолчанию равно `false`.</span><span class="sxs-lookup"><span data-stu-id="b1af7-177">`stripPrefix`: boolean, defaults to `false`.</span></span> 

<span data-ttu-id="b1af7-178">Предыдущая разметка XML, разделяет параметры приложения из строки подключения, но требует все ключи в *web.config* файл для использования указанного префикса.</span><span class="sxs-lookup"><span data-stu-id="b1af7-178">The preceding XML markup separates app settings from connection strings but requires all the keys in the *web.config* file to use the specified prefix.</span></span> <span data-ttu-id="b1af7-179">Например, префикс `AppSetting` должны добавляться `ServiceID` ключ («AppSetting_ServiceID»).</span><span class="sxs-lookup"><span data-stu-id="b1af7-179">For example, the prefix `AppSetting` must be added to the `ServiceID` key ("AppSetting_ServiceID").</span></span> <span data-ttu-id="b1af7-180">С помощью `stripPrefix`, префикс не используется в *web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="b1af7-180">With `stripPrefix`, the prefix is not used in the *web.config* file.</span></span> <span data-ttu-id="b1af7-181">Префикс необходим в источнике построитель конфигурации (например, в среде.) Мы полагаем, что большинство разработчиков будет использовать `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="b1af7-181">The prefix is required in the configuration builder source (for example, in the environment.) We anticipate most developers will use `stripPrefix`.</span></span>

<span data-ttu-id="b1af7-182">Приложения обычно отбросить префикс.</span><span class="sxs-lookup"><span data-stu-id="b1af7-182">Applications typically strip off the prefix.</span></span> <span data-ttu-id="b1af7-183">Следующие *web.config* отделяет префикс:</span><span class="sxs-lookup"><span data-stu-id="b1af7-183">The following *web.config* strips the prefix:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

<span data-ttu-id="b1af7-184">В предыдущем *web.config* файл, `default` ключа входит в обе `<appSettings/>` и `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="b1af7-184">In the preceding *web.config* file, the `default` key is in both the `<appSettings/>` and `<connectionStrings/>`.</span></span>

<span data-ttu-id="b1af7-185">На следующем рисунке показана `<appSettings/>` и `<connectionStrings/>` ключей/значений из предыдущей *web.config* набор файлов в редакторе среды:</span><span class="sxs-lookup"><span data-stu-id="b1af7-185">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![редактор среды](config-builder/static/prefix.png)

<span data-ttu-id="b1af7-187">В следующем примере кода операций чтения `<appSettings/>` и `<connectionStrings/>` ключи/значения, содержащиеся в предыдущем *web.config* файла:</span><span class="sxs-lookup"><span data-stu-id="b1af7-187">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

<span data-ttu-id="b1af7-188">Приведенный выше код будет присвоено значения свойств:</span><span class="sxs-lookup"><span data-stu-id="b1af7-188">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="b1af7-189">Значения в *web.config* файла, если ключи не заданы в переменных среды.</span><span class="sxs-lookup"><span data-stu-id="b1af7-189">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="b1af7-190">Значения среды переменной, если задать.</span><span class="sxs-lookup"><span data-stu-id="b1af7-190">The values of the environment variable, if set.</span></span>

<span data-ttu-id="b1af7-191">Например, с помощью предыдущего *web.config* файл, заданы ключи/значения на предыдущем рисунке редактор среды и предыдущий код, следующие значения:</span><span class="sxs-lookup"><span data-stu-id="b1af7-191">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="b1af7-192">Ключ</span><span class="sxs-lookup"><span data-stu-id="b1af7-192">Key</span></span>              | <span data-ttu-id="b1af7-193">Значение</span><span class="sxs-lookup"><span data-stu-id="b1af7-193">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="b1af7-194">ServiceID</span><span class="sxs-lookup"><span data-stu-id="b1af7-194">ServiceID</span></span>           | <span data-ttu-id="b1af7-195">AppSetting_ServiceID из переменных среды</span><span class="sxs-lookup"><span data-stu-id="b1af7-195">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="b1af7-196">default</span><span class="sxs-lookup"><span data-stu-id="b1af7-196">default</span></span>            | <span data-ttu-id="b1af7-197">Значение AppSetting_default из env</span><span class="sxs-lookup"><span data-stu-id="b1af7-197">AppSetting_default value from env</span></span> |
|    <span data-ttu-id="b1af7-198">default</span><span class="sxs-lookup"><span data-stu-id="b1af7-198">default</span></span>         | <span data-ttu-id="b1af7-199">Val ConnStr_default из env</span><span class="sxs-lookup"><span data-stu-id="b1af7-199">ConnStr_default val from env</span></span>|

### <a name="tokenpattern"></a><span data-ttu-id="b1af7-200">tokenPattern</span><span class="sxs-lookup"><span data-stu-id="b1af7-200">tokenPattern</span></span>

<span data-ttu-id="b1af7-201">`tokenPattern`: Строка, значение по умолчанию — `@"\$\{(\w+)\}"`</span><span class="sxs-lookup"><span data-stu-id="b1af7-201">`tokenPattern`: String, defaults to `@"\$\{(\w+)\}"`</span></span>

<span data-ttu-id="b1af7-202">`Expand` Поведение из построителей осуществляет необработанный код XML для токенов, которые выглядят как `${token}`.</span><span class="sxs-lookup"><span data-stu-id="b1af7-202">The `Expand` behavior of the builders searches the raw XML for tokens that look like `${token}`.</span></span> <span data-ttu-id="b1af7-203">Поиск выполняется с помощью регулярного выражения по умолчанию `@"\$\{(\w+)\}"`.</span><span class="sxs-lookup"><span data-stu-id="b1af7-203">Searching is done with the default regular expression `@"\$\{(\w+)\}"`.</span></span> <span data-ttu-id="b1af7-204">Набор символов, соответствующий `\w` является более строгой, чем XML и множества источников конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b1af7-204">The set of characters that matches `\w` is more strict than XML and many configuration sources allow.</span></span> <span data-ttu-id="b1af7-205">Используйте `tokenPattern` при больше символов, чем `@"\$\{(\w+)\}"` требуются имя токена.</span><span class="sxs-lookup"><span data-stu-id="b1af7-205">Use `tokenPattern` when more characters than `@"\$\{(\w+)\}"` are required in the token name.</span></span>

<span data-ttu-id="b1af7-206">`tokenPattern`: Строка:</span><span class="sxs-lookup"><span data-stu-id="b1af7-206">`tokenPattern`: String:</span></span>

* <span data-ttu-id="b1af7-207">Разработчики могут изменить регулярных выражений, который используется для поиска маркера.</span><span class="sxs-lookup"><span data-stu-id="b1af7-207">Allows developers to change the regex that is used for token matching.</span></span>
* <span data-ttu-id="b1af7-208">Чтобы убедиться в том, что это регулярное выражение правильного формата, отличные от опасных не проверяются.</span><span class="sxs-lookup"><span data-stu-id="b1af7-208">No validation is done to make sure it is a well-formed, non-dangerous regex.</span></span>
* <span data-ttu-id="b1af7-209">Он должен содержать группы записи.</span><span class="sxs-lookup"><span data-stu-id="b1af7-209">It must contain a capture group.</span></span> <span data-ttu-id="b1af7-210">Всего регулярное выражение должно соответствовать весь токен.</span><span class="sxs-lookup"><span data-stu-id="b1af7-210">The entire regex must match the entire token.</span></span> <span data-ttu-id="b1af7-211">Первой записи должно быть имя токена, который ищется в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b1af7-211">The first capture must be the token name to look up in the configuration source.</span></span>

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a><span data-ttu-id="b1af7-212">Построители конфигурации в Microsoft.Configuration.ConfigurationBuilders</span><span class="sxs-lookup"><span data-stu-id="b1af7-212">Configuration builders in Microsoft.Configuration.ConfigurationBuilders</span></span>

### <a name="environmentconfigbuilder"></a><span data-ttu-id="b1af7-213">EnvironmentConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="b1af7-213">EnvironmentConfigBuilder</span></span>

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

<span data-ttu-id="b1af7-214">[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span><span class="sxs-lookup"><span data-stu-id="b1af7-214">The [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span></span>

* <span data-ttu-id="b1af7-215">Это самый простой из построителей конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b1af7-215">Is the simplest of the configuration builders.</span></span>
* <span data-ttu-id="b1af7-216">Считывает значения из среды.</span><span class="sxs-lookup"><span data-stu-id="b1af7-216">Reads values from the environment.</span></span>
* <span data-ttu-id="b1af7-217">Не поддерживает любые дополнительные параметры конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b1af7-217">Does not have any additional configuration options.</span></span>
* <span data-ttu-id="b1af7-218">`name` Значение атрибута является произвольным.</span><span class="sxs-lookup"><span data-stu-id="b1af7-218">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="b1af7-219">**Примечание:** в контейнерной среде Windows, переменные, заданные во время выполнения только вводится в среде процесса точки входа.</span><span class="sxs-lookup"><span data-stu-id="b1af7-219">**Note:** In a Windows container environment, variables set at run time are only injected into the EntryPoint process environment.</span></span> <span data-ttu-id="b1af7-220">Приложения, выполняются как службы или процесса не EntryPoint не получат эти переменные, если только они внедряются в противном случае через механизм в контейнере.</span><span class="sxs-lookup"><span data-stu-id="b1af7-220">Apps that run as a service or a non-EntryPoint process do not pick up these variables unless they are otherwise injected through a mechanism in the container.</span></span> <span data-ttu-id="b1af7-221">Для [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-контейнеров, текущая версия на основе [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) обрабатывает это в *DefaultAppPool* только.</span><span class="sxs-lookup"><span data-stu-id="b1af7-221">For [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-based containers, the current version of [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) handles this in the *DefaultAppPool* only.</span></span> <span data-ttu-id="b1af7-222">Другие варианты контейнера на основе Windows может потребоваться разработать собственный механизм внедрения для процессов без точки входа.</span><span class="sxs-lookup"><span data-stu-id="b1af7-222">Other Windows-based container variants may need to develop their own injection mechanism for non-EntryPoint processes.</span></span>

### <a name="usersecretsconfigbuilder"></a><span data-ttu-id="b1af7-223">UserSecretsConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="b1af7-223">UserSecretsConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="b1af7-224">Никогда не хранить пароли, строки соединения конфиденциальные или других конфиденциальных данных в исходном коде.</span><span class="sxs-lookup"><span data-stu-id="b1af7-224">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="b1af7-225">Не следует использовать секреты рабочей среды для разработки или тестирования.</span><span class="sxs-lookup"><span data-stu-id="b1af7-225">Production secrets should not be used for development or test.</span></span>

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

<span data-ttu-id="b1af7-226">Этот построитель конфигурации предоставляет аналогичную функцию [менеджера секретов ASP.NET Core](/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="b1af7-226">This configuration builder provides a feature similar to [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span></span>

<span data-ttu-id="b1af7-227">[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) можно использовать в проектах .NET Framework, но должен быть указан файл секретов.</span><span class="sxs-lookup"><span data-stu-id="b1af7-227">The [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) can be used in .NET Framework projects, but a secrets file must be specified.</span></span> <span data-ttu-id="b1af7-228">Кроме того, можно определить `UserSecretsId` свойство в проекте файл и создает файл необработанных секретов в нужное место для чтения.</span><span class="sxs-lookup"><span data-stu-id="b1af7-228">Alternatively, you can define the `UserSecretsId` property in the project file and create the raw secrets file in the correct location for reading.</span></span> <span data-ttu-id="b1af7-229">Чтобы сохранить внешние зависимости из проекта, файл с секретом является формате XML.</span><span class="sxs-lookup"><span data-stu-id="b1af7-229">To keep external dependencies out of your project, the secret file is XML formatted.</span></span> <span data-ttu-id="b1af7-230">Форматирование XML-кода — это деталь реализации, а формат не следует полагаться на.</span><span class="sxs-lookup"><span data-stu-id="b1af7-230">The XML formatting is an implementation detail, and the format should not be relied upon.</span></span> <span data-ttu-id="b1af7-231">Если вам нужно совместно использовать *secrets.json* файл с проектами .NET Core, рассмотрите возможность использования [SimpleJsonConfigBuilder](#simplejsonconfig).</span><span class="sxs-lookup"><span data-stu-id="b1af7-231">If you need to share a *secrets.json* file with .NET Core projects, consider using the [SimpleJsonConfigBuilder](#simplejsonconfig).</span></span> <span data-ttu-id="b1af7-232">`SimpleJsonConfigBuilder` Форматирования для .NET Core следует также рассматривать деталь реализации может быть изменена.</span><span class="sxs-lookup"><span data-stu-id="b1af7-232">The `SimpleJsonConfigBuilder` format for .NET Core should also be considered an implementation detail subject to change.</span></span>

<span data-ttu-id="b1af7-233">Атрибуты конфигурации для `UserSecretsConfigBuilder`:</span><span class="sxs-lookup"><span data-stu-id="b1af7-233">Configuration attributes for `UserSecretsConfigBuilder`:</span></span>

* <span data-ttu-id="b1af7-234">`userSecretsId` -Это предпочтительный способ идентификации секретные данные в XML-файл.</span><span class="sxs-lookup"><span data-stu-id="b1af7-234">`userSecretsId` - This is the preferred method for identifying an XML secrets file.</span></span> <span data-ttu-id="b1af7-235">Он работает аналогично в .NET Core, который использует `UserSecretsId` свойство для хранения этого идентификатора проекта.</span><span class="sxs-lookup"><span data-stu-id="b1af7-235">It works similar to .NET Core, which uses a `UserSecretsId` project property to store this identifier.</span></span> <span data-ttu-id="b1af7-236">Строка должна быть уникальной, он не обязательно должен быть GUID.</span><span class="sxs-lookup"><span data-stu-id="b1af7-236">The string must be unique, it doesn't need to be a GUID.</span></span> <span data-ttu-id="b1af7-237">С этим атрибутом `UserSecretsConfigBuilder` искать в хорошо известных локальное расположение (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) для файла секреты, принадлежащего этот идентификатор.</span><span class="sxs-lookup"><span data-stu-id="b1af7-237">With this attribute, the `UserSecretsConfigBuilder` look in a well-known local location (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) for a secrets file belonging to this identifier.</span></span>
* <span data-ttu-id="b1af7-238">`userSecretsFile` — Необязательный атрибут, указав файл, содержащий секреты.</span><span class="sxs-lookup"><span data-stu-id="b1af7-238">`userSecretsFile` - An optional attribute specifying the file containing the secrets.</span></span> <span data-ttu-id="b1af7-239">`~` Символ в начале может использоваться для ссылки на корневой каталог приложения.</span><span class="sxs-lookup"><span data-stu-id="b1af7-239">The `~` character can be used at the start to reference the application root.</span></span> <span data-ttu-id="b1af7-240">Либо данный атрибут или `userSecretsId` атрибут является обязательным.</span><span class="sxs-lookup"><span data-stu-id="b1af7-240">Either this attribute or the `userSecretsId` attribute is required.</span></span> <span data-ttu-id="b1af7-241">Если указаны оба, `userSecretsFile` имеет более высокий приоритет.</span><span class="sxs-lookup"><span data-stu-id="b1af7-241">If both are specified, `userSecretsFile` takes precedence.</span></span>
* <span data-ttu-id="b1af7-242">`optional`: логическое значение, значение по умолчанию `true` -исключение, если не удается найти файл секретов.</span><span class="sxs-lookup"><span data-stu-id="b1af7-242">`optional`: boolean, default value `true` - Prevents an exception if the secrets file cannot be found.</span></span> 
* <span data-ttu-id="b1af7-243">`name` Значение атрибута является произвольным.</span><span class="sxs-lookup"><span data-stu-id="b1af7-243">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="b1af7-244">Файл секретов имеет следующий формат:</span><span class="sxs-lookup"><span data-stu-id="b1af7-244">The secrets file has the following format:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a><span data-ttu-id="b1af7-245">AzureKeyVaultConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="b1af7-245">AzureKeyVaultConfigBuilder</span></span>

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

<span data-ttu-id="b1af7-246">[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) считывает значения, хранящиеся в [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span><span class="sxs-lookup"><span data-stu-id="b1af7-246">The [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) reads values stored in the [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

<span data-ttu-id="b1af7-247">`vaultName` — требуется либо (имя хранилища), либо URI в хранилище.</span><span class="sxs-lookup"><span data-stu-id="b1af7-247">`vaultName` is required (either the name of the vault or a URI to the vault).</span></span> <span data-ttu-id="b1af7-248">Другие атрибуты элемента управления, о каком хранилище для подключения к, но являются только в том случае, если приложение не выполняется в среде, которая работает с `Microsoft.Azure.Services.AppAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="b1af7-248">The other attributes allow control about which vault to connect to, but are only necessary if the application is not running in an environment that works with `Microsoft.Azure.Services.AppAuthentication`.</span></span> <span data-ttu-id="b1af7-249">Библиотека проверки подлинности служб Azure позволяет автоматически загружать сведения о соединении из среды выполнения, если это возможно.</span><span class="sxs-lookup"><span data-stu-id="b1af7-249">The Azure Services Authentication library is used to automatically pick up connection information from the execution environment if possible.</span></span> <span data-ttu-id="b1af7-250">Вы можете переопределить автоматически получить сведения о соединении, предоставляя строку подключения.</span><span class="sxs-lookup"><span data-stu-id="b1af7-250">You can override automatically pick up of connection information by providing a connection string.</span></span>

* <span data-ttu-id="b1af7-251">`vaultName` — Обязательный параметр, если `uri` в не предоставляется.</span><span class="sxs-lookup"><span data-stu-id="b1af7-251">`vaultName` - Required if `uri` in not provided.</span></span> <span data-ttu-id="b1af7-252">Задает имя хранилища в подписке Azure, из которого считывается пары "ключ значение".</span><span class="sxs-lookup"><span data-stu-id="b1af7-252">Specifies the name of the vault in your Azure subscription from which to read key/value pairs.</span></span>
* <span data-ttu-id="b1af7-253">`connectionString` -Строку подключения можно использовать с [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span><span class="sxs-lookup"><span data-stu-id="b1af7-253">`connectionString` - A connection string usable by [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span></span>
* <span data-ttu-id="b1af7-254">`uri` — Подключается к другим поставщикам хранилища ключей с указанным `uri` значение.</span><span class="sxs-lookup"><span data-stu-id="b1af7-254">`uri` - Connects to other Key Vault providers with the specified `uri` value.</span></span> <span data-ttu-id="b1af7-255">Если не указано, Azure (`vaultName`) является поставщиком хранилища.</span><span class="sxs-lookup"><span data-stu-id="b1af7-255">If not specified, Azure (`vaultName`) is the vault provider.</span></span>
* <span data-ttu-id="b1af7-256">`version` — Для azure Key Vault предоставляет возможность управления версиями для секретов.</span><span class="sxs-lookup"><span data-stu-id="b1af7-256">`version` - Azure Key Vault provides a versioning feature for secrets.</span></span> <span data-ttu-id="b1af7-257">Если `version` указан, сборщик только извлекает секретные данные, соответствующие этой версии.</span><span class="sxs-lookup"><span data-stu-id="b1af7-257">If `version` is specified, the builder only retrieves secrets matching this version.</span></span>
* <span data-ttu-id="b1af7-258">`preloadSecretNames` — По умолчанию, этот построитель querys **все** имен в хранилище ключей разделов, при инициализации.</span><span class="sxs-lookup"><span data-stu-id="b1af7-258">`preloadSecretNames` - By default, this builder querys **all** key names in the key vault when it is initialized.</span></span> <span data-ttu-id="b1af7-259">Чтобы предотвратить чтение всех значений ключа, установите значение `false`.</span><span class="sxs-lookup"><span data-stu-id="b1af7-259">To prevent reading all key values, set this attribute to `false`.</span></span> <span data-ttu-id="b1af7-260">Этому параметру `false` считывает секреты одному за раз.</span><span class="sxs-lookup"><span data-stu-id="b1af7-260">Setting this to `false` reads secrets one at a time.</span></span> <span data-ttu-id="b1af7-261">Чтение одного объекта одновременно может оказаться полезным, если хранилище позволяет доступа «Get», но не «список» доступ к операциям с секретами.</span><span class="sxs-lookup"><span data-stu-id="b1af7-261">Reading secrets one at a time can useful if the vault allows "Get" access but not "List" access.</span></span> <span data-ttu-id="b1af7-262">**Примечание:** при использовании `Greedy` режиме `preloadSecretNames` должно быть `true` (по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="b1af7-262">**Note:** When using `Greedy` mode, `preloadSecretNames` must be `true` (the default.)</span></span>

### <a name="keyperfileconfigbuilder"></a><span data-ttu-id="b1af7-263">KeyPerFileConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="b1af7-263">KeyPerFileConfigBuilder</span></span>

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

<span data-ttu-id="b1af7-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) — это построитель базовой конфигурации, который использует файлы каталога в качестве источника значений.</span><span class="sxs-lookup"><span data-stu-id="b1af7-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) is a basic configuration builder that uses a directory's files as a source of values.</span></span> <span data-ttu-id="b1af7-265">Имя файла является ключом, и содержимое значение.</span><span class="sxs-lookup"><span data-stu-id="b1af7-265">A file's name is the key, and the contents are the value.</span></span> <span data-ttu-id="b1af7-266">Этот построитель конфигурации можно использовать при работе в среде организованным контейнера.</span><span class="sxs-lookup"><span data-stu-id="b1af7-266">This configuration builder can be useful when running in an orchestrated container environment.</span></span> <span data-ttu-id="b1af7-267">Системы, такие как Docker Swarm и Kubernetes предоставляют `secrets` на их контейнеры организованным windows таким образом ключ каждого файла.</span><span class="sxs-lookup"><span data-stu-id="b1af7-267">Systems like Docker Swarm and Kubernetes provide `secrets` to their orchestrated windows containers in this key-per-file manner.</span></span>

<span data-ttu-id="b1af7-268">Сведения об атрибуте:</span><span class="sxs-lookup"><span data-stu-id="b1af7-268">Attribute details:</span></span>

* <span data-ttu-id="b1af7-269">`directoryPath` — обязательный.</span><span class="sxs-lookup"><span data-stu-id="b1af7-269">`directoryPath` - Required.</span></span> <span data-ttu-id="b1af7-270">Указывает путь для поиска значения.</span><span class="sxs-lookup"><span data-stu-id="b1af7-270">Specifies a path to look in for values.</span></span> <span data-ttu-id="b1af7-271">Docker для Windows, секреты хранятся в *C:\ProgramData\Docker\secrets* каталог по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b1af7-271">Docker for Windows secrets are stored in the *C:\ProgramData\Docker\secrets* directory by default.</span></span>
* <span data-ttu-id="b1af7-272">`ignorePrefix` -Исключаются файлы, которые начинаются с этого префикса.</span><span class="sxs-lookup"><span data-stu-id="b1af7-272">`ignorePrefix` - Files that start with this prefix are excluded.</span></span> <span data-ttu-id="b1af7-273">По умолчанию «ignore».</span><span class="sxs-lookup"><span data-stu-id="b1af7-273">Defaults to "ignore.".</span></span>
* <span data-ttu-id="b1af7-274">`keyDelimiter` -Значение по умолчанию — `null`.</span><span class="sxs-lookup"><span data-stu-id="b1af7-274">`keyDelimiter` - Default value is `null`.</span></span> <span data-ttu-id="b1af7-275">Если указано, построитель конфигурации проходит через несколько уровней в каталоге, создавая имена ключей, используя этот разделитель.</span><span class="sxs-lookup"><span data-stu-id="b1af7-275">If specified, the configuration builder traverses multiple levels of the directory, building up key names with this delimiter.</span></span> <span data-ttu-id="b1af7-276">Если это значение равно `null`, построитель конфигурации осуществляет только на верхнем уровне каталога.</span><span class="sxs-lookup"><span data-stu-id="b1af7-276">If this value is `null`, the configuration builder only looks at the top level of the directory.</span></span>
* <span data-ttu-id="b1af7-277">`optional` -Значение по умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="b1af7-277">`optional` -  Default value is `false`.</span></span> <span data-ttu-id="b1af7-278">Указывает, должен ли построитель конфигурации вызвать ошибки, если исходный каталог не существует.</span><span class="sxs-lookup"><span data-stu-id="b1af7-278">Specifies whether the configuration builder should cause errors if the source directory doesn't exist.</span></span>

### <a name="simplejsonconfigbuilder"></a><span data-ttu-id="b1af7-279">SimpleJsonConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="b1af7-279">SimpleJsonConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="b1af7-280">Никогда не хранить пароли, строки соединения конфиденциальные или других конфиденциальных данных в исходном коде.</span><span class="sxs-lookup"><span data-stu-id="b1af7-280">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="b1af7-281">Не следует использовать секреты рабочей среды для разработки или тестирования.</span><span class="sxs-lookup"><span data-stu-id="b1af7-281">Production secrets should not be used for development or test.</span></span>

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

<span data-ttu-id="b1af7-282">Проекты .NET core часто используется JSON-файлов конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b1af7-282">.NET Core projects frequently use JSON files for configuration.</span></span> <span data-ttu-id="b1af7-283">[SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder позволяет .NET Core JSON-файлов для использования в .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b1af7-283">The [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder allows .NET Core JSON files to be used in the .NET Framework.</span></span> <span data-ttu-id="b1af7-284">Этот построитель конфигурации — это обеспечивает основные сопоставление из источника неструктурированного ключ/значение в области определенной ключ значение конфигурации .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b1af7-284">This configuration builder is provides a basic mapping from a flat key/value source into specific key/value areas of .NET Framework configuration.</span></span> <span data-ttu-id="b1af7-285">Делает этот конфигурации **не** обеспечения иерархической конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b1af7-285">This configuration builder does **not** provide for hierarchical configurations.</span></span> <span data-ttu-id="b1af7-286">Резервный файл JSON аналогична словарь, не сложные иерархические объект.</span><span class="sxs-lookup"><span data-stu-id="b1af7-286">The JSON backing file is similar to a dictionary, not a complex hierarchical object.</span></span> <span data-ttu-id="b1af7-287">Можно использовать многоуровневые иерархические файл.</span><span class="sxs-lookup"><span data-stu-id="b1af7-287">A multi-level hierarchical file can be used.</span></span> <span data-ttu-id="b1af7-288">Этот поставщик `flatten`s глубина путем добавления имени свойства на каждом уровне с помощью `:` как разделитель.</span><span class="sxs-lookup"><span data-stu-id="b1af7-288">This provider `flatten`s the depth by appending the property name at each level using `:` as a delimiter.</span></span>

<span data-ttu-id="b1af7-289">Сведения об атрибуте:</span><span class="sxs-lookup"><span data-stu-id="b1af7-289">Attribute details:</span></span>

* <span data-ttu-id="b1af7-290">`jsonFile` — обязательный.</span><span class="sxs-lookup"><span data-stu-id="b1af7-290">`jsonFile` - Required.</span></span> <span data-ttu-id="b1af7-291">Задает файл для чтения из JSON.</span><span class="sxs-lookup"><span data-stu-id="b1af7-291">Specifies the JSON file to read from.</span></span> <span data-ttu-id="b1af7-292">`~` Символ в начале может использоваться для ссылки на корня приложения.</span><span class="sxs-lookup"><span data-stu-id="b1af7-292">The `~` character can be used at the start to reference the app root.</span></span>
* <span data-ttu-id="b1af7-293">`optional` -Логическое, значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="b1af7-293">`optional` - Boolean,  default value is `true`.</span></span> <span data-ttu-id="b1af7-294">Предотвращает создание исключений, если не удается найти файл JSON.</span><span class="sxs-lookup"><span data-stu-id="b1af7-294">Prevents throwing exceptions if the JSON file cannot be found.</span></span>
* <span data-ttu-id="b1af7-295">`jsonMode` - `[Flat|Sectional]`.</span><span class="sxs-lookup"><span data-stu-id="b1af7-295">`jsonMode` - `[Flat|Sectional]`.</span></span> <span data-ttu-id="b1af7-296">Значение по умолчанию — `Flat`.</span><span class="sxs-lookup"><span data-stu-id="b1af7-296">`Flat` is the default.</span></span> <span data-ttu-id="b1af7-297">Когда `jsonMode` является `Flat`, JSON-файл является источником одной неструктурированный ключ значение.</span><span class="sxs-lookup"><span data-stu-id="b1af7-297">When `jsonMode` is `Flat`, the JSON file is a single flat key/value source.</span></span> <span data-ttu-id="b1af7-298">`EnvironmentConfigBuilder` И `AzureKeyVaultConfigBuilder` также являются источники неструктурированных один ключ значение.</span><span class="sxs-lookup"><span data-stu-id="b1af7-298">The `EnvironmentConfigBuilder` and `AzureKeyVaultConfigBuilder` are also single flat key/value sources.</span></span> <span data-ttu-id="b1af7-299">Когда `SimpleJsonConfigBuilder` настраивается в `Sectional` режиме:</span><span class="sxs-lookup"><span data-stu-id="b1af7-299">When the `SimpleJsonConfigBuilder` is configured in `Sectional` mode:</span></span>

  * <span data-ttu-id="b1af7-300">JSON-файл делится только на верхнем уровне несколько словарей.</span><span class="sxs-lookup"><span data-stu-id="b1af7-300">The JSON file is conceptually divided just at the top level into multiple dictionaries.</span></span>
  * <span data-ttu-id="b1af7-301">Каждый из словари применяется только в раздел конфигурации, который соответствует имени свойства верхнего уровня, присоединенными к ним.</span><span class="sxs-lookup"><span data-stu-id="b1af7-301">Each of the dictionaries is only applied to the configuration section that matches the top-level property name attached to them.</span></span> <span data-ttu-id="b1af7-302">Пример:</span><span class="sxs-lookup"><span data-stu-id="b1af7-302">For example:</span></span>

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a><span data-ttu-id="b1af7-303">Реализация построителя конфигурации пользовательский ключ значение</span><span class="sxs-lookup"><span data-stu-id="b1af7-303">Implementing a custom key/value configuration builder</span></span>

<span data-ttu-id="b1af7-304">Если построителей конфигурации не соответствуют требованиям, можно создать новый.</span><span class="sxs-lookup"><span data-stu-id="b1af7-304">If the configuration builders don't meet your needs, you can write a custom one.</span></span> <span data-ttu-id="b1af7-305">`KeyValueConfigBuilder` Базовый класс обрабатывает режимы подстановки и большинство проблем префикс.</span><span class="sxs-lookup"><span data-stu-id="b1af7-305">The `KeyValueConfigBuilder` base class handles substitution modes and most prefix concerns.</span></span> <span data-ttu-id="b1af7-306">Требуются только проекте реализации:</span><span class="sxs-lookup"><span data-stu-id="b1af7-306">An implementing project need only:</span></span>

* <span data-ttu-id="b1af7-307">Наследовать от базового класса и реализации основных источника пар "ключ значение" через `GetValue` и `GetAllValues`:</span><span class="sxs-lookup"><span data-stu-id="b1af7-307">Inherit from the base class and implement a basic source of key/value pairs through the `GetValue` and `GetAllValues`:</span></span>
* <span data-ttu-id="b1af7-308">Добавить [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) в проект.</span><span class="sxs-lookup"><span data-stu-id="b1af7-308">Add the [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) to the project.</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

<span data-ttu-id="b1af7-309">`KeyValueConfigBuilder` Базовый класс предоставляет большую часть рабочих и согласованное поведение через ключ/значение построителей конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b1af7-309">The `KeyValueConfigBuilder` base class provides much of the work and consistent behavior across key/value configuration builders.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1af7-310">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b1af7-310">Additional resources</span></span>

* [<span data-ttu-id="b1af7-311">Репозиторий GitHub построители конфигурации</span><span class="sxs-lookup"><span data-stu-id="b1af7-311">Configuration Builders GitHub repository</span></span>](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [<span data-ttu-id="b1af7-312">Служба служба проверки подлинности в хранилище ключей Azure с помощью .NET</span><span class="sxs-lookup"><span data-stu-id="b1af7-312">Service-to-service authentication to Azure Key Vault using .NET</span></span>](/azure/key-vault/service-to-service-authentication#connection-string-support)
