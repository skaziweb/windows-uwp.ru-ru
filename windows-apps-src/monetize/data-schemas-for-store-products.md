---
description: Описание расширенной схемы данных JSON для продуктов из Магазина в пространстве имен Windows.Services.Store.
title: Схемы данных для продуктов из Магазина
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp, ExtendedJsonData, продукты Store, схема
ms.localizationpriority: medium
ms.openlocfilehash: 77f63ce409a576b3c873d95df0d2e8d0f0933808
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372533"
---
# <a name="data-schemas-for-store-products"></a>Схемы данных для продуктов из Магазина

При отправке в Магазин какого-либо продукта, например приложения или надстройки, Магазин поддерживает полный набор данных для продукта и его лицензий. В коде вашего приложения вы может программным способом получить доступ к некоторым из этих данных с помощью свойств в пространстве имен [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store). Например, вы можете получить описание и цену текущего приложения или надстройки для текущего приложения с помощью свойств [StoreProduct.Description](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.Description) и [StoreProduct.Price](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.Price).

Тем не менее, значительная часть данных для продуктов в Магазине не обеспечивает доступ с помощью предопределенных свойств в пространстве имен [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store). Чтобы получить доступ ко всем данным продукта в коде, вместо этого можно использовать следующие общие свойства:

* [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData)
* [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.ExtendedJsonData)
* [StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.ExtendedJsonData)
*   [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData)
*   [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData)
* [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.ExtendedJsonData)
*   [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData)

Эти свойства возвращают полный набор данных для соответствующего объекта в строке формата JSON. В этой статье представлена полная схема данных, возвращаемых этими свойствами.

> [!NOTE]
> Продукты в Магазине упорядочены в виде иерархии из объектов: [продукт](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct), [SKU](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) и [доступность](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability). Дополнительные сведения см. в разделе [Продукты, SKU и доступности](in-app-purchases-and-trials.md#products-skus).

## <a name="schema-for-storeproduct-storesku-storeavailability-and-storecollectiondata"></a>Схема для StoreProduct, StoreSku, StoreAvailability и StoreCollectionData

Следующая схема содержит строку в формате JSON, возвращаемую свойством [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData). Свойства [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.ExtendedJsonData), [StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.ExtendedJsonData) и [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData) возвращают только те части этой схемы, которые определены в разделах пути `DisplaySkuAvailabilities\Sku`, `DisplaySkuAvailabilities\Availabilities` и `DisplaySkuAvailabilities\Sku\CollectionData`, соответственно.

Пример строки формата JSON, возвращаемой свойством [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData), см. [в этом разделе](#product-example).

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonData.json#L1-L729)]

<span id="product-example" />

### <a name="example"></a>Пример

В следующем примере показана строка формата JSON, возвращенная свойством [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) для приложения.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonDataExample.json#L1-L268)]

## <a name="schema-for-storeapplicense-and-storelicense"></a>Схема для StoreAppLicense и StoreLicense

Следующая схема содержит строку в формате JSON, возвращаемую свойством [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData). Свойство [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.ExtendedJsonData) возвращает только те части схемы, которые определены в разделе пути `productAddOns`.

Пример строки формата JSON, возвращаемой свойством [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData), см. [в этом разделе](#license-example).

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonData.json#L1-L80)]

<span id="license-example" />

### <a name="example"></a>Пример

В следующем примере показана строка формата JSON, возвращенная свойством [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) для приложения.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonDataExample.json#L1-L28)]

## <a name="schema-for-storepurchaseproperties"></a>Схема для StorePurchaseProperties

Следующая схема содержит строку в формате JSON, возвращаемую свойством [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData).

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StorePurchaseProperties.ExtendedJsonData.json#L1-L12)]

## <a name="related-topics"></a>См. также

* [Покупки из приложения и пробные версии](in-app-purchases-and-trials.md)
* [Получите сведения о продукте для приложения и надстройки](get-product-info-for-apps-and-add-ons.md)
* [Получить сведения о лицензии для приложений и надстроек](get-license-info-for-apps-and-add-ons.md)
* [Включить покупки из приложений, приложений и надстроек](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Включить покупки готовых к использованию надстройки](enable-consumable-add-on-purchases.md)
* [Реализуйте пробную версию приложения](implement-a-trial-version-of-your-app.md)
