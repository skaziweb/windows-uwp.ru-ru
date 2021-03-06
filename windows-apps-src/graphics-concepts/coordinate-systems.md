---
title: Системы координат
description: Как правило, в приложениях с трехмерной графикой используется один или два типа Декартовой системы координат (левостороннюю и правостороннюю). В обеих системах координат положительная ось x указывает вправо, а положительная ось y — вверх.
ms.assetid: 138D9B81-146F-4E9F-B742-1EDED8FBF2AE
keywords:
- Системы координат
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f85bf490bd1dd68e2d0ba31335f2fc0f89fe27b0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655799"
---
# <a name="coordinate-systems"></a>Системы координат


Как правило, в приложениях с трехмерной графикой используется один или два типа Декартовой системы координат: левостороннюю и правостороннюю. В обеих системах координат положительная ось x указывает вправо, а положительная ось y — вверх.

## <a name="span-idleftandrighthandedcoordinatesspanspan-idleftandrighthandedcoordinatesspanspan-idleftandrighthandedcoordinatesspanleft-and-right-handed-coordinates"></a><span id="Left_and_right_handed_coordinates"></span><span id="left_and_right_handed_coordinates"></span><span id="LEFT_AND_RIGHT_HANDED_COORDINATES"></span>Влево и вправо передаются координаты


Можно запомнить, в каком направлении указывает положительная ось z, указав пальцами левой или правой руки в направлении положительной оси x и скруглив их в направлении положительной оси y. Направление, в котором указывают ваши пальцы (к вам или от вас), — это направление, в котором указывает положительная ось z в этой системе координат. На следующем рисунке показаны эти две системы координат.

![изображение левосторонней и правосторонней Декартовых систем координат](images/leftrght.png)

В Direct3D используется левосторонняя система координат. Несмотря на то что левосторонние и правосторонние системы координат относятся к наиболее распространенным, в программном обеспечении для работы с трехмерными объектами существует множество других систем координат. Например, приложения для трехмерного моделирования часто используют систему координат, в которой ось y указывает в направлении смотрящего или от него, а ось z — вверх.

## <a name="span-idverticesandvectorsspanspan-idverticesandvectorsspanspan-idverticesandvectorsspanvertices-and-vectors"></a><span id="Vertices_and_vectors"></span><span id="vertices_and_vectors"></span><span id="VERTICES_AND_VECTORS"></span>Вершины и векторы


В системе координат координата x, y или z может определять точку в пространстве ("вершину") или трехмерное направление ("вектор").

Коллекцию вершин можно использовать для определения строк и фигур. Простейшие объекты, определяемые вершинами, называются [Примитивы](primitives.md), а более сложный объект, определяемый набором примитивов, — "сетка".

Ключевые операции, выполняемые с сетками, которые определены в трехмерной системе координат: преобразование, поворот и масштабирование. Можно комбинировать эти базовые трансформации и создавать матрицу трансформаций. Подробные сведения см. в разделе [Преобразования](transforms.md).

При объединении этих операций результаты не будут коммутативными; важен порядок умножения матриц.

## <a name="span-idportingfromaright-handedcoordinatesystemspanspan-idportingfromaright-handedcoordinatesystemspanspan-idportingfromaright-handedcoordinatesystemspanporting-from-a-right-handed-coordinate-system"></a><span id="Porting_from_a_right-handed_coordinate_system"></span><span id="porting_from_a_right-handed_coordinate_system"></span><span id="PORTING_FROM_A_RIGHT-HANDED_COORDINATE_SYSTEM"></span>Перенос приложений из правостороннюю систему координат


При переносе приложения, основанного на правосторонней системе координат, необходимо внести два изменения в данные, передаваемые Direct3D:

-   Измените порядок вершин треугольников так, чтобы система обходила их по часовой стрелке от передней. Другими словами, если имеются вершины v0, v1, v2, передайте их в Direct3D как v0, v2, v1.
-   Используйте матрицу представления для масштабирования пространства на -1 по оси z. Чтобы сделать это, менять знак \_31, \_32, \_33, и \_34 член структуры матрицы, используемого для представления матрицы.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Связанные разделы


[Системы координат и геометрия](coordinate-systems-and-geometry.md)

 

 




