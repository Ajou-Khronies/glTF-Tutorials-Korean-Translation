Previous: [Advanced Material](gltfTutorial_014_AdvancedMaterial.md) | [Table of Contents](README.md) | Next: [Cameras](gltfTutorial_016_Cameras.md)

# Simple Cameras - 간단한 카메라

The previous sections showed how a basic scene structure with geometric objects is represented in a glTF asset, and how different materials can be applied to these objects. This did not yet include information about the view configuration that should be used for rendering the scene. This view configuration is usually described as a virtual *camera* that is contained in the scene, at a certain position, and pointing in a certain direction.   
이전 절에서는 glTF 자산에 기하 객체를 표현하는 기본 장면 구조를 표현하는지 살펴 보았고, 서로 다른 재질을 기하 객체에 적용하는 방법을 살펴 보았다. 아직까지 장면을 렌더링하는데 필요한 시점에 대한 설정이 되지 않은 상태이다. 이 시점에 대한 설정은 보통 장면 내에 가상의 *카메라*의 위치와 어떤 방향을 보고 있는지 등의 설정을 하게 된다.  

The following is a simple, complete glTF asset. It is similar to the assets that have already been shown: it defines a simple `scene` containing `node` objects and a single geometric object that is given as a `mesh`, attached to one of the nodes. But this asset additionally contains two [`camera`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-camera) objects:   
다음은 간단하지만 완전한 glTF 자산이다. 앞서 살펴본 자산과 비슷하다. 간단한 `scene` 은 `node` 객체를 갖고 있고, 단일 기하 객체를 갖고 있다. 기해 객체는 `mesh`로 주어져 노드들 중 하나에 첨부된다. 반면, 이 어셋은 추가적으로 두개의  [`camera`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-camera) 객체를 갖고 있다.


```javascript
{
  "scene": 0,
  "scenes" : [
    {
      "nodes" : [ 0, 1, 2 ]
    }
  ],
  "nodes" : [
    {
      "rotation" : [ -0.383, 0.0, 0.0, 0.924 ],
      "mesh" : 0
    },
    {
      "translation" : [ 0.5, 0.5, 3.0 ],
      "camera" : 0
    },
    {
      "translation" : [ 0.5, 0.5, 3.0 ],
      "camera" : 1
    }
  ],

  "cameras" : [
    {
      "type": "perspective",
      "perspective": {
        "aspectRatio": 1.0,
        "yfov": 0.7,
        "zfar": 100,
        "znear": 0.01
      }
    },
    {
      "type": "orthographic",
      "orthographic": {
        "xmag": 1.0,
        "ymag": 1.0,
        "zfar": 100,
        "znear": 0.01
      }
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
      "uri" : "data:application/octet-stream;base64,AAABAAIAAQADAAIAAAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAAAAAAAAAgD8AAAAAAACAPwAAgD8AAAAA",
      "byteLength" : 60
    }
  ],
  "bufferViews" : [
    {
      "buffer" : 0,
      "byteOffset" : 0,
      "byteLength" : 12,
      "target" : 34963
    },
    {
      "buffer" : 0,
      "byteOffset" : 12,
      "byteLength" : 48,
      "target" : 34962
    }
  ],
  "accessors" : [
    {
      "bufferView" : 0,
      "byteOffset" : 0,
      "componentType" : 5123,
      "count" : 6,
      "type" : "SCALAR",
      "max" : [ 3 ],
      "min" : [ 0 ]
    },
    {
      "bufferView" : 1,
      "byteOffset" : 0,
      "componentType" : 5126,
      "count" : 4,
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

The geometry in this asset is a simple unit square. It is rotated by -45 degrees around the x-axis, to emphasize the effect of the different cameras. Image 15a shows three options for rendering this asset. The first examples use the cameras from the asset. The last example shows how the scene looks from an external, user-defined viewpoint.   
이 자산에 있는 기하 객체는 간단한 단위 정사각형이다. x-축을 기준으로 -45도 회전하여 서로 다른 카메라 효과를 강조하였다. 그림 15a는 이 자산을 렌더링하는 3개의 옵션을 보여 준다. 처음 두개의 예제 에서는 자산에 있는 카메라를 사용하였고, 마지막 예제는 외부의 사용자가 정의한 시점을 사용하였다. 

<p align="center">
<img src="images/cameras.png" /><br>
<a name="cameras-png"></a>Image 15a: The effect of rendering the scene with different cameras. - 그림 15a: 서로 다른 카메라로 장면을 렌더링한 결과
</p>


## Camera definitions - 카메라 정의

The new top-level element of this glTF asset is the `cameras` array, which contains the  [`camera`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-camera) objects:   

이 glTF 자산의 최상위 요소는 `cameras` 배열로, [`camera`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-camera) 객체를 포함하고 있다. 


```javascript
"cameras" : [
  {
    "type": "perspective",
    "perspective": {
      "aspectRatio": 1.0,
      "yfov": 0.7,
      "zfar": 100,
      "znear": 0.01
    }
  },
  {
    "type": "orthographic",
    "orthographic": {
      "xmag": 1.0,
      "ymag": 1.0,
      "zfar": 100,
      "znear": 0.01
    }
  }
],
```

When a camera object has been defined, it may be attached to a `node`. This is accomplished by assigning the index of the camera to the `camera` property of a node. In the given example, two new nodes have been added to the scene graph, one for each camera:   

카메라 객체가 정의되고 나면, `node`에 첨부될 수 있다. 이는 카메라의 인덱스를 노드의 `camera` 속성에 지정하는 방법을 수행한다. 주어진 예에서는 장면 그래프의 두개의 노드에 각각 카메라가 하나씩 지정되었다. 

```javascript
"nodes" : {
  ...
  {
    "translation" : [ 0.5, 0.5, 3.0 ],
    "camera" : 0
  },
  {
    "translation" : [ 0.5, 0.5, 3.0 ],
    "camera" : 1
  }
},
```

The differences between perspective and orthographic cameras and their properties, the effect of attaching the cameras to the nodes, and the management of multiple cameras will be explained in detail in the [Cameras](gltfTutorial_016_Cameras.md) section.

원근 투시와 평행 투시에 대한 차이점과 이를 위한 카메라 특성, 카메라를 노드에 첨부했을 때 나타나는 효과, 여러개의 카메라를 관리하는 방법등에 대해서는 [Cameras](gltfTutorial_016_Cameras.md)절에서 상세하게 설명한다.


이전: [Advanced Material](gltfTutorial_014_AdvancedMaterial.md) | [Table of Contents](README.md) | 다음: [Cameras](gltfTutorial_016_Cameras.md)
