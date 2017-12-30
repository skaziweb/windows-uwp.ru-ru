---
title: "Преобразование типов данных"
description: "В следующих разделах описано, как выполняется обработка преобразований между типами данных в Direct3D."
ms.assetid: B50AB8DE-CAED-465B-B18C-81F3A984B8AC
keywords: "Преобразование типов данных"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 6813f7b55957c185a85fe82b90297b6ba5a470eb
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="data-type-conversion"></a>Преобразование типов данных


В следующих разделах описано, как выполняется обработка преобразований между типами данных в Direct3D.

## <a name="span-iddatatypeterminologyspanspan-iddatatypeterminologyspanspan-iddatatypeterminologyspandata-type-terminology"></a><span id="Data_Type_Terminology"></span><span id="data_type_terminology"></span><span id="DATA_TYPE_TERMINOLOGY"></span>Терминология типов данных


В дальнейшем термины из следующего набора используются для описания преобразований различных форматов.

| Термин  | Определение                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SNORM | Нормализованное целое число со знаком, то есть для n-разрядного числа в дополнительном коде максимальное значение означает 1,0f (например, 5-разрядное значение 01111 сопоставляется с 1,0f), а минимальное значение означает -1,0f (например, 5-разрядное значение 10000 сопоставляется с -1,0f). Кроме того, второе после минимального число сопоставляется с -1,0f (например, 5-разрядное значение 10001 сопоставляется с -1,0f). Таким образом, существует два представления целых чисел для -1,0f. Для 0,0f и 1,0f существует по одному представлению. В результате получается набор представлений целых чисел для равномерно распределенных значений с плавающей точкой в диапазоне от -1,0f до 0,0f и дополнительный набор представлений для чисел в диапазоне от 0,0f до 1,0f |
| UNORM | Нормализованное целое число без знака, то есть для n-разрядного числа все нули означают 0,0f, а все единицы— 1,0f. Представлена последовательность равномерно распределенных значений с плавающей точкой от 0,0f до 1,0f. Например, 2-разрядное число UNORM представляет 0,0f, 1/3, 2/3 и 1,0f.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| SINT  | Целое число со знаком. Целое число в дополнительном коде. Например, 3-разрядное число SINT представляет целочисленные значения -4, -3, -2, -1, 0, 1, 2, 3.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| UINT  | Целое число без знака. Например, 3-разрядное число UINT представляет целочисленные значения 0, 1, 2, 3, 4, 5, 6, 7.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| FLOAT | Значение с плавающей точкой в любом из представлений, определенных Direct3D.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| SRGB  | Аналог UNORM в том смысле, что для n-разрядного числа все нули означают 0,0f, а все единицы— 1,0f. Но в случае с SRGB, в отличие от UNORM, последовательность целочисленных кодировок без знака в диапазоне между всеми нулями и всеми единицами представляет нелинейную последовательность в интерпретации чисел с плавающей точкой в диапазоне от 0,0f до 1,0f. Грубо говоря, если нелинейная последовательность SRGB отображается как последовательность цветов, для "среднего" наблюдателя при "средних" условиях просмотра на "среднем" дисплее она будет выглядеть, как линейный набор уровней яркости. Полные сведения см. в цветовом стандарте SRGB IEC 61996-2-1 на сайте Международной электротехнической комиссии (IEC).                |

 

Указанные выше термины часто используются как "модификаторы имени формата" для описания того, как данные размещены в памяти и какое преобразование следует выполнить на пути транспортировки (возможно, включая фильтрацию) между памятью и блоком конвейера, например шейдером.

## <a name="span-idfloatingpointconversionspanspan-idfloatingpointconversionspanspan-idfloatingpointconversionspanfloating-point-conversion"></a><span id="Floating_Point_Conversion"></span><span id="floating_point_conversion"></span><span id="FLOATING_POINT_CONVERSION"></span>Преобразование с плавающей точкой


При выполнении преобразования с плавающей точкой между разными представлениями, включая представления без плавающей точки, действуют следующие правила.

### <a name="span-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanspan-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanspan-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanconververting-from-a-higher-range-representation-to-a-lower-range-representation"></a><span id="Conververting_from_a_higher_range_representation_to_a_lower_range_representation"></span><span id="conververting_from_a_higher_range_representation_to_a_lower_range_representation"></span><span id="CONVERVERTING_FROM_A_HIGHER_RANGE_REPRESENTATION_TO_A_LOWER_RANGE_REPRESENTATION"></span>Преобразование из представления верхнего диапазона в представление нижнего диапазона

