---
description: Используйте этот метод в API аналитики для Microsoft Store для получения трассировки стека для ошибки в Xbox One игры.
title: Получение трассировки стека при возникновении ошибки в Xbox One игры
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, службы Store, API аналитики для Microsoft Store, трассировка стека, ошибка
ms.localizationpriority: medium
ms.openlocfilehash: fd43305c54245c3281a0e840d3df4c5c87ff7ad8
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/27/2018
ms.locfileid: "7834213"
---
# <a name="get-the-stack-trace-for-an-error-in-your-xbox-one-game"></a>Получение трассировки стека при возникновении ошибки в Xbox One игры

Используйте этот метод в API аналитики Microsoft Store для получения трассировки стека для ошибки в Xbox One, игры, была добавлена через портал разработчика Xbox (XDP) и доступны на информационной панели центра партнеров аналитики XDP. Этот метод позволяет скачать трассировку стека только для ошибок, возникших за последние 30 дней.

Прежде чем использовать этот метод, сначала необходимо использовать метод [получения сведений об ошибке в игры Xbox One](get-details-for-an-error-in-your-xbox-one-game.md) для получения идентификатора CAB-файла, связанного с ошибкой, для которого требуется получить трассировку стека.

## <a name="prerequisites"></a>Необходимые условия


Для использования этого метода сначала необходимо сделать следующее:

* Если вы еще не сделали этого, выполните все [необходимые условия](access-analytics-data-using-windows-store-services.md#prerequisites) для API аналитики для Microsoft Store.
* [Получите маркер доступа Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), который будет использоваться в заголовке запроса этого метода. После получения маркера доступа у вас будет 60минут, чтобы использовать его до окончания срока действия маркера. После истечения срока действия токена можно получить новый токен.
* Получите идентификатор CAB-файла, связанного с ошибкой, для которой требуется получить трассировку стека. Для получения этого идентификатора используйте метод [получения сведений об ошибке в Xbox One игры](get-details-for-an-error-in-your-xbox-one-game.md) , чтобы получить подробные сведения об определенной ошибке в вашем приложении и используйте значение **cabId** в тексте ответа этого метода.

## <a name="request"></a>Запрос


### <a name="request-syntax"></a>Синтаксис запроса

| Метод | URI запроса                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace``` |


### <a name="request-header"></a>Заголовок запроса

| Заголовок        | Тип   | Описание                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Обязательный. Маркер доступа Azure AD в формате **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Параметры запроса

| Параметр        | Тип   |  Описание      |  Обязательный  |
|---------------|--------|---------------|------|
| applicationId | string | КОД продукта для игры Xbox One, по которому запрашиваются трассировку стека. Чтобы получить код продукта для игры, перейдите к игре на портале разработчиков для Xbox (XDP) и получите код продукта из URL-адреса. Кроме того Если загрузить данные работоспособности из аналитического отчета центра партнеров Windows, код продукта включается в TSV-файл. |  Да  |
| cabId | строка | Уникальный идентификатор CAB-файла, связанного с ошибкой, для которой требуется получить трассировку стека. Для получения этого идентификатора используйте метод [получения сведений об ошибке в Xbox One игры](get-details-for-an-error-in-your-xbox-one-game.md) , чтобы получить подробные сведения об определенной ошибке в вашем приложении и используйте значение **cabId** в тексте ответа этого метода. |  Да  |

 
### <a name="request-example"></a>Пример запроса

Следующий пример демонстрирует получение трассировки стека для Xbox One игры с помощью этого метода. Замените значение *applicationId* код продукта для вашей игры.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace?applicationId=BRRT4NJ9B3D1&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Ответ


### <a name="response-body"></a>Тело ответа

| Значение      | Тип    | Описание                  |
|------------|---------|--------------------------------|
| Значение      | array   | Массив объектов, каждый из которых содержит один кадр данных трассировки стека. Дополнительные сведения о данных в каждом объекте см. далее в разделе [Значения трассировки стека](#stack-trace-values). |
| @nextLink  | string  | При наличии дополнительных страниц данных эта строка содержит универсальный код ресурса (URI), который можно использовать для запроса следующей страницы данных. Например, это значение возвращается в том случае, если параметр **top** запроса имеет значение 10 000, но для данного запроса имеется больше 10 000 строк с информацией об ошибках. |
| TotalCount | integer | Общее количество строк в результирующих данных для запроса.          |


### <a name="stack-trace-values"></a>Значения трассировки стека

Элементы в массиве *Value* содержат следующие значения.

| Значение           | Тип    | Описание      |
|-----------------|---------|----------------|
| level            | строка  |  Номер кадра, который этот элемент представляет в стеке вызовов.  |
| image   | строка  |   Имя образа исполняемого файла или библиотеки, содержащего функцию, которая вызывается в этом кадре стека.           |
| function | строка  |  Имя функции, вызываемой в этом кадре стека. Эта функция доступна только в том случае, если ваша игра содержит символы из исполняемого файла или библиотеки.              |
| offset     | строка  |  Смещение в байтах текущей инструкции относительно начала функции.      |


### <a name="response-example"></a>Пример ответа

В следующем примере демонстрируется пример тела ответа JSON на данный запрос.

```json
{
  "Value": [
    {
      "level": "0",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.DoWork",
      "offset": "0x25C"
    }
    {
      "level": "1",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.Initialize",
      "offset": "0x26"
    }
    {
      "level": "2",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.Start",
      "offset": "0x66"
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}

```

## <a name="related-topics"></a>Статьи по теме

* [Доступ к аналитическим данным с помощью служб Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Получение данных системы отчетов для Xbox One игры](get-error-reporting-data-for-your-xbox-one-game.md)
* [Получение сведений об ошибке в Xbox One игры](get-details-for-an-error-in-your-xbox-one-game.md)
* [Скачать CAB-файл для ошибки в игры Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)