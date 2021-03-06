---
description: Используйте этот метод в API аналитики для Microsoft Store для получения данных клуба Xbox Live.
title: Получение данных клуба в Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, uwp, службы Store, API аналитики для Microsoft Store, аналитика Xbox Live, клубы
ms.localizationpriority: medium
ms.openlocfilehash: e5fc116c2b868ddf093aabea09d59934301f49ec
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321864"
---
# <a name="get-xbox-live-club-data"></a>Получение данных клуба в Xbox Live

Используйте этот метод в API аналитики для Microsoft Store для получения данных клуба для своей [игры с поддержкой Xbox Live](https://docs.microsoft.com/gaming/xbox-live/index.md). Эта информация также доступна в [отчет по аналитике в Xbox](../publish/xbox-analytics-report.md) в центре партнеров.

> [!IMPORTANT]
> Этот метод поддерживает только игры для Xbox или игры, использующие службы Xbox Live. Эти игры, в том числе игры, опубликованные [партнерами Майкрософт](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#microsoft-partners), и отправленные в рамках программы [ID@Xbox, должны пройти [процесс утверждения концепции](../gaming/concept-approval.md)](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#id). В настоящее время этот метод не поддерживает игры, опубликованные в рамках программы [Xbox Live Creators Program](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

## <a name="prerequisites"></a>Предварительные требования

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
| applicationId | строка | [Код продукта в Store](in-app-purchases-and-trials.md#store-ids) для игры, по которой требуется получить данные клуба Xbox Live.  |  Да  |
| metricType | строка | Строка, определяющая тип аналитических данных Xbox Live, которые необходимо получить. Для этого метода укажите значение **communitymanagerclub**.  |  Да  |
| startDate | date | Начальная дата диапазона дат, для которого требуется получить данные клуба. По умолчанию используется текущая дата минус 30 дней. |  Нет  |
| endDate | date | Конечная дата диапазона дат, для которого требуется получить данные клуба. По умолчанию используется текущая дата. |  Нет  |
| top | ssNoversion | Количество строк данных, возвращаемых в запросе. Максимальное значение и значение по умолчанию (если параметр не указан) — 10 000. Если в запросе содержится больше строк, то тело ответа будет содержать ссылку "Далее", которую можно использовать для запроса следующей страницы данных |  Нет  |
| skip | ssNoversion | Количество строк, пропускаемых в запросе. Используйте этот параметр для постраничного перемещения по большим наборам данных. Например, при top=10000 и skip=0 извлекаются первые 10 000 строк данных; при top=10000 и skip=10000 извлекаются следующие 10 000 строк данных и т. д. |  Нет  |


### <a name="request-example"></a>Пример запроса

Ниже приведен пример запроса на получение данных клуба для вашей игры с поддержкой Xbox Live. Замените значение *applicationId* кодом продукта в Store для вашей игры.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=communitymanagerclub&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Ответ

| Значение      | Тип   | Описание                  |
|------------|--------|-------------------------------------------------------|
| Значение      | array  | Массив, содержащий один объект [ProductData](#productdata), содержащий данные клубов, относящихся к вашей игре, и один объект [XboxwideData](#xboxwidedata), содержащий данные клубов для всех пользователей Xbox Live. Эти данные включаются для сравнения с данными по вашей игре.  |
| @nextLink  | строка | При наличии дополнительных страниц данных эта строка содержит универсальный код ресурса (URI), который можно использовать для запроса следующей страницы данных. Например, это значение возвращается в том случае, если параметр **top** запроса имеет значение 10000, но для данного запроса имеется больше 10000 строк данных. |
| TotalCount | ssNoversion    | Общее количество строк в результирующих данных для запроса. |


### <a name="productdata"></a>ProductData

Этот ресурс содержит данные клуба для вашей игры.

| Значение           | Тип    | Описание        |
|-----------------|---------|------|
| date            |  строка |   Дата для данных клуба.   |
|  applicationId               |    строка     |  [Код продукта в Store](in-app-purchases-and-trials.md#store-ids) для игры, по которой получены данные клуба.   |
|  clubsWithTitleActivity               |    ssNoversion     |  Число клубов, участвующих в социальных взаимодействиях с вашей игрой.   |     
|  clubsExclusiveToGame               |   ssNoversion      |  Число клубов, участвующих в социальных взаимодействиях только с вашей игрой.   |     
|  clubFacts               |   array      |   Содержит один или несколько объектов [ClubFacts](#clubfacts) по всем клубам, участвующим в социальных взаимодействиях с вашей игрой.   |


### <a name="xboxwidedata"></a>XboxwideData

Этот ресурс содержит среднее значение данных клуба по всем пользователям Xbox Live.

| Значение           | Тип    | Описание        |
|-----------------|---------|------|
| date            |  строка |   Дата для данных клуба.   |
|  applicationId  |    строка     |   В объекте **XboxwideData** эта строка всегда является значением **XBOXWIDE**.  |
|  clubsWithTitleActivity               |   ssNoversion     |  Среднее число клубов с пользователями, которые участвуют в социальных взаимодействиях с игрой с поддержкой Xbox Live.    |     
|  clubsExclusiveToGame               |   ssNoversion      |  Среднее число клубов с пользователями, которые участвуют в социальных взаимодействиях только с определенной игрой с поддержкой Xbox Live.   |     
|  clubFacts               |   Объект      |  Содержит один объект [ClubFacts](#clubfacts). Этот объект не имеет смысла в контексте объекта **XboxwideData** и содержит значения по умолчанию.  |


### <a name="clubfacts"></a>ClubFacts

В объекте **ProductData** этот объект содержит данные для конкретного клуба, в котором есть активность, связанная с вашей игрой. Этот объект не имеет смысла в контексте объекта **XboxwideData** и содержит значения по умолчанию.

| Значение           | Тип    | Описание        |
|-----------------|---------|--------------------|
|  name            |  строка  |   В объекте **ProductData** это имя клуба. В объекте **XboxwideData** этот элемент всегда имеет значение **XBOXWIDE**.           |
|  memberCount               |    ssNoversion     | В объекте **ProductData**это число членов клуба, за исключением членов, которые просто посещают клуб. В объекте **XboxwideData** это всегда 0.    |
|  titleSocialActionsCount               |    ssNoversion     |  В объекте **ProductData** это количество социальных действий, которые выполнили члены в клубе, относящихся к вашей игре. В объекте **XboxwideData** это всегда 0.   |
|  isExclusiveToGame               |    Boolean (Логическое)     |  В объекте **ProductData** это означает, участвует ли текущий клуб в социальных взаимодействиях исключительно с вашей игрой. В объекте **XboxwideData** это всегда true.  |


### <a name="response-example"></a>Пример ответа

В следующем примере демонстрируется пример тела ответа JSON на данный запрос.

```json
{
  "Value": [
    {
      "ProductData": [
        {
          "date": "2018-01-30",
          "applicationId": "9NBLGGGZ5QDR",
          "clubsWithTitleActivity": 3,
          "clubsExclusiveToGame": 1,
          "clubFacts": [
            {
              "name": "Club Contoso Racing",
              "memberCount": 1,
              "titleSocialActionsCount": 1,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Sports",
              "memberCount": 12,
              "titleSocialActionsCount": 9,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Maze",
              "memberCount": 4,
              "titleSocialActionsCount": 2,
              "isExclusiveToGame": false
            }
          ]
        }
      ],
      "XboxwideData": [
        {
          "date": "2018-01-30",
          "applicationId": "XBOXWIDE",
          "clubsWithTitleActivity": 25,
          "clubsExclusiveToGame": 3,
          "clubFacts": [
            {
              "name": "XBOXWIDE",
              "memberCount": 0,
              "titleSocialActionsCount": 0,
              "isExclusiveToGame": true
            }
          ]
        }
      ]
    }
  ],
  "@nextLink": "gameanalytics?applicationId=9NBLGGGZ5QDR&metricType=communitymanagerclub&top=1&skip=1&startDate=2018/01/04&endDate=2018/02/02&orderby=date desc",
  "TotalCount": 27
}
```

## <a name="related-topics"></a>См. также

* [Доступ к данным аналитики с помощью служб Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Получить аналитические данные Xbox Live](get-xbox-live-analytics.md)
* [Получение данных достижения Xbox Live](get-xbox-live-achievements-data.md)
* [Получение данных работоспособности Xbox Live](get-xbox-live-health-data.md)
* [Получение данных концентратора игр Xbox Live](get-xbox-live-game-hub-data.md)
* [Получение данных в многопользовательской, Xbox Live](get-xbox-live-multiplayer-data.md)
