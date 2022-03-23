Previous: [Basic glTF Structure](gltfTutorial_002_BasicGltfStructure.md) | [Table of Contents](README.md) | Next: [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md)


# A Minimal glTF File - 최소 glTF 파일

The following is a minimal but complete glTF asset, containing a single, indexed triangle. You can copy and paste it into a `gltf` file, and every glTF-based application should be able to load and render it. This section will explain the basic concepts of glTF based on this example.

다음은 glTF 자산을 완성하기 위해 필요한 최소한의 것들을 모은 것으로, 하나의 인덱싱된 삼각형을 갖고 있습니다. 이것을 복사해서 `gltf` 파일에 붙여넣기 하며, glTF 기반의 응용 프로그램에서 로딩하여 렌더링 해 볼 수 있다. 이 절은 glTF 기초 개념을 예제로 설명하기 위한 것이다.  

```javascript
{
  "scene": 0,
  "scenes" : [
    {
      "nodes" : [ 0 ]
    }
  ],
  
  "nodes" : [
    {
      "mesh" : 0
    }
  ],
  
  "meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1
        },
        "indices" : 0
      } ]
    }
  ],

  "buffers" : [
    {
      "uri" : "data:application/octet-stream;base64,AAABAAIAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAACAPwAAAAA=",
      "byteLength" : 44
    }
  ],
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
  
  "asset" : {
    "version" : "2.0"
  }
}
```

<p align="center">
<img src="images/triangle.png" /><br>
<a name="triangle-png"></a>Image 3a: A single triangle - 그림 3a: 하나의 삼각형
</p>


## The `scene` and `nodes` structure - `scene` 과 `nodes` 구조

The [`scenes`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-scene) array is the entry point for the description of the scenes that are stored in the glTF. When parsing a glTF JSON file, the traversal of the scene structure will start here. Each scene contains an array called `nodes`, which contains the indices of [`node`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-node) objects. These nodes are the root nodes of a scene graph hierarchy.

[`scenes`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-scene) 배열은 장면 정의에 대한 시작점으로 glTF 파일에 저장됩니다. glTF JSON 파일을 파싱할때, 장면 구조에 대한 탐색이 여기에서 시작하게 됩니다. 각각의 장면은 `nodes`라고 불리는 배열을 갖고 있으며, 여기에는 [`node`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-node) 객체의 인덱스를 갖고 있다. 이들 노드들이 장면 그래프의 루트 노드가 된다. 

The example here consists of a single scene. The `scene` property indicates that this scene is the default scene that should be displayed when the asset is loaded. The scene refers to the only node in this example, which is the node with the index 0. This node, in turn, refers to the only mesh, which has the index 0:

이 예제에서는 하나의 장면만을 갖고 있다. `scene` 속성은 이 장면이 기본 장면(default scene)이라는 것을 알려주고 있다. 따라서, 자산이 로딩될때 이 장면이 디스플레이되어야 한다. 이 예제에서는 장면은 단 하나의 노드만을 참조하고 있기 때문에, 노드의 인덱스는 0 이 되며, 이 노드는 인덱스가 0인 메쉬를 참조하게 된다. 


```javascript
  "scene": 0,
  "scenes" : [
    {
      "nodes" : [ 0 ]
    }
  ],
  
  "nodes" : [
    {
      "mesh" : 0
    }
  ],
```

More details about scenes and nodes and their properties will be given in the [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md) section.

`scenes` 와 `nodes` 와 이에대한 속성에 대한 상세한 내용은 [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md)절에서 다룬다.


## The `meshes`

A [`mesh`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh) represents an actual geometric object that appears in the scene. The mesh itself usually does not have any properties, but only contains an array of [`mesh.primitive`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh-primitive) objects, which serve as building blocks for larger models. Each mesh primitive contains a description of the geometry data that the mesh consists of.

[`mesh`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh)는 장면에 나타날 실제 기하 형상을 표현한다. 메쉬 그 자신은 일반적으로 어떤 속성도 갖고 있지 않고, [`mesh.primitive`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh-primitive) 객체의 배열만을 갖게 된다. 이 객체는 더 큰 모델을 위한 빌딩 블록의 역할을 하게된다. 각각의 메쉬 프리미티브는 메쉬를 구성하는 기하 정보가 포함되어 있다.

The example consists of a single mesh, and has a single `mesh.primitive` object. The mesh primitive has an array of `attributes`. These are the attributes of the vertices of the mesh geometry, and in this case, this is only the `POSITION` attribute, describing the positions of the vertices. The mesh primitive describes an *indexed* geometry, which is indicated by the `indices` property. By default, it is assumed to describe a set of triangles, so that three consecutive indices are the indices of the vertices of one triangle.

