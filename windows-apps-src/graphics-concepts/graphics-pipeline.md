---
title: Графический конвейер
description: Графический конвейер Direct3D предназначен для создания графики для игр, действие в которых разворачивается в реальном времени. Данные поступают с устройств ввода на устройства вывода, проходя через все настраиваемые или программируемые этапы.
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- Графический конвейер
- Этапы конвейера
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1b931268dc20f40c1bc1d7c700f346d29d6aa9d6
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370615"
---
# <a name="graphics-pipeline"></a>Графический конвейер


Графический конвейер Direct3D предназначен для создания графики для игр, действие в которых разворачивается в реальном времени. Данные поступают с устройств ввода на устройства вывода, проходя через все настраиваемые или программируемые этапы.

Все этапы можно настроить с помощью API Direct3D. Этапы с использованием общих ядер шейдеров (прямоугольные блоки со скругленными краями) можно программировать с помощью языка программирования [HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl). Это обеспечивает высочайшую гибкость и адаптивность конвейера.

Наиболее распространены этапы шейдера вершин (VS) и построителя текстуры (PS). Если не предоставить даже эти этапы шейдеров, используются холостая команда по умолчанию, сквозной шейдер вершин и построитель текстуры.

![схема потока данных в программируемом конвейере direct3d 11](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>Этап сборщика входных данных

|-|-| |Назначение|Этап [сборщика входных данных (IA)](input-assembler-stage--ia-.md) поставляет данные о смежности и примитивах в контейнер, например о треугольниках, линиях и точках, включая семантические идентификаторы, чтобы повысить эффективность шейдеров путем снижения объемов обработки до примитивов, которые еще не были обработаны.| |Входные данные|Данные примитивов (треугольники, линии или точки) из заполненных пользователем буферов в памяти. А также, возможно, данные смежности. Для каждого треугольника есть данные трех вершин и, возможно, еще трех смежных вершин.| |Выходные данные|Примитивы с присоединенными системными значениями (такими как ИД примитива, ИД экземпляра или ИД вершины).|

## <a name="vertex-shader-stage"></a>Этап шейдера вершин

|-|-| |Назначение|На [этапе шейдера вершин (VS)](vertex-shader-stage--vs-.md) происходит обработка вершин, обычно с выполнением таких операций, как преобразования, скиннинг и освещение. Шейдер вершин принимает одну входную вершину и создает одну выходную вершину. Отдельные операции по вершинам, такие как преобразования, скиннинг, морфинг и освещение по вершинам.| |Входные данные|Одна вершина с созданными системой значениями VertexID и InstanceID. Каждая входная вершина шейдера вершин может состоять из 32-разрядных векторов (до 4 компонентов в каждом) в количестве до 16 штук).| |Выходные данные|Одна вершина. Каждая выходная вершина может состоять из 16 32-разрядных четырехкомпонентных векторов.|
 
## <a name="hull-shader-stage"></a>Этап шейдера поверхностей
 
|-|-| |Назначение| [Этап шейдера поверхностей (HS)](hull-shader-stage--hs-.md) — один из этапов тесселяции, на котором одна поверхность модели фактически разбивается на множество треугольников. Шейдер поверхностей вызывается единожды для каждого участка и преобразует входные контрольные точки, определяющие поверхность низкого порядка, в контрольные точки, формирующие участок. Он также выполняет некоторые вычисления для каждого участка, чтобы предоставить данные для этапа тесселяции (TS) и шейдера доменов (DS).| |Входные данные|От 1 до 32 входных контрольных точек, которые вместе определяют поверхность низкого порядка.| |Выходные данные|От 1 до 32 выходных контрольных точек, которые вместе образуют участок. Шейдер поверхностей объявляет состояние для этапа тесселяции (TS), включая количество контрольных точек, тип поверхности участка и нужный тип секционирования при тесселяции.|

## <a name="tessellator-stage"></a>Этап тесселяции

|-|-| |Назначение|На [этапе тесселяции (TS)](tessellator-stage--ts-.md) создается шаблон выборки домена, представляющего участок геометрии, и генерируется набор небольших объектов (треугольников, точек или линий), соединяющих эти пиксели.| |Входные данные|Тесселяция выполняется единожды для каждого участка с использованием факторов тесселяции (указывают тщательность тесселяции домена) и типа секционирования (указывает алгоритм разделения участка на части), переданных с предыдущего этапа шейдера поверхностей. | |Выходные данные|Тесселяция выводит координаты uv (дополнительно — w) и топологию поверхности для этапа шейдера доменов.|

## <a name="domain-shader-stage"></a>Этап шейдера доменов

|-|-| |Назначение|На [этапе шейдера доменов (DS)](domain-shader-stage--ds-.md) вычисляется положение вершины составной точки в выходном участке. Вычисляется положение вершины, соответствующее каждой выборке домена. Шейдер доменов запускается единожды на каждую выходную точку этапа тесселяции и имеет доступ только для чтения к выходному участку шейдера поверхности и константам выходного участка, а также к координатам UV выходных данных этапа тесселяции.| |Входные данные|Шейдер доменов использует выходные контрольные точки [этапа шейдера поверхности (HS)](hull-shader-stage--hs-.md). Выходные данные шейдера поверхности включают следующее: Контрольные точки, постоянные данные исправления и (факторы тесселяции может содержать значения, используемые тесселяции фиксированными функциями, а также необработанных значений - перед округлением по тесселяции целое число — которая упрощает geomorphing, например с факторами тесселяции ). Шейдер доменов вызывается единожды на каждую выходную координату [этапа тесселяции (TS)](tessellator-stage--ts-.md).| |Выходные данные|Этап шейдера доменов (DS) выводит положение вершины составной точки в выходном участке.|

