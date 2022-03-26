이전: [Simple Skin](gltfTutorial_019_SimpleSkin.md) | [목차](README.md)


# Skins - 스킨

The process of vertex skinning is a bit complex. It brings together nearly all elements that are contained in a glTF asset. This section will explain the basics of vertex skinning, based on the example in the [Simple Skin](gltfTutorial_019_SimpleSkin.md) section.

버텍스 스키닝 과정은 다소 복잡하다. glTF 자산의 거의 모든 요소가 동원되어야 한다. 이 절에서는 버텍스 스키닝의 기초에 대하여 [Simple Skin](gltfTutorial_019_SimpleSkin.md)절의 예제를 통해 설명한다. 


## The geometry data - 기하 데이터

The geometry of the vertex skinning example is an indexed triangle mesh, consisting of 8 triangles and 10 vertices. They form a rectangle in the xy-plane, with a width of 1.0 and a height of 2.0. The bottom center of the rectangle is at the origin (0,0,0). So the positions of the vertices are   
버텍스 스키닝 예제의 기하 형상은 인덱싱된 삼각형 메쉬로, 8개의 삼각형들이 10개 버텍스로 구성되어 있다. xy-평면상의 사각형으로, 폭은 1.0이고 높이는 2.0이다. 사각형의 바닥 중심이 원점(0,0,0)에 위치하고 있다. 따라서 버텍스의 좌표는 다음과 같다. 

    -0.5, 0.0, 0.0,
     0.5, 0.0, 0.0,
    -0.5, 0.5, 0.0,
     0.5, 0.5, 0.0,
    -0.5, 1.0, 0.0,
     0.5, 1.0, 0.0,
    -0.5, 1.5, 0.0,
     0.5, 1.5, 0.0,
    -0.5, 2.0, 0.0,
     0.5, 2.0, 0.0

and the indices of the triangles are - 삼각형들의 인덱스를 다음과 같다. 

    0, 1, 3,
    0, 3, 2,
    2, 3, 5,
    2, 5, 4,
    4, 5, 7,
    4, 7, 6,
    6, 7, 9,
    6, 9, 8,

The raw data is stored in the first `buffer`. The indices and vertex positions are defined by the `bufferView` objects at index 0 and 1, and the corresponding `accessor` objects at index 0 and 1 offer typed access to these buffer views. Image 20a shows this geometry with outline rendering to better show the structure.

첫번째 `buffer`에 저장된 데이터이다. 인덱스와 버텍스 좌표는 `bufferView` 객체의 인덱스 0과 1로 정의되어 있고, 이에 해당하는 `accessor` 객체는 인덱스 0과 1로 이 버퍼 뷰를 접근할 수 있도록 한다. 그림 20a는 구조를 보여 주기 좋도록 외곽선으로 렌더링 된 기하 형상을 나타낸다.

<p align="center">
<img src="images/simpleSkinOutline01.png" /><br>
<a name="simpleSkinOutline01-png"></a>Image 20a: The geometry for the skinning example, with outline rendering, in its initial configuration. - 그림 20a: 스키닝 예제의 초기 설정 기하 형상을 외곽선으로 렌더링한 결과. 
</p>

This geometry data is contained in the mesh primitive of the only mesh, which is attached to the main node of the scene. The mesh primitive contains additional attributes, namely the `"JOINTS_0"` and `"WEIGHTS_0"` attributes. The purpose of these attributes will be explained below.

이 기하형상 데이터는 하나의 메쉬내에 메쉬 프리미티브를 갖고 있고, 장면의 메인 노드에 첨부된다. 이 메쉬 프리미티브는 추가적인 속성으로 `"JOINTS_0"` 과 `"WEIGHTS_0"` 라는 이름의 속성을 갖고 있다. 이 속성의 목적을 아래에서 설명한다. 


## The skeleton structure - 골격 구조 

In the given example, there are two nodes that define the skeleton. They are referred to as "skeleton nodes", or "joint nodes", because they can be imagined as the joints between the bones of the skeleton. The `skin` refers to these nodes, by listing their indices in its `joints` property.

주어진 예제에서, 두개의 노드들이 골격을 정의한다. 이를 "골격 노드 (skeleton nodes)", 혹은 "관절 노드 (joint nodes)"라고 하는데, 이는 골격구조에서 두개의 뼈에 연결된 관절로 쉽게 상상할 수 있기 때문이다. `skin`은 `joint` 속성의 인덱스를 열거하여 이들 노드를 참조한다.  

