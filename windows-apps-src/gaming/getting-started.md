---
title: Начало работы
description: Руководство по разработке игр для обучения
ms.assetid: 40490837-6c7f-4f82-96b5-14f6858982b3
ms.date: 01/25/2018
ms.topic: article
keywords: Windows 10, uwp, игры, Приступая к работе
localizationpriority: medium
ms.openlocfilehash: f818837a6f8703721520a8be8c0ed9b062cf797a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653559"
---
# <a name="getting-started"></a>Начало работы

В этой статье — Вводное руководство для разработчиков, которые хотят разрабатывать игры в Windows или Xbox. 

Ниже приведены некоторые вопросы, которые помогут найти необходимые сведения о:
* Вы опытный разработчик игр и должны все сведения? См. в разделе [руководство по разработке игр для Windows 10](e2e.md).
* Никогда не работали с написания кода? Что-то удобным образом [Minecraft час учебники кода](https://code.org/minecraft) может представлять интерес.
* Просто ищете отличных игр? Ознакомьтесь с [Microsoft Store](https://www.microsoft.com/store).
* Готовы приступить к разработке отличных игр для Windows или Xbox?  Теперь вы в нужном месте.

## <a name="quick-start-guide"></a>Краткое руководство

Шаги в разработке игр прямо сейчас.

### <a name="step-1-get-the-software-and-tools"></a>Шаг 1. Получите программное обеспечение и средства

Убедитесь, что у вас установлено на устройстве Windows 10 и последнюю версию обновления.

Установите подходящий интегрированную среду разработки вроде Visual Studio. Visual Studio Community 2017 доступна для бесплатной загрузки. Дополнительные сведения см. в разделе [загрузки Visual Studio](https://www.visualstudio.com/downloads/).

Если вы планируете использовать игровое ядро и другого по промежуточного слоя, см. в разделе [мостов, игровых движков и по промежуточного слоя](e2e.md#bridges-game-engines-and-middleware) статьи [руководство по разработке игр для Windows 10](e2e.md). Сведения о разработке игр Windows и Xbox, с помощью определенного модуля игры нужно будет перейти к документации игровое ядро.

### <a name="step-2-prepare-your-hardware-for-development"></a>Шаг 2. Подготовка оборудования для разработки

Если вы выполняете разработку в первый раз, необходимо включить режим разработчика на вашем устройстве. Дополнительные сведения см. в разделе [включить устройство для разработки](../get-started/enable-your-device-for-development.md).

Для тех, кто планирует разрабатывать игры Xbox, с помощью консоли Xbox розничной торговли необходимо также активировать и включить режим разработчика на нем. Дополнительные сведения см. в разделе [активации один режим разработчика Xbox](../xbox-apps/devkit-activation.md) и [Приступая к работе с разработкой приложений для универсальной платформы Windows на Xbox](../xbox-apps/getting-started.md). 

> [!Note]
> Вам потребуется зарегистрироваться [центра партнеров](https://partner.microsoft.com/dashboard) учетной записи, чтобы можно было включить режим разработчика на консоли Xbox. Дополнительные сведения о регистрации для учетной записи центра партнеров, см. в разделе [шаг 5](#step-5-sign-up-for-a-partner-center-account) ниже.

### <a name="step-3-run-a-sample-and-see-how-it-works"></a>Шаг 3. Запуск примера и ознакомиться с принципами работы

Чтобы начать работу с разработкой приложений DirectX для универсальной платформы Windows, см. в разделе [создание простой игрой UWP с помощью DirectX](tutorial--create-your-first-uwp-directx-game.md). Если вы просто хотите прочитать и знакомы с основными понятиями DirectX, например какие буфера, см. в разделе [понятия графики Direct3D](../graphics-concepts/index.md).

Дополнительные примеры см. в разделе [игр примеры](e2e.md#game-samples).

### <a name="step-4-consider-joining-a-program"></a>Шаг 4. Рассмотрите возможность присоединения к программе

Если требуется разработать игру Xbox или использовать в игре для Xbox Live пополнена join либо [Xbox Live Creators Program](https://developer.microsoft.com/games/xbox/xboxlive/creator) или [ ID@Xbox ](https://www.xbox.com/Developers/id) программы. 

Дополнительные сведения о функциях, Xbox Live, которые доступны для каждой из программ, см. в разделе [таблицы компонентов](../xbox-live/developer-program-overview.md#feature-table). Дополнительные сведения см. в разделе [программы для разработчиков](e2e.md#developer-programs).

> [!Note]
> Xbox Live Creators Program доступна для всех разработчиков. **Любой пользователь** можно опубликовать в игру на Xbox. Чтобы сделать заголовок части, которая Xbox Live Creators Program, необходимо просто включите этот параметр, из центра партнеров. Дополнительные сведения о регистрации для учетной записи центра партнеров, см. в разделе [шаг 5](#step-5-sign-up-for-a-partner-center-account) ниже.

### <a name="step-5-sign-up-for-a-partner-center-account"></a>Шаг 5. Зарегистрировать учетную запись центра партнеров

Учетную запись центра партнеров дает доступ к [центра партнеров](https://partner.microsoft.com/dashboard), который позволяет управлять и отправить все ваши приложения и игры для устройств Windows в одном месте.

Для разработки игр для Windows вы можете ожидать, пока не требуется доступ к центра партнеров или если вы хотите использовать функции Xbox Live в игре.

Для разработки игр Xbox вам следует зарегистрировать учетную запись центра партнеров, его необходимо настроить в приложении retail Xbox для разработки приложений. См. в разделе [шаг 2](#step-2-prepare-your-hardware-for-development) подробные сведения.

Дополнительные сведения см. в разделе [публикация Windows-приложений и игр](../publish/index.md).

## <a name="useful-links"></a>Полезные ссылки

* [Руководство по разработке игр для Windows 10](e2e.md)
* [Что такое приложение UWP?](../get-started/universal-application-platform-guide.md)
* [С помощью облачных служб для игр для универсальной платформы Windows](cloud-for-games.md)
* [Повышение уровня доступности игры](accessibility-for-games.md)