-   При преобразовании в другой формат с плавающей точкой используется округление к нулю Если целевое значение является целым числом или числом с плавающей точкой, используется округление до ближайшего четного числа, если в описании преобразования явно не указана необходимость использования другого метода округления, такого как округление к ближайшему при преобразовании из FLOAT в SNORM, из FLOAT в UNORM или из FLOAT в SRGB Исключениями также являются инструкции шейдеров ftoi и ftou, в которых используется округление к нулю. Наконец, для преобразований чисел с плавающей точкой в числа с фиксированной точкой, используемых текстурным дискретизатором и средством прорисовки, указывается допуск от бесконечно точного идеала, измеряемый единицей наименьшей точности.
-   В случае с исходными значениями за пределами динамического диапазона конечного формата нижнего диапазона (например, большое 32-разрядное значение с плавающей точкой записывается в 16-разрядный RenderTarget с плавающей точкой) получается максимальное представляемое (с соответствующим знаком) значение БЕЗ учета бесконечности со знаком (из-за описанного выше округления к нулю).
-   Не число в формате верхнего диапазона преобразуется в представление не числа в формате нижнего диапазона, если представление не числа существует в формате нижнего диапазона. Если в формате нижнего диапазона нет представления не числа, результатом будет 0.
-   Бесконечное число в формате верхнего диапазона преобразуется в бесконечное число в формате нижнего диапазона, если возможно. Если в формате нижнего диапазона нет представления бесконечного числа, оно преобразуется в максимальное представляемое значение. Знак будет сохранен, если он доступен в конечном формате.
-   Денормализованное число в формате верхнего диапазона преобразуется в представление денормализованного числа в формате нижнего диапазона, если оно есть в формате нижнего диапазона и преобразование возможно. В противном случае результатом будет 0. Бит знака будет сохранен, если он доступен в конечном формате.

### <a name="span-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanspan-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanspan-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanconverting-from-a-lower-range-representation-to-a-higher-range-representation"></a><span id="Converting_from_a_lower_range_representation_to_a_higher_range_representation"></span><span id="converting_from_a_lower_range_representation_to_a_higher_range_representation"></span><span id="CONVERTING_FROM_A_LOWER_RANGE_REPRESENTATION_TO_A_HIGHER_RANGE_REPRESENTATION"></span>Преобразование из представления нижнего диапазона в представление верхнего диапазона

-   Не число в формате нижнего диапазона преобразуется в представление не числа в формате верхнего диапазона, если оно доступно в формате верхнего диапазона. Если в формате верхнего диапазона нет представления не числа, оно преобразуется в 0.
-   Бесконечное число в формате нижнего диапазона преобразуется в представление бесконечного числа в формате верхнего диапазона, если оно доступно в формате верхнего диапазона. Если в формате верхнего диапазона нет представления бесконечного числа, оно преобразуется в максимальное представляемое значение (MAX\_FLOAT в соответствующем формате). Знак будет сохранен, если он доступен в конечном формате.
-   Денормализованное число в формате нижнего диапазона преобразуется в нормализованное представление в формате верхнего диапазона, если это возможно, или в представление денормализованного числа в формате верхнего диапазона при наличии такого представления. Если ни одну из этих операций не удается выполнить из-за отсутствия представления денормализованного числа в формате верхнего диапазона, число преобразуется в 0. Знак будет сохранен, если он доступен в конечном формате. Обратите внимание, что 32-разрядные числа с плавающей точкой считаются форматом без представления денормализованного числа, так как денормализованные числа в операциях с 32-разрядными числами с плавающей точкой приводятся к нулю с сохранением знака.

## <a name="span-idintegerconversionspanspan-idintegerconversionspanspan-idintegerconversionspaninteger-conversion"></a><span id="Integer_Conversion"></span><span id="integer_conversion"></span><span id="INTEGER_CONVERSION"></span>Преобразование целых чисел


В следующей таблице описаны преобразования из различных описанных выше представлений в другие представления. Показаны только преобразования, которые фактически происходят в Direct3D.