```javascript
  "nodes" : [ 
   ...
   {
    "children" : [ 2 ]
   }, 
   {
    "translation" : [ 0.0, 1.0, 0.0 ],
    "rotation" : [ 0.0, 0.0, 0.0, 1.0 ]
   }
  ],

```

The first joint node is located at the origin, and does not contain any transformations. The second node has a `translation` property, defining a translation about 1.0 along the y-axis, and a `rotation` property that initially describes a rotation about 0 degrees (thus, no rotation at all). This rotation will later be changed by the animation to let the skeleton bend left and right and show the effect of the vertex skinning.

첫번째 관절 노드는 원점에 위치하고 있고, 어떤 변환도 갖고 있지 않다. 두번째 노드는 `translation` 속성을 갖고 있어, y축으로 1.0 만큼 이동을 정의하고, `rotation` 속성은 초기 값은 0 (회전하지 않은 상태)로 정의되어 있다. 이 회전은 추후 애니메이션에 의해 변경되어 골격이 왼쪽과 오른쪽으로 구부러져 버텍스 스키닝 효과를 만들어 준다.

## The skin - 스킨


The `skin` is the core element of the vertex skinning. In the example, there is a single skin:

`skin`은 버텍스 스키닝의 핵심 요소이다. 이 예제에서는 하나의 스킨을 갖고 있다.

```javascript
  "skins" : [ 
   {
    "inverseBindMatrices" : 4,
    "joints" : [ 1, 2 ]
   }
  ],

```

The skin contains an array called `joints`, which lists the indices of the nodes that define the skeleton hierarchy. Additionally, the skin contains a reference to an accessor in the property `inverseBindMatrices`. This accessor provides one matrix for each joint. Each of these matrices transforms the geometry into the space of the respective joint. This means that each matrix is the *inverse* of the global transform of the respective joint, in its initial configuration. 

스킨은 `joints`라고 불리는 배열을 갖고 있다. 여기에는 노드들의 인덱스들의 리스트로 골격 계층을 정의한다. 추가로, 스킨에는 `inverseBindMatrices` 속성 내의 억세서에 대한 참조를 갖고 있다. 이 억세서는 각 관절마다 하나의 행렬을 갖고 있다. 이들 각 행렬은 기하형상을 해당 관절의 공간으로 변환하여. 이는 각 행렬은 초기 설정에서 해당 관절의 글로벌 변환에 대한 *역행렬 inverse* 로 생각할 수 있다. 

In the given example, joint `0` does not have an explicit transform, meaning that its global transform is the identity matrix. Therefore, the inverse bind matrix of joint `0` is also the identity matrix. 

주어진 예제에서는, 관절 `0`은 어떤 변환도 갖고 있지 않다. 즉, 글로벌 변환이 단위 행렬이다. 따라서 관절 `0`에 대한 연결 역행렬 역시 단위 행렬이 된다. 


Joint contains a translation about 1.0 along the y-axis. The inverse bind matrix of joint `1` is therefore

y-축으로 1.0 만큼 이동한 관절 `1`에 대한 연결 역행렬은 따라서 다음과 같다. 

    1.0   0.0   0.0    0.0   
    0.0   1.0   0.0   -1.0   
    0.0   0.0   1.0    0.0   
    0.0   0.0   0.0    1.0  

This matrix translates the mesh about -1.0 along the y-axis, as shown Image 20b.   
이 행렬은 메쉬를 y-축에 대하여 -1.0 만큼 이동한 것이다. 그림 20b 참조.

<p align="center">
<img src="images/skinInverseBindMatrix.png" /><br>
<a name="skinInverseBindMatrix-png"></a>Image 20b: The transformation of the geometry with the inverse bind matrix of joint 1. - 그림 20b: 관절 1의 연결 역행렬로 기하 형상의 변환
</p>

This transformation may look counterintuitive at first glance. But the goal of this transformation is to bring the coordinates of the skinned vertices into the same space as each joint.

변환을 처음 보면, 직관적으로 느끼는 것과 반대라고 노껴진다. 하지만, 이 변환의 목표는 스키닝 버텍스의 좌표를 가져다가 각 관절과 동일한 공간으로 가져가는 것이다. 


## Vertex skinning implementation - 버텍스 스키닝 구현

Users of existing rendering libraries will hardly ever have to manually process the vertex skinning data contained in a glTF asset: the actual skinning computations usually take place in the vertex shader, which is a low-level implementation detail of the respective library. However, knowing how the vertex skinning data is supposed to be processed may help to create proper, valid models with vertex skinning. So this section will give a short summary of how the vertex skinning is applied, using some pseudocode and examples in GLSL.

