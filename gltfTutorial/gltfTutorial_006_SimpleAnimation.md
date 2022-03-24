이전: [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md) | [Table of Contents](README.md) | 다음: [Animations](gltfTutorial_007_Animations.md)


# A Simple Animation - 간단한 애니메이션

As shown in the [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md) section, each node can have a local transform. This transform can be given either by the `matrix` property of the node or by using the `translation`, `rotation`, and `scale` (TRS) properties.

[Scenes and Nodes](gltfTutorial_004_ScenesNodes.md)절에서 살펴본 바와 같이, 각 노드는 로컬 변환을 가질 수 있다. 이 변환은 `matrix` 속성이나 `translation`, `rotation`, `scale` (TRS) 속성을 통헤 주어질 수 있다.

When the transform is given by the TRS properties, an [`animation`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-animation) can be used to describe how the `translation`, `rotation`, or `scale` of a node changes over time.

TRS 속성으로 변환이 주어진 경우, [`animation`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-animation)을 노드의 `translation`, `rotation`, `scale`을 통해서 기술할 수 있다. 

The following is the [minimal glTF file](gltfTutorial_003_MinimalGltfFile.md) that was shown previously, but extended with an animation. This section will explain the changes and extensions that have been made to add this animation.

다음은 이전에 살펴보았던 [minimal glTF file](gltfTutorial_003_MinimalGltfFile.md)에 애니메이션을 확정한 예제이다. 이 절에서는 이 애니메이션을 추가하기 위해 변경해야 하는 부분과 확장해야 하는 부분을 설명할 것이다.  


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
      "mesh" : 0,
      "rotation" : [ 0.0, 0.0, 0.0, 1.0 ]
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
  
  "animations": [
    {
      "samplers" : [
        {
          "input" : 2,
          "interpolation" : "LINEAR",
          "output" : 3
        }
      ],
      "channels" : [ {
        "sampler" : 0,
        "target" : {
          "node" : 0,
          "path" : "rotation"
        }
      } ]
    }
  ],

  "buffers" : [
    {
      "uri" : "data:application/octet-stream;base64,AAABAAIAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAACAPwAAAAA=",
      "byteLength" : 44
    },
    {
      "uri" : "data:application/octet-stream;base64,AAAAAAAAgD4AAAA/AABAPwAAgD8AAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAD0/TQ/9P00PwAAAAAAAAAAAACAPwAAAAAAAAAAAAAAAPT9ND/0/TS/AAAAAAAAAAAAAAAAAACAPw==",
      "byteLength" : 100
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
    },
    {
      "buffer" : 1,
      "byteOffset" : 0,
      "byteLength" : 100
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
      "bufferView" : 2,
      "byteOffset" : 0,
      "componentType" : 5126,
      "count" : 5,
      "type" : "SCALAR",
      "max" : [ 1.0 ],
      "min" : [ 0.0 ]
    },
    {
      "bufferView" : 2,
      "byteOffset" : 20,
      "componentType" : 5126,
      "count" : 5,
      "type" : "VEC4",
      "max" : [ 0.0, 0.0, 1.0, 1.0 ],
      "min" : [ 0.0, 0.0, 0.0, -0.707 ]
    }
  ],
  
  "asset" : {
    "version" : "2.0"
  }
  
}
```

<p align="center">
<img src="images/animatedTriangle.gif" /><br>
<a name="animatedTriangle-gif"></a>Image 6a: A single, animated triangle. - 그림 6a: 애니메이션이 포함된 단일 삼각형
</p>


## The `rotation` property of the `node` - `node`의 `rotation` 속성

The only node in the example now has a `rotation` property. This is an array containing the four floating point values of the [quaternion](https://en.wikipedia.org/wiki/Quaternion) that describes the rotation:  

이 예제에 있는 유일한 노드에는 `rotation` 속성을 갖고 있다. 이는 4개의 부동소수점 값을 갖고 있는 [quaternion](https://en.wikipedia.org/wiki/Quaternion)으로 회전을 기술하고 있다. 

```javascript
  "nodes" : [
    {
      "mesh" : 0,
      "rotation" : [ 0.0, 0.0, 0.0, 1.0 ]
    }
  ],
