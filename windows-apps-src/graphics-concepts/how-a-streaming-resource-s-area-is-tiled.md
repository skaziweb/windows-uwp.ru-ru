---
title: Размещение плиток в области потокового ресурса
description: При создании потокового ресурса измерения, размер элемента формата и количество текстур и(или) фрагментов массива (если применимо) определяют количество плиток, необходимых для резервирования всей области поверхности.
ms.assetid: 3485FD8D-2A06-4B0A-8810-ECF37736F94B
keywords:
- Размещение плиток в области потокового ресурса
- область ресурса, плиточный
- таблицы размеров, ресурсы, плиточный
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 28fac411ad814167dcef3b02424c866bd3453344
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370597"
---
# <a name="how-a-streaming-resources-area-is-tiled"></a>Размещение плиток в области потокового ресурса


При создании потокового ресурса измерения, размер элемента формата и количество текстур и(или) фрагментов массива (если применимо) определяют количество плиток, необходимых для резервирования всей области поверхности. Структура пикселей/байтов в плитках определяется реализацией. Число пикселей, помещающихся в плитке, зависит от размера элемента формата, фиксировано и одинаково как при использовании стандартного преобразования ссылок при перемещении данных с внешнего устройства, так и без него.

Число плиток, которые будут использоваться при определенном размере поверхности и ширине элемента формата, можно точно определить и спрогнозировать на основе таблиц в следующих разделах. Для ресурсов, содержащих MIP-карты, а также в случаях, когда размеры поверхности не заполняют плитку, существуют ограничения. См. раздел [Упаковка MIP-карт](mipmap-packing.md).

Разные потоковые ресурсы могут указывать на идентичную память с различными форматами, если приложения не зависят от результатов записи в память в одном формате и считывания в другом. Но приложения могут зависеть от результатов записи в память в одном формате и считывания в другом, если эти форматы относятся к одному семейству (то есть если у них один нетипизированный родительский формат). Например, DXGI\_ФОРМАТ\_R8G8B8A8\_UNORM и DXGI\_ФОРМАТ\_R8G8B8A8\_целое число без знака, совместимы друг с другом, но не с DXGI\_ФОРМАТ\_R16G16\_UNORM.

Исключением является тот случай когда переход данных из одного формата с присвоением псевдонима другого четко определено: если все биты плитки содержат нули, эту плитку можно использовать с любым форматом, интерпретирующим это содержимое памяти как нули (независимо от структуры памяти). Таким образом, удалось очистить плитки для 0x00 с форматом DXGI\_ФОРМАТ\_R8\_UNORM, а затем использовать в формате вида DXGI\_ФОРМАТ\_R32G32\_число с плавающей запятой и он будет отображаться содержимое по-прежнему (0, 0f, 0, 0F).

Структура данных в плитке не зависит от сопоставления плитки в ресурсе в целом. Например, плитку можно повторно использовать одновременно в разных расположениях поверхности с единым поведением.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>В этом разделе


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Раздел</th>
<th align="left">Описание</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="texture2d-and-texture2darray-subresource-tiling.md">Texture2D и массивы Texture2DArray динамическое разделение подресурсов</a></p></td>
<td align="left"><p>В таблицах ниже показано, как размещаются на плитках вложенные ресурсы <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d"><strong>Texture2D</strong></a> и <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray"><strong>Texture2DArray</strong></a>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture3d-subresource-tiling.md">Динамическое разделение подресурсов Texture3D</a></p></td>
<td align="left"><p>В этой таблице показано, как размещаются на плитках вложенные ресурсы <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture3d"><strong>Texture3D</strong></a>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="buffer-tiling.md">Мозаичное заполнение буфера</a></p></td>
<td align="left"><p><a href="introduction-to-buffers.md">Буферный</a> ресурс разделен на плитки по 64 КБ. В последней плитке остается некоторое свободное пространство, если размер не кратен 64 КБ.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="mipmap-packing.md">Упаковочный MIP-карт</a></p></td>
<td align="left"><p>Некоторое количество MIP-карт (для каждого фрагмента массива) можно упаковывать в определенное число плиток в зависимости от размеров потокового ресурса, формата, количества MIP-карт и фрагментов массива.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Связанные разделы


[Создание потоковой передачи ресурсов](creating-streaming-resources.md)

 

 




