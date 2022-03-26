이전: [Simple Cameras](gltfTutorial_015_SimpleCameras.md) | [Table of Contents](README.md) | 다음: [Simple Morph Target](gltfTutorial_017_SimpleMorphTarget.md)

# Cameras - 카메라

The example in the [Simple Cameras](gltfTutorial_017_SimpleCameras.md) section showed how to define perspective and orthographic cameras, and how they can be integrated into a scene by attaching them to nodes. This section will explain the differences between both types of cameras, and the handling of cameras in general.  

[Simple Cameras](gltfTutorial_017_SimpleCameras.md)절의 예제에서는 원근 투영과 평행 투영 카메라를 정의하는 방법을 살펴보았고, 노드에 첨부하여 장면에 통합하는 방법을 살펴보았다. 이 절에서는 이 두 카메라의 차이점을 설명하고, 일반적으로 카메라를 다루는 방법에 대하여 설명한다.

## Perspective and orthographic cameras - 원근 투영과 평행 투영 카메라

There are two kinds of cameras: *Perspective* cameras, where the viewing volume is a truncated pyramid (often referred to as "viewing frustum"), and *orthographic*  cameras, where the viewing volume is a rectangular box. The main difference is that rendering with a *perspective* camera causes a proper perspective distortion, whereas rendering with an *orthographic* camera causes a preservation of lengths and angles.

두 종류의 카메라가 있다. 하나는 *원근 투영 (perspective)* 카메라로 피라미드의 윗 부분을 자른 모양의 공간(뷰잉 프러스텀, 각뿔대, frustum) 을 보게 된다. *평행 투영 (orthographic)* 의 경우 직육면체 박스 공간을 보게 된다. 둘 사이의 가장 큰 차이점은 *원근 투영*은 원근법에 의한 변형이 생기지만, *평행투영* 카메라는 길이와 각도가 유지된다.


The example in the [Simple Cameras](gltfTutorial_015_SimpleCameras.md) section contains one camera of each type, a perspective camera at index 0 and an orthographic camera at index 1:   
[Simple Cameras](gltfTutorial_015_SimpleCameras.md)절에서 살펴본 카메라의 예는 인덱스 0이 원근 투영 카메라이고 인덱스 1이 평행투영 카메라 이다.  

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


The `type` of the camera is given as a string, which can be `"perspective"` or  `"orthographic"`. Depending on this type, the `camera` object contains a [`camera.perspective`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-camera-perspective) object or a [`camera.orthographic`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-camera-orthographic) object. These objects contain additional parameters that define the actual viewing volume.

카메라의 `type` 속성 값은 `"perspective"` 또는 `"orthographic"` 중 하나로 주어진다. 이 타입에 따라 `camera` 객체는 [`camera.perspective`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-camera-perspective) 객체나 혹은 [`camera.orthographic`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-camera-orthographic) 객체를 갖게 된다. 이들 객체는 뷰 볼륨 (Viewing Volumn)을 결정하는 추가적인 파라메터를 갖는다.

The `camera.perspective` object contains an `aspectRatio` property that defines the aspect ratio of the viewport. Additionally, it contains a property called `yfov`, which stands for *Field Of View in Y-direction*. It defines the "opening angle" of the camera and is given in radians.

`camera.perspective` 객체는 `aspectRatio` 속성을 갖는다. 이 속성은 뷰포트의 종횡비(aspect ratio)를 정의한다. 추가로, `yfov`라는 속성을 갖는데, *Y-방향 시야각 - Field Of View in Y-direction* 을 의미한다. 카메라의 수직 방향의 *열린 각도*를 정의하는데 단위는 라디안을 사용한다.

The `camera.orthographic` object contains `xmag` and `ymag` properties. These define the magnification of the camera in x- and y-direction, and basically describe the width and height of the viewing volume.

`camera.orthographic` 객체는 `xmag` 와 `ymag` 속성을 갖고 있다. 이들은 x-방향과 y-방향을 크기를 정의한다. 기본적으로 뷰 볼륨의 폭과 높이를 정의한다.  

Both camera types additionally contain `znear` and `zfar` properties, which are the coordinates of the near and far clipping plane. For perspective cameras, the `zfar` value is optional. When it is missing, a special "infinite projection matrix" will be used.

두 카메라 유형 모두 추가적으로 `znear` 와 `zfar` 속성을 갖고 있는데, 앞과 뒤쪽의 클리핑 평면의 거리를 정의한다. 원근 투영 카메라에서는 `zfar` 값은 옵션이다. 이 값이 주어지지 않으면 zfar 값이 무한대로 가정하고 "무한 투영 행렬"이 사용된다.

Explaining the details of cameras, viewing, and projections is beyond the scope of this tutorial. The important point is that most graphics APIs offer methods for defining the viewing configuration that are directly based on these parameters. In general, these parameters can be used to compute a *camera matrix*. The camera matrix can be inverted to obtain the *view matrix*, which will later be post-multiplied with the *model matrix* to obtain the *model-view matrix*, which is required by the renderer.

카메라, 뷰, 투영에 대한 세부 정보를 설명하는 것은 이 튜토리얼의 범위가 아니다. 중요한 점은 대부분의 그래픽 API가 이들 패러메터에 직접적으로 기반한 뷰일 설정 방법을 제공한다는 점이다. 일반적으로 이러한 매개변수를 사용하여 *카메라 행렬 Camera Matrix*를 계산할 수 있다. 카메라 행렬은 *뷰 행렬 View-Matrix*을 얻기 위해 역행렬을 계산하고, 나중에 *모델 행렬 - Model Matrix*과 곱해져서 렌더러에 필요한 *모델-뷰 행렬 - Model-View Matrix*을 얻는다.


