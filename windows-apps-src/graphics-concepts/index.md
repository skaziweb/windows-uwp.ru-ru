---
title: "Обучающее руководство по графике Direct3D"
description: "Описывает графические концепции, служащие основой для Microsoft Direct3D."
ms.assetid: c3850a92-4d05-4f72-bf0f-6a0c79e841eb
keywords: "Обучающее руководство по графике Direct3D"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 9d46a13844fafc5f517fce16c39e33257ff8e9a5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="direct3d-graphics-learning-guide"></a>Обучающее руководство по графике Direct3D


Описывает графические концепции, служащие основой для Microsoft Direct3D. Этот комплект документов практически не привязан к какой-либо конкретной версии Direct3D и предназначен для разработчиков графики, которым требуется дополнительная общая информация, не представленная в документации по определенным версиям API.

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
<td align="left"><p>[Системы координат и геометрия](coordinate-systems-and-geometry.md)</p></td>
<td align="left"><p>Программирование приложений Direct3D требует понимания принципов работы с трехмерными геометрическими фигурами. В этом разделе вводятся наиболее важные для создания трехмерных сцен геометрические концепции.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Буферы вершин и индексов](vertex-and-index-buffers.md)</p></td>
<td align="left"><p><em>Буферы вершин</em> — это буферы памяти, которые содержат данные вершин; вершины в буфере вершин обрабатываются в целях выполнения преобразований, освещения и кадрирования. <em>Буферы индексов</em> — это буферы памяти, содержащие данные индексов, представляющие собой целочисленные указатели для буферов вершин, которые используются для отрисовки примитивов.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Устройства](devices.md)</p></td>
<td align="left"><p>Устройство Direct3D является выполняющим отрисовку графики Direct3D компонентом. Устройство инкапсулирует и сохраняет состояние отрисовки, выполняет операции преобразования и освещения и растеризует изображения на поверхности.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Освещение](lights-and-materials.md)</p></td>
<td align="left"><p>Для освещения объектов в сцене используются источники света. Цвет каждой вершины объекта основан на текущей карте текстур, цветах вершин и источниках света.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Буферы глубины и трафаретов](depth-and-stencil-buffers.md)</p></td>
<td align="left"><p><em>Буфер глубины</em> хранит сведения о глубине для управления тем, какие области многоугольников отображаются, а какие скрываются в текущем отображении. <em>Буфер трафарета</em> используется для применения маски к пикселям в изображении, для создания специальных эффектов, включая компоновку, перевод изображений, растворение, затемнение и вытеснение, контуры и силуэты, а также двусторонний трафарет.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Текстуры](textures.md)</p></td>
<td align="left"><p>Текстуры — это очень мощное средство для обеспечения реалистичности созданных на компьютере трехмерных изображений. Direct3D поддерживает широкий набор функций для работы с текстурами, предоставляя разработчикам удобный доступ к дополнительным методам обработки текстур.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Графический конвейер](graphics-pipeline.md)</p></td>
<td align="left"><p>Графический конвейер Direct3D предназначен для создания графики для игр, действие в которых разворачивается в реальном времени. Данные поступают с устройств ввода на устройства вывода, проходя через все настраиваемые или программируемые этапы.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Представления](views.md)</p></td>
<td align="left"><p>Термин &quot;представление&quot; используется для обозначения &quot;данных в требуемом формате&quot;. Например «Представление буфера констант» (CBV) является набором данных буфера констант в правильном формате. В этом разделе описаны наиболее распространенные и полезные представления.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Конвейер вычислений](compute-pipeline.md)</p></td>
<td align="left"><p>Конвейер вычислений Direct3D предназначен для обработки вычислений, которые могут в основном выполняться параллельно графическому конвейеру.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Ресурсы](resources.md)</p></td>
<td align="left"><p>Ресурс — это область памяти, которая может быть доступна конвейеру Direct3D. Чтобы конвейер получал доступ к памяти эффективным образом, данные, предоставляемые конвейеру (например, входные геометрические данные, ресурсы шейдеров и текстуры), должны храниться в ресурсах. Существует два типа ресурсов, из которых образуются все ресурсы Direct3D: буфер и текстура. На каждом этапе конвейера могут быть активны до 128 ресурсов.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Потоковые ресурсы](streaming-resources.md)</p></td>
<td align="left"><p><em>Потоковые ресурсы</em>— это большие логические ресурсы, потребляющие малые объемы физической памяти. Вместо того, чтобы передавать весь большой ресурс, потоком передаются только малые части ресурса. Ранее потоковые ресурсы назывались <em>мозаичными ресурсами</em>.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Приложения](appendix.md)</p></td>
<td align="left"><p>Эти разделы содержат подробные технические сведения.</p></td>
</tr>
</tbody>
</table>

 

 

 