예제에서는 하나의 메쉬가, 하나의 `mesh.primitive` 객체를 갖고 있다. 이 메쉬 프리미티브는 `attributes` 배열을 갖고 있다. 이들은 메쉬 기하의 버텍스의 속성 정보를 갖고 있다. 이 예제에서는 단지 `POSITION` 속성만으로 버텍스의 좌표를 표현하고 있다. 메쉬 프리미티브는 *인덱스(indexed)* 기하를 표현하는데, `indices` 속성을 써서 나타낸다. 기본적으로 삼각형의 집합을 표현한다고 가정하기 때문에, 3개의 연속된 인덱스들은 하나의 삼각형을 구성하는 인덱스가 된다.

The actual geometry data of the mesh primitive is given by the `attributes` and the `indices`. These both refer to `accessor` objects, which will be explained below.

실제 메쉬 프리미티브의 기하 데이터는 `attributes` 와 `indices`를 사용하여 주어지게 된다. 이들은 `accessor` 객체를 참조할 수 있는데, 다음 부분에서 설명된다.

```javascript
  "meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1
        },
        "indices" : 0
      } ]
    }
  ],
```

A more detailed description of meshes and mesh primitives can be found in the [meshes](gltfTutorial_009_Meshes.md) section.

메쉬와 메쉬 프리미티브에 대한 상세한 설명은 [meshes](gltfTutorial_009_Meshes.md)절에서 다룬다.


## The `buffer`, `bufferView`, and `accessor` concepts

The `buffer`, `bufferView`, and `accessor` objects provide information about the geometry data that the mesh primitives consist of. They are introduced here quickly, based on the specific example. A more detailed description of these concepts will be given in the [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md) section.

`buffer`, `bufferView`, `accessor` 객체는 메쉬 프리미티브가 구성하는 기하 데이터에 대한 정보를 제공한다. 여기에서는 예제에 있는 내용으로 간단히 소개하고 상세한 내용은 [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md) 절에서 설명한다.

### Buffers - 버퍼

