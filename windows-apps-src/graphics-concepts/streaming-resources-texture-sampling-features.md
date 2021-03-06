---
title: Функции создания образцов текстур для потоковых ресурсов
description: Дискретизация текстур потоковых ресурсов включает в себя обратную связь от шейдера о состоянии сопоставленных областей. При этом проверяется, были ли все данные, к которым запрашивается доступ, сопоставлены в ресурсе, выполняется сжатие, чтобы помочь шейдерам избежать областей в потоковых ресурсах, которые не были сопоставлены, и определяется минимальный уровень детализации, полностью сопоставленной для всего фильтра текстуры.
ms.assetid: C2B2DD69-8354-417A-894D-6235A8B48B53
keywords:
- Функции создания образцов текстур для потоковых ресурсов
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: eb0e870aa467641f82d24f03278a199ab56d0c8d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370976"
---
# <a name="streaming-resources-texture-sampling-features"></a>Функции создания образцов текстур для потоковых ресурсов


Дискретизация текстур потоковых ресурсов включает в себя обратную связь от шейдера о состоянии сопоставленных областей. При этом проверяется, были ли все данные, к которым запрашивается доступ, сопоставлены в ресурсе, выполняется сжатие, чтобы помочь шейдерам избежать областей в потоковых ресурсах, которые не были сопоставлены, и определяется минимальный уровень детализации, полностью сопоставленной для всего фильтра текстуры.

## <a name="span-idrequirementsofstreamingresourcestexturesamplingfeaturesspanspan-idrequirementsofstreamingresourcestexturesamplingfeaturesspanspan-idrequirementsofstreamingresourcestexturesamplingfeaturesspanrequirements-of-streaming-resources-texture-sampling-features"></a><span id="Requirements_of_streaming_resources_texture_sampling_features"></span><span id="requirements_of_streaming_resources_texture_sampling_features"></span><span id="REQUIREMENTS_OF_STREAMING_RESOURCES_TEXTURE_SAMPLING_FEATURES"></span>Требования к потоковой передачи ресурсов текстуры функции выборки


Для описанных здесь функций выборки текстур потоковых ресурсов требуется [2 уровень](tier-2.md) поддержки потоковых ресурсов.

## <a name="span-idshaderstatusfeedbackaboutmappedareasspanspan-idshaderstatusfeedbackaboutmappedareasspanspan-idshaderstatusfeedbackaboutmappedareasspanshader-status-feedback-about-mapped-areas"></a><span id="Shader_status_feedback_about_mapped_areas"></span><span id="shader_status_feedback_about_mapped_areas"></span><span id="SHADER_STATUS_FEEDBACK_ABOUT_MAPPED_AREAS"></span>Сообщения о состоянии шейдера о сопоставленных областей


Любая инструкция шейдера, которая считывает из или записывает в потоковый ресурс приводит к записи информации о статусе. Этот статус предоставляется как дополнительное возвращаемое значение в каждой инструкции доступа к ресурсу, которая попадает в 32-разрядный временный регистр. Содержимое возвращаемого значения является непрозрачным. То есть его прямое чтения программой-шейдером запрещено. Однако вы можете использовать функцию [**CheckAccessFullyMapped**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/checkaccessfullymapped), чтобы извлечь сведения о статусе.

## <a name="span-idfullymappedcheckspanspan-idfullymappedcheckspanspan-idfullymappedcheckspanfully-mapped-check"></a><span id="Fully_mapped_check"></span><span id="fully_mapped_check"></span><span id="FULLY_MAPPED_CHECK"></span>Полностью сопоставленные проверки


Функция [**CheckAccessFullyMapped**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/checkaccessfullymapped) интерпретирует статус, возвращенный при доступе к памяти и указывает, все ли запрошенные данные были сопоставлены в ресурсе. **CheckAccessFullyMapped** возвращает значение true (0xFFFFFFFF), если данные сопоставлены и значение false (0x00000000) в ином случае.

Иногда во время выполнения операций фильтров вес определенного текселя окажется равным 0.0. Пример приведен пример линейной по координатам текстуры, которые попадают непосредственно в центре текселя. 3 текселя (какие из них, они могут зависеть от оборудования) участвуют в фильтре, но с весом 0. Эти тексели с нулевым весом совсем не влияют на результат фильтра, так что если они попадут на плитки **NULL**, они не будут считаться доступом без сопоставления. Обратите внимание, что те же гарантии действуют для фильтров текстур с несколькими уровнями MIP-карт. Если тексели в одной из этих карт не сопоставлены, но их вес равен 0, то эти тексели не будут считаться как доступ без сопоставления.

При выборке из формата, имеющих менее 4 компоненты (такие как DXGI\_ФОРМАТ\_R8\_UNORM), любой пикселей текстуры, приходящиеся на **NULL** плитки приводят к **NULL** сопоставить благосостояние доступа сообщаемые, независимо от того, какие компоненты шейдера смотрит в результате. Например, считывание R8\_UNORM и маскирования чтения результат в шейдере с.gba/.yzw бы по-видимому вообще прочитать текстуры. Но если адрес текселя это плитка, сопоставленная с **NULL**, операция все равно считается как доступ к **NULL** карте.