## <a name="geometry-shader-stage"></a>Этап шейдера геометрии

|-|-| |Назначение|[Этап шейдера геометрии (GS)](geometry-shader-stage--gs-.md) обрабатывает целые примитивы: треугольники, линии и точки, а также смежные с ними вершины. Он поддерживает усложнение и упрощение геометрии. Он полезен для таких алгоритмов, как расширение спрайтов точек, динамические системы частиц, создание меха/"плавников", создание объемных теней, однопроходная отрисовка с использованием кубической карты, замена материалов по примитивам и настройка материалов по примитивам, включая создание барицентрических координат в качестве данных примитива, чтобы построитель текстуры мог выполнить настраиваемую интерполяцию атрибутов. | |Входные данные|В отличие от шейдеров вершин, работающих с одной вершиной, входными данными шейдера геометрии являются вершины полного примитива (три вершины для треугольников, две вершины для линий или одна вершина для точек).| |Выходные данные|Этап шейдера геометрии может выводить несколько вершин, формирующих единую выбранную топологию. Доступны следующие выходные топологии шейдера геометрии: <strong>tristrip</strong>, <strong>linestrip</strong> и <strong>pointlist</strong>. Количество сгенерированных примитивов может свободно меняться при каждом вызове шейдера геометрии, но максимальное количество генерируемых вершин необходимо объявить статически. Длины полос, сгенерированных в результате вызова шейдера геометрии, могут быть произвольными, а новые полосы можно создавать с помощью функции HLSL [RestartStrip](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-so-restartstrip).|

## <a name="stream-output-stage"></a>Этап потокового вывода

|-|-| |Назначение|[Этап потокового вывода (SO)](stream-output-stage--so-.md) непрерывно выводит (или выполняет потоковую передачу) данных вершин из предыдущего активного этапа в один или несколько буферов в памяти. Поток данных, выведенный в память, можно снова вернуть в конвейер как входные данные или обратно считать из ЦП.| |Входные данные|Данные вершин из предыдущего этапа конвейера.| |Выходные данные|Этап потокового вывода (SO) непрерывно выдает (ведет потоковую передачу) данных о вершинах из предыдущего активного этапа в один или несколько буферов памяти. Если этап шейдера геометрии (GS) неактивен, а этап потокового вывода (SO) активен, оно непрерывно выводит данные вершин из этапа шейдера доменов (DS) в буферы в памяти (а если этап шейдера доменов также неактивен, то из этапа шейдера вершин (VS)).|

## <a name="rasterizer-stage"></a>Этап растеризации

|-|-| |Назначение|[Этап растеризации (RS)](rasterizer-stage--rs-.md) обрезает примитивы, которых нет в представлении, готовит их для этапа построителя текстуры (PS) и определяет, как вызывать эти построители. Векторные данные (состоящие из фигур или примитивов) преобразуются в растровое изображение (состоящее из пикселей) для отображения трехмерной графики в режиме реального времени.| |Входные данные|Предполагается, что вершины (x, y, z, w), поступающие на этап средства программной прорисовки, находятся в однородном пространстве обрезки. В этом пространстве координат точки оси X указывают вправо, точки Y — вверх, а точки Z — по направлению от камеры.| |Выходные данные|Фактические пиксели для отрисовки. Включает некоторые атрибуты вершин для интерполяции построителем текстуры.|

## <a name="pixel-shader-stage"></a>Этап построителя текстуры
 
|-|-| |Назначение|На [этапе построителя текстуры (PS)](pixel-shader-stage--ps-.md) принимаются интерполированные данные для примитива и генерируются данные для пикселей, такие как цвет.| |Входные данные|Если конвейер настроен без шейдера геометрии, шейдер текстуры ограничивается 16 32-разрядными и 4-компонентными входными данными. В противном случае шейдер пикселей может принимать до 32 32-разрядных, 4-компонентных входных данных. Входные данные шейдера пикселей включают атрибуты вершин (которые можно интерполировать с коррекцией перспективы или без нее) или могут рассматриваться как константы примитивов. Входные данные шейдера пикселей интерполируются на основе атрибутов вершин растеризуемого примитива в зависимости от объявленного режима интерполяции. Если примитив обрезается до растеризации, режим интерполяции также учитывается в процессе обрезки. | |Выходные данные|Пиксельный шейдер может вывести до 8 32-разрядных 4-компонентных цветов (если пиксель отклоняется, цвет не предоставляется). Компоненты регистрации выходных данных шейдера пикселей следует объявить перед использованием. У каждого регистра может быть отдельная маска вывода записи.|

## <a name="output-merger-stage"></a>Этап слияния вывода
 
|-|-| |Назначение|На [этапе слияния вывода](output-merger-stage--om-.md) выходные данные различных типов (значения пиксельного шейдера, информация о глубине и наборе элементов) объединяются с содержимым целевого объекта отрисовки и буферов глубины и набора элементов для создания окончательного результата конвейера.| |Входные данные|Входные данные средства слияния вывода — это состояние конвейера, пиксельные данные, сгенерированные построителями текстур, содержимое целевых объектов отрисовки и буферов глубины и трафаретов.| |Выходные данные|Окончательный цвет отрисованных пикселей.|

## <a name="related-topics"></a>См. также

- [Руководство по обучения графики Direct3D](index.md)

- [Конвейер вычислений](compute-pipeline.md)
