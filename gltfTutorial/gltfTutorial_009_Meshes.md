이전: [Simple Meshes](gltfTutorial_008_SimpleMeshes.md) | [Table of Contents](README.md) | 다음: [Materials](gltfTutorial_010_Materials.md)

# Meshes - 메쉬

The [Simple Meshes](gltfTutorial_008_SimpleMeshes.md) example from the previous section showed a basic example of a [`mesh`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh) with a [`mesh.primitive`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh-primitive) object that contained several attributes. This section will explain the meaning and usage of mesh primitives, how meshes may be attached to nodes of the scene graph, and how they can be rendered with different materials.

이전 절 [Simple Meshes](gltfTutorial_008_SimpleMeshes.md)에서 여러 속성을 갖는 [`mesh.primitive`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh-primitive)객체를 이용해 간단한  [`mesh`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh)예제를 살펴 보았다. 이 절에서는 메쉬 프리미티브의 의미와 사용법에 대해서 설명하고, 어떻게 메쉬를 장면 그래프의 노드에 첨부하는지를 설명하고 여러 다른 재질(materials)를 이요하여 렌더링하는 방법을 설명한다. 

## Mesh primitives - 메쉬 프리미티브

Each `mesh` contains an array of `mesh.primitive` objects. These mesh primitive objects are smaller parts or building blocks of a larger object. A mesh primitive summarizes all information about how the respective part of the object will be rendered.

각각의 `mesh`는 `mesh.primitive` 객체의 배열을 갖고 있다. 이들 메쉬 프리미티브 객체들은 작은 부품들로, 혹은 큰 물체의 빌딩 블록이라고 생각할 수 있다. 메쉬 프리미티브는 어떻게 물체를 이루는 각의 부분들이 어떻게 렌더링하는지를 지정하는 모든 정보를 갖고 있다.  


### Mesh primitive attributes - 메쉬 프리미티브 속성

A mesh primitive defines the geometry data of the object using its `attributes` dictionary. This geometry data is given by references to `accessor` objects that contain the data of vertex attributes. The details of the `accessor` concept are explained in the [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md) section.

메쉬 프리미티브는 `attributes` 딕셔너리를 사용하여 물체의 기하 데이터를 정의한다. 이 기하 데이터는 `accessor` 객체를 참조하는 방법으로 주어진다. 여기에는 버텍스 속성이 포함되어 있다. `accessor` 개념에 대한 상세한 설명은 [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md) 절에 있다. 

In the given example, there are two entries in the `attributes` dictionary. The entries refer to the `positionsAccessor` and the `normalsAccessor`:    
다음 예제에서, `attributes` 딕셔너리에 두개 항목이 있다. 이 항목은 `positionsAccessor` 과 `normalsAccessor`를 참조한다. 

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

Together, the elements of these accessors define the attributes that belong to the individual vertices, as shown in Image 9a.   
이와함께, 이들 억세서에 대한 요소들은 각각의 버텍스에 속한 속성을 정의 한다. 그림 9a 참조.


<p align="center">
<img src="images/meshPrimitiveAttributes.png" /><br>
<a name="meshPrimitiveAttributes-png"></a>Image 9a: Mesh primitive accessors containing the data of vertices. - 그림 9a: 버텍스 데이터를 갖고 있는 메쉬 프리미티브 억세서 
</p>


### Indexed and non-indexed geometry - 인덱싱과 비인덱싱 기하

The geometry data of a `mesh.primitive` may be either *indexed* geometry or geometry without indices. In the given example, the `mesh.primitive` contains *indexed* geometry. This is indicated by the `indices` property, which refers to the accessor with index 0, defining the data for the indices. For non-indexed geometry, this property is omitted.

`mesh.primitive`의 기하 데이터는 *인덱싱(indexed)* 기하 또는 인덱싱이 없는 기하로 정의될 수 있다. 주어진 예제에서는, `mesh.primitive`는 *인덱싱* 기하를 갖고 있다. 이는 `indices` 속성에 의해서 지정되는데, 인덱스 0으로 억세서를 참조하여, 인덱스를 위한 데이터를 정의한다. 비인덱싱 기하여에서는 이 속성은 생략된다. 


### Mesh primitive mode - 메쉬 프리미티브 모드

By default, the geometry data is assumed to describe a triangle mesh. For the case of *indexed* geometry, this means that three consecutive elements of the `indices` accessor are assumed to contain the indices of a single triangle. For non-indexed geometry, three elements of the vertex attribute accessors are assumed to contain the attributes of the three vertices of a triangle.