기존의 렌더링 라이브러리를 사용하는 사용자는 glTF 자산에 들어 있는 버텍스 스키닝을 수작업으로 처리하는 것은 꽤 어려운 일이다. 실제 스키닝 계산은 일반적으로 버텍스 쉐이더에서 일어나며, 라이브러러의 하위 계층에서 구현하는 것이 필요하다. 하지만, 버텍스 스키닝 데이터가 어떻게 처리되어야 하는지 알면 버텍스 스키닝을 사용하여 적절하고 유효한 모델을 만드는 데 도움이 될 수 있다. 따라서 이 절에서는 슈도 코드와 GLSL 예제를 통해 버텍스 스키닝이 어떻게 적용되는지 간단하게 요약 설명해 줄 것이다. 

### The joint matrices - 관절 행렬

The vertex positions of a skinned mesh are eventually computed by the vertex shader. During these computations, the vertex shader has to take into account the current pose of the skeleton in order to compute the proper vertex position. This information is passed to the vertex shader as an array of matrices, namely as the *joint matrices*. This is an array - that is, a `uniform` variable - that contains one 4&times;4 matrix for each joint of the skeleton. In the shader, these matrices are combined to compute the actual skinning matrix for each vertex:

스키닝된 메쉬의 버텍스 좌표는 결국 버텍스 셰이더에 의해 계산된다. 이러한 계산 동안 버텍스 셰이더는 적절한 버텍스 좌표를 계산하기 위해 골격의 현재 포즈를 고려해야 한다. 이 정보는 행렬의 배열, 즉 *조인트 행렬*로 버텍스 셰이더에 전달된다. 이는 골격의 각 관절에 대해 하나의 4&times;4 행렬을 포함하는 배열(즉, 'uniform' 변수)이다. 셰이더에서 다음 행렬을 결합하여 각 버텍스에 대한 실제 스키닝 행렬을 계산한다.

```glsl
...
uniform mat4 u_jointMat[2];

...
void main(void)
{
    mat4 skinMat =
        a_weight.x * u_jointMat[int(a_joint.x)] +
        a_weight.y * u_jointMat[int(a_joint.y)] +
        a_weight.z * u_jointMat[int(a_joint.z)] +
        a_weight.w * u_jointMat[int(a_joint.w)];
    ....
}
```

The joint matrix for each joint has to perform the following transformations to the vertices:    
가 관절의 관절 행렬은 버텍스들에 대해 다음 변환을 수행한다.

- The vertices have to be transformed with the `inverseBindMatrix` of the joint node, to bring them into the same coordinate space as the joint.
-관절과 동일한 좌표 공간으로 가져오기 위해 관절 노드의 'inverseBindMatrix'로  버텍스들을 변환 한다.
- The vertices have to be transformed with the *current* global transform of the joint node. Together with the transformation from the `inverseBindMatrix`, this will cause the vertices to be transformed only based on the current transform of the node, in the coordinate space of the current joint node.
- 버텍스는 조인트 노드의 *현재* 글로벌 변환을 사용해 변환되어야 한다. 'inverseBindMatrix'의 변환과 함께 이것은 현재 관절 노드의 좌표 공간에서 노드의 현재 변환을 기반으로만 버텍스가 변환되도록 한다.

So the pseudocode for computing the joint matrix of joint `j` may look as follows:   
관절 `j`의 관절 행렬를 계산하기 위한 슈도코드는 다음과 같다. 

    jointMatrix(j) =
      globalTransformOfJointNode(j) *
      inverseBindMatrixForJoint(j);
      
Note: Vertex skinning in other contexts often involves a matrix that is called "Bind Shape Matrix". This matrix is supposed to transform the geometry of the skinned mesh into the coordinate space of the joints. In glTF, this matrix is omitted, and it is assumed that this transform is either premultiplied with the mesh data, or postmultiplied to the inverse bind matrices. 

주: 다른 맥락에서의 버텍스 스키닝은 종종 "바인드 형상 행렬 - Bind Shape Matrix"이라는 행렬이 관련된다. 이 행렬은 스키닝된 메쉬의 기하 형상을 관절의 좌표 공간으로 변환을 수행한다. glTF에서는 이 행렬은 생략되고, 이 변환은 메쉬 데이터와 사전 곱해지거나 혹은 바인드 역행렬에 사후에 곱해진다고 가정한다.

Image 20c shows the transformations that are done to the geometry in the [Simple Skin](gltfTutorial_019_SimpleSkin.md) example, using the joint matrix of joint 1. The image shows the transformation for an intermediate state of the animation, namely, when the rotation of the joint node has already been modified by the animation, to describe a rotation about 45 degrees around the z-axis.

