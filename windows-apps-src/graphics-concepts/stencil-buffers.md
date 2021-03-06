---
title: Буферы трафаретов
description: Буфер трафаретов применяется для маскирования пикселей в изображения, то есть для формирования специальных эффектов.
ms.assetid: 544B3B9E-31E3-41DA-8081-CC3477447E94
keywords:
- Буферы трафаретов
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bd59c1d32b4f09b58b7e78281e468fbb00a777d9
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291882"
---
# <a name="stencil-buffers"></a>Буферы трафаретов

*Буфер трафарета* используется для применения маски к пикселям в изображении для создания специальных эффектов. Маска определяет рисуется пиксель или нет. Специальные эффекты включают компоновку, перевод изображений, растворение, затемнение и вытеснение, контуры и силуэты, а также двусторонний трафарет. Некоторые из наиболее распространенных эффектов показаны ниже.

Буфер трафарета включает или отключает отрисовку на целевую поверхность попиксельно. На своем фундаментальном уровне буфер позволяет приложениям маскировать части выводимого изображения так, чтобы они не отображались. Приложения часто используют буферы трафарета для специальных эффектов, таких как рассеивание, наложение декалей и контуров.

Данные буфера трафарета внедряются в данные z-буфера.

## <a name="span-idhowthestencilbufferworksspanspan-idhowthestencilbufferworksspanspan-idhowthestencilbufferworksspanhow-the-stencil-buffer-works"></a><span id="How_the_Stencil_Buffer_Works"></span><span id="how_the_stencil_buffer_works"></span><span id="HOW_THE_STENCIL_BUFFER_WORKS"></span>Как работает буфер шаблона

Direct3D выполняет попиксельный тест содержимого буфера трафарета. Для каждого пикселя на целевой поверхности выполняется тест с использованием соответствующего значения из буфера трафарета, контрольного значения буфера и значения маски буфера. В тестовых проходах Direct3D выполняет действие. Тест проводится в несколько шагов.

1.  Выполняется побитовая операция AND над контрольным значением трафарета и маской трафарета.
2.  Выполняется побитовая операция AND со значением буфера трафарета для текущего пикселя и маской трафарета.
3.  Сравнивается результат 1-го шага с результатом 2-го шага 2 с помощью функции сравнения.

Описанные выше шаги показаны в следующей строчке кода:

```cpp
(StencilRef & StencilMask) CompFunc (StencilBufferValue & StencilMask)
```

-   StencilRef представляет собой контрольное значение трафарета.
-   StencilMask это значение маски трафарета.
-   CompFunc является функции сравнения.
-   StencilBufferValue — это содержимое буфера трафарета для текущего пикселя.
-   Символ амперсанда (&) обозначает побитовую операцию AND.

Текущий пиксель записывается на целевую поверхность, если тест трафаретом пройден, в ином случае пиксель пропускается. Стандартный результат сравнения — записать пиксель, независимо от того как разрешилась каждого побитовая операция. Это поведение можно поменять, изменив значение типа перечисления, тем самым указав нужную функцию сравнения.

Приложение может настроить работу буфера трафарета. Приложение может задать функцию сравнения, маску трафарета и контрольное значение трафарета. Таким же образом задается действие, которое Direct3D выполняет при успешном или неудачном происхождении теста.

## <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>Совмещение


Ваше приложение может использовать буфер трафарета для создания двух- или трехмерных изображений в трехмерной сцене. Маска в буфере трафарета используется для ограждения области, которая является поверхностью однобуферной прорисовки. Сохраненную двухмерную информацию, такую как текст или точечные рисунки, затем можно записать в огороженную область. Кроме того, ваше приложение может отрисовывать дополнительные трехмерные примитивы в замаскированный трафаретом регион поверхности однобуферной прорисовки. Приложение даже может отрисовывать всю сцену.

Игры часто предполагают компоновку нескольких трехмерных сцен. Так, в симуляторах вождения, как правило, показано отражение в зеркале заднего вида. В нем отражается происходящее за водителем в трехмерном формате. Это вторая трехмерная сцена, скомпонованная с передним обзором водителя.

## <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>Decaling


В приложениях Direct3D пользователи с помощью переводных картинок контролируют, какие пиксели из определенного изображения-примитива рисуются на поверхность однобуферной прорисовки. Приложения применяют переводные картинки к изображениям примитивов для правильной отрисовки многоугольников.

Например, следы шин и желтая дорожная разметка должны отображаться непосредственно на поверхности дороги. Однако значения разметки и дорожной поверхности по оси Z совпадают. Поэтому велика вероятность нечеткого разделения этих сущностей буфером глубины. Некоторые пиксели в примитиве заднего вида могут отображаться поверх пикселей в примитиве переднего вида, и наоборот. В результате изображение может дрожать при смене кадра. Этот эффект называется *z-конфликт* или *мерцание*.

Чтобы решить эту проблему, используйте трафарет для маскировки раздела примитива заднего вида, в котором будет отображаться переводная картинка. Выключите Z-буферизацию и отрисуйте изображение примитива переднего обзора в замаскированной области поверхности однобуферной прорисовки.

Хоть эту проблему и можно решить наложением нескольких текстур, но такой подход ограничит число других спецэффектов в приложении. Применение буфера трафарета для наложения декалей освобождает этапы наложения текстур для других эффектов.

