---
title: Вопросы и ответы
description: Вопросы и ответы о UWP на консолях Xbox.
ms.date: 03/29/2017
ms.topic: article
keywords: Windows 10, uwp
ms.assetid: 265fe827-bd4a-48d4-b362-8793b9b25705
ms.localizationpriority: medium
ms.openlocfilehash: e134e64c441aececdc50b1ac868efeb2b31bd5e5
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259831"
---
# <a name="frequently-asked-questions"></a>Вопросы и ответы

Что-то работает не так, как вы ожидали? Прочитайте вопросы и ответы на этой странице. Также изучите раздел [Известные проблемы](known-issues.md) и посетите форум [разработки универсальных приложений для Windows](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

### <a name="why-arent-my-games-and-apps-working"></a>Почему игры и приложения не работают?

Если игры и приложения не работают или если у вас нет доступа к Store или службам Live, возможно, вы работаете в режиме разработчика. Чтобы определить, какой режим вы используете в данный момент, нажмите кнопку **Главная** на контроллере. Если при этом откроется главная страница разработчика вместо главной страницы розничной компании, вы работаете в режиме разработчика. Если вы хотите играть в игры, откройте Dev Home и переключитесь в коммерческий режим с помощью кнопки **Выйти из режима разработчика**.

### <a name="why-cant-i-connect-to-my-xbox-one-using-visual-studio"></a>Почему не удается подключиться к Xbox One с помощью Visual Studio?

Сначала убедитесь, что используется режиме разработчика, а не коммерческий режим. Вы не сможете подключиться к консоли Xbox One, если она находится в коммерческом режиме. Чтобы определить, какой режим вы используете в данный момент, нажмите кнопку **Главная** на контроллере. Если вы видите содержимое Gold и Live вместо главной страницы разработчика, вы работаете в коммерческом режиме и вам необходимо запустить приложение Dev Mode Activation, чтобы перейти в режим разработчика.

> [!NOTE]
> Для развертывания приложения пользователь должен выполнить вход.

Дополнительные сведения см. в разделе [Устранение ошибок развертывания](#fixing-deployment-failures) далее на этой странице.

### <a name="how-do-i-switch-between-retail-mode-and-developer-mode"></a>Как переключаться между коммерческим режимом и режимом разработчика?

Следуйте инструкциям для [активации режима разработчика Xbox One](devkit-activation.md), чтобы больше узнать об этих состояниях.

### <a name="how-do-i-know-if-i-am-in-retail-mode-or-developer-mode"></a>Как определить текущий режим — коммерческий или разработчика?

Следуйте инструкциям для [активации режима разработчика Xbox One](devkit-activation.md), чтобы больше узнать об этих состояниях. 

Чтобы определить, какой режим вы используете в данный момент, нажмите кнопку **Главная** на контроллере. 
- Если при этом откроется главная страница разработчика, вы работаете в режиме разработчика.
- Если вы увидите содержимое Gold или Live, вы в коммерческом режиме.

### <a name="will-my-games-and-apps-still-work-if-i-activate-developer-mode"></a>Будут ли игры и приложения по-прежнему работать, если включить режим разработчика?

Да, вы можете переключиться из режима разработчика в коммерческий режим, чтобы играть в игры. Дополнительные сведения см. на странице [Активация режима разработчика Xbox One](devkit-activation.md). 

### <a name="can-i-develop-and-publish-x86-apps-for-xbox"></a>Можно ли разрабатывать и публиковать приложения x86 для Xbox?
Xbox больше не поддерживает разработку приложений x86 или их отправку в магазин. 

### <a name="will-i-lose-my-games-and-apps-or-saved-changes"></a>Потеряю ли я игры и приложения или сохраненные изменения?

Если вы захотите выйти из программы для разработчиков, вы не потеряете установленные игры и приложения. Кроме того, если вы были подключены к Интернету, когда играли в игры, все сохраненные данные игр будут размещены в облачном профиле учетной записи Live, поэтому вы их не потеряете.

### <a name="how-do-i-leave-the-developer-program"></a>Как прекратить участие в программе для разработчиков?

Сведения о выходе из программы для разработчиков см. в разделе [Деактивация режима разработчика Xbox One](devkit-deactivation.md).

### <a name="i-sold-my-xbox-one-and-left-it-in-developer-mode-how-do-i-deactivate-developer-mode"></a>Я продал консоль Xbox One и оставил ее в режиме разработчика. Как отключить режим разработчика?

Если у вас больше нет доступа к Xbox One, вы можете отключить ее в центре партнеров Windows. Дополнительные сведения см. в разделе деактивация **консоли с помощью центра партнеров** статьи о [деактивации в режиме Xbox One Developer](devkit-deactivation.md#deactivate-your-console-using-partner-center) . 

### <a name="i-left-the-developer-program-using-partner-center-but-im-in-still-developer-mode-what-do-i-do"></a>Я оставил программу разработчиков, используя центр партнеров, но в режиме разработчика. Что делать?

Запустите Dev Home и нажмите кнопку **Выйти из режима разработчика**. Это приведет к перезапуску консоли в коммерческом режиме. 

### <a name="can-i-publish-my-app"></a>Можно ли опубликовать приложение?

Вы можете [опубликовать приложения](../publish/index.md) через центр партнеров, если у вас есть [учетная запись разработчика](https://developer.microsoft.com/store/register). Приложения UWP, созданные и протестированные на коммерческих консолях Xbox One, будут проходить такой же процесс проверки и публикации, как и приложения для Windows, с применением дополнительных проверок на соответствие современным стандартам Xbox One.

### <a name="can-i-publish-my-game"></a>Можно ли опубликовать игру?

Вы можете использовать UWP и Xbox One в режиме разработчика для построения и тестирования ваших игр на Xbox One. Для публикации игр UWP необходимо зарегистрировать [ID@XBOX](https://www.xbox.com/Developers/id)или участвовать в [Xbox Live создателям программы](https://developer.microsoft.com/games/xbox/xboxlive/creator). Дополнительные сведения см. в разделе [Обзор программы разработчика](https://developer.microsoft.com/games/xbox/docs/xboxlive/get-started/developer-program-overview.html).

### <a name="will-the-standard-game-engines-work"></a>Будут ли работать стандартные игровые модули?

Изучите страницу [Известные проблемы](known-issues.md) для этого выпуска.

### <a name="what-capabilities-and-system-resources-are-available-to-uwp-games-on-xbox-one"></a>Какие возможности и системные ресурсы доступны играм UWP на Xbox One? 

Информацию см. в разделе [Системные ресурсы для приложений UWP и игр для Xbox One](system-resource-allocation.md).

### <a name="if-i-create-a-directx-12-uwp-game-will-it-run-on-my-xbox-one-in-developer-mode"></a>Если я создаю игру UWP на основе DirectX 12, будет ли она запускаться на Xbox One в режиме разработчика?

Информацию см. в разделе [Системные ресурсы для приложений UWP и игр для Xbox One](system-resource-allocation.md).

### <a name="will-the-entire-uwp-api-surface-be-available-on-xbox"></a>Будет ли вся поверхность UWP API доступна на Xbox?

Изучите страницу [Известные проблемы](known-issues.md) для этого выпуска.

### <a name="fixing-deployment-failures"></a>Устранение ошибок развертывания

Если вам не удается развернуть приложение из Visual Studio, эти действия помогут вам устранить проблему. Если проблема сохраняется, посетите форум.

> [!NOTE]
> Для развертывания приложения пользователь должен выполнить вход. Если вы получили сообщение об ошибке 0x87e10008, убедитесь, что какой-либо пользователь выполнил вход, и повторите попытку.

Если Visual Studio не может подключиться к Xbox One:

1. Убедитесь, что вы используете режим разработчика (описано выше на этой странице).
2. Убедитесь, что вы настроили компьютер разработки правильно. Вы следовали *всем* инструкциям из раздела [Начало разработки приложений UWP на Xbox One](getting-started.md)? 

3. Если нет, прочитайте разделы [Настройка среды разработки](development-environment-setup.md) и [Вводные сведения об инструментах Xbox One](introduction-to-xbox-tools.md).

4. Убедитесь, что вы можете связаться с IP-адресом консоли с компьютера разработки.
  > [!NOTE]
  > Для повышения производительности развертывания рекомендуется использовать проводное подключение к консоли.

5. Убедитесь, что в раскрывающемся списке "Проверка подлинности" на вкладке **Отладка** выбран пункт "Универсальный (незашифрованный протокол)". Дополнительные сведения см. в разделе [Настройка среды разработки](development-environment-setup.md).


### <a name="if-im-building-an-app-using-htmljavascript-how-do-i-enable-gamepad-navigation"></a>Как включить навигацию геймпада, если я создаю приложения с помощью HTML и JavaScript?

TVHelpers — это набор примеров и библиотек JavaScript и XAML/C#, которые помогут вам создавать замечательные приложения для Xbox One и телевизоров на языках JavaScript и C#. TVJS — это библиотека, которая позволяет создавать великолепные приложения UWP для Xbox One. TVJS включает поддержку автоматической навигации контроллера, богатые возможности воспроизведения мультимедиа, функцию поиска и многое другое. TVJS настолько же удобно использовать с размещенным веб-приложением, как и с упакованным веб-приложением UWP с полным доступом к API среды выполнения Windows.

Дополнительные сведения см. в разделах [TVHelpers](https://github.com/Microsoft/TVHelpers) и [Справка](https://github.com/Microsoft/TVHelpers/wiki).

## <a name="see-also"></a>См. также
- [Известные проблемы с UWP на Xbox One](known-issues.md)
- [Приложения UWP для Xbox One](index.md)
- [Приложения UWP для Xbox One](index.md)