```

The given value is the quaternion describing a "rotation about 0 degrees," so the triangle will be shown in its initial orientation.

주어진 쿼터니언은 "0도 회전"을 정의한 것으로서, 삼각형은 초기 방향에서 그대로 보여지게 된다.

## The animation data - 애니메이션 데이터

Three elements have been added to the top-level arrays of the glTF JSON to encode the animation data:

애니메이션 데이터를 인코딩하기 위해서는 glTF JSON의 최상위 배열에 다음 3개의 요소를 추가해야 한다.

- A new `buffer` containing the raw animation data;
- 원시 애니메이션 데이터를 포함하는 새로운 `buffer` 
- A new `bufferView` that refers to the buffer;
- 이 버퍼를 참조하는 새로운 `bufferView`
- Two new `accessor` objects that add structural information to the animation data.
- 두개의 새로운 `accessor` 객체로 애니메이션 데이터에 대한 구조 정보를 포함

### The `buffer` and the `bufferView` for the raw animation data - 애니메이션 데이터의 `buffer`와 `bufferView` 데이터

A new `buffer` has been added, which contains the raw animation data. This buffer also uses a [data URI](gltfTutorial_002_BasicGltfStructure.md#binary-data-in-data-uris) to encode the 100 bytes that the animation data consists of:

새로운 `buffer`를 추가하고, 여기에는 원시 애니메이션 데이터가 포함되어 있다. 이 버퍼는 [data URI](gltfTutorial_002_BasicGltfStructure.md#binary-data-in-data-uris)로 100바이트의 애니메이션 데이터를 포함하고 있다. 

```javascript
  "buffers" : [
    ...
    {
      "uri" : "data:application/octet-stream;base64,AAAAAAAAgD4AAAA/AABAPwAAgD8AAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAD0/TQ/9P00PwAAAAAAAAAAAACAPwAAAAAAAAAAAAAAAPT9ND/0/TS/AAAAAAAAAAAAAAAAAACAPw==",
      "byteLength" : 100
    }
  ],

  "bufferViews" : [
    ...
    {
      "buffer" : 1,
      "byteOffset" : 0,
      "byteLength" : 100
    }
  ],
```

There is also a new `bufferView`, which here simply refers to the new `buffer` with index 1, which contains the whole animation buffer data. Further structural information is added with the `accessor` objects described below.

또한 새로운 `bufferView`에는 단순하게 새 `buffer`를 인덱스 1로 참조하고 있다. 즉, 버퍼 데이터 전체를 사용하고 있다. 구조적인 정보는 바로 다음에 설명될 `accessor` 객체로 추가 된다. 

Note that one could also have appended the animation data to the existing buffer that already contained the geometry data of the triangle. In this case, the new buffer view would have referred to the `buffer` with index 0, and used an appropriate `byteOffset` to refer to the part of the buffer that then contained the animation data.

주의할 점은, 애니메이션 데이터를 기존의 삼각형 기하 형상 데이터가 들어 있는 버퍼에 추가해서 넣을 수 있다는 것이다. 이때에는, 새로운 버퍼 뷰는 `buffer` 속성으로 인덱스 0으로 참조하여야 하고, 적절한 `byteOffset` 값을 지정하여 애니메이션 데이터가 들어 있는 부분을 참조할 수 있도록 해 주어야 한다.

In the example that is shown here, the animation data is added as a new buffer to keep the geometry data and the animation data separated.

이 예제에서는 보는 바와 같이 애니메이션 데이터가 새로운 버퍼로 추가되었고, 기하 형상 데이터와 애니메이션 데이터를 분리한 상태로 유지 될 것이다.

### The `accessor` objects for the animation data - 애니메이션 데이터를 위한 `accessor` 객체 

Two new `accessor` objects have been added, which describe how to interpret the animation data. The first accessor describes the *times* of the animation key frames. There are five elements (as indicated by the `count` of 5), and each one is a scalar `float` value (which is 20 bytes in total). The second accessor says that after the first 20 bytes, there are five elements, each being a 4D vector with `float` components. These are the *rotations* that correspond to the five key frames of the animation, given as quaternions.

새로운 `accessor` 객체를 추가하여, 애니메이션을 어떻게 해석할지를 지정하여야 한다. 첫번째 억세서는 애니메이션 키프레임들의 *시간 (times)* 을 지정한다. 여기 에는 5개의 요소가 있다. (`count`가 5로 지정) 각 요소는 `float` 값으로 된 스칼라 값이다. (따라서 총 20 바이트) 두번째 억세서는 20바이트가 지난 후에 있는 데이터로 역시 5개의 요소가 있다. `float` 값으로 되어 있는 4D 벡터 값이다. 쿼터니언으로 주어진 다섯개의 *회전 (rotations)* 키프레임 값이 들어 있다.  

```javascript
  "accessors" : [
    ...
    {
      "bufferView" : 2,
      "byteOffset" : 0,
      "componentType" : 5126,
      "count" : 5,
      "type" : "SCALAR",
      "max" : [ 1.0 ],
      "min" : [ 0.0 ]
    },
    {
      "bufferView" : 2,
      "byteOffset" : 20,
      "componentType" : 5126,
      "count" : 5,
      "type" : "VEC4",
      "max" : [ 0.0, 0.0, 1.0, 1.0 ],
      "min" : [ 0.0, 0.0, 0.0, -0.707 ]
    }
  ],