그림 20c는 [Simple Skin](gltfTutorial_019_SimpleSkin.md)예제의 기하 형상에 적용된 변환을 보여 준다. 이 변환은 관절 `1` 의 행렬을 사용하였다. 이 그림은 애니메이션의 중간 상태, 즉 관절 노드의 회전이 애니메이션에 의해 이미 수정된 경우 Z축을 중심으로 약 45도 회전을 설명하는 변환을 보여준다.

<p align="center">
<img src="images/skinJointMatrices.png" /><br>
<a name="skinJointMatrices-png"></a>Image 20c: The transformation of the geometry done for joint 1. - 그림 20c: 관절 1의 기하 형상 변환
</p>

The last panel of Image 20c shows how the geometry would look like if it were *only* transformed with the joint matrix of joint 1. This state of the geometry is never really visible: The *actual* geometry that is computed in the vertex shader will *combine* the geometries as they are created from the different joint matrices, based on the joints- and weights that are explained below.

그림 20c의 마지막 패널은 기하 형상이 *단지* 관절 1의 관절 행렬으로만 변환된 경우 어떻게 보일지 보여준다. 기하 형상의 이 상태는 실제로는 볼 수 없다. 버텍스 셰이더에서 계산되는 *실제* 기하 형상은 아래에 설명된 관절 및 가중치를 기반으로 서로 다른 관절 행렬에서 생성된 기하형상들을 *결합* 한다.


### The skinning joints and weights - 스키닝 관절들과 가중치

As mentioned above, the mesh primitive contains new attributes that are required for the vertex skinning. Particularly, these are the `"JOINTS_0"` and the `"WEIGHTS_0"` attributes. Each attribute refers to an accessor that provides one data element for each vertex of the mesh.

앞서 언급한 바와 같이, 메쉬 프리미티브는 버텍스 스키닝을 위해 필요한 새로운 속성을 갖고 있다. 특히 `"JOINTS_0"`과 `"WEIGHTS_0"` 속성이 있다. 각 속성은 억세서를 참조하여 메쉬의 각 버텍스의 데이터 요소를 제공하는 억세서를 참조한다.

The `"JOINTS_0"` attribute refers to an accessor that contains the indices of the joints that should have an influence on the vertex during the skinning process. For simplicity and efficiency, these indices are usually stored as 4D vectors, limiting the number of joints that may influence a vertex to 4. In the given example, the joints information is very simple:

`"JOINTS_0"` 속성은 억세를 참조하는데 스키닝 처리중에 영향을 받는 버텍스에 관절의 인덱스를 포함하고 있다. 단순하고 효과적으로 만들기 위해, 이들 인덱스는 4D 벡터로 저장되어, 각 버텍스의 영향을 주는 관절의 수를 4로 제한하고 있다. 주어진 예제는 아주 간단한 경우이다. 

    Vertex 0:  0, 0, 0, 0,
    Vertex 1:  0, 0, 0, 0,
    Vertex 2:  0, 1, 0, 0,
    Vertex 3:  0, 1, 0, 0,
    Vertex 4:  0, 1, 0, 0,
    Vertex 5:  0, 1, 0, 0,
    Vertex 6:  0, 1, 0, 0,
    Vertex 7:  0, 1, 0, 0,
    Vertex 8:  0, 1, 0, 0,
    Vertex 9:  0, 1, 0, 0,

This means that every vertex may be influenced by joint 0 and joint 1, except the first two vertices are influenced only by joint 0, and the last two vertices are influenced only by joint 1. The last 2 components of each vector are ignored here. If there were multiple joints, then one entry of this accessor could, for example, contain   
이것이 의미하는 것은 모든 버텍스는 관절 0과 1의 영향을 받는데, 처음 두개의 버텍스는 관절 0의 영향만을 받고, 마지막 2개의 버텍스는 관절 1에만 영향을 받는다. 나머지 2개의 요소는 무시된다. 관절이 좀더 많이 있다면 이 나머지 요소도 값을 가질 수 있다. 예를 들면, 

    3, 1, 8, 4,

meaning that the corresponding vertex should be influenced by the joints 3, 1, 8, and 4.  
는 해당 버텍스가 관절 3, 1, 8, 4의 영향을 받는 것을 의미한다. 

