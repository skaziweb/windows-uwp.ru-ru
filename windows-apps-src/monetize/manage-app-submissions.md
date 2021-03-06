---
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: Используйте эти методы в API отправки Microsoft Store, чтобы управлять отправкой для приложений, зарегистрированных в учетной записи центра партнеров.
title: Управление отправками приложений
ms.date: 04/30/2018
ms.topic: article
keywords: Windows 10, UWP, API отправки в Microsoft Store, отправки приложений
ms.localizationpriority: medium
ms.openlocfilehash: 0575127096a016c54c1ee84c1e57c2f22054384b
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210830"
---
# <a name="manage-app-submissions"></a>Управление отправками приложений

API отправки в Microsoft Store предоставляет методы, которые можно использовать для управления отправками ваших приложений, включая постепенные выпуски пакетов. Введение в API отправки в Microsoft Store, включая необходимые условия для использования этого API, см. в разделе [Создание отправок и управление ими с помощью служб Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Если для создания отправки приложения используется API отправки Microsoft Store, обязательно внесите дальнейшие изменения в отправку только с помощью API, а не из центра партнеров. Если вы используете Центр партнеров для изменения отправки, изначально созданной с помощью API, вы больше не сможете изменить или зафиксировать эту отправку с помощью API. В некоторых случаях отправка может оставаться в состоянии ошибки, когда продолжить процесс отправки невозможно. В этом случае необходимо удалить отправку и создать новую.

> [!IMPORTANT]
> Этот API нельзя использовать для публикации отправок для [покупок в больших объемах через Microsoft Store для бизнеса и Microsoft Store для образования](../publish/organizational-licensing.md) и для публикации отправок [бизнес-приложений](../publish/distribute-lob-apps-to-enterprises.md) непосредственно для предприятий. В обоих сценариях для публикации отправки необходимо использовать центр партнеров.


<span id="methods-for-app-submissions" />

## <a name="methods-for-managing-app-submissions"></a>Методы для управления отправками приложений

Используйте следующие методы для получения, создания, обновления, фиксации и удаления отправки приложения. Прежде чем использовать эти методы, приложение должно уже существовать в учетной записи центра партнеров, и сначала необходимо создать одну отправку для приложения в центре партнеров. Дополнительные сведения см. в разделе [Необходимые условия](create-and-manage-submissions-using-windows-store-services.md#prerequisites).

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Метод</th>
<th align="left">URI</th>
<th align="left">Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-app-submission.md">Получение существующей отправки приложения</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-app-submission.md">Получение состояния существующего отправленного приложения</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions</td>
<td align="left"><a href="create-an-app-submission.md">Создание новой отправки приложения</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-app-submission.md">Обновление существующей отправки приложения</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-app-submission.md">Фиксация новой или обновленной отправки приложения</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-app-submission.md">Удаление отправки приложения</a></td>
</tr>
</tbody>
</table>

<span id="create-an-app-submission">

### <a name="create-an-app-submission"></a>Создание отправки приложения

Чтобы создать отправку для приложения, выполните следующие действия.

1. Если вы еще не сделали этого, выполните все [необходимые условия](create-and-manage-submissions-using-windows-store-services.md#prerequisites) для API отправки в Microsoft Store.
    > [!NOTE]
    > Убедитесь, что приложение уже содержит не менее одной завершенной отправки, для которой указаны сведения [возрастной категории](https://docs.microsoft.com/windows/uwp/publish/age-ratings).

2. [Получение токена доступа Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Этот маркер доступа необходимо передавать методам из API отправки в Microsoft Store. После получения токена доступа у вас будет 60 минут, чтобы использовать его до окончания его срока действия. После истечения срока действия токена можно получить новый токен.

3. [Создание отправки приложения](create-an-app-submission.md) путем выполнения следующего метода в API отправки в Microsoft Store. Этот метод создает новую выполняющуюся отправку, которая является копией последней опубликованной отправки.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
    ```

    Тело ответа содержит ресурс [отправки приложения](#app-submission-object), включающий в себя идентификатор новой отправки, URI подписанного URL-адреса (Shared Access Signature, SAS) для передачи любых необходимых файлов для отправки в хранилище BLOB-объектов Azure (таких как пакеты приложения, изображения для описания и файлы трейлеров), а также все данные новой отправки (в том числе все описания и информацию о ценах).

    > [!NOTE]
    > URI SAS предоставляет доступ к защищенному ресурсу в хранилище Azure без необходимости в использовании ключей учетной записи. Основные сведения об URI SAS и их использовании с хранилищем BLOB-объектов Azure см. в разделах [Подписанные URL-адреса, часть 1. Общие сведения о модели SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/) и [Подписанные URL-адреса, часть 2. Создание и использование SAS с хранилищем BLOB-объектов](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

4. При добавлении новых пакетов, изображений для описания и файлов трейлеров для отправки [подготовьте пакеты приложения](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements) и [подготовьте снимки экранов, изображения и трейлеры](https://docs.microsoft.com/windows/uwp/publish/app-screenshots-and-images). Добавьте все эти файлы в ZIP-архив.

5. Проверьте [отправку приложения](#app-submission-object), внеся все необходимые для новой отправки изменения, затем выполните следующий метод для [обновления отправки приложения](update-an-app-submission.md).

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Если в отправку добавляются новые файлы, обязательно обновите данные отправки, чтобы они ссылались на имя и относительный путь этих файлов в ZIP-архиве.

4. При добавлении новых пакетов, изображений описания и файлов трейлеров для отправки выложите ZIP-архив в [хранилище BLOB-объектов Azure](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage), используя URI SAS, полученный в теле ответа метода POST, вызов которого был выполнен ранее. Существуют разные библиотеки Azure, которые можно использовать в этих целях на различных платформах, включая следующие.

    * [Клиентская библиотека службы хранилища Azure для .NET](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Пакет SDK службы хранилища Azure для Java](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [Пакет SDK службы хранилища Azure для Python](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    В следующем фрагменте кода на C# показано, как передать ZIP-архив в хранилище BLOB-объектов Azure с помощью класса [CloudBlockBlob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob) клиентской библиотеки службы хранилища Azure для .NET. В этом примере кода предполагается, что ZIP-архив уже был записан в потоковый объект.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. [Подтвердите отправку приложения](commit-an-app-submission.md), выполнив следующий метод. В результате вы получите оповещение об этом центре партнеров, и теперь ваши обновления должны быть применены к вашей учетной записи.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
    ```

6. Проверка состояния подтверждения путем выполнения следующего метода для [получения сведений о состоянии отправки приложения](get-status-for-an-app-submission.md).

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
    ```

    Чтобы проверить состояние отправки, посмотрите значение поля *status* в теле ответа. Это значение должно измениться с **CommitStarted** на **PreProcessing**, если запрос выполнен успешно, или на **CommitFailed**, если в запросе возникли ошибки. Если имеются ошибки, поле *statusDetails* содержит дополнительные сведения об ошибке.

7. После успешного завершения подтверждения отправки она отправляется в Магазин для внедрения. Вы можете продолжить отслеживать ход отправки, используя предыдущий метод или посетив центр партнеров.

<span id="manage-gradual-package-rollout">

## <a name="methods-for-managing-a-gradual-package-rollout"></a>Методы для управления постепенным выпуском пакета

Вы можете постепенно выполнять выпуск обновленных пакетов в отправке приложения для части пользователей вашего приложения в Windows 10. Это позволяет контролировать отзывы и аналитические данные для конкретных пакетов, чтобы вы могли убедиться в правильности обновления до его более широкого распространения. Можно изменить процент выпуска (или остановить обновление) для опубликованной отправки без необходимости создания новой отправки. Дополнительные сведения, в том числе инструкции по включению постепенного развертывания пакетов и управлению ими в центре партнеров, см. в [этой статье](../publish/gradual-package-rollout.md).

Чтобы программным образом включить постепенный выпуск пакета для отправки приложения, выполните следующие действия используя методы в API отправки в Microsoft Store.

  1. [Создайте отправку приложения](create-an-app-submission.md) или [получите существующую отправку приложения](get-an-app-submission.md).
  2. В данных ответа найдите ресурс [packageRollout](#package-rollout-object), задайте в поле *isPackageRollout* значение **true**, а в поле *packageRolloutPercentage* задайте процент пользователей вашего приложения, которые должны получить обновленные пакеты.
  3. Передайте обновленные данные отправки приложения методу [обновления отправки приложения](update-an-app-submission.md).

После включения постепенного выпуска пакета для отправки приложения можно использовать следующие методы, чтобы программным образом получать, обновлять, останавливать или завершать постепенный выпуск.

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Метод</th>
<th align="left">URI</th>
<th align="left">Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-an-app-submission.md">Получение сведений об постепенном переходе для отправки приложения</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-an-app-submission.md">Обновить процент постепенного развертывания для отправки приложения</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-an-app-submission.md">Остановка постепенного развертывания для отправки приложения</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-an-app-submission.md">Завершение постепенного развертывания для отправки приложения</a></td>
</tr>
</tbody>
</table>


## <a name="code-examples-for-managing-app-submissions"></a>Примеры кода для управления отправками приложений

Следующие статьи содержат подробные примеры кода на нескольких разных языках программирования, демонстрирующие процесс создания отправки приложения.

* [C#Пример: отправка приложений, надстроек и рейсов](csharp-code-examples-for-the-windows-store-submission-api.md)
* [C#Пример: отправка приложения с помощью параметров игры и трейлеров](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Пример Java: отправка приложений, надстроек и рейсов](java-code-examples-for-the-windows-store-submission-api.md)
* [Пример для Java: отправка приложения с помощью параметров игры и трейлеров](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Пример Python. Отправка приложений, надстроек и рейсов](python-code-examples-for-the-windows-store-submission-api.md)
* [Пример Python. Отправка приложений с помощью параметров игры и трейлеров](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>Модуль StoreBroker PowerShell

В качестве альтернативы прямому вызову API отправки в Microsoft Store также предоставляется модуль PowerShell с открытым исходным кодом, в котором поверх API реализован интерфейс командной строки. Этот модуль называется [StoreBroker](https://github.com/Microsoft/StoreBroker). Этот модуль можно использовать для управления приложением, тестируемой возможностью и отправками надстроек из командной строки, вместо того чтобы вызывать API отправки в Microsoft Store напрямую. Кроме того, можно просмотреть дополнительные примеры вызова этого API, изучив исходный код. Модуль StoreBroker активно используется Microsoft в качестве основного способа отправки многих собственных приложений компании в Магазин.

Дополнительные сведения см. на нашей [странице StoreBroker на сайте GitHub](https://github.com/Microsoft/StoreBroker).

<span/>

## <a name="data-resources"></a>Ресурсы данных
Для управления отправками приложений методы API отправки в Microsoft Store используют следующие ресурсы данных JSON.

<span id="app-submission-object" />

### <a name="app-submission-resource"></a>Ресурс отправки приложения

Этот ресурс описывает отправку приложения.

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2",
    "isAdvancedPricingModel": true
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
          "epub"
        ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
          "Free ebook reader"
        ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "description": "Main page",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1"
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/387a9ea8-a412-43a9-8fb3-a38d03eb483d?sv=2014-02-14&sr=b&sig=sdd12JmoaT6BhvC%2BZUrwRweA%2Fkvj%2BEBCY09C2SZZowg%3D&se=2016-06-17T18:32:26Z&sp=rwl",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2",
  "trailers": []
}
```

У этого ресурса есть следующие значения.

| Значение      | Тип   | Описание      |
|------------|--------|-------------------|
| id            | string  | Идентификатор отправки. Этот идентификатор доступен в данных ответа на запросы на [создание отправки приложения](create-an-app-submission.md), [получение всех приложений](get-all-apps.md) и [получение определенного приложения](get-an-app.md). Для отправки, созданной в центре партнеров, этот идентификатор также доступен в URL-адресе страницы отправки в центре партнеров.  |
| applicationCategory           | string  |   Строка, указывающая [категорию или подкатегорию](https://docs.microsoft.com/windows/uwp/publish/category-and-subcategory-table) для вашего приложения. Категории и подкатегории объединяются в одну строку с помощью символа подчеркивания "_", например **BooksAndReference_EReader**.      |  
| pricing           |  object  | [Ресурс цены](#pricing-object), содержащий сведения о цене приложения.        |   
| видимость           |  string  |  Видимость приложения. Может принимать одно из следующих значений. <ul><li>Hidden</li><li>Открытый</li><li>Private (Частное)</li><li>NotSet (Не задано)</li></ul>       |   
| targetPublishMode           | string  | Режим публикации для отправки. Может принимать одно из следующих значений. <ul><li>Немедленный</li><li>Вручную</li><li>SpecificDate (Указанная дата)</li></ul> |
| targetPublishDate           | string  | Дата публикации отправки в формате ISO 8601, если для *targetPublishMode* задано значение SpecificDate.  |  
| перечисления           |   object  |  Словарь пар "ключ-значение", где каждый ключ является кодом страны, а каждое значение — объектом [ресурса описания](#listing-object), содержащим данные описания приложения.       |   
| hardwarePreferences           |  array  |   Массив строк, определяющих [предпочтения оборудования](https://docs.microsoft.com/windows/uwp/publish/enter-app-properties) для приложения. Может принимать одно из следующих значений. <ul><li>Сенсорный ввод</li><li>Клавиатура</li><li>Мышь</li><li>Камера</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Телефония</li></ul>     |   
| automaticBackupEnabled           |  Логический  |   Указывает, может ли Windows включать данные этого приложения в автоматические резервные копии, записываемые в OneDrive. Подробные сведения см. в разделе [Объявления приложений](https://docs.microsoft.com/windows/uwp/publish/app-declarations).   |   
| canInstallOnRemovableMedia           |  Логический  |   Указывает, могут ли клиенты устанавливать приложение на съемный носитель. Подробные сведения см. в разделе [Объявления приложений](https://docs.microsoft.com/windows/uwp/publish/app-declarations).     |   
| isGameDvrEnabled           |  Логический |   Указывает, включена ли для приложения функция DVR для игр.    |   
| gamingOptions           |  array |   Массив, содержащий один [ресурс параметров игры](#gaming-options-object), который определяет относящиеся к игре параметры для приложения.     |   
| hasExternalInAppProducts           |     Логический          |   Указывает, позволяет ли приложение пользователям делать покупки без использования коммерческой системы Microsoft Store. Подробные сведения см. в разделе [Объявления приложений](https://docs.microsoft.com/windows/uwp/publish/app-declarations).     |   
| meetAccessibilityGuidelines           |    Логический           |  Указывает, проверено ли приложение на соответствие рекомендациям по специальным возможностям. Подробные сведения см. в разделе [Объявления приложений](https://docs.microsoft.com/windows/uwp/publish/app-declarations).      |   
| notesForCertification           |  string  |   Содержит [заметки по сертификации](https://docs.microsoft.com/windows/uwp/publish/notes-for-certification) приложения.    |    
| состояние           |   string  |  Состояние отправки. Может принимать одно из следующих значений. <ul><li>Нет</li><li>Canceled (Отменено)</li><li>PendingCommit (Ожидание фиксации)</li><li>CommitStarted (Фиксация запущена)</li><li>CommitFailed (Сбой фиксации)</li><li>PendingPublication (Ожидание публикации)</li><li>публикация</li><li>Published</li><li>PublishFailed (Сбой публикации)</li><li>PreProcessing (Предварительная обработка)</li><li>PreProcessingFailed (Сбой предварительной обработки)</li><li>Сертификация</li><li>CertificationFailed (Сбой сертификации)</li><li>Release</li><li>ReleaseFailed (Сбой выпуска)</li></ul>      |    
| statusDetails           |   object  | [Ресурс сведений о состоянии](#status-details-object), который содержит дополнительные сведения о состоянии отправки, включая сведения об ошибках.       |    
| fileUploadUrl           |   string  | URI подписанного URL-адреса (SAS) для передачи пакетов для отправки. При добавлении новых пакетов, изображений описания или файлов трейлеров для отправки выложите ZIP-архив, содержащий пакеты и изображения, по этому URI. Дополнительную информацию см. в разделе [Создание отправки приложения](#create-an-app-submission).       |    
| applicationPackages           |   array  | Массив [ресурсов пакета приложения](#application-package-object), предоставляющий сведения о каждом пакете в отправке |    
| packageDeliveryOptions    | object  | [Ресурс параметров доставки пакета](#package-delivery-options-object), который содержит постепенный выпуск пакета и обязательные параметры обновления для отправки.  |
| enterpriseLicensing           |  string  |  Одно из значений, связанных с [корпоративным лицензированием](#enterprise-licensing), указывающих на поведение приложения в отношении корпоративного лицензирования.  |    
| allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies           |  Логический   |  Указывает, разрешено ли Майкрософт [делать приложение доступным для будущих семейств устройств Windows 10.](https://docs.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability).    |    
| allowTargetFutureDeviceFamilies           | object   |  Словарь пар "ключ-значение", в котором каждый ключ представляет собой [семейство устройств Windows 10](https://docs.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability), а каждое значение является логическим значением, которое указывает, разрешено ли нацеливание приложения на указанное семейство устройств.     |    
| friendlyName           |   string  |  Понятное имя отправки, как показано в центре партнеров. Это значение создается при создании отправки.       |  
| trailers           |  array |   Массив, содержащий до 15 [ресурсов трейлеров](#trailer-object), представляющие видеотрейлеры для описания приложения.<br/><br/>   |  


<span id="pricing-object" />

### <a name="pricing-resource"></a>Ресурс цены

Этот ресурс содержит сведения о ценах для приложения. У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание        |
|-----------------|---------|------|
|  trialPeriod               |    string     |  Строка, указывающая срок действия пробной версии приложения. Может принимать одно из следующих значений. <ul><li>NoFreeTrial (Нет пробного периода)</li><li>OneDay (Один день)</li><li>TrialNeverExpires (Бессрочный пробный период)</li><li>SevenDays (7 дней)</li><li>FifteenDays (15 дней)</li><li>ThirtyDays (30 дней)</li></ul>    |
|  marketSpecificPricings               |    object     |  Словарь пар "ключ-значение", где каждый ключ представляет собой код страны ISO 3166-1 alpha-2 из двух букв, а каждое значение — [ценовую категорию](#price-tiers). Эти элементы представляют [особые цены на ваше приложение для определенных рынков](https://docs.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection). Любые элементы этого словаря переопределяют базовую цену, заданную значением *priceId* для указанного рынка.      |     
|  sales               |   array      |  **Не рекомендуется**. Массив [ресурсов продажи](#sale-object), содержащих сведения о продажах для приложения.   |     
|  priceId               |   string      |  [Ценовая категория](#price-tiers), указывающая [базовую цену](https://docs.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection) для приложения.   |     
|  isAdvancedPricingModel               |   Логический      |  Если **true**, ваша учетная запись разработчика имеет доступ к расширенному набору ценовых категорий от 0,99 долл. США до 1999,99 долл. США. Если **false**, ваша учетная запись разработчика имеет доступ к стандартному набору ценовых категорий от 0,99 долл. США до 999,99 долл. США. Дополнительные сведения о разных категориях см. в разделе [Ценовые категории](#price-tiers).<br/><br/>**Примечание**.&nbsp;&nbsp;Данное поле предназначено только для чтения.   |


<span id="sale-object" />

### <a name="sale-resource"></a>Ресурс продажи

Этот ресурс содержит сведения о продажах для приложения.

> [!IMPORTANT]
> Ресурс **продажи** больше не поддерживается, и в настоящее время невозможно получить или изменить данные продажи для отправки приложения с помощью API отправки в Microsoft Store. В дальнейшем мы обновим API отправки в Microsoft Store, чтобы предложить пользователям новый способ программного доступа к информации о продажах для отправок приложений.
>    * После вызова [метода GET для получения отправки приложения](get-an-app-submission.md) значение *sales* будет пустым. Вы можете продолжить использовать центр партнеров для получения данных о продажах для отправки приложения.
>    * При вызове [метода PUT для обновления отправки приложения](update-an-app-submission.md) сведения в значении *sales* будут игнорироваться. Вы можете продолжить использовать центр партнеров для изменения данных о продажах для отправки приложения.

У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание    |
|-----------------|---------|------|
|  name               |    string     |   Имя продажи.    |     
|  basePriceId               |   string      |  [Ценовая категория](#price-tiers), используемая для базовой цены продажи.    |     
|  startDate               |   string      |   Дата начала для продажи в формате ISO 8601.  |     
|  endDate               |   string      |  Дата окончания для продажи в формате ISO 8601.      |     
|  marketSpecificPricings               |   object      |   Словарь пар "ключ-значение", где каждый ключ представляет собой код страны ISO 3166-1 alpha-2 из двух букв, а каждое значение — [ценовую категорию](#price-tiers). Эти элементы представляют [особые цены на ваше приложение для определенных рынков](https://docs.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection). Любые элементы этого словаря переопределяют базовую цену, заданную значением *basePriceId* для указанного рынка.    |


<span id="listing-object" />

### <a name="listing-resource"></a>Ресурс описания

Этот ресурс содержит информацию описания для приложения. У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание                  |
|-----------------|---------|------|
|  baseListing               |   object      |  Информация [базового описания](#base-listing-object) для приложения, которая определяет описание по умолчанию на всех платформах.   |     
|  platformOverrides               | object |   Словарь пар "ключ-значение", где каждый ключ является строкой, идентифицирующей платформу, для которой необходимо переопределить информацию в описании, а каждое значение является ресурсом [базового описания](#base-listing-object) (содержащим только значения от описания до заголовка), определяющим информацию для переопределения описания для указанной платформы. Ключи могут иметь следующие значения: <ul><li>Неизвестно</li><li>Windows80</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |      |     

<span id="base-listing-object" />

### <a name="base-listing-resource"></a>Ресурс базового описания

Этот ресурс содержит базовую информацию описания для приложения. У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание       |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   string      |  Необязательные [сведения об авторских правах и (или) товарных знаках](https://docs.microsoft.com/windows/uwp/publish/create-app-descriptions).  |
|  ключевые слова                |  array       |  Массив [ключевых слов](https://docs.microsoft.com/windows/uwp/publish/create-app-descriptions), способствующих появлению вашего приложения в результатах поиска.    |
|  licenseTerms                |    string     | Необязательные [условия лицензионного соглашения](https://docs.microsoft.com/windows/uwp/publish/create-app-descriptions) для вашего приложения.     |
|  privacyPolicy                |   string      |   Это значение устарело. Чтобы задать или изменить URL-адрес политики конфиденциальности для приложения, необходимо сделать это на странице [свойств](../publish/enter-app-properties.md#privacy-policy-url) в центре партнеров. Это значение можно исключить из вызовов к API отправки. Если это значение задано, оно будет игнорироваться.       |
|  supportContact                |   string      |  Это значение устарело. Чтобы задать или изменить URL-адрес или адрес электронной почты службы поддержки для приложения, необходимо сделать это на странице [свойств](../publish/enter-app-properties.md#support-contact-info) в центре партнеров. Это значение можно исключить из вызовов к API отправки. Если это значение задано, оно будет игнорироваться.        |
|  websiteUrl                |   string      |  Это значение устарело. Чтобы задать или изменить URL-адрес веб страницы для приложения, необходимо сделать это на странице [свойств](../publish/enter-app-properties.md#website) в центре партнеров. Это значение можно исключить из вызовов к API отправки. Если это значение задано, оно будет игнорироваться.      |    
|  description               |    string     |   [Описание](https://docs.microsoft.com/windows/uwp/publish/create-app-descriptions) для описания приложения.   |     
|  возможности               |    array     |  Массив размером до 20 строк со списком [функций](https://docs.microsoft.com/windows/uwp/publish/create-app-descriptions) вашего приложения.     |
|  releaseNotes               |  string       |  [Заметки о выпуске](https://docs.microsoft.com/windows/uwp/publish/create-app-descriptions) для вашего приложения.    |
|  images               |   array      |  Массив ресурсов [изображений и значков](#image-object) для описания приложения.  |
|  recommendedHardware               |   array      |  Массив размером до 11 строк со списком [рекомендуемой конфигурации оборудования](../publish/create-app-store-listings.md#additional-information) для вашего приложения.     |
|  minimumHardware               |     string    |  Массив размером до 11 строк со списком [минимальной конфигурации оборудования](../publish/create-app-store-listings.md#additional-information) для вашего приложения.    |  
|  title               |     string    |   Название для описания приложения.   |  
|  shortDescription               |     string    |  Используется только для игр. Это описание отображается в разделе **Информация** в центре игр на Xbox One и помогает пользователям узнать больше об игре.   |  
|  shortTitle               |     string    |  Краткая версия названия вашего продукта. Если оно указывается, это короткое название может отображаться в различных местах на Xbox One (во время установки, при получении достижения т. д.) вместо полного названия продукта.    |  
|  sortTitle               |     string    |   Если ваш продукт можно добавить в алфавитный список разными способами, вы можете ввести другую версию названия в этом поле. Это может помочь пользователям быстрее найти продукт при поиске.    |  
|  voiceTitle               |     string    |   Альтернативное имя вашего продукта, которое, если возможно, можно использовать с голосовыми функциями Xbox One при использовании Kinect или гарнитуры.    |  
|  devStudio               |     string    |   Задайте это значение, если хотите включить поле **Developed by** в описание. (В поле **Published by** будет указываться отображаемое имя издателя, связанное с вашей учетной записью, независимо от того, указали ли вы значение *devStudio*.)    |  

<span id="image-object" />

### <a name="image-resource"></a>Ресурс изображений

Этот ресурс содержит данные изображений и значков для описания приложения. Дополнительные сведения об изображениях и значках для описания приложения см. в разделе [Снимки экранов приложения и изображения](../publish/app-screenshots-and-images.md). У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание           |
|-----------------|---------|------|
|  fileName               |    string     |   Имя файла изображения в ZIP-архиве, который был передан для отправки.    |     
|  fileStatus               |   string      |  Состояние файла изображения. Может принимать одно из следующих значений. <ul><li>Нет</li><li>PendingUpload (Ожидает передачи)</li><li>Uploaded (Передан)</li><li>PendingDelete (Ожидает удаления)</li></ul>   |
|  id  |  string  | Идентификатор для изображения. Это значение предоставляется центром партнеров.  |
|  description  |  string  | Описание изображения.  |
|  imageType  |  string  | Указывает тип изображения. В настоящее время поддерживаются перечисленные ниже строки. <p/>[Изображения снимков экрана](../publish/app-screenshots-and-images.md#screenshots): <ul><li>Screenshot (используйте это значение для снимка экрана рабочего стола)</li><li>MobileScreenshot (Снимок экрана мобильного устройства)</li><li>XboxScreenshot (Снимок экрана Xbox)</li><li>SurfaceHubScreenshot (Снимок экрана SurfaceHub)</li><li>HoloLensScreenshot (Снимок экрана HoloLens)</li></ul><p/>[Логотипы в Магазине](../publish/app-screenshots-and-images.md#store-logos):<ul><li>StoreLogo9x16 </li><li>StoreLogoSquare</li><li>Icon (используйте это значение для логотипа 1:1 300 x 300 пикселей)</li></ul><p/>[Рекламные изображения](../publish/app-screenshots-and-images.md#promotional-images): <ul><li>PromotionalArt16x9</li><li>PromotionalArtwork2400X1200 (Рекламное изображение 2400X1200)</li></ul><p/>[Изображения для Xbox](../publish/app-screenshots-and-images.md#xbox-images): <ul><li>XboxBrandedKeyArt</li><li>XboxTitledHeroArt</li><li>XboxFeaturedPromotionalArt</li></ul><p/>[Дополнительные рекламные изображения](../publish/app-screenshots-and-images.md#optional-promotional-images): <ul><li>SquareIcon358X358 (Фоновое изображение 358X358)</li><li>BackgroundImage1000X800 (Фоновое изображение 1000X800)</li><li>PromotionalArtwork414X180 (Рекламное изображение 414X180)</li></ul><p/> <!-- The following strings are also recognized for this field, but they correspond to image types that are no longer for listings in the Store.<ul><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>WideIcon358X173</li><li>Unknown</li></ul> -->   |


<span id="gaming-options-object" />

### <a name="gaming-options-resource"></a>Ресурс параметров игры

Этот ресурс содержит относящиеся к игре параметры для приложения. Значения в этом ресурсе соответствуют [параметрам игры](../publish/enter-app-properties.md#game-settings) для отправки в центре партнеров.

```json
{
  "gamingOptions": [
    {
      "genres": [
        "Games_ActionAndAdventure",
        "Games_Casino"
      ],
      "isLocalMultiplayer": true,
      "isLocalCooperative": true,
      "isOnlineMultiplayer": false,
      "isOnlineCooperative": false,
      "localMultiplayerMinPlayers": 2,
      "localMultiplayerMaxPlayers": 12,
      "localCooperativeMinPlayers": 2,
      "localCooperativeMaxPlayers": 12,
      "isBroadcastingPrivilegeGranted": true,
      "isCrossPlayEnabled": false,
      "kinectDataForExternal": "Enabled"
    }
  ],
}
```

У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание        |
|-----------------|---------|------|
|  genres               |    array     |  Массив из одной или нескольких приведенных ниже строк, которые описывают жанры игры: <ul><li>Games_ActionAndAdventure</li><li>Games_CardAndBoard</li><li>Games_Casino</li><li>Games_Educational</li><li>Games_FamilyAndKids</li><li>Games_Fighting</li><li>Games_Music</li><li>Games_Platformer</li><li>Games_PuzzleAndTrivia</li><li>Games_RacingAndFlying</li><li>Games_RolePlaying</li><li>Games_Shooter</li><li>Games_Simulation</li><li>Games_Sports</li><li>Games_Strategy</li><li>Games_Word</li></ul>    |
|  isLocalMultiplayer               |    Логический     |  Указывает, поддерживает ли игра локальный многопользовательский режим.      |     
|  isLocalCooperative               |   Логический      |  Указывает, поддерживает ли игра локальный совместный режим.    |     
|  isOnlineMultiplayer               |   Логический      |  Указывает, поддерживает ли игра онлайн-многопользовательский режим.    |     
|  isOnlineCooperative               |   Логический      |  Указывает, поддерживает ли игра онлайн-совместный режим.    |     
|  localMultiplayerMinPlayers               |   int      |   Определяет минимальное поддерживаемое число игроков, поддерживаемое игрой в локальном многопользовательском режиме.   |     
|  localMultiplayerMaxPlayers               |   int      |   Определяет максимальное поддерживаемое число игроков, поддерживаемое игрой в локальном многопользовательском режиме.  |     
|  localCooperativeMinPlayers               |   int      |   Определяет минимальное поддерживаемое число игроков, поддерживаемое игрой в локальном совместном режиме.  |     
|  localCooperativeMaxPlayers               |   int      |   Определяет максимальное поддерживаемое число игроков, поддерживаемое игрой в локальном совместном режиме.  |     
|  isBroadcastingPrivilegeGranted               |   Логический      |  Указывает, поддерживает ли игра трансляции.   |     
|  isCrossPlayEnabled               |   Логический      |   Указывает, поддерживает ли игра многопользовательские сеансы между игроками, использующими компьютеры под управлением ОС Windows 10 и устройства Xbox.  |     
|  kinectDataForExternal               |   string      |  Одно из следующих строковых значений, указывающее, может ли игра получать данные Kinect и отправлять их внешним службам: <ul><li>NotSet (Не задано)</li><li>Неизвестно</li><li>Активировать</li><li>Отключено.</li></ul>   |

> [!NOTE]
> Ресурс *gamingOptions* был добавлен в мае 2017 г. после первого выпуска API отправки в Microsoft Store для разработчиков. Если вы создали отправку для приложения с помощью API отправки до появления этого ресурса и эта отправка все еще выполняется, этот ресурс будет иметь значение null для отправок приложения, пока вы успешно не выполните отправку или не удалите ее. Если ресурс *gamingOptions* недоступен для отправок приложения, поле *hasAdvancedListingPermission* в [ресурсе Application](get-app-data.md#application_object), возвращаемом методом [получения приложения](get-an-app.md), будет иметь значение false.

<span id="status-details-object" />

### <a name="status-details-resource"></a>Ресурс сведений о состоянии

Этот ресурс содержит дополнительные сведения о состоянии отправки. У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание         |
|-----------------|---------|------|
|  errors               |    object     |   Массив [ресурсов сведений о состоянии](#status-detail-object), который содержит информацию об ошибках для отправки.    |     
|  предупреждения               |   object      | Массив [ресурсов сведений о состоянии](#status-detail-object), который содержит информацию о предупреждениях для отправки.      |
|  certificationReports               |     object    |   Массив [ресурсов отчета сертификации](#certification-report-object), который предоставляет доступ к данным отчета о сертификации для отправки. В случае сбоя сертификации можно проверить эти отчеты для получения дополнительной информации.   |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Ресурс сведений о состоянии

Этот ресурс содержит дополнительные сведения обо всех связанных ошибках или предупреждениях для отправки. У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание        |
|-----------------|---------|------|
|  code               |    string     |   [Код состояния отправки](#submission-status-code), описывающий тип ошибки или предупреждения.   |     
|  подробности               |     string    |  Сообщение с дополнительными сведениями о проблеме.     |


<span id="application-package-object" />

### <a name="application-package-resource"></a>Ресурс пакета приложения

Этот ресурс содержит сведения о пакете приложения для отправки.

```json
{
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
}
```

У этого ресурса есть следующие значения.  

> [!NOTE]
> При вызове метода для [обновления отправки приложения](update-an-app-submission.md) в тексте запроса требуются только значения *fileName*, *fileStatus*, *minimumDirectXVersion* и *minimumSystemRam* этого объекта. Другие значения заполняются центром партнеров.

| Значение           | Тип    | Описание                   |
|-----------------|---------|------|
| fileName   |   string      |  Имя пакета.    |  
| fileStatus    | string    |  Состояние пакета. Может принимать одно из следующих значений. <ul><li>Нет</li><li>PendingUpload (Ожидает передачи)</li><li>Uploaded (Передан)</li><li>PendingDelete (Ожидает удаления)</li></ul>    |  
| id    |  string   |  Идентификатор, который уникально идентифицирует пакет. Это значение предоставляется центром партнеров.   |     
| version    |  string   |  Версия пакета приложения. Дополнительные сведения см. в разделе [Нумерация версий пакета](https://docs.microsoft.com/windows/uwp/publish/package-version-numbering).   |   
| архитектура    |  string   |  Архитектура пакета (например, ARM).   |     
| языки    | array    |  Массив кодов языков, которые поддерживает приложение. Дополнительные сведения см. в разделе [Поддерживаемые языки](https://docs.microsoft.com/windows/uwp/publish/supported-languages).    |     
| capabilities    |  array   |  Массив возможностей, необходимых для этого пакета. Дополнительные сведения о возможностях см. в разделе [Объявления возможностей приложения](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).   |     
| minimumDirectXVersion    |  string   |  Минимальная версия DirectX, поддерживаемая пакетом приложения. Может задаваться только для приложений, предназначенных для Windows 8.x. Для приложений, предназначенных для других версий ОС, это значение должно присутствовать при вызове метода [обновления отправки приложения](update-an-app-submission.md), но указанное значение игнорируется. Может принимать одно из следующих значений. <ul><li>Нет</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | string    |  Минимальный объем ОЗУ, необходимый для пакета приложения. Может задаваться только для приложений, предназначенных для Windows 8.x. Для приложений, предназначенных для других версий ОС, это значение должно присутствовать при вызове метода [обновления отправки приложения](update-an-app-submission.md), но указанное значение игнорируется. Может принимать одно из следующих значений. <ul><li>Нет</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | array    |  Массив строк, представляющих семейства устройств, для которых предназначен пакет. Это значение используется только для пакетов, предназначенных для Windows 10; для пакетов, предназначенных для предыдущих выпусков, это значение равно **None**. В настоящее время поддерживаются следующие строки семейств устройств для пакетов Windows 10, где *{0}* представляет собой строку версии Windows 10, например 10.0.10240.0, 10.0.10586.0 или 10.0.14393.0: <ul><li>Windows.Universal min version *{0}*</li><li>Windows.Desktop min version *{0}*</li><li>Windows.Mobile min version *{0}*</li><li>Windows.Xbox min version *{0}*</li><li>Windows.Holographic min version *{0}*</li></ul>   |    

<span/>

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Ресурс отчета о сертификации

Этот ресурс предоставляет доступ к данным отчета о сертификации для отправки. У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание             |
|-----------------|---------|------|
|     date            |    string     |  Дата и время создания отчета в формате ISO 8601.    |
|     reportUrl            |    string     |  URL-адрес, по которому можно получить доступ к отчету.    |


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>Ресурс параметров доставки пакета

Этот ресурс содержит постепенный выпуск пакета и обязательные параметры обновления для отправки.

```json
{
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
}
```

У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание        |
|-----------------|---------|------|
| packageRollout   |   object      |  [Ресурс выпуска пакета](#package-rollout-object), который содержит параметры постепенного выпуска пакета для отправки.   |  
| isMandatoryUpdate    | Логический    |  Указывает, следует ли рассматривать пакеты в этой отправке как обязательные для самоустанавливающихся обновлений приложений. Дополнительные сведения об обязательных пакетах самоустанавливаемых обновлений приложений см. в разделе [Загрузка и установка обновлений пакетов для приложения](../packaging/self-install-package-updates.md).    |  
| mandatoryUpdateEffectiveDate    |  date   |  Дата и время, когда пакеты в этой отправке станут обязательными, в формате ISO 8601 и по часовому поясу UTC.   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>Ресурс выпуска пакета

Этот ресурс содержит [параметры постепенного выпуска пакета](#manage-gradual-package-rollout) для отправки. У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание        |
|-----------------|---------|------|
| isPackageRollout   |   Логический      |  Указывает, включен ли постепенный выпуск пакета для этой отправки.    |  
| packageRolloutPercentage    | float    |  Процент пользователей, которые будут получать пакеты при постепенном выпуске.    |  
| packageRolloutStatus    |  string   |  Одна из следующих строк, указывающая состояние постепенного выпуска пакета: <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  string   |  Идентификатор отправки, который будет поступать от пользователей, которые не получают пакеты постепенного выпуска.   |          

> [!NOTE]
> Значения *паккажероллаутстатус* и *Фаллбакксубмиссионид* назначаются центром партнеров и не предназначены для установки разработчиком. Если эти значения содержатся в теле запроса, они будут проигнорированы.

<span id="trailer-object" />

### <a name="trailers-resource"></a>Ресурс трейлеров

Этот ресурс представляет видеотрейлер для описания приложения. Значения в этом ресурсе соответствуют параметрам [трейлеров](../publish/app-screenshots-and-images.md#trailers) для отправки в центре партнеров.

Можно добавить до 15 ресурсов трейлеров в массив *trailers* в [ресурсе отправки приложения](#app-submission-object). Для отправки видеофайлов трейлеров и эскизов изображений для отправки поместите эти файлы в ZIP-архив, содержащий пакеты и изображения для описания для этой отправки, а затем передайте этот ZIP-архив URI подписанного URL-адреса (SAS) для отправки. Дополнительные сведения по передаче ZIP-архива URI подписанного URL-адреса (SAS) см. в разделе [Создание отправки приложения](#create-an-app-submission).

```json
{
  "trailers": [
    {
      "id": "1158943556954955699",
      "videoFileName": "Trailers\\ContosoGameTrailer.mp4",
      "videoFileId": "1159761554639123258",
      "trailerAssets": {
        "en-us": {
          "title": "Contoso Game",
          "imageList": [
            {
              "fileName": "Images\\ContosoGame-Thumbnail.png",
              "id": "1155546904097346923",
              "description": "This is a still image from the video."
            }
          ]
        }
      }
    }
  ]
}
```

У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание        |
|-----------------|---------|------|
|  id               |    string     |   Идентификатор для трейлера. Это значение предоставляется центром партнеров.   |
|  videoFileName               |    string     |    Имя файла видеотрейлера в ZIP-архиве, который содержит файлы для отправки.    |     
|  videoFileId               |   string      |  Идентификатор файла видеотрейлера. Это значение предоставляется центром партнеров.   |     
|  trailerAssets               |   object      |  Словарь пар "ключ-значение", где каждый ключ является кодом языка, а каждое значение — [набором ресурсов трейлера](#trailer-assets-object), содержащим относящиеся к языковым стандартам ресурсы для трейлера. Дополнительные сведения о кодах поддерживаемых языков см. в разделе [Поддерживаемые языки](https://docs.microsoft.com/windows/uwp/publish/supported-languages).    |     

> [!NOTE]
> Ресурс *trailers* был добавлен в мае 2017 г. после первого выпуска API отправки в Microsoft Store для разработчиков. Если вы создали отправку для приложения с помощью API отправки до появления этого ресурса и эта отправка все еще выполняется, этот ресурс будет иметь значение null для отправок приложения, пока вы успешно не выполните отправку или не удалите ее. Если ресурс *trailers* недоступен для отправок приложения, поле *hasAdvancedListingPermission* в [ресурсе Application](get-app-data.md#application_object), возвращаемом методом [получения приложения](get-an-app.md), будет иметь значение false.

<span id="trailer-assets-object" />

### <a name="trailer-assets-resource"></a>Набор ресурсов трейлера

Этот ресурс содержит дополнительные данные, относящиеся к языковым стандартам, для трейлера, который определен в [ресурсе трейлера](#trailer-object). У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание        |
|-----------------|---------|------|
| title   |   string      |  Локализованное название трейлера. Заголовок отображается, когда пользователь воспроизводит трейлер в полноэкранном режиме.     |  
| imageList    | array    |   Массив, содержащий один ресурс [image](#image-for-trailer-object), который предоставляет эскиз для трейлера. В этот массив можно включать только один ресурс [image](#image-for-trailer-object).  |   


<span id="image-for-trailer-object" />

### <a name="image-resource-for-a-trailer"></a>Ресурс изображения (для трейлера)

Этот ресурс описывает эскиз для трейлера. У этого ресурса есть следующие значения.

| Значение           | Тип    | Описание           |
|-----------------|---------|------|
|  fileName               |    string     |   Имя файла эскиза в ZIP-архиве, который был передан для отправки.    |     
|  id  |  string  | Идентификатор для эскиза. Это значение предоставляется центром партнеров.  |
|  description  |  string  | Описание эскиза. Это значение представляет собой лишь метаданные и не отображается пользователям.   |

<span/>

## <a name="enums"></a>Перечисления

Эти методы используют следующие перечисления.

<span id="price-tiers" />

### <a name="price-tiers"></a>Ценовые категории

Следующие значения представляют доступные ценовые категории в ресурсе [ценовой ресурс](#pricing-object) для отправки приложения.

| Значение           | Описание        |
|-----------------|------|
|  Система исчисления               |   Ценовая категория не задана; используется базовая цена для приложения.      |     
|  NotAvailable              |   Приложение недоступно в указанном регионе.    |     
|  Free              |   Бесплатное приложение.    |    
|  Tier*xxx*               |   Строка, указывающая ценовую категорию приложения в формате **Tier<em>xxxx</em>** . В настоящее время поддерживаются следующие ценовые категории.<br/><br/><ul><li>Если значение *isAdvancedPricingModel*[ценового ресурса](#pricing-object) — **true**, для вашей учетной записи доступны такие ценовые категории: **Tier1012** - **Tier1424**.</li><li>Если значение *isAdvancedPricingModel*[ценового ресурса](#pricing-object) — **false**, для вашей учетной записи доступны такие ценовые категории: **Tier2** - **Tier96**.</li></ul>Чтобы просмотреть полную таблицу ценовых уровней, доступных для вашей учетной записи разработчика, включая цены, связанные с каждым из уровней рынка, перейдите на страницу **цены и доступности** для всех отправок приложения в центре партнеров и щелкните ссылку **Просмотреть таблицу** в разделе " **рынки и настраиваемые цены** " (для некоторых учетных записей разработчиков эта ссылка находится в разделе " **цены** ").    |


<span id="enterprise-licensing" />

### <a name="enterprise-licensing-values"></a>Значения корпоративного лицензирования

Следующие значения представляют поведение корпоративного лицензирования для приложения. Дополнительные сведения об этих параметрах см. в разделе [Параметры корпоративного лицензирования](https://docs.microsoft.com/windows/uwp/publish/organizational-licensing).

> [!NOTE]
> Хотя параметры корпоративного лицензирования можно настроить для отправки приложения с помощью API отправки, этот API нельзя использовать для публикации отправок [для приобретения в больших объемах через Microsoft Store для бизнеса и Microsoft Store для образования](../publish/organizational-licensing.md). Чтобы опубликовать отправку в Microsoft Store для бизнеса и Microsoft Store для образовательных учреждений, необходимо использовать центр партнеров.


| Значение           |  Описание      |
|-----------------|---------------|
| Нет            |     Приложение недоступно организациям путем корпоративного лицензирования от Магазина (в режиме онлайн).         |     
| В сети        |     Приложение доступно организациям путем корпоративного лицензирования от Магазина (в режиме онлайн).  |
| OnlineAndOffline | Приложение доступно организациям путем корпоративного лицензирования от Магазина (в режиме онлайн); кроме того, приложение доступно организациям путем лицензирования без подключения (в автономном режиме). |


<span id="submission-status-code" />

### <a name="submission-status-code"></a>Код состояния отправки

Следующие значения представляют код состояния отправки.

| Значение           |  Описание      |
|-----------------|---------------|
| Нет            |     Код не указан.         |     
| InvalidArchive        |     ZIP-архив, содержащий пакет, является недопустимым или имеет неизвестный формат архива.  |
| MissingFiles | В ZIP-архиве отсутствуют файлы, перечисленные в данных отправки, или они находятся в неправильном месте в архиве. |
| PackageValidationFailed | Один или несколько пакетов в вашей отправке не прошли проверку. |
| InvalidParameterValue | Один из параметров в теле запроса не является допустимым. |
| InvalidOperation | Операция, которую вы пытались выполнить, является недопустимой. |
| InvalidState | Операция, которую вы пытались выполнить, является недопустимой для текущего состояния тестового пакета. |
| ResourceNotFound | Не удалось найти указанный тестовый пакет. |
| ServiceError | Запрос не был успешно выполнен из-за внутренней ошибки службы. Попробуйте повторить запрос. |
| ListingOptOutWarning | Разработчик удалил описание из предыдущей отправки или не включил данные описания, которые поддерживаются пакетом. |
| ListingOptInWarning  | Разработчик добавил описание. |
| UpdateOnlyWarning | Разработчик пытается вставить какой-то элемент, который имеет только поддержку обновления. |
| Прочее  | Отправка находится в неизвестном состоянии или состоянии, не отнесенном ни к одной из категорий. |
| PackageValidationWarning | В процессе проверки пакета создано предупреждение. |

<span/>

## <a name="related-topics"></a>Связанные разделы

* [Создание отправок и управление ими с помощью служб Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Получение данных приложения с помощью API отправки Microsoft Store](get-app-data.md)
* [Отправка приложений в центре партнеров](https://docs.microsoft.com/windows/uwp/publish/app-submissions)