## <a name="span-iddissolvesfadesandswipesspanspan-iddissolvesfadesandswipesspanspan-iddissolvesfadesandswipesspandissolves-fades-and-swipes"></a><span id="Dissolves__fades__and_swipes"></span><span id="dissolves__fades__and_swipes"></span><span id="DISSOLVES__FADES__AND_SWIPES"></span>Растворение затухание и вставляет


Все чаще в приложениях применяются специальные эффекты, широко распространенные в фильмах и видеороликах, такие как, рассеивание, скольжение и исчезания.

При эффекте растворения одно изображение постепенно заменяется на другое в ходе плавной смены кадров. Несмотря на то, что в Direct3D есть методы для формирования такого же эффекта с помощью наложения нескольких текстур, приложения, применяющие буфер трафарета для рассеивания, могут задействовать возможности по наложению текстур в других эффектах одновременно с рассеиванием.

Когда приложение выполняет рассеивание, необходимо проводить отрисовку двух разных изображений. В этом случае буфер трафарета применяется для контроля отрисовки пикселей из каждого изображения на целевую поверхность. Вы можете определить серию масок трафарета и копировать их в буфер трафарета по мере поступления кадров. Либо же, вы можете определить базовую маску трафарета для первого кадра и постепенно ее менять.

В начале эффекта рассеивания приложение задает функцию и маску трафарета так, чтобы большая часть пикселей начального изображения проходил тест трафаретом. Большинство пикселей из конечного изображения не должно проходить тест трафаретом. В последующих кадрах маска трафарета обновляется так, чтобы все меньше и меньше пикселей начального изображения проходило тест. В дальнейших кадрах все меньше и меньше пикселей конечного изображения не проходят тест. Таким образом приложение может выполнить рассеивание с любым произвольным узором.

Эффект постепенного появления или исчезновения — это особый случай рассеивания. При постепенном появлении, буфер трафарета используется для рассеивания из черного или белого изображения в отрисовку 3D-сцены. Постепенное исчезновение выполняется наоборот, приложение начинает с отрисовки трехмерной сцены и рассеивает ее в черное или белое изображение. Такой переход можно выполнить с любым желаемым узором.

В приложениях Direct3D используется похожий метод для эффекта скольжения. Например? когда приложение выполняет скольжение слева направо, конечное изображение постепенно накатывается поверх начального изображения в направлении слева направо. Как и в случае рассеивания следует определить серию масок трафарета, которые загружаются в буфер трафарета для последующих кадров, или последовательно менять начальную маску трафарета. Маски трафарета используются для отключения записи пикселей из начального изображения и для включения записи пикселей конечного изображения.

Скольжение как эффект несколько более сложен чем рассеивание, так как приложению нужно считывать пиксели конечного изображения в порядке обратном направлению скольжения. То есть, если скольжение идет слева направо, приложение должно считывать пиксели конечного изображения справа налево.

## <a name="span-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanoutlines-and-silhouettes"></a><span id="Outlines_and_silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span>Контуры и силуэтам


Буфер трафарета можно использовать и для более абстрактных эффектов, таких как создание структур и силуэтов.

Если приложение применит к изображению примитива маску трафарета той же формы но чуть меньшего размера, то итоговое изображение будет содержать только контур примитива. Затем приложение может заполнить замаскированную трафаретом область изображения сплошным цветом, создавая для примитива эффект приподнятости.

Если маска трафарета имеет те же размеры и форму, что и примитив, который вы отрисовываете, на полученном изображении на месте примитива будет отверстие. Затем приложение может заполнить это отверстие черным, создавая силуэт примитива.

## <a name="span-idtwo-sidedstencilspanspan-idtwo-sidedstencilspanspan-idtwo-sidedstencilspantwo-sided-stencil"></a><span id="Two-sided_stencil"></span><span id="two-sided_stencil"></span><span id="TWO-SIDED_STENCIL"></span>Двухсторонний шаблон


Теневые тома используются для рисования теней с использованием буфера трафарета. Приложение вычисляет теневые тома, переданные геометрией ограждения, вычисляя края силуэта и вытягивая их от света в набор трехмерных томов. Затем эти тома дважды отрисовываются в буфер трафарета.

В ходе первой отрисовки рисуются многоугольники переднего обзора и увеличиваются значения буфера трафарета. В ходе второй отрисовки рисуются многоугольники заднего вида, относящиеся к теневому тому, а значения буфера трафарета уменьшаются. Как правило, значения увеличения и уменьшения уравновешивают друг друга. Однако сцена уже была отрисована с нормальной геометрией, из-за чего некоторые пиксели не прошли проверку Z-буфера при отрисовке теневого тома. Значения, оставшиеся в буфере трафарета, соответствуют пикселям в тени. Оставшееся содержимое буфера трафарета используется в качестве маски, чтобы выполнить альфа-смешение крупного, всеобъемлющего черного квартета со сценой. Если буфер трафарета используется в качестве маски, в результате затеняются пиксели, которые находятся в тени.

Это означает, что теневая геометрия рисуется дважды для каждого источника света, оказывая давление на пропускную способность вершин графического процессора. Для устранения этого эффекта разработана функция двустороннего трафарета. При таком подходе существует два набора состояний трафарета (они указаны ниже): один — для треугольников переднего обзора, другой — для треугольников заднего вида. В этом случае для каждого теневого тома рисуется по одному проходу на источник освещения.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Связанные разделы


[Буферы глубины и трафаретов](depth-and-stencil-buffers.md)

 

 




