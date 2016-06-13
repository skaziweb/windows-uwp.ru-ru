---
author: mcleanbyron
ms.assetid: 5fa16a27-fdc0-43b2-84cd-8547fd4915de
description: Узнайте, как назначать свойства ** AdControl ** в HTML.
title: Пример свойств HTML

---

# Пример свойств HTML


\[ Обновлено для приложений UWP в Windows 10. Статьи о Windows 8.x, см. в [архиве](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

В следующем примере показано, как назначать свойства [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) в HTML. **applicationId** и **adUnitId** являются обязательными свойствами. Другие свойства и события являются дополнительными. Если вы не зададите значения для дополнительных свойств, элемент управления будет использовать значения по умолчанию, обеспечивающие согласованность рекламного объявления с процессом взаимодействия пользователя с приложением.

Последние пять строк представляют собой пример регистрации функций для событий **AdControl**. Вы можете зарегистрировать только те функции, которые были определены в коде JavaScript.

Эти значения являются примерами. В своем коде вы зададите значения для этих функций и свойств таким образом, чтобы они подходили для вашего приложения.

``` syntax
data-win-control="MicrosoftNSJS.Advertising.AdControl"
data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1',
                    adUnitId: '10865270',
                    isAutoRefreshEnabled: false,
                    onAdRefreshed: myAdRefreshed,
                    onErrorOccurred: myAdError,
                    onEngagedChanged: myAdEngagedChanged,
                    onPointerDown: myPointerDown,
                    onPointerUp: myPointerUp }"
```

## Связанные разделы

* [Примеры рекламы на GitHub](http://aka.ms/githubads)

 


<!--HONumber=May16_HO2-->

