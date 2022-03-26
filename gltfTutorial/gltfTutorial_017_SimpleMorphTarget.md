이전: [Cameras](gltfTutorial_016_Cameras.md) | [Table of Contents](README.md) | 다음: [Morph Targets](gltfTutorial_018_MorphTargets.md)

# A Simple Morph Target - 간단한 모핑 타겟

Starting with version 2.0, glTF supports the definition of *morph targets* for meshes. A morph target stores displacements or differences for certain mesh attributes. At runtime, these differences may be added to the original mesh, with different weights, in order to animate parts of the mesh. This is often used in character animations, for example, to encode different facial expressions of a virtual character.

glTF 2.0 버전 부터, 메쉬에 대한 *모핑 타겟 - morph targets* 을 정의할 수 있다. 모핑 타겟은 어떤 메쉬 속성에 대해 이동이나 차이를 저장하는 것이다. 런타임에, 이들 차이 값은 원래의 메쉬에 서로 다른 가중치 값을 사용해 더해져, 메쉬의 부분의 모양을 바꾸는 애니메이션을 만들 수 있다. 이는 가상 캐릭터의 서로 다른 얼굴 표현등의 캐릭터 애니메이션에 자주 사용된다. 

The following is a minimal example that shows a mesh with two morph targets. The new elements will be summarized here, and the broader concept of morph targets and how they are applied at runtime will be explained in the next section.

다음은 하나의 메쉬가 두개 모핑 타겟을 갖는 최소한의 예제를 보여준다. 새로운 요소들에 대하여 간단히 설명하고, 모핑 타겟에 대한 개념과 런타임에 적용하는 방법에 대해서는 다음 절에서 상세히 다룬다. 

```javascript
{
  "scene": 0,
  "scenes":[
    {
      "nodes":[
        0
      ]
    }
  ],
  "nodes":[
    {
      "mesh":0
    }
  ],
  "meshes":[
    {
      "primitives":[
        {
          "attributes":{
            "POSITION":1
          },
          "targets":[
            {
              "POSITION":2
            },
            {
              "POSITION":3
            }
          ],
          "indices":0
        }
      ],
      "weights":[
        1.0,
        0.5
      ]
    }
  ],

  "animations":[
    {
      "samplers":[
        {
          "input":4,
          "interpolation":"LINEAR",
          "output":5
        }
      ],
      "channels":[
        {
          "sampler":0,
          "target":{
            "node":0,
            "path":"weights"
          }
        }
      ]
    }
  ],

  "buffers":[
    {
      "uri":"data:application/gltf-buffer;base64,AAABAAIAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAA/AAAAPwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIC/AACAPwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIA/AACAPwAAAAA=",
      "byteLength":116
    },
    {
      "uri":"data:application/gltf-buffer;base64,AAAAAAAAgD8AAABAAABAQAAAgEAAAAAAAAAAAAAAAAAAAIA/AACAPwAAgD8AAIA/AAAAAAAAAAAAAAAA",
      "byteLength":60
    }
  ],
  "bufferViews":[
    {
      "buffer":0,
      "byteOffset":0,
      "byteLength":6,
      "target":34963
    },
    {
      "buffer":0,
      "byteOffset":8,
      "byteLength":108,
      "byteStride":12,
      "target":34962
    },
    {
      "buffer":1,
      "byteOffset":0,
      "byteLength":20
    },
    {
      "buffer":1,
      "byteOffset":20,
      "byteLength":40
    }
  ],
  "accessors":[
    {
      "bufferView":0,
      "byteOffset":0,
      "componentType":5123,
      "count":3,
      "type":"SCALAR",
      "max":[
        2
      ],
      "min":[
        0
      ]
    },
    {
      "bufferView":1,
      "byteOffset":0,
      "componentType":5126,
      "count":3,
      "type":"VEC3",
      "max":[
        1.0,
        0.5,
        0.0
      ],
      "min":[
        0.0,
        0.0,
        0.0
      ]
    },
    {
      "bufferView":1,
      "byteOffset":36,
      "componentType":5126,
      "count":3,
      "type":"VEC3",
      "max":[
        0.0,
        1.0,
        0.0
      ],
      "min":[
        -1.0,
        0.0,
        0.0
      ]
    },
    {
      "bufferView":1,
      "byteOffset":72,
      "componentType":5126,
      "count":3,
      "type":"VEC3",
      "max":[
        1.0,
        1.0,
        0.0
      ],
      "min":[
        0.0,
        0.0,
        0.0
      ]
    },
    {
      "bufferView":2,
      "byteOffset":0,
      "componentType":5126,
      "count":5,
      "type":"SCALAR",
      "max":[
        4.0
      ],
      "min":[
        0.0
      ]
    },
    {
      "bufferView":3,
      "byteOffset":0,
      "componentType":5126,
      "count":10,
      "type":"SCALAR",
      "max":[
        1.0
      ],
      "min":[
        0.0
      ]
    }
  ],

  "asset":{
    "version":"2.0"
  }
}

```

