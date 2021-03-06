---
title: Доступные операции с потоковыми ресурсами
description: В этом разделе перечислены операции, которые можно выполнить с потоковыми ресурсами.
ms.assetid: 700D8C54-0E20-4B2B-BEA3-20F6F72B8E24
keywords:
- Доступные операции с потоковыми ресурсами
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 922798fad97754421541297a5434a81e9c660b2b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655519"
---
# <a name="operations-available-on-streaming-resources"></a>Доступные операции с потоковыми ресурсами


В этом разделе перечислены операции, которые можно выполнить с потоковыми ресурсами.

-   Операции обновления сопоставлений плиток, возвращающие void, и операции копирования сопоставлений плиток, возвращающие void — эти операции служат указателями для расположений плиток в потоковом ресурсе на расположения в пулах плиток или на значение NULL, или на оба вида значений. Эти операции могут обновлять раздельные подмножества указателей плиток.
-   Операции копирования и обновления — все интерфейсы API, которые могут копировать данные в пул поверхности по умолчанию и из него, работают с потоковыми ресурсами. Чтение из несопоставленных плиток дает 0, а операции записи в несопоставленные плитки не выполняются.
-   Операции копирования плитки и обновления плитки — эти операции предусмотрены для копирования плиток на уровне детализации 64 КБ в любой потоковый ресурс и из него, а также в любой ресурс буфера и из него в стандартной схеме памяти. Оборудование и драйвер экрана выполняют все операции адресации памяти, необходимые для потокового ресурса.
-   Привязки к конвейеру Direct3D и привязки к созданным представлениям, которые работают на ресурсах, не относящихся к потоковым, также работают и на потоковых ресурсах.

Элементы управления плиткой доступны в немедленных и отложенных контекстах (так же, как и обновления типичных ресурсов) и после выполнения влияют на последующие обращения к плиткам (но не ранее отправленные операции).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Связанные разделы


[Создание потоковой передачи ресурсов](creating-streaming-resources.md)

 

 




