---
author: mcleanbyron
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: "Используйте эти методы в API отправки Магазина Windows для получения данных о приложениях, которые зарегистрированы в вашей учетной записи Центра разработки для Windows."
title: "Получение данных приложения с помощью API отправки Магазина Windows"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 99d609decc8c38952961deac5bb8ec6926d91c88

---

# Получение данных приложения с помощью API отправки Магазина Windows

Используйте указанные ниже методы в API отправки Магазина Windows для получения данных о приложениях, которые зарегистрированы в вашей учетной записи Центра разработки для Windows. Введение в API отправки Магазина Windows см. в разделе [Создание отправок и управление ими с помощью служб Магазина Windows](create-and-manage-submissions-using-windows-store-services.md).

>**Примечание.**&nbsp;&nbsp;Эти методы могут применяться только для учетных записей Центра разработки для Windows, у которых имеется разрешение на использование API отправки Магазина Windows. Такое разрешение имеется не у всех учетных записей. Эти методы могут использоваться только для получения данных для приложений. Для создания отправок для приложений и управления ими см. описания методов в разделе [Управление отправками приложений](manage-app-submissions.md).

| Метод        | URI    | Описание                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications``` | Получает данные для всех приложений, которые зарегистрированы в вашей учетной записи Центра разработки для Windows. Дополнительную информацию см. в разделе [Получение всех приложений](get-all-apps.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}``` | Получение данных о конкретном приложении, которое зарегистрировано в вашей учетной записи Центра разработки для Windows. Дополнительную информацию см. в разделе [Получение приложения](get-an-app.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts``` | Получение списка надстроек (также называемых внутренними продуктами приложения или IAP) для приложения, которое зарегистрировано в вашей учетной записи Центра разработки для Windows. Дополнительную информацию см. в разделе [Получение надстроек для приложения](get-add-ons-for-an-app.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights``` | Получение списка тестовых пакетов для приложения, которое зарегистрировано в вашей учетной записи Центра разработки для Windows. Дополнительные сведения см. в разделе [Получение тестовых пакетов для приложения](get-flights-for-an-app.md). |

<span/>

## Необходимые условия

Если вы еще не сделали этого, выполните все [необходимые условия](create-and-manage-submissions-using-windows-store-services.md#prerequisites) для API отправки Магазина Windows, прежде чем использовать любой из этих методов.

## Ресурсы

В этих методах для форматирования данных используются указанные ниже ресурсы.

<span id="application_object" />
### Приложение

Этот ресурс представляет приложение, зарегистрированное в вашей учетной записи. В следующем примере показан формат этого ресурса.

```json
{
  "id": "9NBLGGH4R315",
  "primaryName": "ApiTestApp",
  "packageFamilyName": "30481DevCenterAPITester.ApiTestAppForDevbox_ng6try80pwt52",
  "packageIdentityName": "30481DevCenterAPITester.ApiTestAppForDevbox",
  "publisherName": "CN=…",
  "firstPublishedDate": "1601-01-01T00:00:00Z",
  "lastPublishedApplicationSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621086517"
  },
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621243487"
  }
}
```

Этот ресурс содержит следующие значения.

| Значение           | Тип    | Описание                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | string  | Код продукта в Магазине для приложения. Дополнительные сведения о коде продукта в Магазине см. в разделе [Просмотр сведений об идентификации приложения](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).   |
| primaryName   | string  | Основное имя приложения.                                                                                                                                                   |
| packageFamilyName | string  | Имя семейства пакетов для приложения.                                                                                                                                                                                                         |
| packageIdentityName          | string  | Имя идентификации пакета для приложения.                                                                                                                                                              |
| publisherName       | string  | Идентификатор издателя Windows, который связан с приложением. Этот параметр соответствует значению **Пакет/идентификатор/издатель**, которое отображается на странице [Удостоверение приложения](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details) для приложения на информационной панели Центра разработки для Windows.                                                                                             |
| firstPublishedDate      | string  | Дата первой публикации приложения в формате ISO 8601.                                                                                         |
| lastPublishedApplicationSubmission       | object | Объект, который предоставляет сведения о последней опубликованной отправке для приложения. Дополнительные сведения см. ниже в разделе [Отправка](#submission_object).                                                                                                                                                          |
| pendingApplicationSubmission        | object  |  Объект, который предоставляет сведения о текущей ожидающей отправке для приложения. Дополнительные сведения см. ниже в разделе [Отправка](#submission_object).  |   |


<span id="add-on-object" />
### Надстройка

Этот ресурс содержит сведения о надстройке. В следующем примере показан формат этого ресурса.

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

Этот ресурс содержит следующие значения.

| Значение           | Тип    | Описание                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inAppProductId            | string  | Код продукта в Магазине для этой надстройки. Это значение предоставляется Магазином. Пример кода продукта в Магазине: 9NBLGGH4TNMP.   |


<span id="flight-object" />
### Тестируемая возможность

Этот ресурс содержит сведения о тестовом пакете для приложения. В следующем примере показан формат этого ресурса.

```json
{
    "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
    "friendlyName": "myflight",
    "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
    },
    "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
    },
    "groupIds": [
        "1152921504606962205"
    ],
    "rankHigherThan": "Non-flighted submission"
}
```

Этот ресурс содержит следующие значения.

| Значение           | Тип    | Описание                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | string  | Идентификатор для тестового пакета. Это значение предоставляется Центром разработки.  |
| friendlyName           | string  | Имя тестового пакета, указанное разработчиком.   |
| lastPublishedFlightSubmission       | object | Объект, который предоставляет сведения о последней опубликованной отправке для тестового пакета. Дополнительные сведения см. ниже в разделе [Отправка](#submission_object).  |
| pendingFlightSubmission        | object  |  Объект, который предоставляет сведения о текущей ожидающей отправке для тестового пакета. Дополнительные сведения см. ниже в разделе [Отправка](#submission_object).  |    
| groupIds           | array  | Массив строк, содержащий идентификаторы тестовых групп, которые связаны с тестовым пакетом. Дополнительные сведения о тестовых группах см. в разделе [Тестовые пакеты](https://msdn.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | string  | Понятное имя тестового пакета, приоритет которого на единицу ниже приоритета текущего тестового пакета. Дополнительные сведения о задании приоритетов тестовых групп см. в разделе [Тестовые пакеты](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |


<span id="submission_object" />
### Отправка

Этот ресурс содержит сведения об отправке. В следующем примере показан формат этого ресурса.

```json
{
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9WZDNCRD9MMD/submissions/1152921504621243487"
  }
}
```

Этот ресурс содержит следующие значения.

| Значение           | Тип    | Описание                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | string  | Идентификатор отправки.    |
| resourceLocation   | string  | Относительный путь, который можно добавить к базовому URI запроса ```https://manage.devcenter.microsoft.com/v1.0/my/```, чтобы получить полные данные для отправки.                                                                                                                                               |
 
<span/>

## Связанные разделы

* [Создание отправок и управление ими с помощью служб Магазина Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Управление отправками приложений с помощью API отправки Магазина Windows](manage-app-submissions.md)
* [Получение всех приложений](get-all-apps.md)
* [Получение приложения](get-an-app.md)
* [Получение надстроек для приложения](get-add-ons-for-an-app.md)
* [Получение тестовых пакетов для приложения](get-flights-for-an-app.md)



<!--HONumber=Aug16_HO5-->

