이전: [Simple Morph Target](gltfTutorial_017_SimpleMorphTarget.md) | [Table of Contents](README.md) | 다음: [SimpleSkin](gltfTutorial_019_SimpleSkin.md)

# Morph Targets - 모핑 타겟

The example in the previous section contains a mesh that consists of a single triangle with two morph targets:   
이전 절에서 설명한 두개의 모핑 타겟을 갖는 삼각형으로 구성된 메쉬의 예제는 다음과 같다. 

```javascript
{
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
```


The actual base geometry of the mesh, namely the triangle geometry, is defined by the `mesh.primitive` attribute called `"POSITIONS"`. The morph targets of the `mesh.primitive` are dictionaries that map the attribute name `"POSITIONS"` to `accessor` objects that contain the *displacements* for each vertex. Image 18a shows the initial triangle geometry in black, and the displacement for the first morph target in red, and the displacement for the second morph target in green.

실제 기반 기하 형태는 삼각형 기하로, `mesh.primitive` 버텍스 속성에 `"POSITIONS"`라는 이름으로 정의되어 있다. `mesh.primitive` 모핑 타겟은 딕셔너리로서 `accessor` 객체에 대한  `"POSITIONS"` 버텍스 속성명을 매핑하여 각각의 버텍스의 *이동 - displacements* 정보를 포함한다. 그림 18a는 초기 삼강형 기하는 검은색으로 보여주고 있고, 첫번째 모핑 타겟에 의한 이동은 붉은색으로, 두번째 모핑 타겟에 의한 이동은 초록색으로 표현하고 있다. 

<p align="center">
<img src="images/simpleMorphInitial.png" /><br>
<a name="simpleMorphInitial-png"></a>Image 18a: The initial triangle and morph target displacements. - 그림 18a: 초기 삼각형과 모핑 타겟 이동
</p>

The `weights` of the mesh determine how these morph target displacements are added to the initial geometry in order to obtain the current state of the geometry. The pseudocode for computing the rendered vertex positions for a mesh `primitive` is as follows:

메쉬의 `weights`는 이들 모핑 타겟의 이동이 초기 기하에 이들 모핑 타겟이 어떻게 추가되는지를 결정하여 현재 상태의 기하를 만들게 된다. 메쉬 `primitive`에 대한 렌더링된 버텍스 좌표를 계산하기 위한 슈도 코드는 다음과 같다. 

```
renderedPrimitive.POSITION = primitive.POSITION + 
  weights[0] * primitive.targets[0].POSITION +
  weights[1] * primitive.targets[1].POSITION;
```

This means that the current state of the mesh primitive is computed by taking the initial mesh primitive geometry and adding a linear combination of the morph target displacements, where the `weights` are the factors for the linear combination.

이것은 메쉬 프리미티브의 현재 상태는 메쉬 프리미티브 기하의 초기 상태를 가져와 모핑 타겟의 이동과 선형 보간을 하여 계산되며, 이때 `weights`가 두개의 선형 보간의 가중치로 사용된다.

The asset additionally contains an `animation` that affects the weights for the morph targets. The following table shows the key frames of the animated weights:

어셋에는 추가적으로 `animation`을 갖고 있어, 모핑 타겟에 대한 가중치 값에 영향을 주게 된다. 다음 표는 애니메이션된 가중치의 키프레임을 보여 준다. 

| Time | Weights   |
|:----:|:---------:|
|  0.0 | 0.0, 0.0  |
|  1.0 | 0.0, 1.0  |
|  2.0 | 1.0, 1.0  |
|  3.0 | 1.0, 0.0  |
|  4.0 | 0.0, 0.0  |


Throughout the animation, the weights are interpolated linearly, and applied to the morph target displacements. At each point, the rendered state of the mesh primitive is updated accordingly. The following is an example of the state that is computed at 1.25 seconds.

애니메이션의 결과로, 가중치는 선형으로 보간되어, 모핑 타겟의 이동에 적용된다. 각 포인트에서, 메쉬 프리미티브의 렌더링 상태는 계속 갱신된다. 다음은 1.25 초에서 계산된 상태의 예제이다. 

<p align="center">
<img src="images/simpleMorphIntermediate.png" /><br>
<a name="simpleMorphIntermediate-png"></a>Image 18b: An intermediate state of the morph target animation. - 그림 18b: 모핑 타겟 애니메이션의 중간 상태
</p>




이전: [Simple Morph Target](gltfTutorial_017_SimpleMorphTarget.md) | [Table of Contents](README.md) | 다음: [SimpleSkin](gltfTutorial_019_SimpleSkin.md)







