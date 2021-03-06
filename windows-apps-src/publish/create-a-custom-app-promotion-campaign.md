---
description: Помимо создания рекламной кампании для вашего приложения, которая будет работать в приложениях для Windows, вы также можете продвигать свое приложение по другим каналам.
title: Создание пользовательской кампании по продвижению приложения
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, пользовательский, приложение, продвижение, кампания
ms.localizationpriority: medium
ms.openlocfilehash: 407a34294155e688e672db392c262e1607c01a39
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653739"
---
# <a name="create-a-custom-app-promotion-campaign"></a>Создание пользовательской кампании по продвижению приложения

Помимо создания [рекламной кампании для вашего приложения](create-an-ad-campaign-for-your-app.md), которая будет работать в приложениях для Windows, вы также можете продвигать свое приложение по другим каналам. Например, вы можете продвигать свое приложение, используя стороннего поставщика услуг по маркетингу приложений или публиковать ссылки на ваше приложение на сайтах социальных сетей. Эти действия называются *пользовательскими кампаниями*.

Если вы используете пользовательские кампании для вашего приложения, вы можете отслеживать относительную эффективность каждой кампании, создав разные URL-адреса приложения для каждой пользовательской кампании, где каждый URL-адрес содержит собственный *идентификатор кампании*. Когда клиент под управлением Windows 10 щелкает URL-адрес, который содержит идентификатор кампании, Microsoft связывает нажатием с соответствующей пользовательских кампании и делает их доступными для вас в [центра партнеров](https://partner.microsoft.com/dashboard).

> [!IMPORTANT]
> Эти данные будет отслеживаться только для клиентов в Windows 10. Пользователи, использующие другие операционные системы, могут по-прежнему переходить по ссылке на страницу с описанием вашего приложения, но данные об этих действиях пользователя не будут включены.

Существует два основных типа данных, связанных с пользовательскими кампаниями: *просмотры страницы* с описанием вашего приложения в Store и *конверсии*. Конверсия — это приобретение приложения, к которому привел просмотр пользователем страницы с описанием вашего приложения в Магазине при переходе по URL-адресу, содержащему идентификатор пользовательской кампании. Дополнительные сведения о конверсиях см. в подразделе [Общие сведения о том, какие приобретения считаются конверсиями](#understanding-how-acquisitions-qualify-as-conversions) далее в этом разделе.

Вы можете получить данные о производительности пользовательской кампании для вашего приложения следующими способами:

* Можно просмотреть данные о просмотров страниц и преобразования для приложения или надстройки с **просмотры страниц приложения и преобразования по Идентификатору кампании** и **всего преобразования кампании** диаграммы в [ввод в эксплуатацию отчет](acquisitions-report.md).
* Если ваше приложение является приложением универсальной платформы Windows (UWP), для него можно применять API в пакете Windows SDK, позволяющий программным образом получать идентификатор пользовательской кампании, которая привела к конверсии.

## <a name="example-custom-campaign-scenario"></a>Примерный сценарий пользовательской кампании

Представим разработчика игр, который только что создал новую игру и хотел бы продвигать ее среди пользователей своих текущих игр. Она публикует объявление о выходе новой игры на своей странице в Facebook и указывает ссылку на описание игры в Store. Многие из пользователей, играющих в ее игры, также читают ее в Twitter, поэтому она также публикует объявление в Twitter, указывая ссылку на описание игры в Store.

Чтобы отслеживать успех каждого из этих каналов продвижений, разработчик создает два варианта URL-адреса для описания игры в Store.

* URL-адрес, который она получит на своей странице Facebook включает идентификатор пользовательской кампании `my-facebook-campaign`

* URL-адрес, который она будет публиковать в Twitter включает идентификатор пользовательской кампании `my-twitter-campaign`

Ее читатели в Facebook и Twitter щелкают URL-ссылки, а Майкрософт отслеживает каждый щелчок и сопоставляет его с соответствующей пользовательской кампанией. Далее соответствующие приобретения игры и любые покупки надстроек сопоставляются с пользовательской кампанией и регистрируются как конверсии.

<span id="conversions" />

## <a name="understanding-how-acquisitions-qualify-as-conversions"></a>Общие сведения о том, какие приобретения считаются конверсиями

*Конверсия* пользовательской компании — это приобретение приложения, к которому привел щелчок по URL-ссылке, продвигаемой через пользовательскую кампанию. Существуют различные сценарии для уточнения как преобразование для **просмотры страниц приложения и преобразования по Идентификатору кампании** и **всего преобразования кампании** диаграммы в [отчет о приобретениях ](acquisitions-report.md) и предназначен для уточнения как преобразование для [программно извлечение идентификатора кампании](#programmatically).

### <a name="qualifying-conversions-in-the-acquisitions-report"></a>Уточнение преобразования в отчете ввод в эксплуатацию

Следующие сценарии считаются конверсией на диаграммах **Число просмотров и конверсий страницы приложения по идентификатору кампании** и **Общее количество конверсий в ходе кампании** в [Отчете о приобретениях ](acquisitions-report.md).

* Пользователь *с актуальной учетной записью Майкрософт (или без нее)* щелкает URL-адрес приложения, содержащий идентификатор пользовательской кампании, и перенаправляется на страницу с описанием приложения в Магазине. Затем тот же пользователь приобретет приложение в течение 24 часов после первого щелчка URL-адреса Microsoft Store с идентификатором пользовательской кампании.

* Если клиент приобретает приложение на другом устройстве, отличном от того, на котором он щелкнул URL-адрес Магазина Windows с идентификатором пользовательской кампании, конверсия будет учитываться, только если клиент при щелчке по URL-адресу предварительно выполнил вход с помощью той же учетной записи Майкрософт.

> [!NOTE]
> Для приобретений приложения, которые считаются конверсиями в пользовательской кампании, все покупки надстроек из этого приложения также считаются конверсиями в этой же пользовательской кампании.

### <a name="qualifying-conversions-when-programmatically-retrieving-the-campaign-id"></a>Правила подсчета конверсий при программном получении идентификатора кампании

Чтобы установка считалась конверсией при программном получении идентификатора кампании, связанного с приложением, должны выполняться следующие условия:

* На устройстве под управлением **Windows 10 версии 1607 или более поздней**: Клиент (ли со знаком распознанных учетную запись Майкрософт или нет), щелкает URL-адрес, который содержит идентификатор пользовательской кампании и перенаправляется на страницу Store для приложения. Клиент получает приложение во время просмотра описания в Магазине в результате щелчка по URL-адресу.

* На устройстве под управлением **Windows 10 версии 1511 или более ранней версии**: (Который должен быть подписан распознанных учетную запись Майкрософт) клиент щелкает URL-адрес, который содержит идентификатор пользовательской кампании и перенаправляется на страницу Store для приложения. Клиент получает приложение во время просмотра описания в Магазине в результате щелчка по URL-адресу. В этих версиях Windows 10 пользователю необходимо выполнить вход с помощью актуальной учетной записи Майкрософт, чтобы приобретение считалась конверсией при программном получении идентификатора кампании.

> [!NOTE]
> Если пользователь покидает страницу с описанием в Магазине и затем возвращается на нее (на том же устройстве или на другом устройстве, если он выполнил вход с помощью учетной записи Майкрософт) в течение 24 часов и приобретает приложение, это **будет** считаться конверсией на диаграммах **Число просмотров и конверсий страницы приложения по идентификатору кампании** и **Общее количество конверсий в ходе кампании** в [Отчете о приобретениях](acquisitions-report.md). Однако это **не будет** считаться конверсией при программном получении идентификатора кампании.

## <a name="embed-a-custom-campaign-id-to-your-apps-microsoft-store-page-url"></a>Встраивание идентификатора пользовательской кампании в URL-адрес страницы Microsoft Store с вашим приложением

Чтобы создать URL-адрес страницы Microsoft Store для вашего приложения с идентификатором пользовательской кампании, выполните следующие действия:

1.  Создайте строку идентификатора для вашей пользовательской кампании. Эта строка может содержать до 100 символов, хотя мы рекомендуем определять короткие идентификаторы кампании.

   > [!NOTE]
   > Строка с идентификатором кампании может быть видна другим разработчикам, когда они просматривают [Отчет о приобретениях](acquisitions-report.md) для своих приложений. Это может произойти, когда пользователь щелкает ваш идентификатор пользовательской кампании для входа в Магазин и приобретает приложение другого разработчика в ходе того же сеанса, таким образом присваивая эту конверсию вашему идентификатору компании. Этот разработчик будет видеть, сколько конверсий его собственного приложения стали следствием начального щелчка по вашему идентификатору кампании, включая имя идентификатора кампании, однако он не будет видеть никакие данные о том, сколько пользователей купили ваши собственные приложения (или приложения от любых других разработчиков) после щелчка по вашему идентификатору кампании.

2.  Получите ссылку на страницу описания приложения в Store в формате HTML или протокола.

    * Используйте URL-ссылку в формате HTML, если вы хотите, чтобы пользователи переходили на страницу с описанием вашего приложения в Магазине, используя браузер в любой операционной системе. На устройствах с Windows приложение Магазина также запустит и отобразит страницу вашего приложения. Этот URL-адрес имеет формат **`https://www.microsoft.com/store/apps/*your app ID*`**. Например, URL-адрес в формате HTML для Skype — `https://www.microsoft.com/store/apps/9wzdncrfj364`. Вы можете найти этот URL-адрес на странице [Удостоверение приложения](view-app-identity-details.md#link-to-your-apps-listing).

    * Используйте формат протокола, если вы продвигаете свое приложение из других приложений для Windows, работающих на устройстве или компьютере с установленным приложением UWP, или когда вы знаете, что ваши клиенты используют устройство, поддерживающее Microsoft Store. Эта ссылка будет перенаправлять непосредственно на страницу с описанием вашего приложения в Магазине без открытия браузера. Этот URL-адрес имеет формат **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**. Например, URL-адрес протокола в Skype — `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`.

3.  Добавляйте следующую строку в конец URL-адреса для вашего приложения:

    * Для URL-адреса в формате HTML добавьте строку **`?cid=*my custom campaign ID*`**. Например, если Скайп вводит идентификатор кампании, со значением **пользовательских\_кампании**, новый URL-адрес, включая идентификатор останется кампании: `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`.

    * Для URL-адресов в формате протокола добавьте строку **`&cid=*my custom campaign ID*`**. Например, если Скайп вводит идентификатор кампании, со значением **пользовательских\_кампании**, нового протокола URL, включая идентификатор останется кампании: `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`.

<span id="programmatically" />

## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>Программное получение идентификатора пользовательской кампании для приложения

Если ваше приложение является приложением UWP, вы можете программным образом получить идентификатор пользовательской кампании, связанный с приобретением приложения, используя API в пакете Windows SDK. Эти API позволяют использовать многие сценарии аналитики и монетизации. Например, вы можете узнать, получил ли текущий пользователь ваше приложение, найдя его через кампанию в Facebook, а затем настроить соответствующим образом взаимодействие с пользователем в своем приложении. Кроме того, если вы используете стороннего поставщика услуг маркетинга приложений, вы можете предоставить ему эти данные.

Эти API возвратят строку идентификатора кампании, только если пользователь щелкнул ваш URL-адрес со встроенным идентификатором кампании, просмотрел страницу Microsoft Store для вашего приложения, а затем приобрел ваше приложение, не закрывая страницу с описанием в Store. Если пользователь покидает страницу, а позже возвращается на нее и приобретает приложение, это не будет [считаться конверсией](#conversions) при использовании API.

Вы можете использовать разные API в зависимости от версии Windows 10, для которой предназначено ваше приложение:

* Windows 10 версии 1607 или более поздней версии: Используйте [ **StoreContext** ](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) в класс **Windows.Services.Store** пространства имен. При использовании этого API вы можете получить идентификаторы пользовательской кампании для любых [соответствующих приобретений](#conversions) независимо от того, выполнил ли пользователь вход с помощью актуальной учетной записи Майкрософт.

* Windows 10 версии 1511 или более ранней версии: Используйте [ **CurrentApp** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) в класс **Windows.ApplicationModel.Store** пространства имен. При использовании этого API вы можете получить идентификаторы пользовательской кампании для [соответствующих приобретений](#conversions), только если пользователь выполнил вход с помощью актуальной учетной записи Майкрософт.

> [!NOTE]
> Хотя пространство имен **Windows.ApplicationModel.Store** доступно во всех версиях Windows 10, рекомендуется использовать API в пространстве имен **Windows.Services.Store**, если ваше приложение предназначено для Windows 10 версии 1607 или выше. Дополнительные сведения о различиях между этими пространствами имен см. в разделе [Покупки из приложения и пробные версии](../monetize/in-app-purchases-and-trials.md#choose-namespace). В следующем примере кода показано, как структурировать код для использования обоих API в одном проекте.

### <a name="code-example"></a>Пример кода

В следующем примере кода показано, как получить идентификатор пользовательской кампании. В этом примере используются оба набора API в пространствах имен **Windows.Services.Store** и **Windows.ApplicationModel.Store**, используя [адаптивный код версии](../debug-test-perf/version-adaptive-code.md). При выполнении описанного далее процесса ваш код может выполняться в Windows 10 любой версии. Чтобы использовать этот код, целевая версия ОС вашего проекта должна быть **Windows 10 Anniversary Edition (10.0; сборка 14394)** или более поздней версии, хотя минимальная версия ОС может быть более ранней.

``` csharp
// This example assumes the code file has using statements for
// System.Linq, System.Threading.Tasks, Windows.Data.Json,
// and Windows.Services.Store.
public async Task<string> GetCampaignId()
{
    // Use APIs in the Windows.Services.Store namespace if they are available
    // (the app is running on a device with Windows 10, version 1607, or later).
    if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent(
         "Windows.Services.Store.StoreContext"))
    {
        StoreContext context = StoreContext.GetDefault();

        // Try to get the campaign ID for users with a recognized Microsoft account.
        StoreProductResult result = await context.GetStoreProductForCurrentAppAsync();
        if (result.Product != null)
        {
            StoreSku sku = result.Product.Skus.FirstOrDefault(s => s.IsInUserCollection);

            if (sku != null)
            {
                return sku.CollectionData.CampaignId;
            }
        }

        // Try to get the campaign ID from the license data for users without a
        // recognized Microsoft account.
        StoreAppLicense license = await context.GetAppLicenseAsync();
        JsonObject json = JsonObject.Parse(license.ExtendedJsonData);
        if (json.ContainsKey("customPolicyField1"))
        {
            return json["customPolicyField1"].GetString();
        }

        // No campaign ID was found.
        return String.Empty;
    }
    // Fall back to using APIs in the Windows.ApplicationModel.Store namespace instead
    // (the app is running on a device with Windows 10, version 1577, or earlier).
    else
    {
#if DEBUG
        return await Windows.ApplicationModel.Store.CurrentAppSimulator.GetAppPurchaseCampaignIdAsync();
#else
        return await Windows.ApplicationModel.Store.CurrentApp.GetAppPurchaseCampaignIdAsync() ;
#endif
    }
}
```

Этот код выполняет указанные ниже функции.

1. Во-первых, он проверяет, доступен ли класс [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) в пространстве имен **Windows.Services.Store** на текущем устройстве (это означает, что устройство работает под управлением Windows 10 версии 1607 или выше). Если это так, код использует данный класс.

2. Затем он пытается получить идентификатор пользовательской кампании для случая, когда у текущего пользователя есть актуальная учетная запись Майкрософт. Для этого код получает объект [**StoreSku**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreSku), представляющий текущий SKU приложения, а затем получает доступ к свойству [**CampaignId**](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.CampaignId), чтобы получить идентификатор кампании, если он доступен.
3. Код пытается получить идентификатор кампании для случая, когда у текущего пользователя нет актуальной учетной записи Майкрософт. В этом случае идентификатор кампании внедрен в лицензию приложения. Код извлекает лицензию с помощью метода [**GetAppLicenseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetAppLicenseAsync) и затем анализирует содержимое JSON лицензии, чтобы получить значение ключа с именем *customPolicyField1*. Это значение содержит идентификатор кампании.

4. Если класс [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) в пространстве имен **Windows.Services.Store** недоступен, код возвращается к использованию метода [**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_GetAppPurchaseCampaignIdAsync) в пространстве имен **Windows.ApplicationModel.Store** для получения идентификатора пользовательской кампании (это пространство имен доступно во всех версиях Windows 10, включая 1511 и более ранние). Обратите внимание, что при использовании этого метода вы можете получить идентификаторы пользовательских кампаний для [соответствующих приобретений](#conversions) только при наличии у пользователя актуальной учетной записи Майкрософт.

### <a name="specify-the-campaign-id-in-the-proxy-file-for-the-windowsapplicationmodelstore-namespace"></a>Укажите идентификатор кампании в прокси-файле для пространства имен Windows.ApplicationModel.Store

Пространство имен **Windows.ApplicationModel.Store** содержит [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator), специальный класс, имитирующий операции Магазина для тестирования кода перед отправкой приложения в Магазин. Этот класс получает данные из [локального файла с именем Windows.StoreProxy.xml](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator). В предыдущем примере кода показано, как включить использование **CurrentApp** и **CurrentAppSimulator** в отладку и не отлаживать код в проекте. Чтобы проверить этот код в среде отладки, добавьте элемент **AppPurchaseCampaignId** в файл WindowsStoreProxy.xml на компьютере разработчика, как показано в следующем примере. При запуске приложения метод [**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.GetAppPurchaseCampaignIdAsync) всегда будет возвращать это значение.

``` xml
<CurrentApp>
    ...
    <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
</CurrentApp>
```

Пространство имен **Windows.Services.Store** не предоставляет класс, который можно использовать для имитации лицензионных сведений во время тестирования. Вместо этого необходимо опубликовать приложение в Магазине и скачать его на свое устройство разработки, чтобы использовать его лицензию для тестирования. Подробнее см. в разделе [Покупки из приложения и пробные версии](../monetize/in-app-purchases-and-trials.md#testing).

## <a name="test-your-custom-campaign"></a>Проверка вашей пользовательской кампании

Перед продвижением URL-адреса пользовательской кампании рекомендуем проверить пользовательскую кампанию, выполнив следующие действия:

1.  Выполните вход в учетную запись Майкрософт на устройстве, используемом для проверки.

2.  Щелкните URL-адрес пользовательской кампании. Убедитесь, что открылась страница вашего приложения, а затем закройте приложение UWP или страницу браузера.

3.  Щелкните URL-адрес несколько раз, закрывая приложение UWP или страницу браузера после каждого посещения страницы вашего приложения. Во время **одного** из посещений страницы вашего приложения приобретите приложение для создания конверсии. Посчитайте количество щелчков URL-адреса.

4. Подтвердите, отображается ли ожидаемый просмотров и преобразования в **просмотры страниц приложения и преобразования по Идентификатору кампании** и **всего преобразования кампании** диаграммы в [отчет о приобретениях ](acquisitions-report.md)и проверить код приложения чтобы убедиться, он может ли успешно получить идентификатор кампании, с помощью интерфейсов API, описанных выше.
