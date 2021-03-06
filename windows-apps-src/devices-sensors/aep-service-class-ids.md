---
ms.assetid: 23001DA5-C099-4C02-ACE9-3597F06ECBF4
title: Идентификаторы класса службы AEP
description: Службы конечной точки связи (AEP) предоставляют программный контракт для служб, поддерживаемых устройством по определенному протоколу. У некоторых из этих служб имеются установленные идентификаторы, которые следует использовать при обращении к ним.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 67bba732efd199c5093bb75e9b0a2c41b67e568c
ms.sourcegitcommit: 28bd367ab8acc64d4b6f3f73adca12100cbd359f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/24/2020
ms.locfileid: "82148574"
---
# <a name="aep-service-class-ids"></a>Идентификаторы класса службы AEP



**Важные API**

- [**Windows. Devices. Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)

Службы конечной точки связи (AEP) предоставляют программный контракт для служб, поддерживаемых устройством по определенному протоколу. У некоторых из этих служб имеются установленные идентификаторы, которые следует использовать при обращении к ним. Эти контракты определяются с помощью свойства **System.Devices.AepService.ServiceClassId**. В этом разделе перечислены некоторые распространенные идентификаторы класса службы AEP. Идентификаторы класса службы AEP также можно применять к протоколам с настраиваемыми идентификаторами класса.

Разработчикам приложений следует использовать фильтры расширенного синтаксиса запросов (AQS) на основе идентификатора класса, чтобы ограничить количество запросов к службам AEP, которые они планируют использовать. Таким образом, результаты запросов будут поступать только от необходимых служб, что значительно увеличит производительность, время работы от батареи и качество обслуживания устройства. Например, приложение может применять эти идентификаторы класса службы, чтобы использовать устройство в качестве обработчика синхронизации Miracast или мультимедиа DLNA. Дополнительные сведения о взаимодействии устройств и служб см. в разделе [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind).

## <a name="bluetooth-and-bluetooth-le-services"></a>Bluetooth и службы Bluetooth с низким энергопотреблением

Службы Bluetooth используют один из двух протоколов — Bluetooth или Bluetooth с низким энергопотреблением. Эти протоколы имеют такие идентификаторы:

-   Идентификатор протокола Bluetooth: {e0cbf06c-cd8b-4647-bb8a-263b43f0f974}
-   Идентификатор протокола Bluetooth с низким энергопотреблением: {bb7bb05e-5972-42b5-94fc-76eaa7084d49}

Протокол Bluetooth поддерживает ряд служб, в которых используется один и тот же базовый формат. Первые четыре цифры GUID меняются в зависимости от службы, но все идентификаторы GUID для Bluetooth заканчиваются на **0000-0000-1000-8000-00805F9B34FB**. Например, для службы RFCOMM используются цифры 0x0003 в начале, поэтому полный идентификатор будет иметь вид **00030000-0000-1000-8000-00805F9B34FB**. В таблице ниже приведены некоторые из основных служб Bluetooth.

| Имя службы                         | Идентификатор GUID                                     |
|--------------------------------------|------------------------------------------|
| RFCOMM                               | **00030000-0000-1000-8000-00805F9B34FB** |
| GATT — Alert notification service (Служба уведомлений)    | **18110000-0000-1000-8000-00805F9B34FB** |
| GATT — Automation IO (Автоматизация ввода-вывода)                 | **18150000-0000-1000-8000-00805F9B34FB** |
| GATT — Battery service (Служба батареи)               | **180F0000-0000-1000-8000-00805F9B34FB** |
| GATT — Blood pressure (Кровяное давление)                | **18100000-0000-1000-8000-00805F9B34FB** |
| GATT — Body composition (Состав тела)              | **181B0000-0000-1000-8000-00805F9B34FB** |
| GATT — Bond management (Управление связями)               | **181E0000-0000-1000-8000-00805F9B34FB** |
| GATT — Continuous glucose monitoring (Непрерывное отслеживание уровня глюкозы) | **181F0000-0000-1000-8000-00805F9B34FB** |
| GATT — Current time service (Служба текущего времени)          | **18050000-0000-1000-8000-00805F9B34FB** |
| GATT — Cycling power (Сила вращения)                 | **18180000-0000-1000-8000-00805F9B34FB** |
| GATT — Cycling speed and cadence (Скорость и частота вращения)     | **18160000-0000-1000-8000-00805F9B34FB** |
| GATT — Device information (Сведения об устройстве)            | **180A0000-0000-1000-8000-00805F9B34FB** |
| GATT — Environmental sensing (Обследование состояния окружающей среды)         | **181A0000-0000-1000-8000-00805F9B34FB** |
| GATT — Generic access (Универсальный доступ)                | **18000000-0000-1000-8000-00805F9B34FB** |
| GATT — Generic attribute (Универсальный атрибут)             | **18010000-0000-1000-8000-00805F9B34FB** |
| GATT — Glucose (Глюкоза)                       | **18080000-0000-1000-8000-00805F9B34FB** |
| GATT — Health thermometer (Медицинский термометр)            | **18090000-0000-1000-8000-00805F9B34FB** |
| GATT — Heart rate (Частота пульса)                    | **180D0000-0000-1000-8000-00805F9B34FB** |
| GATT — Human interface device (Устройство HID)        | **18120000-0000-1000-8000-00805F9B34FB** |
| GATT — Immediate alert (Мгновенное оповещение)               | **18020000-0000-1000-8000-00805F9B34FB** |
| GATT — Indoor positioning (Позиционирование в помещении)            | **18210000-0000-1000-8000-00805F9B34FB** |
| GATT — Internet protocol support (Поддержка протокола IP)     | **18200000-0000-1000-8000-00805F9B34FB** |
| GATT — Link loss (Потеря связи)                     | **18030000-0000-1000-8000-00805F9B34FB** |
| GATT — Location and navigation (Расположение и навигация)       | **18190000-0000-1000-8000-00805F9B34FB** |
| GATT — Next DST change service (Служба следующего перехода на летнее время)       | **18070000-0000-1000-8000-00805F9B34FB** |
| GATT — Phone alert status service (Служба оповещения о статусе телефона)    | **180E0000-0000-1000-8000-00805F9B34FB** |
| GATT — Pulse oximeter (Пульсовой оксиметр)                | **18220000-0000-1000-8000-00805F9B34FB** |
| GATT — Reference time update service (Служба обновления начала отсчета времени) | **18060000-0000-1000-8000-00805F9B34FB** |
| GATT — Running speed and cadence (Скорость и темп бега)     | **18140000-0000-1000-8000-00805F9B34FB** |
| GATT — Scan parameters (Параметры сканирования)               | **18130000-0000-1000-8000-00805F9B34FB** |
| GATT — Tx power (Питание для передачи)                      | **18040000-0000-1000-8000-00805F9B34FB** |
| GATT — User data (Данные пользователя)                     | **181C0000-0000-1000-8000-00805F9B34FB** |
| GATT — Weight scale (Шкала масс)                  | **181D0000-0000-1000-8000-00805F9B34FB** |

 

