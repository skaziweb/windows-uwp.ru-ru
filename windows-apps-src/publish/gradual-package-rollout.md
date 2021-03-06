---
Description: При публикации обновления отправки можно выбрать постепенное развертывание обновленных пакетов для некоторой процентной доли пользователей вашего приложения в Windows 10.
title: Постепенный выпуск пакета
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 65d578a6-4e26-484c-90af-b2cd916f3634
ms.localizationpriority: medium
ms.openlocfilehash: 8ee4c3cbdfea80178c992b484103071c621aff1c
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/24/2019
ms.locfileid: "63791000"
---
# <a name="gradual-package-rollout"></a>Постепенный выпуск пакета

При публикации обновления на сообщение, вы можете для постепенного развертывания обновленных пакетов в процентах от клиентов вашего приложения в Windows 10 (включая Xbox). Это позволяет контролировать отзывы и аналитические данные для конкретных пакетов, чтобы вы могли убедиться в правильности обновления до его более широкого распространения. Увеличить процент (или остановить обновление) можно в любое время без необходимости создания новой отправки. 

> [!IMPORTANT]
> Выбранные параметры выпуска применяются ко всем пакетам, но касаются только клиентов, использующих версии ОС, которые поддерживают тестовые пакеты, а именно Windows.Desktop (сборка 10586 или более поздней версии), Windows.Mobile (сборка 10586.63 или более поздней версии) и Xbox. Сюда относятся любые клиенты, получающие приложение путем [лицензирования от Магазина (в режиме онлайн)](organizational-licensing.md) через [Магазин Майкрософт для бизнеса](https://businessstore.microsoft.com/store) или [Магазин Майкрософт для образовательных учреждений](https://educationstore.microsoft.com/store). При использовании постепенного выпуска пакета пользователи более ранних версий ОС не получат пакеты из последней отправки до окончательного выпуска пакета, как описано ниже.

Обратите внимание, что все пользователи будут видеть сведения из описания в Магазине, которые были введены при последней отправке. Параметры выпуска применяются только к получаемым пользователями пакетам, как для новых приобретений, так и для обновления существующих пользователей.

> [!TIP]
> При выпуске пакеты распространяются для указанной процентной части пользователей, которые выбираются случайным образом. Для распространения конкретных пакетов выбранным вами пользователям можно использовать тестовые пакеты. Можно также объединить выпуск с тестовыми пакетами, если требуется постепенно распространять обновление в одной из тестовых групп.


## <a name="setting-the-rollout-percentage"></a>Задание процентного отношения для выпуска

Выпуск обновления можно выбрать на странице **Пакеты** обновленной отправки. Для этого установите флажок **Выпускать обновления постепенно после публикации отправки (только для пользователей Windows 10)**. Затем введите процент пользователей, которые должны получить обновление при первой публикации отправки. Например, можно ввести значение 5, если вы хотите начать выпуск обновления только для небольшого процента пользователей вашего приложения.

Нажмите кнопку **Обновить**, чтобы сохранить выбранные параметры. После завершения процесса сертификации вашего приложения пакеты будет распространяться для пользователей в соответствии с указанным процентным отношением, как для новых приобретений, так и для обновления существующих пользователей.


## <a name="adjusting-the-rollout-after-the-submission-is-published"></a>Настройка выпуска после публикации отправки

Чтобы настроить выпуск после публикации отправки, перейдите на страницу "Обзор" приложения. Перетаскивая селектор, можно изменить процент пользователей, которые получают пакеты из новейшей отправки. Нажмите кнопку **Обновить**, чтобы сохранить выбранные параметры. Пакеты будет распространяться пользователям в соответствии с указанным процентным отношением, как для новых приобретений, так и для обновления существующих пользователей.


## <a name="completing-the-rollout"></a>Завершение выпуска

Перед созданием новой отправки необходимо завершить выпуск пакета. Можно **завершить** выпуск и распространить новейшие пакеты для всех пользователей, или **остановить** выпуск, чтобы прекратить распространение новейших пакетов.

Если вы уверены в обновлении и хотите сделать его доступным всем пользователям, щелкните **Finalize package rollout**, чтобы новейшие пакеты распространялись для всех пользователей.

> [!TIP]
> Изменение процента выпуска на 100% не гарантирует, что все пользователи получат пакеты из последних отправок, поскольку версии ОС некоторых пользователей могут не поддерживать выпуск. Необходимо завершить выпуск, чтобы прекратить распространение старых пакетов и обновить всех существующих пользователей с использованием новых пакетов.

Если возникли проблемы с обновлением и вы больше не хотите распространять его, можно щелкнуть **Halt package rollout**, чтобы прекратить распространение пакетов из последней отправки. После остановки выпуска пакета такие пакеты больше не будут распространяться никаким пользователям; для новых и обновляемых пользователей будут использоваться только пакеты из предыдущей отправки. Однако у всех пользователей, которые уже получили новые пакеты, эти пакеты сохранятся; для них не будет выполнен откат к предыдущей версии. Для обновления этих пользователей необходимо создать новую отправку с пакетами, которые они должны получить. Обратите внимание, что при использовании постепенного выпуска при следующей отправке пользователям, получившим пакет, распространение которого было остановлено, новое обновление будет предлагаться в том же порядке, в котором им предлагался остановленный пакет. Новый выпуск будет находиться между последней завершенной отправкой и вашей новейшей отправкой; после остановки выпуска пакета эти пакеты больше не распространяются пользователям.
