이전: [Animations](gltfTutorial_007_Animations.md) | [Table of Contents](README.md) | 다음: [Meshes](gltfTutorial_009_Meshes.md)

# Simple Meshes - 간단한 메쉬

A [`mesh`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh) represents a geometric object that appears in a scene. An example of a `mesh` has already been shown in the [minimal glTF file](gltfTutorial_003_MinimalGltfFile.md). This example had a single `mesh` attached to a single `node`, and the mesh consisted of a single [`mesh.primitive`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh-primitive) that contained only a single attribute&mdash;namely, the attribute for the vertex positions. But usually, the mesh primitives will contain more attributes. These attributes may, for example, be the vertex normals or texture coordinates.    
[`mesh`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh)는 장면에 표시될 기하 물체를 나타낸다.  `mesh`의 간단한 예는 이미 [minimal glTF file](gltfTutorial_003_MinimalGltfFile.md)에서 살펴보았다. 이 예제에는 하나의 `mesh`가 하나의 `node`에 첨부되었고, 메쉬는 단 하나의 [`mesh.primitive`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh-primitive)로 구성되어 있어 단 하나의 속성 - 버텍스 좌표만을 갖고 있다. 하지만 일반적으로는, 메쉬 프리미티브는 좀 더 많은 속성을 갖는다. 이들 속성에는 예를 들면, 버텍스의 법선 벡터나 텍스처 좌표 등이 포함될 수 있다.

The following is a glTF asset that contains a simple mesh with multiple attributes, which will serve as the basis for explaining the related concepts:    
다음 glTF 자산은 여러개의 속성을 갖는 간단한 메쉬를 보여준다. 이 예제를 사용하여 관련된 개념을 설명하는데 사용할 것이다. 

```javascript
{
  "scene": 0,
  "scenes" : [
    {
      "nodes" : [ 0, 1]
    }
  ],
  "nodes" : [
    {
      "mesh" : 0
    },
    {
      "mesh" : 0,
      "translation" : [ 1.0, 0.0, 0.0 ]
    }
  ],
  
  "meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1,
          "NORMAL" : 2
        },
        "indices" : 0
      } ]
    }
  ],

  "buffers" : [
    {
      "uri" : "data:application/octet-stream;base64,AAABAAIAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAAAAAAAAAgD8AAAAAAAAAAAAAgD8AAAAAAAAAAAAAgD8=",
      "byteLength" : 80
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
      "byteLength" : 72,
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
    },
    {
      "bufferView" : 1,
      "byteOffset" : 36,
      "componentType" : 5126,
      "count" : 3,
      "type" : "VEC3",
      "max" : [ 0.0, 0.0, 1.0 ],
      "min" : [ 0.0, 0.0, 1.0 ]
    }
  ],
  
  "asset" : {
    "version" : "2.0"
  }
}
```

Image 8a shows the rendered glTF asset.    
그림 8a는 glTF 자산의 렌더링 결과를 보여 준다.

<p align="center">
<img src="images/simpleMeshes.png" /><br>
<a name="simpleMeshes-png"></a>Image 8a: A simple mesh, attached to two nodes.
</p>


## The mesh definition - 메쉬 정의

The given example still contains a single mesh that has a single mesh primitive. But this mesh primitive contains multiple attributes:     
이 예제는 아직 메쉬는 하나의 메쉬 프리미티브를 갖는 하나의 메쉬만을 갖고 있다. 하지만, 이 메쉬 프리미티브가 여러개의 버텍스 속성을 갖고 있다.

```javascript
  "meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1,
          "NORMAL" : 2
        },
        "indices" : 0
      } ]
    }
  ],
```

In addition to the `"POSITION"` attribute, it has a `"NORMAL"` attribute. This refers to the `accessor` object that provides the vertex normals, as described in the [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md) section.    
`"POSITION"` 버텍스 속성에 새로운 속성인 `"NORMAL"`이 추가되었다. 이는 `accessor` 객체를 참조하며 버텍스의 법선 벡터 정보를 갖고 있다. 이에 대한 내용은 [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md)를 참조하기 바란다. 


## The rendered mesh instances - 렌더링된 메쉬 인스턴스 

As can be seen in Image 8a, the mesh is rendered *twice*. This is accomplished by attaching the mesh to two different nodes:   
그림 8a 에서 보는 바와 같이, 메쉬가 *두번* 렌더링 되었다. 이는 두개의 노드를 만들어 메쉬를 양쪽에 첨부하여 만들 수 있다.

```javascript
  "nodes" : [
    {
      "mesh" : 0
    },
    {
      "mesh" : 0,
      "translation" : [ 1.0, 0.0, 0.0 ]
    }
  ],
```

The `mesh` property of each node refers to the mesh that is attached to the node, using the index of the mesh. One of the nodes has a `translation` that causes the attached mesh to be rendered at a different position.    
각 노드의 `mesh` 속성은 메쉬를 참조하여, 노드에 첨부하게 된다. 이때 메쉬의 인덱스를 사용한다. 두 노드 중 하나의 노드는 `translation` 속성을 갖고 있어 렌더링 결과를 보면 다른 위치에 표시된 볼 수 있다.  

The [next section](gltfTutorial_009_Meshes.md) will explain meshes and mesh primitives in more detail.   
[다음 절](gltfTutorial_009_Meshes.md)에서는 메쉬와 메쉬 프리미티브에 대해 상세하게 설명한다. 



이전: [Animations](gltfTutorial_007_Animations.md) | [Table of Contents](README.md) | 다음: [Meshes](gltfTutorial_009_Meshes.md)