В случае с целыми числами, если не указано другое, все описанные ниже преобразования между представлениями целых чисел и чисел с плавающей точкой выполняются точно.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Тип исходных данных</th>
<th align="left">Тип конечных данных</th>
<th align="left">Правило преобразования</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">SNORM</td>
<td align="left">FLOAT</td>
<td align="left"><p>Преобразование n-разрядного целочисленного значения, представляющего диапазон со знаком [от -1,0f до 1,0f], в число с плавающей точкой происходит следующим образом.</p>
<ul>
<li>Наименьшее отрицательное значение сопоставляется с -1,0f. Например, 5-разрядное значение 10000 сопоставляется с - 1,0f.</li>
<li>Все остальные значение преобразуются в числа с плавающей точкой (назовем их c), а результат вычисляется по формуле c * (1,0f / (2⁽ⁿ⁻¹⁾-1)). Например, 5-разрядное значение 10001 преобразуется в -15,0f и делится на 15,0f— в результате получается -1,0f.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">SNORM</td>
<td align="left"><p>Преобразование числа с плавающей точкой в n-разрядное целочисленное значение, представляющее диапазон со знаком [от -1,0f до 1,0f], происходит следующим образом.</p>
<ul>
<li>Пусть c представляет начальное значение.</li>
<li>Если c не число, результатом будет 0.</li>
<li>Если c &gt; 1,0f, включая бесконечные числа, оно прикрепляется к 1,0f.</li>
<li>Если c &lt; -1,0f, включая отрицательные бесконечные числа, оно прикрепляется к -1,0f.</li>
<li>Преобразование из шкалы чисел с плавающей точкой в шкалу целых чисел: c = c * (2ⁿ⁻¹-1).</li>
<li>Преобразование в целое число происходит следующим образом.
<ul>
<li>Если c &gt;= 0, то c = c + 0,5f, в противном случае c = c - 0,5f.</li>
<li>Десятичная дробь опускается, а оставшееся значение с плавающей точкой (целочисленное) напрямую преобразуется в целое число.</li>
</ul></li>
</ul>
<p>Для этого преобразования разрешается допуск в виде единицы наименьшей точности D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_Unit-Last-Place (с целочисленной стороны). Это значит, что после преобразования из шкалы чисел с плавающей точкой в целочисленную шкалу любое значение в пределах единицы наименьшей точности D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP представляемого значения конечного формата может быть сопоставлено с этим значением. Дополнительное требование к обратимости данных гарантирует, что преобразование остается неубывающим в пределах всего диапазона, а все выходные значения— достигаемыми. (В указанных здесь константах <em>xx</em> следует заменить версией Direct3D, например 10, 11 или 12.)</p></td>
</tr>
<tr class="odd">
<td align="left">UNORM</td>
<td align="left">FLOAT</td>
<td align="left"><p>Начальное n-разрядное значение преобразуется в число с плавающей точкой (0,0f, 1,0f, 2,0f и т.д.), а затем делится на (2ⁿ-1).</p></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">UNORM</td>
<td align="left"><p>Пусть c представляет начальное значение.</p>
<ul>
<li>Если c не число, результатом будет 0.</li>
<li>Если c &gt; 1,0f, включая бесконечные числа, оно прикрепляется к 1,0f.</li>
<li>Если c &lt; 0,0f, включая отрицательные бесконечные числа, оно прикрепляется к 0,0f.</li>
<li>Преобразование из шкалы чисел с плавающей точкой в шкалу целых чисел: c = c * (2ⁿ-1).</li>
<li>Преобразование в целое число.
<ul>
<li>c = c + 0,5f.</li>
<li>Десятичная дробь опускается, а оставшееся значение с плавающей точкой (целочисленное) напрямую преобразуется в целое число.</li>
</ul></li>
</ul>
<p>Для этого преобразования разрешается допуск в виде единицы наименьшей точности D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP (с целочисленной стороны). Это значит, что после преобразования из шкалы чисел с плавающей точкой в целочисленную шкалу любое значение в пределах единицы наименьшей точности D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP представляемого значения конечного формата может быть сопоставлено с этим значением. Дополнительное требование к обратимости данных гарантирует, что преобразование остается неубывающим в пределах всего диапазона, а все выходные значения— достигаемыми.</p></td>
</tr>
<tr class="odd">
<td align="left">SRGB</td>
<td align="left">FLOAT</td>
<td align="left"><p>Ниже описано идеальное преобразование SRGB в FLOAT.</p>
<ul>
<li>Возьмем начальное n-разрядное значение, преобразуем его в целое число (0,0f, 1,0f, 2,0f и т.д.); результат обозначим c.</li>
<li>c = c * (1,0f / (2ⁿ-1))</li>
<li>Если (c &lt; = D3D<em>xx</em>_SRGB_TO_FLOAT_THRESHOLD), то: результат = c / D3D<em>xx</em>_SRGB_TO_FLOAT_DENOMINATOR_1, иначе: результат = ((c + D3D<em>xx</em>_SRGB_TO_FLOAT_OFFSET)/D3D<em>xx</em>_SRGB_TO_FLOAT_DENOMINATOR_2)D3D<em>xx</em>_SRGB_TO_FLOAT_EXPONENT</li>
</ul>
<p>Для этого преобразования разрешается допуск в виде единицы наименьшей точности D3D<em>xx</em>_SRGB_TO_FLOAT_TOLERANCE_IN_ULP (со стороны SRGB).</p></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">SRGB</td>
<td align="left"><p>Ниже описано идеальное преобразование FLOAT -&gt; SRGB.</p>
<p>При условии, что конечный цветовой компонент SRGB содержит n бит.</p>
<ul>
<li>Пусть c— это начальное значение.</li>
<li>Если c не число, результатом будет 0.</li>
<li>Если c &gt; 1,0f, включая бесконечные числа, оно прикрепляется к 1,0f.</li>
<li>Если c &lt; 0,0f, включая отрицательные бесконечные числа, оно прикрепляется к 0,0f.</li>
<li>Если (c &lt;= D3D<em>xx</em>_FLOAT_TO_SRGB_THRESHOLD), то: c = D3D<em>xx</em>_FLOAT_TO_SRGB_SCALE_1 * c, иначе: c = D3D<em>xx</em>_FLOAT_TO_SRGB_SCALE_2 * c(D3D<em>xx</em>_FLOAT_TO_SRGB_EXPONENT_NUMERATOR/D3D<em>xx</em>_FLOAT_TO_SRGB_EXPONENT_DENOMINATOR) - D3D<em>xx</em>_FLOAT_TO_SRGB_OFFSET</li>
<li>Преобразование из шкалы чисел с плавающей точкой в шкалу целых чисел: c = c * (2ⁿ-1).</li>
<li>Преобразование в целое число:
<ul>
<li>c = c + 0,5f.</li>
<li>Десятичная дробь опускается, а оставшееся значение с плавающей точкой (целочисленное) напрямую преобразуется в целое число.</li>
</ul></li>
</ul>
<p>Для этого преобразования разрешается допуск в виде единицы наименьшей точности D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP (с целочисленной стороны). Это значит, что после преобразования из шкалы чисел с плавающей точкой в целочисленную шкалу любое значение в пределах единицы наименьшей точности D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP представляемого значения конечного формата может быть сопоставлено с этим значением. Дополнительное требование к обратимости данных гарантирует, что преобразование остается неубывающим в пределах всего диапазона, а все выходные значения— достигаемыми.</p></td>
</tr>
<tr class="odd">
<td align="left">SINT</td>
<td align="left">SINT с дополнительным числом бит</td>
<td align="left"><p>Для преобразования SINT в SINT с дополнительным числом бит старший разряд (MSB) начального числа &quot;дополняется знаком&quot; до дополнительных битов, доступных в конечном формате.</p></td>
</tr>
<tr class="even">
<td align="left">UINT</td>
<td align="left">SINT с дополнительным числом бит</td>
<td align="left"><p>Для преобразования SINT в SINT с дополнительным числом бит число копируется в младшие разряды (LSB) конечного формата, а дополнительные старшие разряды заполняются нулями.</p></td>
</tr>
<tr class="odd">
<td align="left">SINT</td>
<td align="left">UINT с дополнительным числом бит</td>
<td align="left"><p>Для преобразования SINT в UINT с дополнительным числом бит значение прикрепляется к 0, если оно отрицательное. В противном случае число копируется в младшие разряды конечного формата, а дополнительные старшие разряды заполняются нулями.</p></td>
</tr>
<tr class="even">
<td align="left">UINT</td>
<td align="left">UINT с дополнительным числом бит</td>
<td align="left"><p>Для преобразования UINT в UINT с дополнительным числом бит число копируется в младшие разряды конечного формата, а дополнительные старшие разряды заполняются нулями.</p></td>
</tr>
<tr class="odd">
<td align="left">SINT или UINT</td>
<td align="left">SINT или UINT с дополнительным или одинаковым числом бит</td>
<td align="left"><p>Для преобразования SINT или UINT в SINT или UINT с дополнительным или одинаковым числом бит (или изменения наличия знака) начальное значение прикрепляется к диапазону конечного формата.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idfixedpointintegerconversionspanspan-idfixedpointintegerconversionspanspan-idfixedpointintegerconversionspanfixed-point-integer-conversion"></a><span id="Fixed_Point_Integer_Conversion"></span><span id="fixed_point_integer_conversion"></span><span id="FIXED_POINT_INTEGER_CONVERSION"></span>Преобразование целого числа с фиксированной точкой


