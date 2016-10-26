---
author: mcleanbyron
ms.assetid: C09F4B7C-6324-4973-980A-A60035792EFC
description: "Используйте этот метод в API отправки Магазина Windows для создания новой отправки надстройки для приложения, которое зарегистрировано в вашей учетной записи Центра разработки для Windows."
title: "Создание отправки надстройки с помощью API отправки Магазина Windows"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: b7de4b00fb4d48b9f4c542437c38e0282e155a29

---

# Создание отправки надстройки с помощью API отправки Магазина Windows




Используйте этот метод в API отправки Магазина Windows для создания новой отправки надстройки (также называется внутренним продуктом приложения или IAP) для приложения, которое зарегистрировано в вашей учетной записи Центра разработки для Windows. После успешного создания новой отправки с помощью этого метода [обновите отправку](update-an-add-on-submission.md), чтобы внести любые необходимые изменения в данные отправки, а затем [зафиксируйте отправку](commit-an-add-on-submission.md) для внедрения и публикации.

Дополнительные сведения об использовании этого метода в процессе создания отправки надстройки с помощью API отправки Магазина Windows см. в разделе [Управление отправками надстроек](manage-add-on-submissions.md).

>**Примечание.**&nbsp;&nbsp;При использовании этого метода выполняется создание отправки для имеющейся надстройки. Чтобы создать надстройку, используйте метод [Создание надстройки](create-an-add-on.md).

## Необходимые условия

Для использования этого метода необходимо выполнить следующие действия:

* Если вы еще не сделали этого, выполните все [необходимые условия](create-and-manage-submissions-using-windows-store-services.md#prerequisites) для API отправки Магазина Windows.
* [Получите маркер доступа Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), который будет использоваться в заголовке запроса этого метода. После получения маркера доступа у вас будет 60минут, чтобы использовать его до окончания его срока действия. После истечения срока действия маркера можно получить новый маркер.
* Создайте надстройку для приложения в учетной записи Центра разработки. Это можно сделать на панели мониторинга Центра разработки или с помощью метода [Создание надстройки](create-an-add-on.md).

>**Примечание.**&nbsp;&nbsp;Этот метод можно использовать только для учетных записей Центра разработки для Windows, у которых есть разрешение на использование API отправки Магазина Windows. Такое разрешение имеется не у всех учетных записей.

## Запрос

У этого метода следующий синтаксис. Примеры использования и описание текста заголовка и запроса приведены в следующих разделах.

| Метод | URI запроса                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions``` |

<span/>
 

### Заголовок запроса

| Заголовок        | Тип   | Описание                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Строка | Обязательный. Маркер доступа Azure AD в формате **Bearer** &lt;*token*&gt;. |

<span/>

### Параметры запроса

| Имя        | Тип   | Описание                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | Строка | Обязательный. Код продукта в Магазине для надстройки, для которой необходимо создать отправку. Этот код отображается на панели мониторинга Центра разработки, а также добавляется в данные ответов для запросов на [Создание надстройки](create-an-add-on.md) или [Получение сведений о надстройке](get-all-add-ons.md).  |

<span/>

### Текст запроса

Предоставлять текст запроса для этого метода не требуется.

### Пример запроса

В следующем примере кода показано, как создать новую отправку для надстройки.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## Ответ

В следующем примере представлен текст ответа JSON, обеспечивающий успешный вызов этого метода. В тексте ответа содержатся сведения о новой отправке. Дополнительные сведения о значениях в тексте ответа см. в разделе [Ресурс отправки надстройки](manage-add-on-submissions.md#add-on-submission-object).

```json
{
  "id": "1152921504621243680",
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [
      {
         "name": "Sale1",
         "basePriceId": "Free",
         "startDate": "2016-05-21T18:40:11.7369008Z",
         "endDate": "2016-05-22T18:40:11.7369008Z",
         "marketSpecificPricings": {
            "RU": "NotAvailable"
         }
      }
    ],
    "priceId": "Free"
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [
      {
        "code": "None",
        "details": "string"
      }
    ],
    "warnings": [
      {
        "code": "ListingOptOutWarning",
        "details": "You have removed listing language(s): []"
      }
    ],
    "certificationReports": [
      {
      }
    ]
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl",
  "friendlyName": "Submission 2"
}
```

## Коды ошибок

Если запрос не удается выполнить, ответ будет содержать один из следующих кодов ошибок HTTP.

| Код ошибки |  Описание   |
|--------|------------------|
| 400  | Не удалось создать отправку. Недопустимый запрос. |
| 409  | Не удалось создать отправку из-за текущего состояния приложения или в связи с тем, что приложение использует компонент панели мониторинга Центра разработки, [который в настоящее время не поддерживается API отправки Магазина Windows](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>


## Связанные разделы

* [Создание отправок и управление ими с помощью служб Магазина Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Управление отправками надстроек](manage-add-on-submissions.md)
* [Получение отправки надстройки](get-an-add-on-submission.md)
* [Подтверждение отправки надстройки](commit-an-add-on-submission.md)
* [Обновление отправки надстройки](update-an-add-on-submission.md)
* [Удаление отправки надстройки](delete-an-add-on-submission.md)
* [Получение состоянии отправки надстройки](get-status-for-an-add-on-submission.md)



<!--HONumber=Aug16_HO5-->

