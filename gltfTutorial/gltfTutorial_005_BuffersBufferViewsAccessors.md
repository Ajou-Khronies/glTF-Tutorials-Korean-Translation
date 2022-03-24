이전: [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md) | [Table of Contents](README.md) | 다음: [Simple Animation](gltfTutorial_006_SimpleAnimation.md)


# Buffers, BufferViews, and Accessors - 버퍼, 버퍼뷰, 억세서

An example of `buffer`, `bufferView`, and `accessor` objects was already given in the [Minimal glTF File](gltfTutorial_003_MinimalGltfFile.md) section. This section will explain these concepts in more detail.

`buffer`, `bufferView`, `accessor` 객체에 대한 예는 [Minimal glTF File](gltfTutorial_003_MinimalGltfFile.md)절 에서 살펴 보았다. 여기에서는 이 개념에 대해 좀더 상세히 살펴본다. 

## Buffers - 버퍼

A [`buffer`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-buffer) represents a block of raw binary data, without an inherent structure or meaning. This data is referred to by a buffer using its `uri`. This URI may either point to an external file, or be a [data URI](gltfTutorial_002_BasicGltfStructure.md#binary-data-in-buffers) that encodes the binary data directly in the JSON file. The [minimal glTF file](gltfTutorial_003_MinimalGltfFile.md) contained an example of a `buffer`, with 44 bytes of data, encoded in a data URI:

[`buffer`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-buffer)는 원시 바이너리 데이터를 나타낸다. 여기에는 상속된 구조나 의미가 전혀 없다. 이 데이터는 버퍼가 `uri`를 사용해 참조하게 된다. 이 URI는 외부 파일을 가리킬 수도 있고 또는  [data URI](gltfTutorial_002_BasicGltfStructure.md#binary-data-in-buffers)를 사용하여 바이너리 데이터를 인코딩하여 직접 JSON 파일에 삽입될 수 있다. [minimal glTF file](gltfTutorial_003_MinimalGltfFile.md)의 예제에서는 44바이트의 인코딩된 URI를 사용했다. (아래 코드 참조)

```javascript
  "buffers" : [
    {
      "uri" : "data:application/octet-stream;base64,AAABAAIAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAACAPwAAAAA=",
      "byteLength" : 44
    }
  ],
```


<p align="center">
<img src="images/buffer.png" /><br>
<a name="buffer-png"></a>Image 5a: The buffer data, consisting of 44 bytes. - 그림 5a: 44바이트로 구성된 버퍼 데이터
</p>

Parts of the data of a `buffer` may have to be passed to the renderer as vertex attributes, or as indices, or the data may contain skinning information or animation key frames. In order to be able to use this data, additional information about the structure and type of this data is required.

`buffer`의 데이터 중 일부분이 버텍스 속성이나 인덱스 혹은 스키닝 정보나 애니메이션 키프레임으로서 렌더러에 전달된다. 이 데이터를 사용하기 위해서는 데이터의 구조나 자료형에 대한 정보가 필요하다. 

## BufferViews - 버퍼뷰

The first step of structuring the data from a `buffer` is with [`bufferView`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-bufferview) objects. A `bufferView` represents a "slice" of the data of one buffer. This slice is defined using an offset and a length, in bytes. The [minimal glTF file](gltfTutorial_003_MinimalGltfFile.md) defined two `bufferView` objects:

`buffer`로 부터 가져온 데이터를 구조화하는 첫번째 단계는 [`bufferView`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-bufferview) 객체를 사용하는 것이다. `bufferView`는 하나의 버퍼의 데이터를 "분힐" 하는 것을 의미한다. 이 분할은 오프셋과 길이 값을 바이트 단위로 정의한다. [minimal glTF file](gltfTutorial_003_MinimalGltfFile.md) 에서는 두개의 `bufferView` 객체를 정의하고 있다. 

```javascript
  "bufferViews" : [
    {
      "buffer" : 0,
      "byteOffset" : 0,
      "byteLength" : 6,
      "target" : 34963
    },
    {
      "buffer" : 0,
      "byteOffset" : 8,
      "byteLength" : 36,
      "target" : 34962
    }
  ],
```

The first `bufferView` refers to the first 6 bytes of the buffer data. The second one refers to 36 bytes of the buffer, with an offset of 8 bytes, as shown in this image:

첫번째 `bufferView`는 버퍼 데이터의 처음 6바이트를 참조한다. 두번째는 버퍼의 8번째 바이트에서 36바이트를 참조한다. 아래 그림을 참조.  

<p align="center">
<img src="images/bufferBufferView.png" /><br>
<a name="bufferBufferView-png"></a>Image 5b: The buffer views, referring to parts of the buffer.
</p>

The bytes that are shown in light gray are padding bytes that are required for properly aligning the accessors, as described below. 
밝은 회색으로 표시된 바이트들은 패딩(padding, 강제로 채움) 바이트로 억세서가 적절하게 접근하는데 필요한 것이다. 다음에 설명된다.

Each `bufferView` additionally contains a `target` property. This property may later be used by the renderer to classify the type or nature of the data that the buffer view refers to. The `target` can be a constant indicating that the data is used for vertex attributes (`34962`, standing for `ARRAY_BUFFER`), or that the data is used for vertex indices (`34963`, standing for `ELEMENT_ARRAY_BUFFER`).

각 `bufferView`는 추가적으로 `target` 속성을 갖고 있다. 이 속성은 추후 버퍼뷰가 참조하는 데이터의 유형이나 성질을 정의하게 위해 사용된다. `target`은 버텍스 속성을 의미하는 상수가 될 수 있고 (`34962`는 `ARRAY_BUFFER`를 의미) 혹은 버텍스 인덱스가 될 수 있다. (`34963`는 `ELEMENT_ARRAY_BUFFER`를 의미)

At this point, the `buffer` data has been divided into multiple parts, and each part is described by one `bufferView`. But in order to really use this data in a renderer, additional information about the type and layout of the data is required.

여기에서는, `buffer` 데이터가 여러개의 부분으로 구분되어 있고, 각각의 부분이 하나의 `bufferView`로 기술된다. 그러나, 렌더러가 이 데이터를 사용하기 위해서는 추가적인 정보 즉, 자료형과 데이터의 레이아웃에 대한 정보가 요구된다. 

## Accessors - 억세서

An [`accessor`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-accessor) object refers to a `bufferView` and contains properties that define the type and layout of the data of this `bufferView`.

[`accessor`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-accessor) 객체는 `bufferView`를 참조하고, 참조한 `bufferView`의 자료형과 데이터 레이아웃을 정의하는 속성을 갖고 있다. 

### Data type - 자료형

The type of an accessor's data is encoded in the `type` and the `componentType` properties. The value of the `type` property is a string that specifies whether the data elements are scalars, vectors, or matrices. For example, the value may be `"SCALAR"` for scalar values, `"VEC3"` for 3D vectors, or `"MAT4"` for 4&times;4 matrices.

억세서의 데이터 자료형은 `type`과 `componentType`속성을 이용하여 인코딩한다. `type`의 값은 문자열로 스칼라 값, 벡터, 행렬 등으로 지정한다. 예를 들면 `"SCALAR"`는 스칼라 값, `"VEC3"` 는 3차원 벡터, `"MAT4"` 는 4&times;4 행렬을 의미한다.  

The `componentType` specifies the type of the components of these data elements. This is a GL constant that may, for example, be `5126` (`FLOAT`) or `5123` (`UNSIGNED_SHORT`), to indicate that the elements have `float` or `unsigned short` components, respectively.

`componentType`은 이들 데이터 요소의 자료형을 정의한다. 이것은 GL에서 상수로 정의된 것으로, 예를 들면 `5126` (`FLOAT`) 또는 `5123` (`UNSIGNED_SHORT`)로 지정하면 각각  `float` 또는 `unsigned short`로 되어 있음을 의미한다. 

Different combinations of these properties may be used to describe arbitrary data types. For example, the [minimal glTF file](gltfTutorial_003_MinimalGltfFile.md) contained two accessors:

임의의 자료형을 표현하기 위해 서로 다른 조합으로 이 속성을 정의할 수 있다. 예를 들면 [minimal glTF file](gltfTutorial_003_MinimalGltfFile.md)에서는 다음과 같은 두개의 억세서를 갖고 있다. 

```javascript
  "accessors" : [
    {
      "bufferView" : 0,
      "byteOffset" : 0,
      "componentType" : 5123,
      "count" : 3,
      "type" : "SCALAR",
      "max" : [ 2 ],
      "min" : [ 0 ]
    },
    {
      "bufferView" : 1,
      "byteOffset" : 0,
      "componentType" : 5126,
      "count" : 3,
      "type" : "VEC3",
      "max" : [ 1.0, 1.0, 0.0 ],
      "min" : [ 0.0, 0.0, 0.0 ]
    }
  ],
```

The first accessor refers to the `bufferView` with index 0, which defines the part of the `buffer` data that contains the indices. Its `type` is `"SCALAR"`, and its `componentType` is `5123` (`UNSIGNED_SHORT`). This means that the indices are stored as scalar `unsigned short` values.

첫번째 억세서는 인덱스 0으로 `bufferView`를 참조하여 인덱스 데이터를 갖고 있는 `buffer`의 일부를 참조하게 된다. 이 데이터의 `type`이 `"SCALAR"` 이고 `componentType` 은 `5123` (`UNSIGNED_SHORT`) 이다. 이것은 인덱스 값이 `unsigned short` 값으로 되어 있음을 의미한다. 

The second accessor refers to the `bufferView` with index 1, which defines the part of the `buffer` data that contains the vertex attributes - particularly, the vertex positions. Its `type` is `"VEC3"`, and its `componentType` is  `5126` (`FLOAT`). So this accessor describes 3D vectors with floating point components.

두번째 억세서는 인덱스 1로 `bufferView`를 참조하여 버텍스 속성 - 버텍스의 좌표 - 를 갖고 있는 `buffer`의 일부를 참조하게 된다. 이 데이터의 `type`은 `"VEC3"`이고 `componentType` 은  `5126` (`FLOAT`)이다. 이것은 부동소수점 요소로 구성된 3차원 벡터를 의미한다. 


### Data layout - 데이터 레이아웃

Additional properties of an `accessor` further specify the layout of the data. The `count` property of an accessor indicates how many data elements it consists of. In the example above, the count has been `3` for both accessors, standing for the three indices and the three vertices of the triangle, respectively. Each accessor also has a `byteOffset` property. For the example above, it has been `0` for both accessors, because there was only one `accessor` for each `bufferView`. But when multiple accessors refer to the same `bufferView`, then the `byteOffset` describes where the data of the accessor starts, relative to the `bufferView` that it refers to.

`accessor`의 추가적인 속성에는 데이터의 레이아웃 정의가 있다. 억세서의 `count` 속성은 얼마나 많은 요소로 구성되 있는지를 정의한다. 위 예에서는, `count` 값이 `3`으로 되어 있으므로, 3개의 인덱스가 있고, 삼각형을 구성하는 3개의 버텍스가 있음을 뜻한다. 각 억세서는 또한 `byteOffset` 속성을 갖고 있다. 위 예에서는, 두개의 억세서 모두 `0`으로 지정되어 있다. 그 이유는 각 `bufferView`에 단지 하나의 억세서만 있기 때문이다. 하지만, 각은 `bufferView`를 참조하는 억세서가 여러개 있다면 `byteOffset`을 사용하여 억세서가 시작하는 지점을 `bufferView`의 상대적인 위치로 지정한다. 


### Data alignment - 데이터 맞춤(alignment)

The data that is referred to by an `accessor` may be sent to the graphics card for rendering, or be used at the host side as animation or skinning data. Therefore, the data of an `accessor` has to be aligned based on the *type* of the data. For example, when the `componentType` of an `accessor` is `5126` (`FLOAT`), then the data must be aligned at 4-byte boundaries, because a single `float` value consists of four bytes. This alignment requirement of an `accessor` refers to its `bufferView` and the underlying `buffer`. Particularly, the alignment requirements are as follows:

`accessor`에 의해 참조되는 데이터는 렌더링을 위해 그래픽스 카드에 전송하거나 호스트에서 애니메이션이나 스키닝 데이터로 사용될 수 있다. 따라서, `accessor`의 데이터는 그 *자료형*에 따라서 맞추어져 있어야 한다. 예를 들면, `accessor`의 `componentType`이 `5126` (`FLOAT`) 인 경우, 데이터는 4-바이트씩 배치되어야 한다. 왜냐하면 하나의 `float` 값은 4바이트를 차지하기 때문이다. `accessor`가 `bufferView`를 참조하여 `buffer`의 데이터를 올바르게 참조하기 위해서는 이런 맞춤이 요구된다. 특히 다음과 같은 맞춤이 요구된다. 

- The `byteOffset` of an `accessor` must be divisible by the size of its `componentType`. 
- `accessor`의 `byteOffset`은 반드시 `componentType`의 크기의 배수이어야 한다.
- The sum of the `byteOffset` of an accessor and the `byteOffset` of the `bufferView` that it refers to must be divisible by the size of its `componentType`.
- `accessor`의 `byteOffset`과 `bufferView`의 `byteOffset`의 합은 반드시 `componentType`의 크기의 배수이어야 한다.

In the example above, the `byteOffset` of the `bufferView` with index 1 (which refers to the vertex attributes) was chosen to be `8`, in order to align the data of the accessor for the vertex positions to 4-byte boundaries. The bytes `6` and `7` of the `buffer` are thus *padding* bytes that do not carry relevant data. 

위 예제에서와 같이, 인덱스 1인 `bufferView`(버텍스 속성을 참조)의 `byteOffset`은 8로 지정되어 있었다. 따라서 버텍스 좌표의 자료형에 맞추어 4-바이트의 배수로 되어 있다. 따라서 `buffer`의 `6`과 `7` 바이트는 *채움(padding)* 으로 의미없는 데이터가 들어 있다. 

Image 5c illustrates how the raw data of a `buffer` is structured using `bufferView` objects and is augmented with data type information using `accessor` objects.

그림 5c는 `buffer`의 원시 데이터가 `bufferView` 객체로 구조화 되고, `accessor`에 의해 자료형을 부여 받는지를 보여준다. 

<p align="center">
<img src="images/bufferBufferViewAccessor.png" /><br>
<a name="bufferBufferViewAccessor-png"></a>Image 5c: The accessors defining how to interpret the data of the buffer views. - 그림 5c: 억세서 정의를 통한 buffer view의 data 해석 
</p>


### Data interleaving - 데이터 인터리빙

The data of the attributes that are stored in a single `bufferView` may be stored as an *Array-Of-Structures*. A single `bufferView` may, for example, contain the data for vertex positions and for vertex normals in an interleaved fashion. In this case, the `byteOffset` of an accessor defines the start of the first relevant data element for the respective attribute, and the `bufferView` defines an additional `byteStride` property. This is the number of bytes between the start of one element of its accessors, and the start of the next one. An example of how interleaved position and normal attributes are stored inside a `bufferView` is shown in Image 5d.

하나의 `bufferView`내의 버텍스 속성 데이터는 *Array-Of-Structures - 구조체의 배열* 형태로 저장되어 있을 수 있다. 단일 `bufferView`는 예를 들면, 버텍스의 좌표(position)과 법선벡터(normal)가 교대로 들어가 있을 수 있다. 이 경우에, 억세서의 `byteOffset` 은 첫번째 해당 데이터를 가리키도록 정의되어야 하고, `byteStride` 속성을 정의해야 한다. 이 속성은 억세서의 다음 요소로 가기 위해 필요한 바이트 수를 정의한다. 아래 그림 5d는 버텍스의 좌표와 법선 속성이 `bufferView` 내에 인터리빙으로 저장되어 있는 모습을 보여 준다.

<p align="center">
<img src="images/aos.png" /><br>
<a name="aos-png"></a>Image 5d: Interleaved accessors in one buffer view.
</p>


### Data contents - 데이터 컨텐츠

An `accessor` also contains `min` and `max` properties that summarize the contents of their data. They are the component-wise minimum and maximum values of all data elements contained in the accessor. In the case of vertex positions, the `min` and `max` properties thus define the *bounding box* of an object. This can be useful for prioritizing downloads, or for visibility detection. In general, this information is also useful for storing and processing *quantized* data that is dequantized at runtime, by the renderer, but details of this quantization are beyond the scope of this tutorial.

`accessor` 



## Sparse accessors


With version 2.0, the concept of *sparse accessors* was introduced in glTF. This is a special representation of data that allows very compact storage of multiple data blocks that have only a few different entries. For example, when there is geometry data that contains vertex positions, this geometry data may be used for multiple objects. This may be achieved by referring to the same `accessor` from both objects. If the vertex positions for both objects are mostly the same and differ for only a few vertices, then it is not necessary to store the whole geometry data twice. Instead, it is possible to store the data only once, and use a sparse accessor to store only the vertex positions that differ for the second object. 

The following is a complete glTF asset, in embedded representation, that shows an example of sparse accessors:

```javascript
{
  "scenes" : [ {
    "nodes" : [ 0 ]
  } ],
  
  "nodes" : [ {
    "mesh" : 0
  } ],
  
  "meshes" : [ {
    "primitives" : [ {
      "attributes" : {
        "POSITION" : 1
      },
      "indices" : 0
    } ]
  } ],
  
  "buffers" : [ {
    "uri" : "data:application/gltf-buffer;base64,AAAIAAcAAAABAAgAAQAJAAgAAQACAAkAAgAKAAkAAgADAAoAAwALAAoAAwAEAAsABAAMAAsABAAFAAwABQANAAwABQAGAA0AAAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAAAAAQAAAAAAAAAAAAABAQAAAAAAAAAAAAACAQAAAAAAAAAAAAACgQAAAAAAAAAAAAADAQAAAAAAAAAAAAAAAAAAAgD8AAAAAAACAPwAAgD8AAAAAAAAAQAAAgD8AAAAAAABAQAAAgD8AAAAAAACAQAAAgD8AAAAAAACgQAAAgD8AAAAAAADAQAAAgD8AAAAACAAKAAwAAAAAAIA/AAAAQAAAAAAAAEBAAABAQAAAAAAAAKBAAACAQAAAAAA=",
    "byteLength" : 284
  } ],
  
  "bufferViews" : [ {
    "buffer" : 0,
    "byteOffset" : 0,
    "byteLength" : 72,
    "target" : 34963
  }, {
    "buffer" : 0,
    "byteOffset" : 72,
    "byteLength" : 168
  }, {
    "buffer" : 0,
    "byteOffset" : 240,
    "byteLength" : 6
  }, {
    "buffer" : 0,
    "byteOffset" : 248,
    "byteLength" : 36
  } ],
  
  "accessors" : [ {
    "bufferView" : 0,
    "byteOffset" : 0,
    "componentType" : 5123,
    "count" : 36,
    "type" : "SCALAR",
    "max" : [ 13 ],
    "min" : [ 0 ]
  }, {
    "bufferView" : 1,
    "byteOffset" : 0,
    "componentType" : 5126,
    "count" : 14,
    "type" : "VEC3",
    "max" : [ 6.0, 4.0, 0.0 ],
    "min" : [ 0.0, 0.0, 0.0 ],
    "sparse" : {
      "count" : 3,
      "indices" : {
        "bufferView" : 2,
        "byteOffset" : 0,
        "componentType" : 5123
      },
      "values" : {
        "bufferView" : 3,
        "byteOffset" : 0
      }
    }
  } ],
  
  "asset" : {
    "version" : "2.0"
  }
}
```

The result of rendering this asset is shown in Image 5e:

<p align="center">
<img src="images/simpleSparseAccessor.png" /><br>
<a name="simpleSparseAccessor-png"></a>Image 5e: The result of rendering the simple sparse accessor asset.
</p>

The example contains two accessors: one for the indices of the mesh, and one for the vertex positions. The one that refers to the vertex positions defines an additional `accessor.sparse` property, which contains the information about the sparse data substitution that should be applied:


```javascript
  "accessors" : [ 
  ...
  {
    "bufferView" : 1,
    "byteOffset" : 0,
    "componentType" : 5126,
    "count" : 14,
    "type" : "VEC3",
    "max" : [ 6.0, 4.0, 0.0 ],
    "min" : [ 0.0, 0.0, 0.0 ],
    "sparse" : {
      "count" : 3,
      "indices" : {
        "bufferView" : 2,
        "byteOffset" : 0,
        "componentType" : 5123
      },
      "values" : {
        "bufferView" : 3,
        "byteOffset" : 0
      }
    }
  } ],
```

This `sparse` object itself defines the `count` of elements that will be affected by the substitution. The `sparse.indices` property refers to a `bufferView` that contains the indices of the elements which will be replaced. The `sparse.values` refers to a `bufferView` that contains the actual data. 

In the example, the original geometry data is stored in the `bufferView` with index 1. It describes a rectangular array of vertices. The `sparse.indices` refer to the `bufferView` with index 2, which contains the indices `[8, 10, 12]`. The `sparse.values` refers to the `bufferView` with index 3, which contains new vertex positions, namely, `[(1,2,0), (3,3,0), (5,4,0)]`. The effect of applying the corresponding substitution is shown in Image 5f.


<p align="center">
<img src="images/simpleSparseAccessorDescription.png" /><br>
<a name="simpleSparseAccessorDescription-png"></a>Image 5f: The substitution that is done with the sparse accessor.
</p>





이전: [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md) | [Table of Contents](README.md) | Next: [Simple Animation](gltfTutorial_006_SimpleAnimation.md)