Целые числа с фиксированной точкой— это целые числа некоторой разрядности с неявным десятичным разделителем в фиксированном расположении.

Распространенный "целочисленный" тип данных— это особый случай целого числа с фиксированной точкой и десятичным числом в конце.

Представления чисел с фиксированной точкой характеризуются как i.f, где i— количество целочисленных бит, а f— дробных бит. Например, 16,8 означает, что за 16 целочисленными битами следуют 8 дробных бит. Целочисленная часть хранится в дополнительном коде, по крайней мере согласно данному здесь определению. Впрочем, для целых чисел без знака можно использовать аналогичное определение. Дробная часть хранится без знака. Дробная часть всегда представляет положительную дробь между двумя ближайшими целочисленными значениями, начиная с наименьшего отрицательного.

Операции сложения и вычитания над числами с фиксированной точкой выполняются с помощью стандартных арифметических действий над целыми числами без учета расположения неявного десятичного разделителя. Добавление 1 к числу с фиксированной точкой 16,8 означает лишь добавление 256, так как десятичный разделитель находится в 8 битах от младшего разряда числа. Другие операции, например умножение, могут также выполняться с помощью арифметических действий над целыми числами, но в этом случае следует учитывать влияние на фиксированный десятичный разделитель. Например, в результате умножения двух целых чисел 16,8 с помощью действия умножения целых чисел получается 32,16.

