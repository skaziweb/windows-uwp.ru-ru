---
title: Представления
description: Термин \ 0034;представление \ 0034; используется для обозначения данных в требуемом формате. Например, "представление буфера констант" (CBV) — это данные буфера констант в правильном формате. В этом разделе описаны наиболее распространенные и полезные представления.
ms.assetid: 0C7FB99F-7391-472F-BA53-576888DFC171
keywords:
- Представления
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: df106a400a7ba8c8f94aa6dd35325aabacd36eca
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663299"
---
# <a name="views"></a>Представления


Термин "представление" используется для обозначения данных в требуемом формате. Например, "представление буфера констант" (CBV) — это данные буфера констант в правильном формате. В этом разделе описаны наиболее распространенные и полезные представления.

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
<td align="left"><p><a href="constant-buffer-view--cbv-.md">Представление буфера констант (CBV)</a></p></td>
<td align="left"><p>Буферы констант содержат постоянные данные шейдера. Их роль в том, что данные сохраняются и остаются доступными любому шейдеру графического процессора до тех пор, пока данные не нужно будет менять.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vertex-buffer-view--vbv-.md">Представления буфера вершин (VBV) и представления буфера индекса (IBV)</a></p></td>
<td align="left"><p>Буфер вершин содержит данные для списка вершин. Данные по каждой вершине могут включать в себя положение, цвет, вектор нормали, координаты текстуры и т. д. Буфер индексов содержит целочисленные индексы (смещения) в буфере вершин и используется для определения и отрисовки объекта, состоящего из подмножества полного списка вершин.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="shader-resource-view--srv-.md">Представление ресурсов шейдера (SRV) и представления для неупорядоченного доступа (UAV)</a></p></td>
<td align="left"><p>Представления ресурсов шейдера обычно оборачивают текстуры в формат, позволяющий шейдерам осуществлять к ним доступ. Представление неупорядоченного доступа обеспечивает аналогичные функциональные возможности, однако позволяет осуществлять чтение и запись текстуры (или другого ресурса) в любом порядке.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="sampler.md">Образец</a></p></td>
<td align="left"><p>Дискретизация — это процесс чтения входных значений из текстуры или другого ресурса. &quot;Дискретизатор&quot; — это любой объект, который осуществляет чтение из ресурсов.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-target-view--rtv-.md">Представление целевого объекта (RTV) прорисовки</a></p></td>
<td align="left"><p>Целевые объекты отрисовки дают возможность визуализировать сцену во временном промежуточном буфере, а не в заднем буфере для отрисовки на экране. Эта функция позволяет использовать сложную сцену, которая может отрисовываться — например, в качестве текстуры отражения или другого элемента в графическом конвейере, или же для добавления дополнительных эффектов пиксельного шейдера к сцене перед отрисовкой.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="depth-stencil-view--dsv-.md">Представление трафарета глубины (DSV)</a></p></td>
<td align="left"><p>Представление трафарета глубины предоставляет формат и буфер для хранения сведений о глубине и трафарете. Буфер глубины используется для удаления отображаемых пикселей, которые не будут видны наблюдателю, так как они закрыты более близким объектом. Буфер трафарета можно использовать для удаления всех отображаемых пикселей за пределами заданной области.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="stream-output-view--sov-.md">Просмотр выходных данных Stream (SOV)</a></p></td>
<td align="left"><p>Представления выходного потока позволяют передавать полученную от шейдеров вершин, тесселяции и геометрии информацию о вершинах обратно приложению для дальнейшего использования. Например, объект, который был искажен этими шейдерами, можно записать обратно в приложение для предоставления более точных входных данных для физического или другого модуля. Однако на практике представления выходного потока — это редко используемая функция графического конвейера.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="rasterizer-ordered-view--rov-.md">Средство программной прорисовки упорядоченные представление (ROV)</a></p></td>
<td align="left"><p>Упорядоченные представления средств программной прорисовки позволяют обойти некоторые ограничения буфера глубины, в частности проблему нескольких текстур, содержащих прозрачность и применяемых к одним и тем же пикселям.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Связанные разделы


[Руководство по обучения графики Direct3D](index.md)

 

 




