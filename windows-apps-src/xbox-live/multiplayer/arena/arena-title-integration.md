---
title: Руководство по интеграции решают title
description: Узнайте, как создавать заголовок в поддержке для платформы Xbox Live решают.
ms.assetid: 470914df-cbb5-4580-b33a-edb353873e32
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, игры, универсальной платформы Windows, windows 10, xbox, один, решают, турнира
ms.localizationpriority: medium
ms.openlocfilehash: 891fa8da1ca6e26128e0a33d28a505a18e99662a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659889"
---
# <a name="arena-title-integration-guide"></a>Руководство по интеграции решают title

## <a name="introduction"></a>Введение

Турнира организаторов (TOs) для создания и эксплуатации соревнования для заголовков, которые поддерживают решают разрешено решают платформой для Xbox. Arena поддерживает как Xbox Live для, позволяющий издателя и соревнования пользователей, так и независимых производителей и инструкции, которые интегрируются с решают. Стиль турнира, например одного исключения или Швейцарии, определяется TO.

Эта статья описывает, как разработчик title Пошаговое руководство по реализации поддержки решают в заголовок вашей. После того как игра решают включен, он будет работать в любой поддерживаемой турнира по TO. Кому организует совпадения названия в соответствии с структура турнира. Заголовок с поддержкой решают затем помещает нужным пользователям в каждое совпадение в игры и выведет результаты сопоставления to.

## <a name="overview"></a>Обзор

Xbox решают использует концепцию, знакомую разработке многопользовательских игр Xbox. Если вы не знакомы с многопользовательской системой Xbox, рекомендуется ознакомиться с этой документации, прежде чем продолжить. Процесс аналогичен пользователя принять приглашение в игры. Когда все готово для пользователя для воспроизведения совпадение в турнира решают всплывающее уведомление появляется для уведомления пользователя. Принятие тост вызывает название для получения стандартное событие активации решают протокола. Если название еще не запущена, он будет запущен в данный момент.

Само совпадение турнира координируется с помощью сеанса каталог сеанса многопользовательскую (MPSD). В случае области сеанс создается с помощью платформы решают, а не путем название. Это делается с помощью шаблона сеанса и дополнительной настройки игр, что были созданы вами как издателем title. Участники совпадение турнира будет уже добавлены в сеанс. В качестве издателя title необходимо настроить шаблон сеанса для области для поддержки арбитраж для получения отчета о результатах. Далее в этом документе включаются полный требования для сеанса. Название может также создать любые дополнительные сеансы, которые необходимы.

