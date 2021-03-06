---
description: Используйте этот метод в API аналитики для Microsoft Store для получения данных одновременного использования Xbox Live.
title: Получение данных параллельного использования Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, uwp, службы Store, API аналитики для Microsoft Store, аналитика Xbox Live, одновременное использование
ms.localizationpriority: medium
ms.openlocfilehash: a1ceef92a533a230c2dca54a835578b56ceb809f
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321763"
---
# <a name="get-xbox-live-concurrent-usage-data"></a>Получение данных параллельного использования Xbox Live


Используйте этот метод в API аналитики для Microsoft Store, чтобы получить данные в режиме близости к реальному времени (с задержкой в 5-15 минут) о среднем числе пользователей, играющих в вашу [игру с поддержкой Xbox Live](https://docs.microsoft.com/gaming/xbox-live/index.md) в каждую минуту, час или день в течение заданного интервала времени. Эта информация также доступна в [отчет по аналитике в Xbox](../publish/xbox-analytics-report.md) в центре партнеров.

> [!IMPORTANT]
> Этот метод поддерживает только игры для Xbox или игры, использующие службы Xbox Live. Эти игры, в том числе игры, опубликованные [партнерами Майкрософт](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#microsoft-partners), и отправленные в рамках программы [ID@Xbox, должны пройти [процесс утверждения концепции](../gaming/concept-approval.md)](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#id). В настоящее время этот метод не поддерживает игры, опубликованные в рамках программы [Xbox Live Creators Program](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

## <a name="prerequisites"></a>предварительные требования

Для использования этого метода сначала необходимо сделать следующее:

* Если вы еще не сделали этого, выполните все [необходимые условия](access-analytics-data-using-windows-store-services.md#prerequisites) для API аналитики для Microsoft Store.
* [Получите маркер доступа Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), который будет использоваться в заголовке запроса этого метода. После получения токена доступа у вас будет 60 минут, чтобы использовать его до окончания его срока действия. После истечения срока действия токена можно получить новый токен.

## <a name="request"></a>Запрос


### <a name="request-syntax"></a>Синтаксис запроса

| Метод | Универсальный код ресурса (URI) запроса       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>Заголовок запроса

| Header        | Тип   | Описание                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | строка | Обязательный. Маркер доступа Azure AD в форме **носителя** &lt; *маркера*&gt;. |


### <a name="request-parameters"></a>Параметры запроса


| Параметр        | Тип   |  Описание      |  Обязательно  
|---------------|--------|---------------|------|
| applicationId | строка | [Код продукта в Store](in-app-purchases-and-trials.md#store-ids) для игры, по которой требуется получить данные одновременного использования Xbox Live.  |  Да  |
| metricType | строка | Строка, определяющая тип аналитических данных Xbox Live, которые необходимо получить. Для этого метода укажите значение **concurrency**.  |  Да  |
| startDate | date | Начальная дата диапазона дат, для которого требуется получить данные одновременного использования. См. описание *aggregationLevel*, чтобы узнать поведение по умолчанию. |  Нет  |
| endDate | date | Конечная дата диапазона дат, для которого требуется получить данные одновременного использования. См. описание *aggregationLevel*, чтобы узнать поведение по умолчанию. |  Нет  |
| aggregationLevel | строка | Определяет диапазон времени, для которого требуется получить сводные данные. Можно использовать следующие строки: **minute**, **hour** или **day**. Если параметр не задан, значение по умолчанию — **day**. <p/><p/>Если вы не укажете *startDate* или *endDate*, по умолчанию в теле ответа содержится следующее: <ul><li>**минуты**: Последние 60 записей доступных данных.</li><li>**час**: За последние 24 записи доступных данных.</li><li>**день**: Последние 7 записей доступных данных.</li></ul><p/>Следующие уровни агрегирования имеют ограничения размера по количеству записей, которые могут быть возвращены. Если диапазон запрошенного времени слишком велик, записи будут усечены. <ul><li>**минуты**: До 1440 записей (данные за 24 часа).</li><li>**час**: До 720 записей (данные за 30 дней).</li><li>**день**: До 60 записей (60 дней данных).</li></ul>  |  Нет  |


### <a name="request-example"></a>Пример запроса

Ниже приведен пример запроса на получение данных одновременного использования для вашей игры с поддержкой Xbox Live. Этот запрос извлекает данные по каждой минуте от 1 февраля 2018 г. до 2 февраля 2018 г. Замените значение *applicationId* кодом продукта в Store для вашей игры.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=concurrency&aggregationLevel=hour&startDate=2018-02-01&endData=2018-02-02 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Ответ

Тело ответа содержит массив объектов, каждый из которых содержит один набор данных одновременного использования для заданной минуты, часа или дня. Каждый объект содержит следующие значения.

| Значение      | Тип   | Описание                  |
|------------|--------|-------------------------------------------------------|
| Count      | number  | Среднее количество пользователей, игравших в вашу игру с поддержкой Xbox Live в заданную минуту, час или день. <p/><p/>**Примечание.** &nbsp;&nbsp;Значение 0 означает, что в определенный промежуток времени не было одновременно игравших пользователей, либо что произошел сбой в процессе сбора данных одновременного использования для игры за определенный промежуток времени. |
| Date  | строка | Дата и время указывает минуту, час или день, к которым относятся данные одновременного использования.  |
| SeriesName | строка    | Всегда имеет значение **UserConcurrency** . |


### <a name="response-example"></a>Пример ответа

В следующем примере демонстрируется пример текста ответа JSON на данный запрос с поминутным агрегированием данных.

```json
[   {
        "Count": 418.0,
        "Date": "2018-02-02T04:42:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 418.0,
        "Date": "2018-02-02T04:43:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 415.0,
        "Date": "2018-02-02T04:44:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 412.0,
        "Date": "2018-02-02T04:45:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 414.0,
        "Date": "2018-02-02T04:46:13.65Z",
        "SeriesName": "UserConcurrency"
    }
]
```

## <a name="related-topics"></a>См. также

* [Доступ к данным аналитики с помощью служб Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Получить аналитические данные Xbox Live](get-xbox-live-analytics.md)
* [Получение данных достижения Xbox Live](get-xbox-live-achievements-data.md)
* [Получение данных работоспособности Xbox Live](get-xbox-live-health-data.md)
* [Получение данных концентратора игр Xbox Live](get-xbox-live-game-hub-data.md)
* [Получение данных клуба Xbox Live](get-xbox-live-club-data.md)
* [Получение данных в многопользовательской, Xbox Live](get-xbox-live-multiplayer-data.md)