The asset contains an animation that interpolates between the different morph targets for a single triangle. A screenshot of this asset is shown in Image 17a.

자산에는 하나의 삼각형에 대해 두개의 서로다른 모핑 타겟을 보간하는 애니메이션이 들어있다. 이 자산에 대한 렌더링 결과는 그림 17a와 같다.  

<p align="center">
<img src="images/simpleMorph.png" /><br>
<a name="simpleMorph-png"></a>Image 17a: A triangle with two morph targets. - 그림 17a: 두개의 모핑 타겟을 갖는 삼각형
</p>


Most of the elements of this asset have already been explained in the previous sections: It contains a `scene` with a single `node` and a single `mesh`. There are two `buffer` objects, one storing the geometry data and one storing the data for the `animation`, and several `bufferView` and `accessor` objects that provide access to this data.

이 자산의 대부분의 요소는 이전 절에서 설명되었다. 자신에는 `scene`에 하나의 `node`가 있고, 여기에 하나의 `mesh`가 있다. 두개의 `buffer` 객체가 있는데 하나에는 기하 데이터를 저장하고 있고, 다른 하나에는 `animation`을 위한 데이터가 저장되어 있다. 여기에 몇개의 `bufferView`와 `accessor` 객체가 있어 데이터에 대한 접근을 지원한다.

The new elements that have been added in order to define the morph targets are contained in the `mesh` and the `animation`:

모핑 타겟들을 정의하기 위해 추가되어야 하는 요소는  `mesh` and the `animation`에 포함된다. 


```javascript
  "meshes":[
    {
      "primitives":[
        {
          "attributes":{
            "POSITION":1
          },
          "targets":[
            {
              "POSITION":2
            },
            {
              "POSITION":3
            }
          ],
          "indices":0
        }
      ],
      "weights":[
        0.5,
        0.5
      ]
    }
  ],

```

The `mesh.primitive` contains an array of [morph `targets`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#_mesh_primitive_targets). Each morph target is a dictionary that maps attribute names to `accessor` objects. In the example, there are two morph targets, both mapping the `"POSITION"` attribute to accessors that contain the morphed vertex positions. The mesh also contains an array of `weights` that defines the contribution of each morph target to the final, rendered mesh. These weights are also the `channel.target` of the `animation` that is contained in the asset:

`mesh.primitive`는 [morph `targets`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#_mesh_primitive_targets)의 배열을 포함하고 있다. 각 모핑 타겟은 딕셔너리로 `accessor` 객체의 버텍스 속성 이름을 매핑한다. 예제에서는 두개의 모핑 타겟이 있는데, 두개 모두 억세서의 `"POSITION"` 속성에 매핑되어 있어, 버텍스의 좌표를 모핑할 수 있게 된다. 메쉬에는 `weights`의 배열이 포함되어 런더링되는 메쉬에 최종적으로 어떤 모핑 타겟이 얼마나 기여할지를 정의한다. 이 가중치는 `animation`의  `channel.target`으로 자산에 포함되어 있다. 

```javascript
  "animations":[
    {
      "samplers":[
        {
          "input":4,
          "interpolation":"LINEAR",
          "output":5
        }
      ],
      "channels":[
        {
          "sampler":0,
          "target":{
            "node":0,
            "path":"weights"
          }
        }
      ]
    }
  ],

```

This means that the animation will modify the `weights` of the mesh that is referred to by the `target.node`. The result of applying the animation to these weights, and the computation of the final, rendered mesh will be explained in more detail in the next section about [Morph Targets](gltfTutorial_018_MorphTargets.md).

이것이 의미하는 것은 애니메이션은 메쉬의 `weights`를 변경하고, `target.node`에 의해 참조 된다. 애니메이션에 이들 가중가 애니메이션에 적용되고, 최종 렌더링 메쉬에 대한 계산에 적용된다. 상세한 내용은 다음 절 [Morph Targets](gltfTutorial_018_MorphTargets.md)에서 설명된다. 



이전: [Cameras](gltfTutorial_016_Cameras.md) | [Table of Contents](README.md) | 다음: [Morph Targets](gltfTutorial_018_MorphTargets.md)