```

The actual data that is provided by the *times* accessor and the *rotations* accessor, using the data from the buffer in the example, is shown in this table:

위 예제에서 버퍼에 들어 있는 *시간 (times)* 과 *회전 (rotation)* 억세서에 해당하는 값은 아래 표와 같다.

|*times* 억세서 |*rotations* 억세서|의미|
|---|---|---|
|0.0| (0.0, 0.0, 0.0, 1.0 )| 0.0 초에, triangle 0도의 회전 값을 가짐 |
|0.25| (0.0, 0.0, 0.707, 0.707)| 0.25 초에, z-축에 대해 90도 회전
|0.5| (0.0, 0.0, 1.0, 0.0)|  0.5 초에, z-축에 대해 180 회전 |
|0.75| (0.0, 0.0, 0.707, -0.707)| 0.75 초에, z-축에 대해 270도 회전 |
|1.0| (0.0, 0.0, 0.0, 1.0)| 1.0 초에, z-축에 대해 0(=360)도 회전 |

So this animation describes a rotation of 360 degrees around the z-axis that lasts 1 second.

따라서 이 애니메이션은 z-축으로 1초에 360도 회전하는 애니메이션을 기술하고 있다.


## The `animation` 

Finally, this is the part where the actual animation is added. The top-level `animations` array contains a single `animation` object. It consists of two elements:

최종적으로, 애니메이션을 추가하는 부분이다. 최상위 `animations` 배열은 하나의 `animation` 객체를 갖는다. 여기에는 두개의 요소가 들어 있다.  

- The `samplers`, which describe the sources of animation data;
- `samplers` 요소는 애니메이션 데이터의 소스를 정의한다.
- The `channels`, which can be imagined as connecting a "source" of the animation data to a "target."
- `channels` 요소는 애니메이션 데이터의 "source"를 "target"에 연결하는데 사용된다.

In the given example, there is one sampler. Each sampler defines an `input` and an `output` property. They both refer to accessor objects. Here, these are the *times* accessor (with index 2) and the *rotations* accessor (with index 3) that have been described above. Additionally, the sampler defines an `interpolation` type, which is `"LINEAR"` in this example.

주어진 예제에서는, 하나의 샘플러 만 존재한다. 각 샘플러는 `input`과 `output` 속성으로 정의된다. 이 둘 모두 억세서 객체를 참조한다. 여기에는 앞서 설명한 *시간* 억세서 (인덱스 2) 와 *회전* 억세서 (인덱스 3) 이 있다. 추가로 `interpolation` 유형을 정의 할 수 있다. 이 예제에서는 `"LINEAR"`를 사용하였다. 

There is also one `channel` in the example. This channel refers to the only sampler (with index 0) as the source of the animation data. The target of the animation is encoded in the `channel.target` object: it contains an `id` that refers to the node whose property should be animated. The actual node property is named in the `path`. So the channel target in the given example says that the `"rotation"` property of the node with index 0 should be animated.

이 예제에는 하나의 `channel`이 있다. 이 채널은 단 하나의 샘플러(인덱스 0)을 애니메이션 데이터의 소스로서 참조한다. 애니메이션의 타겟은 `channel.target` 객체 내에 인코딩된다. 여기에는 `id` 라는 속성으로 애니메이션이 될 노드를 참조한다. 실제 노드의 속성은 `path`라고 이름이 만들어 졌다. 따라서, 예제에서의 채널 타겟은 인덱스 0번 노드의 `"rotation"`속성이 애니메이션 되어야 함을 말하고 있다.


```javascript
  "animations": [
    {
      "samplers" : [
        {
          "input" : 2,
          "interpolation" : "LINEAR",
          "output" : 3
        }
      ],
      "channels" : [ {
        "sampler" : 0,
        "target" : {
          "node" : 0,
          "path" : "rotation"
        }
      } ]
    }
  ],
```

Combining all this information, the given animation object says the following:

이러한 정보를 모두 결합하여, 주어진 애니메이션 객체는 다음과 같이 설명할 수 있다.

> During the animation, the animated values are obtained from the *rotations* accessor. They are interpolated linearly, based on the current simulation time and the key frame times that are provided by the *times* accessor. The interpolated values are then written into the `"rotation"` property of the node with index 0.  
애니메이션 중에, 애니메이션 값은 *회전* 억세서로 부터 얻어진다. 이들은 현재 시뮬레이션 시간과 키프레인으로 정의된 *시간* 억세서를 이용하여 선형으로 보간된다. 보간된 값은 인덱스 0 노드의 `"rotation"` 속성에 들어가게 된다.

A more detailed description and actual examples for the interpolation and the computations that are involved here can be found in the [Animations](gltfTutorial_007_Animations.md) section.

좀더 상세한 정보와 실제 보간과 계산에 관련된 정보는 다음 절 [Animations](gltfTutorial_007_Animations.md)에서 배운다.



이전: [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md) | [Table of Contents](README.md) | 다음: [Animations](gltfTutorial_007_Animations.md)