A [`buffer`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-buffer) defines a block of raw, unstructured data with no inherent meaning. It contains an `uri`, which can either point to an external file that contains the data, or it can be a [data URI](gltfTutorial_002_BasicGltfStructure.md#binary-data-in-data-uris) that encodes the binary data directly in the JSON file.

[`buffer`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-buffer)는 원시, 구조회되지 않은 데이터의 블록을 상속의미 없이 정의한다. 여기에는 `uri` 가 포함되는데, 데이터를 갖고 있는 외부 파일을 가리키거나, [data URI](gltfTutorial_002_BasicGltfStructure.md#binary-data-in-data-uris)를 사용해서 JSON 파일 내부에 직접 인코딩하여 넣을 수 있다. 

In the example file, the second approach is used: there is a single buffer, containing 44 bytes, and the data of this buffer is encoded as a data URI:

이 예제 파일에서는, 두번째 방법이 사용되었다. 하나의 버퍼가 있고, 44 바이트를 갖고 있으며 이 버퍼의 데이터는 data URI로 인코딩 되었다. 

```javascript
  "buffers" : [
    {
      "uri" : "data:application/octet-stream;base64,AAABAAIAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAACAPwAAAAA=",
      "byteLength" : 44
    }
  ],
```

This data contains the indices of the triangle, and the vertex positions of the triangle. But in order to actually use this data as the geometry data of a mesh primitive, additional information about the *structure* of this data is required. This information about the structure is encoded in the `bufferView` and `accessor` objects.

이 데이터는 삼각형의 인덱스와 삼각형을 구성하는 버텍스의 좌표가 들어 있다. 하지만, 실제 이 데이터를 메쉬 프리미티브의 기하 데이터로 사용하기 위해서는, 이 데이터의 *구조(strudcture)* 를 설명하는 추가 정보가 필요하다. 이 구조에 대한 추가 정보는 `bufferView` 와 `accessor` 객체에 인코딩 된다.

### Buffer views - 버퍼 뷰

A [`bufferView`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-bufferview) describes a "chunk" or a "slice" of the whole, raw buffer data. In the given example, there are two buffer views. They both refer to the same buffer. The first buffer view refers to the part of the buffer that contains the data of the indices: it has a `byteOffset` of 0 referring to the whole buffer data, and a `byteLength` of 6. The second buffer view refers to the part of the buffer that contains the vertex positions. It starts at a `byteOffset` of 8, and has a `byteLength` of 36; that is, it extends to the end of the whole buffer.

[`bufferView`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-bufferview) 는 원시 버퍼 데이터 전체에 대한 "뭉치" 혹은 "조각"들을 서술한다. 우리 예제에서는, 두개의 버퍼 뷰가 있다. 두개 모두 같은 버퍼를 참조하고 있다. 첫번째 버퍼 뷰는 버퍼의 일부분을 참조하고 있는데 여기에는 인덱스 정보가 포함되어 있다. `byteOffset` 이 0 이 의미하는 것은 전체 버퍼 데이터를 참조하는 것이며, `byteLength` 가 6 임을 의미한다. 두번째 버퍼 뷰는 버퍼의 다른 부분을 참조하는 것으로 버텍스의 좌표 정보들을 갖고 있다. `byteOffset`이 8이고, `byteLength`가 36으로 버퍼의 나머지 전체 부분을 의미한다. 

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


### Accessors - 억세서

The second step of structuring the data is accomplished with [`accessor`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-accessor) objects. They define how the data of a `bufferView` has to be interpreted by providing information about the data types and the layout.

데이터를 구조화하는 두번째 단계는 [`accessor`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-accessor) 객체로 이루어 진다. 여기에선는 `bufferView`의 데이터들이 자료형과 데이터의 레이아웃 정보가 사용해서 어떻게 해석되야 하는지를 정의한다.

In the example, there are two accessor objects.

이 예제에서는, 두개의 억세서 객체가 있다.

The first accessor describes the indices of the geometry data. It refers to the `bufferView` with index 0, which is the part of the `buffer` that contains the raw data for the indices. Additionally, it specifies the `count` and `type` of the elements and their `componentType`. In this case, there are 3 scalar elements, and their component type is given by a constant that stands for the `unsigned short` type.

첫번째 억세서는 기하 데이터의 인덱스를 설명한다. `bufferView`에 인덱스 0으로 참조하여, 인덱스의 원시 데이터를 갖고 있는 `buffer`의 일부분에 접근한다. 추가적으로  `count`와 `type`, `componentType` 요소를 정의한다. 이 예제에서는 3개의 스칼라 요소로, 각 콤포넌트는 유형은  `unsigned short`를 나타내는 상수로 주어졌다. 

The second accessor describes the vertex positions. It contains a reference to the relevant part of the buffer data, via the `bufferView` with index 1, and its `count`, `type`, and `componentType` properties say that there are three elements of 3D vectors, each having `float` components.

두번째 억세서는 버텍스의 좌표를 지정한다. 여기에는 `bufferView`의 인덱스 1을 사용해 버퍼 데이터에 해당 부분으로 가는 참조를 갖고 있고, 여기에 `count`, `type`, `componentType` 속성으로 3개로 구성된 3D 벡터가 있고, 각각의 자료형은 `float` 임을 정의할 수 있다.


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

As described above, a `mesh.primitive` may now refer to these accessors, using their indices:

위에서 설명한 바와 같이 `mesh.primitive`는 이들 `accessors`를 인덱스를 사용하여 참조할 수 있다. 

```javascript
  "meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1
        },
        "indices" : 0
      } ]
    }
  ],
```

When this `mesh.primitive` has to be rendered, the renderer can resolve the underlying buffer views and buffers and will send the required parts of the buffer to the renderer, together with the information about the data types and layout. A more detailed description of how the accessor data is obtained and processed by the renderer is given in the [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md) section and the [Materials and Techniques](gltfTutorial_013_MaterialsTechniques.md) section.

이 `mesh.primitive`가 렌더링 될 때, 렌더러는 해당 버퍼 뷰와 버퍼를 해석해서 버퍼의 필요한 부분을 렌더러에게 전달해 준다. 이때 자료형과 데이터의 레이아웃 정보를 함께 전달한다. 좀더 상세한 정보는 [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md)절과 [Materials and Techniques](gltfTutorial_013_MaterialsTechniques.md) 절에서 다룬다. 




## The `asset` description - 정의

In glTF 1.0, this property is still optional, but in subsequent glTF versions, the JSON file is required to contain an `asset` property that contains the `version` number. The example here says that the asset complies to glTF version 2.0:

glTF 1.0에서는 이 속성이 옵션이었으나, 다음 버전에서는 JSON 파일에 `asset` 속성에 `version` 번호를 포함하도록 요구하고 있다. 아래 예제는 glTF 2.0 버전으로 설정한 예이다.

```javascript
  "asset" : {
    "version" : "2.0"
  }
```

The `asset` property may contain additional metadata that is described in the [`asset` specification](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-asset).

`asset` 속성은 추가적인 메타데이터를 포함할 수 있다. 상세 내용은 표준문서 [`asset` specification](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-asset)를 참조하기 바란다. 




이전: [Basic glTF Structure](gltfTutorial_002_BasicGltfStructure.md) | [Table of Contents](README.md) | 다음: [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md)