# Camera orientation - 카베라 방향

A `camera` can be transformed to have a certain orientation and viewing direction in the scene. This is accomplished by attaching the camera to a `node`. Each [`node`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-node) may contain the index of a `camera` that is attached to it. In the simple camera example, there are two nodes for the cameras. The first node refers to the perspective camera with index 0, and the second one refers to the orthographic camera with index 1:

`camera`는 장면내에 어떤 방향을 보게되는 방향을 갖게 된다. 이는 카메라를 `node`에 첨부하여 수행된다. 각 [`node`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-node)는 `camera`의 인덱스를 지정하여 첨부하게 된다. 이 간단한 카메라 예제에서는 두개의 카메라 노드가 있다. 첫번째 노드는 인덱스 0으로 원근 투영 카메라를 참조하고, 두번째 노드는 평행 투영 카메라를 인덱스 1로 참조한다. 

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

As shown in the [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md) section, these nodes may have properties that define the transform matrix of the node. The [global transform](gltfTutorial_004_ScenesNodes.md#global-transforms-of-nodes) of a node then defines the actual orientation of the camera in the scene. With the option to apply arbitrary [animations](gltfTutorial_007_Animations.md) to the nodes, it is even possible to define camera flights.

[Scenes and Nodes](gltfTutorial_004_ScenesNodes.md)절에서 살펴본 바와 같이, 이들 노드들은 노드의 변환 행렬을 정의하는 속성을 갖고 있다. 노드의 [global transform](gltfTutorial_004_ScenesNodes.md#global-transforms-of-nodes)이 장면 내에서 실제 카메라의 방향을 결정한다. 이 노드에 대한 임의의 [animations](gltfTutorial_007_Animations.md) 을 적용하면, 카메라가 날아가는 효과를 정의할 수 있다.

When the global transform of the camera node is the identity matrix, then the eye point of the camera is at the origin, and the viewing direction is along the negative z-axis. In the given example, the nodes both have a `translation` about `(0.5, 0.5, 3.0)`, which causes the camera to be transformed accordingly: it is translated about 0.5 in the x- and y- direction, to look at the center of the unit square, and about 3.0 along the z-axis, to move it a bit away from the object.

카메라 노드의 글로벌 변환이 단위 행렬인 경우, 카메라는 원점에 위치하고, 보는 방향은 음의 z-축 방향이다. 주어진 예제에서는 노드들은  `(0.5, 0.5, 3.0)` 위치로 `translation` 되었고, 이에 따라 카메라는 변환된다. 즉, 카메라는 x-, y- 방향으로 0.5 만큼 이동하고, z-축 방향으로 3.0 만큼 이동하여 물체에서 약간 떨어진 위치에 자리를 잡고 단위 정사각형의 중심을 바라보는 위치에 자리를 잡게 된다.


## Camera instancing and management - 카메라 인스턴싱과 관리

There may be multiple cameras defined in the JSON part of a glTF. Each camera may be referred to by multiple nodes. Therefore, the cameras as they appear in the glTF asset are really "templates" for actual camera *instances*: Whenever a node refers to one camera, a new instance of this camera is created.

glTF의 JSON 부분내에 여러개의 카메라를 정의할 수 있다. 각각의 카메라는 다수의 노드들을 참조할 수 있다. 따라서, glTF 자산내의 나타나는 카메라는 실제 카메라의 *인스턴스*에 해당하는 "템플릿"으로 작동하게 된다. 하나의 카메라를 노드가 참조할 때마다, 새로운 카메라의 인스턴스가 생성된다.  

There is no "default" camera for a glTF asset. Instead, the client application has to keep track of the currently active camera. The client application may, for example, offer a dropdown-menu that allows one to select the active camera and thus to quickly switch between predefined view configurations. With a bit more implementation effort, the client application can also define its own camera and interaction patterns for the camera control (e.g., zooming with the mouse wheel). However, the logic for the navigation and interaction has to be implemented solely by the client application in this case. [Image 15a](gltfTutorial_015_SimpleCameras.md#cameras-png) shows the result of such an implementation, where the user may select either the active camera from the ones that are defined in the glTF asset, or an "external camera" that may be controlled with the mouse.

glTF 자산에는 "디폴트" 카메라가 없다. 대신에, 클라이언트 응용이 활성 액티브 카메라를 계속 관리하게 된다. 클라이언트 응용은, 예를 들면, 드롭다운-메뉴를 제공하여 사용자가 활성화된 카메라를 선택하여 사전에 정의된 뷰 설정들을 스위칭할 수 있다. 좀 더 많은 추가 구현을 통해, 클라이언트 응용은 자신의 카메라를 정의하여 카메라 제어를 위한 인터렉션 (예 - 마우스 휠을 사용한 줌) 등을 구현할 수 있다. 하지만, 이 경우 네이게이션이나 인터렉션을 위한 로직은 클라이언트 응용프로그램에 의해서만 구현될 수 있다.  [Image 15a](gltfTutorial_015_SimpleCameras.md#cameras-png)은 이러한 응용의 구현 결과를 보여준다. 사용자는 glTF 자산에 정의된 활성 카메라를 선택하거나, 혹은 마우스로 제어되는 "외부 카메라"를 사용할 수도 있다. 


이전: [Simple Cameras](gltfTutorial_015_SimpleCameras.md) | [Table of Contents](README.md) | 다음: [Simple Morph Target](gltfTutorial_017_SimpleMorphTarget.md)