Для настройки результатов соответствия и отчетов в заголовок вашей использует сеанс. Реагирование на протокол активации и взаимодействия с сеансом MPSD — минимальный уровень интеграции, необходимые заголовки решают. Просмотр и регистрации для соревнования поддерживается через пользовательский Интерфейс решают Xbox. При необходимости заголовки можно получить дополнительные сведения о турнира из службы центра соревнования, обеспечивая более широкие возможности в заголовок. Например с помощью центра соревнования, название можно отображение контекста, например имена команд и обнаружение новых сеансов соответствия для пользователя. Этот поток данных, обозначается зеленой стрелки на схеме ниже, а также подробно описана [требования и рекомендации по обеспечению](#experience-requirements-and-best-practices) раздел. Затененный стрелки указывают, что ссылку от команды на сеанс изменяется со временем, при перемещении от одного совпадения к другому в рамках турнира.

![](../../images/arena/tournament-flow.png)


Активации протокола решают URI содержит сведения о турнира, сеанс для соответствия и название можно вызвать, когда совпадение находится над прямых ссылок. Прямая ссылка возвращает пользователя к пользовательскому Интерфейсу решают Xbox. Эти компоненты URI описаны более подробно в [протокола активации](#1protocol-activation) раздел [основные требования к интеграции решают](#basic-requirements-for-arena-integration).

## <a name="basic-requirements-for-arena-integration"></a>Основные требования к интеграции решают

Этот раздел содержит технические рекомендации и сведения для интеграции с минимальным требованиям для поддержки области заголовка. Этот стиль интеграции использует изображено оранжевый стрелки на схеме ниже общие сведения о потоке данных.

![](../../images/arena/arena-data-flow.png)

Название протокола активируемых в пользовательском Интерфейсе решают Xbox. Это может быть получены из всплывающего уведомления, страницу сведений о турнира или любые другие точки входа для сопоставления. Приведены шаги, выполняемые, если пользователь участвует в сопоставлении.

1.  Название протокола активируемых пользовательским Интерфейсом области Xbox.
2.  Название использует параметры активации для воспроизведения одного совпадения.
3.  При наведении совпадение, название сообщает о результатах к сеансу MPSD игр.
4.  Название дает пользователю возможность возврата к области пользовательского интерфейса.

Следующие разделы разберем каждый из этих действий подробно.

### <a name="1--protocol-activation"></a>1.  Активация протокола

Пользовательский Интерфейс решают запускает вещи, протокол активировав название когда соответствие готов для пользователя. Существует два варианта: название может запускаться в данный момент, или он может запущен. Оба случая встречаются аналогичную что произойдет, если заголовок активируется в ответ на пользователе, принявшем приглашение для игры.

#### <a name="if-your-title-is-launched-at-the-time-of-protocol-activation"></a>Если заголовок вашей запускается во время активации протокола

**Activated** событие запускается в первый раз, когда запускается название. При активации протокола решают, что первоначальная активация название было запущено пользователем, воспроизводимый в турнира. Иметь заголовок вашей как можно быстрее перейти к соответствие, обход экранов меню и входа в систему. Идентификатор пользователя Xbox (XUID) участие пользователя предоставляется в активации URI и уже должны войти в.

#### <a name="if-your-title-is-already-running"></a>Если заголовок вашей уже выполняется

**Activated** событий также могут запускать с помощью протокола активации решают, пока название уже выполняется. **Активации** событие запускается только явное действия пользователя (на всплывающее уведомление, указывающее соответствие, или переходит в соответствие с помощью пользовательского интерфейса решают Xbox). Если заголовок вашей находится в состоянии ожидания на экран меню, у его сразу же перейти к совпадению турнира. Если заголовок вашей получает активации протокола во время игровой процесс, было дать пользователю возможность оставить игры и введите турнира совпадения в наиболее подходящим способом. Дать пользователю возможность при необходимости сохранить своей игры.

Протокол активаций доставляются название через `CoreApplicationView.Activated` событий. Если `IActivatedEventArgs.Kind` аргументов события свойству `Protocol`, активация прошла активация протокола и аргументы могут быть приведены к `ProtocolActivatedEventArgs` класса, где доступны через URI протокола активации `Uri` свойство.

Иметь заголовок вашей проверьте XUID об активации протокол URI. Если он не соответствует текущей проигрывателя, название также потребуется переключения контекстов пользователя.

#### <a name="the-protocol-activation-uri"></a>URI протокола активации

Ниже приведен шаблон для URI.

```URI
ms-xbl-multiplayer://tournament?action=joinGame&joinerXuid={memberId}&organizer={organizerId}&tournamentId={tournamentId}&teamId={teamId}&scid={scid}&templateName={templateName}&name={name}&returnUri={returnUri}&returnPfn={returnPfn}
```

Для заголовков ЭРЫ на консоль схема активации — немного отличается:

```
ms-xbl-{titleIdHex}://
```

Это то же самое, что и для приглашения. Для этой цели title идентификатор должен быть восемь шестнадцатеричных символов, поэтому оно будет содержать начальные нули при необходимости.

Сначала убедитесь, что `Uri.Host` (сразу после разделителя «://») называется «турнира». Это как активаций протокола в области, отличаются от активаций других функций, таких как game приглашения.

Аргументы строки запроса будут в кодировке URL-адрес и зависят от регистра символов. Название не зависит от порядка параметров, и его следует игнорировать неизвестные параметры.


Paramater(s) | Описание
--- | ---
**Действие** | Только поддерживаемые действием является «joinGame». Если **действие** отсутствует или указано другое значение для него, игнорировать активации.
**joinerXuid** | **JoinerXuid** является XUID пользователя вошедшего в систему, который пытается воспроизвести в соответствии турнира. Название необходимо разрешить переключение контекста этого пользователя. Если **joinerXuid** не подписан, название должно предложить пользователю сделать это с помощью переноса средство выбора учетной записи. XUIDs всегда выражаются в десятичном формате.
**organizerId tournamentId** | **OrganizerId** и **tournamentId**, при объединении турнира сопоставление с которым связан уникальный идентификатор формы. Используйте этот идентификатор для получения подробных сведений о турнира от концентратора соревнования, если выбрано для отображения его в заголовок вашей.
**teamId** | **TeamId** — это уникальный идентификатор для группы, в контексте турнира, пользователь (определяется **joinerXuid** параметра) является членом. Как и **organizerId** и **tournamentId** параметров, можно использовать **teamId** при необходимости получить сведения о группе из концентратора соревнования.
**scid, templateName, имя** | Вместе они определяют сеанса. Это те же три параметра пути MPSD URI к сеансу:</br> </br>`https://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templateName}/sessions{name}`.</br></br>В XSAPI, они три параметра для `multiplayer_session_reference `конструктор.
**returnUri returnPfn** | **ReturnUri** — это URI протокола активации для возврата пользователя к пользовательскому Интерфейсу решают Xbox. **ReturnPfn** параметр может или не могут присутствовать. Если это так, это имя семейства продуктов (PFN) для приложения, который предназначен для обработки **returnUri**. Пример кода, в котором показано, как использовать эти значения, см. в разделе [возврат к пользовательскому Интерфейсу решают Xbox](#4returning-to-the-xbox-arena-ui).

### <a name="2--playing-the-match"></a>2.  Воспроизведение соответствия

При создании сеанса MPSD организатором турнира пользователя установлено как неактивные член этого сеанса. Название необходимо немедленно установить проигрыватель активный с помощью `multiplayer_session::join()`. Это указывает на Xbox Live и других пользователей в соответствие, пользователь в заголовок вашей и Готово для воспроизведения.

Время начала для элемента, соответствующего установлено в этот сеанс в `/servers/arbitration/constants/system/startTime` как значение даты и времени в стандартном формате времени UTC (например, «2009-06-15T13:45:30.0900000Z»). (Время начала, также доступны в XSAPI как `multiplayer_session_arbitration_server::arbitration_start_time()`, начиная в 1703 XDK). Кому можно создать сеанс, насколько до времени начала ему (включая параллельно с время начала). Совпадение уведомления отправляются участникам совпадения во время запуска, чтобы заголовки никогда не активировать до начала выполнения. Время начала тоже самое раннее время, по которому MPSD позволит к результатам для сопоставления.

Название просматривает каждый член `/member/{index}/constants/system/team` свойство (`multiplayer_session_member::team_id()`) чтобы выяснить, который team каждого пользователя включен.

Название также использует сеанс, чтобы определить параметры соответствия, например карты и режим. Эти параметры, относящиеся к title можно задать в шаблоне сеанса или как часть определения турнира как пользовательские константы. (Дополнительные сведения см. в разделе [Настройка заголовок для Arena](#configuring-a-title-for-arena).) Пример.

```json
{
    "constants": {
        "custom": {
            "enableCheats": false,
            "bestOf": 3,
            "map": "winter-fall",
            "mode": "capture-the-throne"
        }
    }
}
```

Эти параметры можно получить из сеанса, как объект JavaScript Object Notation (JSON) с помощью `multiplayer_session_constants::session_custom_constants_json()` API.

Как правило название следует относиться сеанса решают так же, как он может обрабатывать свой сеанс MPSD. Например его можно создать маркеры и Подпишитесь на уведомления возврата. Но есть ряд различий. Для этого заголовка нельзя изменить список игр сеанса или использовать функции качества обслуживания (QoS) сеанса, и должны быть включены в разрешения конфликтов. Все сведения о сеансе приведены далее в этом документе.

### <a name="3--reporting-results"></a>3.  Результаты

Сопоставления происходит возврат результатов решают и кому через сеанс, используя компонент, именуемый арбитража. Разрешение конфликтов — это платформа для с помощью сеанса безопасное воспроизведение совпадение, результат отчета.

Сеанс, предоставленный для заголовка в этап активации протокол будет arbitrated сеанса означает, что он имеет фиксированный временной шкалы, обеспечивающей framework арбитража. На этой схеме показана временная шкала арбитража.

![](../../images/arena/arbitration-timeline.png)

По крайней мере один исполнитель должен быть активным в сеансе до времени forfeit, что время начала (`multiplayer_session_arbitration_server::arbitration_start_time()`) плюс forfeit тайм-аут. Время ожидания forfeit — сеанса как количество миллисекунд в `/constants/system/arbitration/forfeitTimeout` (`multiplayer_session_constants::forfeit_timeout()`). Если никто объединения сеанса активным до времени forfeit, соответствие отменяется.

Время ожидания арбитраж указывается в миллисекундах, в `/constants/system/arbitration/arbitrationTimeout` (`multiplayer_session_constants::arbitration_timeout()`). Разрешение конфликтов время — это время начала, а также значение тайм-аут арбитраж и представляет время, по которому игроки должны завершения соответствие и получены результаты. Это значение задается в шаблон сеанса или игрового режима издателем. Задайте его, чтобы разрешить такое время, какое название должен выполнить сопоставление.

Название может сообщить результаты в любое время от времени начала и время разрешения конфликтов. Разрешение конфликтов происходит в любое время между временем forfeit и арбитраж время, после каждого активным участником сеанса отправил результаты. Например если в сеансе во время forfeit активен только один член, они могут рекомендуется) создать и опубликовать результат и арбитраж произойдет. Независимо от того, сколько результатов доступны во время разрешения конфликтов разрешение конфликтов происходит, если приложение уже запущено. Если результаты не отправляются в том случае, когда наступает время арбитраж, все участники в соответствие предоставляется потери.

Возможна также сервер игры для принудительного разрешения конфликтов в любое время путем простого написания arbitrated результата.

Если пользователь входит в сеансе, который уже был arbitrated (либо из-за истечения тайм-аута арбитража, сервер игры управляет сеанса или пользователь не присоединит позднее), название завершает совпадение и отображает arbitrated результат для пользователя.

Разрешение конфликтов результаты всегда включают результат для каждой группы. Когда игрок записывает результат в сеанс, он включает в себя не только команды и результат, но доступ к полному набору результатов для каждой группы.

Результаты в формате JSON будет выглядеть следующим образом.

```json
"results": {
    "red": {
        "outcome": "rank",
        "ranking": 1
    },
    "blue": {
        "outcome": "rank",
        "ranking": 2
    }
}
```

В этом примере команда идентификаторы «red» и «blue». Красная команда был выпущен в первый и синих второй. Другие возможные значения для **результат** являются:

* «win»
* «потери»
* «draw»
* «noshow»

Если **результат** является любой элемент, отличный от «ранг», ранжирование опускается для этой группы.

Отдельным пользователям записывать результаты соответствия для `/members/{index}/properties/system/arbitration` (`multiplayer_session::set_current_user_member_arbitration_results()`). В следующем примере показано, как заголовок может создать и отправить набор результатов, при условии, что название не на базе team (то есть всех игроков находятся на команды одного).

```c++
void Sample::SubmitResultsForArbitration()
{
    std::unordered_map<string_t, tournament_team_result> results;

    for (auto& member : arbitratedSession->members())
    {
        tournament_team_result teamResult;
        teamResult.set_state(tournament_game_result_state::rank);
        teamResult.set_ranking(memberRank);

        results.insert(std::pair<string_t, tournament_team_result>(
            member->team_id(),
            teamResult));
    }

    arbitratedSession->set_current_user_member_arbitration_results(results);
    xboxLiveContext->multiplayer_service().write_session(
arbitratedSession,
multiplayer_session_write_mode::update_existing)
    .then([](xbox_live_result<std::shared_ptr<multiplayer_session>> sessionResult)
    {
        if (sessionResult.err())
        {
            // Handle error.
        }
        else
        {
            // Update local session cache.
        }
    });
}
```

После разрешения конфликтов, MPSD помещает окончательные результаты в `/servers/arbitration/properties/system` (`multiplayer_session::arbitration_server()`), а также несколько других свойств следующим образом.

```json
{
    "resultState": "completed”,
    "resultSource": "arbitration",
    "resultConfidenceLevel": 75,

    "results": { ... }
}
```

Возможности **resultState** включают:

* «завершено»: Всех активных игроков сообщил результат.
* «partialresults»: Некоторые, но не всех активных игроков сообщил результат до истечения времени ожидания арбитража.
* «noresults»: Ни один из активных игроков сообщил результат до истечения времени ожидания арбитража.
* «отменено»: Нет активных игроков получен до истечения времени ожидания forfeit.

В случае «noresults» и «отменено» результаты опускаются.

**ResultSource** — «разрешение конфликтов», когда MPSD выполняет арбитраж на основе результатов, о которых сообщает отдельных игроков. Сервер игры может записывать /servers/arbitration/properties/system, минуя разрешение конфликтов. В этом случае он указывает **resultSource** «сервера». Существует также возможность сервер игры переписывать (то есть исправление или настройте) результаты, в котором прописные наборы **resultSource** «изменить».

**ResultConfidenceLevel** должно быть целым числом от 0 до 100, указывающее уровень консенсуса в результат. 100 означает, что принимаю всех игроков. 0 означает, что произошла не согласие, и результат была выбрана по существу в произвольном порядке из числа отправленных. Когда сервер игры задает результаты арбитража, он устанавливает **resultConfidenceLevel**-обычно до 100, если по некоторым причинам даже сам сервер игры не знает, (например, если отчеты отправляются результаты свой собственный каждого исполнителя голосования процедуры). Задайте **resultConfidenceLevel** также тогда, когда результаты изменяются в соответствии достоверность скорректированное результаты.

Когда **resultSource** — «разрешение конфликтов» (и **resultState** «завершено» или «partialresults»), результаты в являются точную копию объекта по крайней мере один из результатов Сообщенная проигрывателя.

### <a name="4--returning-to-the-xbox-arena-ui"></a>4.  Возвращаясь к Xbox области пользовательского интерфейса

При совпадение со (или, потенциально, в ответ на запрос игрока на поиск совпадения прекращается, выполняется), представить параметр для проигрывателя, чтобы вернуться в пользовательский Интерфейс решают Xbox из к которому они присоединены совпадение. Это можно сделать с экрана результаты после сопоставления или другой пользовательский Интерфейс окончания из игры.

Чтобы вернуться к области пользовательского интерфейса, вызвать **returnUri** переданного из URI протокола активации с помощью `Windows::System::Launcher` класса. Не забудьте включить контекст пользователя.

API запуска предоставляется немного по-разному ЭРЫ игры, чем до игр для универсальной платформы Windows (UWP). ЭРЫ версия API не допускает pfn-ИМЕНИ предоставляется, поэтому PFN может игнорироваться, даже если они присутствуют в активации URI.

Ниже приведен пример кода для ЭПОХИ, чтобы вернуться к области пользовательского интерфейса пользователя.

```c++
void Sample::LaunchReturnUi(Uri ^returnUri, Windows::Xbox::System::User ^currentUser)
{
    auto options = ref new LauncherOptions();
    options->Context = currentUser;

    Launcher::LaunchUriAsync(returnUri, options);
}
```

Можно задать игр для универсальной платформы Windows **TargetApplicationPackageFamilyName** свойство **LauncherOptions** для возврата pfn-ИМЕНИ, если такой об активации протокол URI. Это гарантирует, что определенное приложение запускается, и что пользователь выбирается для Store, если приложение еще не установлен.

Ниже приведен пример кода для приложения универсальной платформы Windows, чтобы вернуться к области пользовательского интерфейса пользователя.

```c++
void Sample::LaunchReturnUi(Uri ^returnUri, String ^returnPfn, User ^currentUser)
{
    auto options = ref new LauncherOptions();

    if (returnPfn != nullptr)
    {
        options->TargetApplicationPackageFamilyName = returnPfn;
    }

    Launcher::LaunchUriForUserAsync(currentUser, returnUri, options);
}
```

После вызова пользовательского интерфейса решают, название по-прежнему работает, возможно на этом же экране ожидания события активации другой протокол. Таким образом, если игрок набрал еще одно совпадение для воспроизведения, название готова к работе. Это упростит работу пользователя, как проигрыватель переходит от совпадения к другому, переключение между заголовком и пользовательского интерфейса решают Xbox.

### <a name="handling-errors-and-edge-cases"></a>Обработка ошибок и крайних случаев

Поток в предыдущем разделе описывает "Золотая" путь через воспроизведение совпадение. Существует много места, где вы можете увидеть неожиданное поведение, или могут возникнуть проблемы. В этом разделе описываются некоторые возможные ошибки или крайних случаев, а также рекомендации для их обработки в вашей должности.

#### <a name="game-session-not-found"></a>Игры сеанс не найден

Если название запускается с ссылочным сеанса для сопоставления, а затем клиенту не удается получить ошибку при запросе сеанса от MPSD, присутствует ошибка для пользователя, указывая, что они не удалось соединиться с совпадение.

#### <a name="user-attempts-to-join-a-match-that-has-been-played"></a>Пользователь пытается присоединиться к, более соответствующие указанным в игре

Если пользователь пытается соединиться совпадение после результаты уже сообщалось, заблокировать этот клиент запуститься новое совпадение и предоставить пользователю с ошибкой. Вы можете проверить ли результаты были зарегистрированы путем итерации по членам сеанса см. в разделе, если у вас есть `/members/{index}/arbitrationStatus` (`multiplayer_session_member::tournament_arbitration_status`) набор на «выполнено».

#### <a name="game-clients-unable-to-establish-a-p2p-connection"></a>Не удается установить подключение p2p игр клиентов

Если заголовок вашей использует живое (p2p) соединений между клиентами для многопользовательскую и не поддерживает для ретрансляций, может быть экземпляры, в которых не удалось создать подключение p2p пользователей, которые сопоставляются друг с другом.

Если клиент не установит для получения сеанса для сопоставления, но не может соединиться с другими клиентами, отчета результат «draw» для каждой команды в разделе `/members/{index}/properties/system/arbitration/results` (`multiplayer_session::set_current_user_member_arbitration_results()`). Это указывает на Xbox Live, что соответствие не выполнены. Она также препятствует эксплойтов, где пользователи могут перемещаться по турнира путем принудительного сбоя подключения p2p.

#### <a name="game-client-disconnects-mid-match"></a>Игры клиент отключается среднего соответствия

Один из наиболее распространенные ошибки при отключать клиентов, пока воспроизводится совпадение. В зависимости от состояния соответствия и реализации существуют несколько вариантов обработке данного случая:

* Если соответствие турнира один-один, или если все члены группы отключиться от других клиентов и/или игровые службы, рекомендуется, вы сообщить «потери» для группы, которая уже не подключен. Это предотвращает случай, в котором пользователи могут принудительно запустить запрос на отключение от совпадение, которое они теряют для предотвращения возвращается результат.
* Если сопоставление выполняется между командами с разными элементами и подмножество клиентов для команды отключения среднего совпадение, вы можете совпадение должно заканчиваться на этом этапе или продолжить выполнение с остальных пользователей. Если вы решили Продолжение поиска соответствия, может также можно разрешить пользователям, отключен повторное присоединение совпадение.

#### <a name="full-teams-not-present"></a>Отсутствует полное группы

Если заголовок вашей поддерживает совпадений с группами из двух или более, могут возникнуть случаи, где не запущен, название для их совпадения некоторые из членов команды. В этом случае можно разрешать членам группы для воспроизведения без их член команды и при необходимости разрешить этот член команды должен соединиться совпадения в процессе выполнения.

Кроме того вы можете автоматически применять результат. Если это сделать, подождите, пока forfeitTimeout время и оставили все участники возможность присоединиться к совпадение, прежде чем принудительно результат. Это позволяет настроить окно с изменениями игрового режима для турнира вместо того, чтобы обновить название.

#### <a name="length-of-match-exceeds-arbitrationtimeout"></a>Длина сопоставления превышает arbitrationTimeout

Задайте значение для **arbitrationTimeout** превышает максимальную продолжительность времени, возможно предпринять совпадение. Неудачна название может обладать режимы, в которых длинами возможное совпадение очень длинными или в котором нет отсутствие максимума. В таких случаях вы рассмотрите следующее.

* Ограничивающим play турнира только режимы длиной фиксированное, максимальное время.
* Сообщая пользователю, лимита времени и создание отчетов результат перед **arbitrationTimeout** зависимости от разрешения ничьих-match.

Так как **arbitrationTimeout** срока действия вызовет автоматическое результата для сопоставления, не ждите, полный **arbitrationTimeout** периода до появления результатов из названия.  Вместо этого построения в буфере времени (например, **arbitrationTimeout** -1 000 мс), или использовать отдельное значение для управления длина максимальное соответствие.

#### <a name="other-cases"></a>Другие варианты

Для других случаях сбоя общая рекомендация является для уведомления пользователя об ошибке и возвращают данные о пользователе в пользовательский интерфейс решают Xbox.

## <a name="experience-requirements-and-best-practices"></a>Интерфейс требования и рекомендации

Поскольку решают только отображает название отдельных совпадений, новые форматы могут появиться в со временем без необходимости обновления для заголовка. Таким образом форматирует взаимодействие с пользователем базового плана для области, интегрированная заголовок должен быть простым и гибким, для работы с несколькими конкурса.

При необходимости можно использовать данные из концентратора турнира в название для создания соревнования интегрированные части интерфейса в игре.  Эту функцию можно найти в разделе «xbox::services::tournaments».  С помощью этих API у вас есть возможность:
* Сведения о Surface турнира в название пользовательского интерфейса (соревнования, имя команды, и т.д.)
* Обеспечивают обнаружение для соревнования в заголовок вашей
* Держать пользователей в заголовок вашей между соответствием решают

## <a name="configuring-a-title-for-arena"></a>Настройка заголовка для области

Чтобы включить заголовок для области, некоторые дополнительные действия требуются при его настройке в портале разработчиков Xbox (XDP) или [центра партнеров](https://partner.microsoft.com/dashboard).

### <a name="enabling-arena-for-your-title"></a>Включение решают названия

Чтобы включить решают, перейдите на страницу конфигурации службы для заголовка в XDP и выберите «Области».

![](../../images/arena/arena-configure-xdp.png)

Здесь вы получите несколько вариантов:

* **Включено решают** — установите этот флажок, чтобы включить области для этой "песочницы".
* **Функции решают** — в этом разделе содержит флажки, чтобы включить соревнования, созданным пользователем, в «песочнице» и включить перекрестную воспроизведения, который позволяет пользователям на нескольких платформах для участия в одном соревнования.
* **Платформы решают** — позволяет выбрать платформы, на которых может воспроизводиться соревнования для заголовка.
* **Активы турнира** — (прежнее название — раздела «Многопользовательскую и подбор игроков».) Это образы турнира названия.

Также можно включить в центре партнеров в области **турнира** меню в узле службы Xbox Live.

![Меню области в центре партнеров](../../images/arena/Arena_On_WDC.JPG)

Конфигурации службы, чтобы изменения вступили в силу, необходимо опубликовать. Конфигурация области самообслуживания через UDC сейчас не поддерживается. Если вы используете UDC для конфигурации службы, работе с разработки менеджеру для подключения с помощью области.

### <a name="setting-up-game-modes"></a>Настройка игровых режимов

Игровые режимы являются возможностью Xbox Live, которая позволяет предварительно настроить параметры для сопоставления турнира издателю. Эти игровые режимы используются при создании турнира для создания правил, обеспечиваемая Xbox Live и название для турнира. В качестве издателя title необходимо по крайней мере одного игрового режима, чтобы включить пользовательские соревнования для заголовка. Элементы игрового режима включают:

#### <a name="supports-ugt"></a>Поддерживает UGT?

Игровые режимы можно включить для использования с созданным пользователем соревнования (UGTs). Если заголовок вашей настроен для поддержки UGT, можно пометить игровых режимов таким образом, они могут быть выбраны пользователями Xbox Live, при создании соревнования для заголовка.

#### <a name="team-size"></a>Численность рабочей группы

Это число пользователей, которые могут участвовать в турнире в команде. Для одного пользователя заголовок или турнира игровой режим имеет размер группы одного. Решают сейчас не поддерживает размер переменной группы для соревнования.

#### <a name="forfeittimeout"></a>forfeitTimeout

Это время в миллисекундах, после соответствие начаться не сразу, соответствие отменяется, если Показывать проигрывателей для его использования (то есть, если нет игроков присваивается сами активной в сеансе). Задайте количество времени, по крайней мере длинный, для проигрывателя запустить название и получение в соответствие турнира из времени, они увидят всплывающее уведомление.

#### <a name="arbitrationtimeout"></a>arbitrationTimeout

Это время в миллисекундах, после соответствие начинаться времени, после чего будут приняты больше нет результатов. Задайте для него значение тайм-аут forfeit, а также возможно потребуется объем времени, превышающего наиболее длинное совпадение. Таким образом, даже если игроков испытали прямо перед forfeit время, они по-прежнему иметь достаточно времени, чтобы завершить сопоставление. Если результаты не отображаются на момент **arbitrationTimeout**, потери приема участников все совпадения. Xbox решают также происходит задержка до **arbitrationTimeout** для всех активных участников, сообщить о результатах перед началом разрешения конфликтов.

Это время ожидания действует как поддержку в случае, если что-то пойдет не так, и результат не переданы для проигрывателя. Вместо того чтобы замедляется работа всего турнира, это время ожидания помещает верхний предел на количество времени, которое будет обслуживать турнира. По этой причине он не должен быть равен слишком высока, так как оно определяет максимальную длину каждого round турнира.

#### <a name="name"></a>Имя

Это имя пользовательского интерфейса для режима игры. Это значение может быть локализована.

#### <a name="description"></a>Описание

Это описание пользовательского интерфейса для режима игры. Это значение может быть локализована.

#### <a name="custom"></a>Настраиваемое

**Пользовательских** разделе для режима игры — контейнер свойств, где разработчики можно вставить все параметры конфигурации конкретного заголовка для элемента, соответствующего турнира. Значений, определенных как часть пользовательского раздела записываются в сеанс MPSD совпадения в группе `/properties/custom/`.

В настоящее время через XDP или UDC игрового режима установки не поддерживается. Для создания игровые режимы, названия, обратитесь к своему менеджеру разработчика.

### <a name="requirements-for-the-session-template"></a>Требования для шаблона сеанса

Как разработчик title необходимо указать шаблон сеанса для TO для использования при создании сеансов, совпадающих с заданной строкой. Существуют некоторые ожидания относительно содержимое шаблона.

```json
{
    "version": 1,
    "inviteProtocol": "tournamentGame",
    "memberInitialization": null,

    "capabilities": {
        "gameplay": true,
        "arbitration": true,

        "large": false,
        "broadcast": false,
        "blockBadMsaReputation": false
    },

    "maxMembersCount": {maxMembersCount},
}
```

Тем не менее требуется, можно задать другие свойства, здесь не показаны.

Сеансы решают всегда являются версии 1. Установка **inviteProtocol** как «tournamentGame» включает отправку участникам турнира готовые соответствия уведомлений. **memberInitialization** задано значение null, чтобы отключить QoS. Необходимо задать возможность игровой процесс, поскольку сеанс представляет соответствие, а также возможность разрешения конфликтов необходим для получения отчета о результатах. **Больших**, **вещания**, и **blockBadMsaReputation** возможностей должен быть отключен, так как они может повлиять на работу сеанса.

Название можно указать собственные параметры в разделе пользовательский шаблон, параметры, которые имеют фиксированное значение для всех соревнования, использующих шаблон. Вот пример.

```json
        "custom": {
            "enableCheats": false,
            "bestOf": 3
        }
```

Наконец **maxMembersCount** системный параметр является обязательным. Ему присвоено общее число исполнителей, которые могут играть в соответствии турнира. Число исполнителей могут быть еще более ограничено параметры режима игры, поэтому убедитесь, что значения, заданного в сеансе представляет наибольшее общее число исполнителей, поддерживается для заголовка. Например, если максимальное число исполнителей, поддерживаемых игры для сопоставления — 5 vs. значение 5, **maxMembersCount** до 10. Этот шаблон MPSD может использоваться для поддержки соответствия любого количества игроков до **maxMembersCount**.