기본 값으로, 기하 데이터는 삼각형 메쉬를 기술하고 있다고 가정한다. *인덱싱* 기하의 경우, 이는 3개의 연속된 `indices`억세서의 요소가 하나의 삼각형을 이룬다고 가정한다. 비인덱싱 기하의 경우, 3개의 버텍스 속성 억세서의 3개 요소가 삼각형를 구성하는 3개의 버텍스의 속성을 갖고 있다고 가정한다. 

Other rendering modes are possible: the geometry data may also describe individual points, lines, or triangle strips. This is indicated by the `mode` that may be stored in the mesh primitive. Its value is a constant that indicates how the geometry data has to be interpreted. The mode may, for example, be `0` when the geometry consists of points, or `4` when it consists of triangles. These constants correspond to the GL constants `POINTS` or `TRIANGLES`, respectively. See the [`primitive.mode` specification](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#_mesh_primitive_mode) for a list of available modes.

이 외에도 다른 모드들을 사용할 수 있다. 기하 데이터가 각각 점들(points), 직선들(lines), 또는 이어진 삼각형을 만들 수 있다. `mode`를 이용하여 지정하고 메쉬 프리미티브에 저장된다. 이 모드 값은 상수로 어떻게 기하데이터를 해석할지를 결정한다. 모드 값은, 예를 들면, `0`은 점 기하이고, `4`는 삼각형들을 의미한다. 이는 GL에서 사용되는 상수인 `POINTS` 또는 `TRIANGLES`과 상응된다. 사용할 수 있는 모드는 [`primitive.mode` specification](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#_mesh_primitive_mode)를 참조하기 바란다.

### Mesh primitive material - 메쉬 프리미티브 재질

The mesh primitive may also refer to the `material` that should be used for rendering, using the index of this material. In the given example, no `material` is defined, causing the objects to be rendered with a default material that just defines the objects to have a uniform 50% gray color. A detailed explanation of materials and the related concepts will be given in the [Materials](gltfTutorial_010_Materials.md) section.

메쉬 프리미티브는 렌더링에 사용될 `material`을 인덱스를 사용해 참조할 수 있다. 이 예에서는, `material`이 정의되지 않았기 때문에, 물체는 디폴트 재질인 균일한 50% 회색으로 렌더링 된다. 재질에 대한 상세한 설명과 관련된 개념에 대한 설명은 [Materials](gltfTutorial_010_Materials.md) 절에 있다.

## Meshes attached to nodes - 노드의 첨부된 메쉬 

In the example from the [Simple Meshes](gltfTutorial_008_SimpleMeshes.md) section, there is a single `scene`, which contains two nodes, and both nodes refer to the same `mesh` instance, which has the index 0:

[Simple Meshes](gltfTutorial_008_SimpleMeshes.md)절의 예제에서는, `scene`이 하나 있으며, 여기에 두개의 노드들을 갖고 있고, 두개의 노드는 인덱스 0인 같은 `mesh` 인스턴스를 참조한다. 

```javascript
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
    { ... } 
  ],
```

The second node has a `translation` property. As shown in the [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md) section, this will be used to compute the local transform matrix of this node. In this case, the matrix will cause a translation of 1.0 along the x-axis. The product of all local transforms of the nodes will yield the [global transform](gltfTutorial_004_ScenesNodes.md#global-transforms-of-nodes). And all elements that are attached to the nodes will be rendered with this global transform.

두번째 노드는 `translation` 속성을 갖고 있다. [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md)절에서 본 바와 같이, 이것은 이 노드의 로컬 변환 행렬을 계산하는데 사용된다. 이 경우에는 행렬은 x-축에 대해 1.0만큼 이동하는 행렬이다. 노드의 모든 로컬 변환을 곱하면 [global transform](gltfTutorial_004_ScenesNodes.md#global-transforms-of-nodes)을 만든다. 이 노드들에 첨부되어 있는 모든 요소들은 이 글로벌 변환을 적용하여 렌더링 된다. 

So in this example, the mesh will be rendered twice because it is attached to two nodes: once with the global transform of the first node, which is the identity transform, and once with the global transform of the second node, which is a translation of 1.0 along the x-axis.

따라서 이 예제는, 메쉬가 두개의 도구에 첨부되어 있기 때문에 메쉬는 두번 렌더링된다. 첫번째 노드에서는 글로벌 변환(단위 변환)을 적용하여 렌더링 되고, 두번째는 두번째 노드의 글로벌 변환 (x-축을 따라 1.0 이동)이 적용되어 렌더링 된다. 

이전: [Simple Meshes](gltfTutorial_008_SimpleMeshes.md) | [Table of Contents](README.md) | 다음: [Materials](gltfTutorial_010_Materials.md)