Более полный список доступных служб Bluetooth см. на страницах о протоколе и службах Bluetooth [здесь](https://www.bluetooth.org/en-us/specification/assigned-numbers/service-discovery) и [здесь](https://go.microsoft.com/fwlink/p/?LinkID=619587). Вы также можете использовать API [**GattServiceUuids**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile.GattServiceUuids), чтобы получить доступ к некоторым распространенным службам GATT.

## <a name="custom-bluetooth-le-services"></a>Настраиваемые службы Bluetooth с низким энергопотреблением

Настраиваемые службы Bluetooth с низким энергопотреблением используют идентификатор протокола {bb7bb05e-5972-42b5-94fc76eaa7084d49}.

Настраиваемые профили имеют собственные идентификаторы GUID. Этот пользовательский GUID следует использовать для идентификатора **System.Devices.AepService.ServiceClassId**.

## <a name="upnp-services"></a>Службы UPnP

Службы UPnP используют следующий идентификатор протокола: {0e261de4-12f0-46e6-91ba-428607ccef64}

Как правило, имена всех служб UPnP хэшируются в GUID с помощью алгоритма, определенного в RFC 4122. В таблице ниже приведены некоторые распространенные службы UPnP, определенные в Windows.

| Имя службы                       | Идентификатор GUID                                      |
|------------------------------------|-------------------------------------------|
| Диспетчер соединений                 | **ba36014c-b51f-51cc-bf71-1ad779ced3c6**  |
| AV transport (транспорт аудио/видео)                       | **deeacb78-707a-52df-b1c6-6f945e7e25bf**  |
| Rendering control (управление отрисовкой)                  | **cc7fe721-a3c7-5a14-8c49-4419dc895513**  |
| Layer 3 forwarding (перенаправление уровня 3)                 | **97d477fa-f403-577b-a714-b29a9007797f**  |
| WAN common interface configuration (конфигурация общего интерфейса WAN) | **e4c1c624-c3c4-5104-b72e-ac425d9d157c**  |
| WAP IP connection (подключение к WAP IP)                  | **e4ac1c23-b5ac-5c27-8814-6bd837d8832c**  |
| WFA WLAN configuration (конфигурация WFA WLAN)             | **23d5f7db-747f-5099-8f21-3ddfd0c3c688**  |
| Printer enhanced (принтер расширенный)                   | **fb9074da-3d9f-5384-922e-9978ae51ef0c**  |
| Printer basic (принтер базовый)                      | **5d2a7252-d45c-5158-87a4-05212da327e1**  |
| Media receiver registrar (регистратор приемника мультимедиа)           | **0b4a2add-d725-5198-b2ba-852b8bf8d183**  |
| Content directory (каталог содержимого)                  | **89e701dd-0597-5279-a31c-235991d0db1c**  |
| DIAL                               | **085dfa4a-3948-53c7-a0d7-16d8ec26b29b**  |

 

## <a name="wsd-services"></a>WSD services (службы WSD)

Службы WSD используют следующий идентификатор протокола: {782232aa-a2f9-4993-971b-aedc551346b0}

Как правило, имена всех служб WSD хэшируются в GUID с помощью алгоритма, определенного в RFC 4122. В таблице ниже приведены некоторые распространенные службы WSD, определенные в Windows.

| Имя службы | Идентификатор GUID                                     |
|--------------|------------------------------------------|
| Принтер      | **65dca7bd-2611-583e-9a12-ad90f47749cf** |
| Сканер      | **56ec8b9e-0237-5cae-aa3f-d322dd2e6c1e** |

 

## <a name="aqs-sample"></a>Пример AQS

Этот код AQS фильтрует все UPnP-объекты **AssociationEndpointService**, которые поддерживают DIAL. В этом случае свойству [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) присваивается значение **AsssociationEndpointService**.

``` syntax
System.Devices.AepService.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}" AND
System.Devices.AepService.ServiceClassId:="{085DFA4A-3948-53C7-A0D7-16D8EC26B29B}"
```

 

 