Шейдер может проверить этот статус и выполнить любую линию поведения в случае ошибки. Например, зарегистрировать «промахи» в журнале (например, через UAV-запись) и/или выдать другую инструкцию чтения, скрепленную с более грубым LOD, который точно сопоставлен. Приложению также может потребоваться отслеживать успешные операции доступа, чтобы понять, какая часть сопоставленного набора плиток находится в работе.

Одна сложность для ведения подобного журнала заключается в том, нет механизма для регистрации точного набора плиток, к которым был осуществлен доступ. Приложение может сделать осторожные догадки, зная координаты доступа, а также инструкцию LOD; например, [**tex2Dlod**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-tex2dlod)) возвращает аппаратное вычисление LOD.

Другая сложность — множество операций доступа будет для одних и тех же плиток, поэтому в журнале будет избыточная информация и возможны конфликты при обращении к памяти. Будет удобно, если оборудованию дать возможность не регистрировать доступ к плитке, если это было сделано где-то раньше. Возможно, состояние такого отслеживания можно будет сбросить из API (скорее всего на границе кадра).

## <a name="span-idper-sampleminlodclampspanspan-idper-sampleminlodclampspanspan-idper-sampleminlodclampspanper-sample-minlod-clamp"></a><span id="Per-sample_MinLOD_clamp"></span><span id="per-sample_minlod_clamp"></span><span id="PER-SAMPLE_MINLOD_CLAMP"></span>Clamp MinLOD-sample


Чтобы помочь шейдерам обойти области потоковых ресурсов с MIP-картами, которые заведомо не сопоставлены, большинство инструкций шейдера с применением дискретизатора (фильтрации) имеют режим, который позволяет шейдеру передать дополнительный параметр float32 скрепления MinLOD в выборку текстуры. Это значение указывается в численном пространстве MIP-карты представления в противоположность нижележащему ресурсу.

Оборудование выполняет` max(fShaderMinLODClamp,fComputedLOD) `в том же месте расчета LOD, где происходит поресурсное скрепление MinLOD, а это [**max**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-max)().

Если результат применения сжатия текстур и-sample и других clamps LOD, определенные в ее пустой набор, результат совпадает с за пределы границы доступа результат clamp minLOD каждого ресурса: 0 для компонентов в формат поверхности и значения по умолчанию для отсутствующих компонентов.

Инструкция LOD (например, [**tex2Dlod**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-tex2dlod)), которая предшествует описанному здесь скреплению minLOD для образцов, возвращает скрепленный и нескрепленный LOD. Скрепленный LOD, возвращенный из этой инструкции LOD, отражает все скрепления, включая скрепления по ресурсам, но не скрепления по выборкам. Скрепление по выборке все равно известно шейдеру и контролируется им, поэтому автор шейдера по желанию может вручную применить это скрепление к возвращенному значением инструкции LOD.

## <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Фильтрация сокращения Min/Max


Приложения могут управлять собственными структурами данных, которые будут давать представление о том, как выглядят сопоставления для потокового ресурса. Примером может послужить поверхность с текселем для хранения информации для каждой плитки в потоковом ресурсе. Например, можно сохранить первый LOD, сопоставленный в заданном расположении плитки. Аккуратно изучая эту структуру данных, таким же образом каким будет проводиться выборка потокового ресурса, можно выяснить каким будет отпечаток минимального LOD, полностью сопоставленного с фильтром всей текстуры. Чтобы упростить этот процесс, Direct3D 11.2 предлагает новый режим дискретизатора общего назначения — min/max фильтрация.

Возможности min/max фильтрации для отслеживания LOD могут пригодиться и для других целей, например, для фильтрации поверхности глубин.

Min/max фильтрации сокращения — это режим дискретизаторов, который извлекает тот же набор текселей,что даст и обычный текстурный фильтр. Но вместо смешивания значений для получения результата, он возвращает min() или max() извлеченных текселей по отдельным компонентам (например, минимум всех R-значений, отдельно от минимума всех G-значений и т. д).

Операции min/max действуют согласно правилам арифметической точности Direct3D. Порядок сравнений не важен.

Иногда во время операций фильтра, которые не относится к режиму min/max, вес определенного текселя может оказаться равен 0.0. Примером является линейная выборка с координатами текстуры, точно попадающими на центр текселя: в данном случае 3 других текселя (какие именно зависит от оборудования) участвуют в фильтре, но с весом равным 0. Любые из этих текселей, которые будут иметь вес 0 в фильтре, отличном от min/max, в случае фильтра min/max, по-прежнему не будут влиять на результат (и веса никаким образом не повлияют на работу фильтра min/max).

Поддержка этой функции зависит от поддержки [2 уровня](tier-2.md) потоковых ресурсов.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Связанные разделы


[Конвейер доступ к потоковой передачи ресурсов](pipeline-access-to-streaming-resources.md)

 

 




