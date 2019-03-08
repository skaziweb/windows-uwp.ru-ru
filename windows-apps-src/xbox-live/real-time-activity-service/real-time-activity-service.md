---
title: Служба в режиме реального времени действия
description: Дополнительные сведения о службе Live активности в реальном времени для Xbox.
ms.assetid: 50de262f-fc55-4301-83b5-0a8a30bc7852
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, игры, универсальной платформы Windows, windows 10, xbox, один, служба действия в режиме реального времени.
ms.localizationpriority: medium
ms.openlocfilehash: 36389fac3bd6dea2d2e24c0935087781118d8046
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592089"
---
# <a name="real-time-activity-service"></a>Служба в режиме реального времени действия

Служба в реальном времени действия возврата позволяет приложению на любом устройстве, для подписки на данные о состоянии, Статистика по пользователям и присутствия. Система допускает подписки в собственных данных и данных других пользователей в названии ни одного зависимости от параметров безопасности. Это позволяет поток данных без необходимости постоянно выполнять запрос, чтобы получить последние данные.


## <a name="developer-scenarios"></a>Сценарии для разработчика

Существует множество сценариев, поддерживаемых возврата. Здесь перечислены только некоторые из них, но реальная мощь возврата является многие сценарии, которые можно будет придумать что мы еще не представить себе еще. Может помочь определить следующее поколение игровой процесс, где пользователи часто иметь под рукой свой Microsoft Surface и Apple iPad при взаимодействии с заголовок вашей консоли. Возврата WebSocket технология используется, поэтому в различных пошаговых руководствах подраздела включает в себя сведения о реализации, с помощью WebSockets API, предоставляемые Windows.

Ниже приведены некоторые простые сценарии, которые можно создать с помощью возврата названия.

-   Приложение ход достижения
-   Приложение игр справки
-   Игра от средства просмотра приложения
-   Средство просмотра статистики
-   Средство просмотра присутствия


## <a name="achievements-progress-app"></a>Приложение ход достижения

Пользователи, подготавливающим почти всегда их хода выполнения определенных достижений, особенно для достижения, требующие выполнения действия определенное число раз. В режиме реального времени доступ к статистике пользователя (который объединяются в службе Xbox динамическая Статистика проигрывателя) можно представить в режиме реального времени для игроков и их его друзьям достижений и этапов, на Xbox One или на устройство-компаньон, пользователю воспроизводится название.


## <a name="game-help-app"></a>Приложение игр справки

Как пользователи переходят по заголовка, в режиме реального времени доступа к данным позволяет представить игр справки, либо рядом с работу на Xbox One или на любые устройства-компаньона. Пользователей может возникать новое сопоставление, нового курса или сложной борьбе начальника, и дополнительное вашей игры помочь смог отобразить пользовательские или сгенерированные разработчика видео и текст, помогающий пользователем при выполнении взаимодействия в заголовок вашей.


## <a name="squad-viewer-app"></a>Игра от средства просмотра приложения

В многопользовательских играх кооперативного игрока и его коллеги совместно для общей цели. С помощью так много проигрывателей бывает для отслеживания общей картины. Доступ к данным в режиме реального времени можно создать дополнительное приложение, которое показывает высокого уровня карты и тепловых карт, из которых может быть действие.


## <a name="statistics-viewer"></a>Средство просмотра статистики

Хотя обычно можно рассматривать карт разрабатываются дополнительные приложения при рассмотрении возврата, также можно использовать для возврата в заголовок вашей core. Например возврата можно использовать для предоставления проигрыватель игры с отображением всех пользователей текущей статистики в игры, возможно проигрывателем, просто нажав клавишу «представление» на контроллере в многопользовательской совпадение.


## <a name="presence-viewer"></a>Средство просмотра присутствия

В основной, полезно иметь актуальные какие друзей подключены к сети, и если они воспроизводят тот же заголовок. Можно подписаться на присутствие пользователя друзей и показать, какие друзей публикуются в Интернете, если они начать воспроизведение название, все в режиме реального времени.


## <a name="subscription-privacy-and-authorization"></a>Подписка конфиденциальности и авторизации

Последнюю версию возврата включает проверки для обеспечения конфиденциальности и изоляции авторизации/content. До тех пор, пока выполняются проверки авторизации и конфиденциальности, приложения могут подписываться на любого статистического показателя, который помечен как возврата с поддержкой. (Дополнительные сведения о том, как сделать статистики возврата включена, см. в разделе [зарегистрироваться для получения уведомлений stat](register-for-stat-notifications.md). Отсутствуют ограничения на виды статистические данные, которые можно создать с поддержкой возврата — именно для вас как разработчика. Однако есть ограничение *номер* статистики, пользователь может подписаться на сеанс приложения. По достижении этого предела, он или она появится сообщение об ошибке для следующей подписки.


## <a name="in-this-section"></a>В этом разделе

[Регистрация проигрывателя stat уведомлений об изменениях](register-for-stat-notifications.md)  
Описывает способ включения в реальном времени действия возврата для статистики или сведений о состоянии.

[Программирование службы действий в реальном времени (возврата), с помощью API-интерфейсы winrt](programming-the-real-time-activity-service.md)  
В этой статье описывается программирование службы в реальном времени действия возврата, с помощью API-интерфейсы WinRT.

[Программирование службу действий в реальном времени (возврата), через интерфейс restful](programming-the-real-time-activity-service.md)  
В этой статье описывается программирование службы в реальном времени действия возврата, с помощью интерфейса RESTful.

[Рекомендации в режиме реального времени действия (возврата)](rta-best-practices.md)  
Рекомендации по использованию службу Xbox служб реального времени действия возврата для подписки на статистические данные и данные о состоянии из платформы данных Xbox.