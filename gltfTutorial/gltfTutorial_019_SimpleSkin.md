Previous: [Morph Targets](gltfTutorial_018_MorphTargets.md) | [Table of Contents](README.md) | Next: [Skins](gltfTutorial_020_Skins.md)

# A Simple Skin - 간단한 스키닝

glTF supports *vertex skinning*, which allows the geometry (vertices) of a mesh to be deformed based on the pose of a skeleton. This is essential in order to give animated geometry, for example of virtual characters, a realistic appearance. The core for the definition of vertex skinning in a glTF asset is the [`skin`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-skin), but vertex skinning in general implies several interdependencies between the elements of a glTF asset that have been presented so far.

glTF는 *버텍스 스키닝* 을 지원한다. 이는 골격(skeleton)의 자세에 따라서 메쉬의 기하형상(버텍스)를 변형하는 것이다. 이는 형상 애니메션을 줄 수 있도록 하는 가장 핵심적인 부분으로, 예를 들면, 가상 캐랙터의 실감나는 외형을 만들어 준다. glTF 자산에서 버텍스 스키닝의 정의를 위한 핵심 부분은 [`skin`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-skin)이지만, 일반적으로 버텍스 스키닝은 지금까지 소개한 여러가지 glTF 자산 요소들간에 상호 의존성을 통해 만들어진다. 

The following is a glTF asset that shows basic vertex skinning for a simple geometry. The elements of this asset will be summarized quickly in this section, referring to the previous sections where appropriate, and pointing out the new elements that have been added for the vertex skinning functionality. The details and background information for vertex skinning will be given in the next section.

다음 glTF 자산은 간단한 기하형상의 버텍스 스키닝을 보여준다. 이 자산의 요소들에 대해 필요한 경우 이전 섹션을 참조하고 버텍스 스키닝 기능에 추가된 새 요소를 설명하는 방식으로 이 절에서 간단하게 요약 설명하고, 버텍스 스키닝에 대한 자세한 내용과 배경 정보는 다음 절에서 설명한다.

```javascript
{
  "scene" : 0,
  "scenes" : [ {
    "nodes" : [ 0, 1 ]
  } ],
  
  "nodes" : [ {
    "skin" : 0,
    "mesh" : 0
  }, {
    "children" : [ 2 ]
  }, {
    "translation" : [ 0.0, 1.0, 0.0 ],
    "rotation" : [ 0.0, 0.0, 0.0, 1.0 ]
  } ],
  
  "meshes" : [ {
    "primitives" : [ {
      "attributes" : {
        "POSITION" : 1,
        "JOINTS_0" : 2,
        "WEIGHTS_0" : 3
      },
      "indices" : 0
    } ]
  } ],

  "skins" : [ {
    "inverseBindMatrices" : 4,
    "joints" : [ 1, 2 ]
  } ],
  
  "animations" : [ {
    "channels" : [ {
      "sampler" : 0,
      "target" : {
        "node" : 2,
        "path" : "rotation"
      }
    } ],
    "samplers" : [ {
      "input" : 5,
      "interpolation" : "LINEAR",
      "output" : 6
    } ]
  } ],
  
  "buffers" : [ {
    "uri" : "data:application/gltf-buffer;base64,AAABAAMAAAADAAIAAgADAAUAAgAFAAQABAAFAAcABAAHAAYABgAHAAkABgAJAAgAAAAAvwAAAAAAAAAAAAAAPwAAAAAAAAAAAAAAvwAAAD8AAAAAAAAAPwAAAD8AAAAAAAAAvwAAgD8AAAAAAAAAPwAAgD8AAAAAAAAAvwAAwD8AAAAAAAAAPwAAwD8AAAAAAAAAvwAAAEAAAAAAAAAAPwAAAEAAAAAA",
    "byteLength" : 168
  }, {
    "uri" : "data:application/gltf-buffer;base64,AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAAAAAAAAAAAAQAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAAAAAAAAAAAAQAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAAAAAAAAAAAgD8AAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAABAPwAAgD4AAAAAAAAAAAAAQD8AAIA+AAAAAAAAAAAAAAA/AAAAPwAAAAAAAAAAAAAAPwAAAD8AAAAAAAAAAAAAgD4AAEA/AAAAAAAAAAAAAIA+AABAPwAAAAAAAAAAAAAAAAAAgD8AAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAA=",
    "byteLength" : 320
  }, {
    "uri" : "data:application/gltf-buffer;base64,AACAPwAAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAAAAAAAAgD8AAAAAAAAAAAAAAAAAAAAAAACAPwAAgD8AAAAAAAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAIC/AAAAAAAAgD8=",
    "byteLength" : 128
  }, {
    "uri" : "data:application/gltf-buffer;base64,AAAAAAAAAD8AAIA/AADAPwAAAEAAACBAAABAQAAAYEAAAIBAAACQQAAAoEAAALBAAAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAkxjEPkSLbD8AAAAAAAAAAPT9ND/0/TQ/AAAAAAAAAAD0/TQ/9P00PwAAAAAAAAAAkxjEPkSLbD8AAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAkxjEvkSLbD8AAAAAAAAAAPT9NL/0/TQ/AAAAAAAAAAD0/TS/9P00PwAAAAAAAAAAkxjEvkSLbD8AAAAAAAAAAAAAAAAAAIA/",
    "byteLength" : 240
  } ],
  
  "bufferViews" : [ {
    "buffer" : 0,
    "byteLength" : 48,
    "target" : 34963
  }, {
    "buffer" : 0,
    "byteOffset" : 48,
    "byteLength" : 120,
    "target" : 34962
  }, {
    "buffer" : 1,
    "byteLength" : 320,
    "byteStride" : 16
  }, {
    "buffer" : 2,
    "byteLength" : 128
  }, {
    "buffer" : 3,
    "byteLength" : 240
  } ],

  "accessors" : [ {
    "bufferView" : 0,
    "componentType" : 5123,
    "count" : 24,
    "type" : "SCALAR"
  }, {
    "bufferView" : 1,
    "componentType" : 5126,
    "count" : 10,
    "type" : "VEC3",
    "max" : [ 0.5, 2.0, 0.0 ],
    "min" : [ -0.5, 0.0, 0.0 ]
  }, {
    "bufferView" : 2,
    "componentType" : 5123,
    "count" : 10,
    "type" : "VEC4"
  }, {
    "bufferView" : 2,
    "byteOffset" : 160,
    "componentType" : 5126,
    "count" : 10,
    "type" : "VEC4"
  }, {
    "bufferView" : 3,
    "componentType" : 5126,
    "count" : 2,
    "type" : "MAT4"
  }, {
    "bufferView" : 4,
    "componentType" : 5126,
    "count" : 12,
    "type" : "SCALAR",
    "max" : [ 5.5 ],
    "min" : [ 0.0 ]
  }, {
    "bufferView" : 4,
    "byteOffset" : 48,
    "componentType" : 5126,
    "count" : 12,
    "type" : "VEC4",
    "max" : [ 0.0, 0.0, 0.707, 1.0 ],
    "min" : [ 0.0, 0.0, -0.707, 0.707 ]
  } ],
 
  "asset" : {
    "version" : "2.0"
  }
}
```