Представления целых чисел с фиксированной точкой используются в Direct3D двумя способами.

-   Положения вершин после отсечения в средстве прорисовки прикрепляются к фиксированной точке для равномерного распределения точности в области RenderTarget. Многие операции средства прорисовки, включая, например, выбор передней поверхности, выполняются над положениями, прикрепленными к фиксированной точке, а в некоторых операциях, таких как настройка интерполятора атрибутов, используются положения, преобразованные в число с плавающей точкой из положений, прикрепленных к фиксированной точке.
-   Координаты текстуры для операций дискретизации прикрепляются к фиксированной точке (после масштабирования по размеру текстуры) для равномерно распределения точности в области текстуры при выборе расположения и веса блоков задержки фильтра. Значения веса преобразуются в числа с плавающей точкой перед выполнением сами арифметических действий для фильтрации.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Тип исходных данных</th>
<th align="left">Тип конечных данных</th>
<th align="left">Правило преобразования</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">FLOAT</td>
<td align="left">Целое число с фиксированной точкой</td>
<td align="left"><p>Ниже приведена общая процедура преобразования числа с плавающей точкой n в целое число с фиксированной точкой i.f, где i— количество целочисленных бит (со знаком), а f— количество дробных бит.</p>
<ul>
<li>Вычисляем FixedMin = -2⁽ⁱ⁻¹⁾</li>
<li>Вычисляем FixedMax = 2⁽ⁱ⁻¹⁾ - 2<sup>(-f)</sup></li>
<li>Если n— не число, результатом будет 0; если n— положительное бесконечное число, результатом будет FixedMax*2<sup>f</sup>; если n— отрицательное бесконечное число, результатом будет = FixedMin*2<sup>f</sup></li>
<li>Если n &gt;= FixedMax, результатом будет Fixedmax*2<sup>f</sup>; если n &lt;= FixedMin, результатом будет FixedMin*2<sup>f</sup></li>
<li>Иначе вычисляем n*2<sup>f</sup> и преобразуем результат в целое число.</li>
</ul>
<p>Для целочисленного результата реализаций разрешается допуск в виде единицы наименьшей точности D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP вместо бесконечно точного значения n*2<sup>f</sup> после последнего указанного выше действия.</p></td>
</tr>
<tr class="even">
<td align="left">Целое число с фиксированной точкой</td>
<td align="left">FLOAT</td>
<td align="left"><p>Предположим, что определенное представление с фиксированной точкой, преобразуемое в число с плавающей точкой, содержит не более 24 бит информации, из которых не более 23 бит относятся к дробному компоненту. Предположим, что данное число с фиксированной точкой, fxp, имеет форму i.f (i целочисленных бит, f дробных бит). Преобразование в число с плавающей точкой похоже на следующий псевдокод.</p>
<p>Итоговое число с плавающей точкой= (float)(fxp &gt;&gt; f) + // извлечь целое число</p>
((float)(fxp &amp; (2<sup>f</sup> - 1)) / (2<sup>f</sup>)); // извлечь дробь</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Статьи по теме


[Приложения](appendix.md)

 

 