The `"WEIGHTS_0"` attribute refers to an accessor that provides information about how strongly each joint should influence each vertex. In the given example, the weights are as follows:   
`"WEIGHTS_0"` 속성은 각 버텍스가 얼마나 강하게 관절의 영향을 받는지를 정의하는 억세서를 참조한다. 주어진 예제에서는, 가중치는 다음과 같이 되어 있다:

    Vertex 0:  1.00,  0.00,  0.0, 0.0,
    Vertex 1:  1.00,  0.00,  0.0, 0.0,
    Vertex 2:  0.75,  0.25,  0.0, 0.0,
    Vertex 3:  0.75,  0.25,  0.0, 0.0,
    Vertex 4:  0.50,  0.50,  0.0, 0.0,
    Vertex 5:  0.50,  0.50,  0.0, 0.0,
    Vertex 6:  0.25,  0.75,  0.0, 0.0,
    Vertex 7:  0.25,  0.75,  0.0, 0.0,
    Vertex 8:  0.00,  1.00,  0.0, 0.0,
    Vertex 9:  0.00,  1.00,  0.0, 0.0,

Again, the last two components of each entry are not relevant, because there are only two joints.   
다시 설명하면, 두개의 관절만 있기 때문에 각 버텍스의 마지막 두개의 요소는 무관하다.

Combining the `"JOINTS_0"` and `"WEIGHTS_0"` attributes yields exact information about the influence that each joint has on each vertex. For example, the given data means that vertex 6 should be influenced to 25% by joint 0 and to 75% by joint 1.

`"JOINTS_0"`와 `"WEIGHTS_0"` 속성을 결합해서 각각의 버텍스에 관절들이 주는 영향을 정확하게 계산할 수 있다. 예를 들면, 위에 주어진 데이터를 보면, 버텍스 6은 관절0의 영향을 25% 받고 관절1의 영향을 75% 받게 된다.

In the vertex shader, this information is used to create a linear combination of the joint matrices. This matrix is called the *skin matrix* of the respective vertex. Therefore, the data of the `"JOINTS_0"` and `"WEIGHTS_0"` attributes are passed to the shader. In this example, they are given as the `a_joint` and `a_weight` attribute variable, respectively:

버텍스 쉐이더에서, 이 정보는 관절 행렬의 선형 조합을 만드는데 사용된다. 이 행렬을 각 버텍스에 대한 *스킨 행렬 - skin matrix* 라고 칭한다. 따라서,   `"JOINTS_0"` 와 `"WEIGHTS_0"` 속성에 있는 데이터는 쉐이더로 전달된다. 이 예제에서는 각각 `a_joint` 와 `a_weight` 라는 버텍스 속성 값으로 주어졌다.

```glsl
...
attribute vec4 a_joint;
attribute vec4 a_weight;

uniform mat4 u_jointMat[2];

...
void main(void)
{
    mat4 skinMat =
        a_weight.x * u_jointMat[int(a_joint.x)] +
        a_weight.y * u_jointMat[int(a_joint.y)] +
        a_weight.z * u_jointMat[int(a_joint.z)] +
        a_weight.w * u_jointMat[int(a_joint.w)];
    vec4 worldPosition = skinMat * vec4(a_position,1.0);
    vec4 cameraPosition = u_viewMatrix * worldPosition;
    gl_Position = u_projectionMatrix * cameraPosition;
}
```
The skin matrix is then used to transform the original position of the vertex into the world space. The transform of the node that the skin is attached to is ignored. The result of this transformation can be imagined as a weighted transformation of the vertices with the respective joint matrices, as shown in Image 20d.

스킨 행렬은 원래 버텍스 좌표를 변환하여 월드 좌표계로 보내는데 사용된다. 스킨이 첨부된 노드의 변환은 무시된다. 이 변환의 결과는 버텍스에 대하여 관절 행렬에 관련된 가중 변환으로 생각할 수 있다. 결과는 그림 20d를 참조.

<p align="center">
<img src="images/skinSkinMatrix.png" /><br>
<a name="skinSkinMatrix-png"></a>Image 20d: Computation of the skin matrix. - 그림 20d: 스킨 행렬의 계산
</p>

The result of applying this skin matrix to the vertices for the given example is shown in Image 20e.   
이 스킨 행렬을 버텍스에 적용한 결과는 그림 20e와 같다.  

<p align="center">
<img src="images/simpleSkinOutline02.png" /><br>
<a name="simpleSkinOutline02-png"></a>Image 20e: The geometry for the skinning example, with outline rendering, during the animation. - 그림 20e: 애니메이션과 외곽선 렌더링을 한 스키닝 예제를 위한 기하 형상 
</p>


이전: [Simple Skin](gltfTutorial_019_SimpleSkin.md) | [목차](README.md)