The result of rendering this asset is shown in Image 19a.   
렌더링의 결과는 그림 19a와 같다. 


<p align="center">
<img src="images/simpleSkin.gif" /><br>
<a name="simpleSkin-gif"></a>Image 19a: A scene with simple vertex skinning. - 그림 19a: 간단한 버텍스 스키닝을 갖는 장면
</p>


## Elements of the simple skin example - 간단한 스키닝 예제의 요소

The elements of the given example are briefly summarized here:  
주어진 예제의 요소들에 대해 간단히 요약하면 다음과 같다. 

- The `scenes` and `nodes` elements have been explained in the [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md) section. For the vertex skinning, new nodes have been added: the nodes at index 1 and 2 define a new node hierarchy for the *skeleton*. These nodes can be considered the joints between the "bones" that will eventually cause the deformation of the mesh.
- `scene`과 `nodes` 요소는 [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md)절에서 설명되었다. 버텍스 스키닝을 하기 위해 새로운 노드들이 추가되었다. 인덱스 1과 2로 된 노드들에서는 *골격(skeleton)* 에 대한 계층을 정의한다. 이들 노드들은 "뼈" 사이를 연결하는 관절과 같은 역할을 하며, 최종적으로 메쉬의 변형을 일으키게 된다. 
- The new top-level dictionary `skins` contains a single skin in the given example. The properties of this skin object will be explained later.
- 예제에서는 새로운 최상위 딕셔너리 `skins`에는 하나의 스킨을 갖고 있다. 스킨 객체의 속성에 대해서는 다음에 설명한다. 
- The concepts of `animations` has been explained in the [Animations](gltfTutorial_007_Animations.md) section. In the given example, the animation refers to the *skeleton* nodes so that the effect of the vertex skinning is actually visible during the animation.
- `animation`의 개념은 [Animations](gltfTutorial_007_Animations.md)절에서 이미 설명하였다. 이 예제에서는, 애니메이션은 *skeleton* 노드들을 참조하여 실제 애니메이션 중에 버텍스 스키닝 효과를 보여줄 수 있도록 해 주었다.
- The [Meshes](gltfTutorial_009_Meshes.md) section already explained the contents of the `meshes` and `mesh.primitive` objects. In this example, new mesh primitive attributes have been added, which are required for vertex skinning, namely the `"JOINTS_0"` and `"WEIGHTS_0"` attributes.
- `meshes` 와 `mesh.primitive`에 대해서는 [Meshes](gltfTutorial_009_Meshes.md)절에서 설명하였다. 이 예제에서는 새로운 메쉬 속성이 추가되었는데, 버텍스 스키닝을 위해 요구되는 속성으로 `"JOINTS_0"` 와 `"WEIGHTS_0"`이다. 
- There are several new `buffers`, `bufferViews`, and `accessors`. Their basic properties have been described in the [Buffers, BufferViews, and Accessors](gltfTutorial_005_BufferBufferViewsAccessors.md) section. In the given example, they contain the additional data required for vertex skinning.
- 새로운 `buffers`, `bufferViews`, `accessors`가 추가되었다. 이들에 대한 기초적인 속성은  [Buffers, BufferViews, and Accessors](gltfTutorial_005_BufferBufferViewsAccessors.md)절에서 설명하였다. 이 예제에서는, 버텍스 스키닝을 위한 데이터가 추가 되었다.

Details about how these elements are interconnected to achieve the vertex skinning will be explained in the [Skins](gltfTutorial_020_Skins.md) section.
이들 요소들이 버텍스 스키닝을 위해 어떻게 서로 연결되는지에 대해서는 다음 절 [Skins](gltfTutorial_020_Skins.md)에서 설명할 것이다.


이전: [Morph Targets](gltfTutorial_018_MorphTargets.md) | [Table of Contents](README.md) | 다음: [Skins](gltfTutorial_020_Skins.md)
