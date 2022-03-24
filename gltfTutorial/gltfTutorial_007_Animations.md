이전: [Simple Animation](gltfTutorial_006_SimpleAnimation.md) | [Table of Contents](README.md) | 다음: [Simple Meshes](gltfTutorial_008_SimpleMeshes.md)

# Animations - 애니메이션

As shown in the [Simple Animation](gltfTutorial_006_SimpleAnimation.md) example, an [`animation`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-animation) can be used to describe how the `translation`, `rotation`, or `scale` properties of nodes change over time.

앞서 [Simple Animation](gltfTutorial_006_SimpleAnimation.md)의 예제에서 살펴본 바와 같이 [`animation`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-animation)을 사용하여 노드의 `translation`, `rotation`, `scale` 속성을 시간에 따라 변경하게 할 수 있다. 

The following is another example of an `animation`. This time, the animation contains two channels. One animates the translation, and the other animates the rotation of a node:

다음은 `animation`의 다른 예로, 두개의 채널에 대한 애니메이션으로 노드에 대해서 하나는 이동을 애니메이션 하고, 다른 하나는 회전을 애니메이션 한다. 

```javascript
  "animations": [
    {
      "samplers" : [
        {
          "input" : 2,
          "interpolation" : "LINEAR",
          "output" : 3
        },
        {
          "input" : 2,
          "interpolation" : "LINEAR",
          "output" : 4
        }
      ],
      "channels" : [ 
        {
          "sampler" : 0,
          "target" : {
            "node" : 0,
            "path" : "rotation"
          }
        },
        {
          "sampler" : 1,
          "target" : {
            "node" : 0,
            "path" : "translation"
          }
        } 
      ]
    }
  ],
```


## Animation samplers - 애니메이션 샘플러

The `samplers` array contains [`animation.sampler`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-animation-sampler) objects that define how the values that are provided by the accessors have to be interpolated between the key frames, as shown in Image 7a.

`sampler` 배열은 [`animation.sampler`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-animation-sampler) 객체를 갖고 있어, 억세서에 의해 제공된 값이 키프레임 사이에서 어떻게 보간되어야 하는지를 정의한다. 아래 그림 7a 참조.

<p align="center">
<img src="images/animationSamplers.png" /><br>
<a name="animationSamplers-png"></a>Image 7a: Animation samplers.
</p>

In order to compute the value of the translation for the current animation time, the following algorithm can be used:

현재 애니메이션 시각에 해당하는 이동 값을 계산하기 위해서는 다음과 같은 알고리듬이 사용된다. 

* Let the current animation time be given as `currentTime`.
* 현재 시각을 `currentTime`으로 주어졌다고 하자. 
* Compute the next smaller and the next larger element of the *times* accessor:
* *시간* 억세서의 요소에서 현재 시각에 가장 가깝고 작은 값과 큰 값을 찾는다. 

    `previousTime` = The largest element from the *times* accessor that is smaller than the `currentTime`   
    `previousTime` = *시간* 억세서에서 `currentTime` 보다 작은 값 중 가장 큰 값을 찾아 넣는다.    

    `nextTime`  = The smallest element from the *times* accessor that is larger than the `currentTime`    
    `nextTime`  = *시간* 억세서에서 `currentTime` 큰 값 중 가장 작은 값을 찾아 넣는다.
* Obtain the elements from the *translations* accessor that correspond to these times:
* *translations* 억세서에서 다음에 해당하는 값을 얻는다. 
    `previousTranslation` = The element from the *translations* accessor that corresponds to the `previousTime`
    `previousTranslation` = *translations* 억세서에서 `previousTime` 에 해당하는 값을 가져온다.
    `nextTranslation` = The element from the *translations* accessor that corresponds to the `nextTime`    
    `nextTranslation`= *translations* 억세서에서 `previousTime` 에 해당하는 값을 가져온다.
* Compute the interpolation value. This is a value between 0.0 and 1.0 that describes the *relative* position of the `currentTime`, between the `previousTime` and the `nextTime`:
* 보간 값을 계산 한다. 여기에서는 `previousTime`과 `nextTime`을 0.0에서 1.0 사이의 *상대적인* 위치로 `currentTime`을 계산한다.
    `interpolationValue = (currentTime - previousTime) / (nextTime - previousTime)`
* Use the interpolation value to compute the translation for the current time:
* 이 보간 값을 사용하여 다음 식으로 현재 시간의 이동 값을 계산한다.    
    `currentTranslation = previousTranslation + interpolationValue * (nextTranslation - previousTranslation)`


### Example: 예제

Imagine the `currentTime` is **1.2**. The next smaller element from the *times* accessor is **0.8**. The next larger element is **1.6**. So

`currentTime`이 **1.2** 이고, 이보다 작은 *시간* 억세서 값이 **0.8** 이고 다음 값이 **1.6** 이라면

    previousTime = 0.8
    nextTime     = 1.6

The corresponding values from the *translations* accessor can be looked up:    
여기에 해당하는 *이동* 억세서 값을 찾으면:

    previousTranslation = (14.0, 3.0, -2.0)
    nextTranslation     = (18.0, 1.0,  1.0)

The interpolation value can be computed:   
보간 값을 계산하면:

    interpolationValue = (currentTime - previousTime) / (nextTime - previousTime)
                       = (1.2 - 0.8) / (1.6 - 0.8)
                       = 0.4 / 0.8         
                       = 0.5

From the interpolation value, the current translation can be computed:  
보간된 이동 값을 계산하면:

    currentTranslation = previousTranslation + interpolationValue * (nextTranslation - previousTranslation)
                       = (14.0, 3.0, -2.0) + 0.5 * ( (18.0, 1.0,  1.0) - (14.0, 3.0, -2.0) )
                       = (14.0, 3.0, -2.0) + 0.5 * (4.0, -2.0, 3.0)
                       = (16.0, 2.0, -0.5)

So when the current time is **1.2**, then the `translation` of the node is **(16.0, 2.0, -0.5)**.
따라서, 시각 **1.2**에 해당하는 노드의 `translation` 속성 값은 **(16.0, 2.0, -0.5)** 이 된다.


## Animation channels - 애니메이션 채널

The animations contain an array of [`animation.channel`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-animation-channel) objects. The channels establish the connection between the input, which is the value that is computed from the sampler, and the output, which is the animated node property. Therefore, each channel refers to one sampler, using the index of the sampler, and contains an [`animation.channel.target`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-animation-channel-target). The `target` refers to a node, using the index of the node, and contains a `path` that defines the property of the node that should be animated. The value from the sampler will be written into this property.

애니메이션에는 [`animation.channel`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-animation-channel) 객체의 배열을 포함하고 있다. 채널들은 샘플러로 부터 계산된 입력 값과 애니메이션될 노드의 속성에 해당하는 출력 값의 연결을 구축한다. 따라서, 각 채널은 하나의 샘플러를 참조하고, 샘플러의 인덱스를 사용하고, [`animation.channel.target`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-animation-channel-target)를 갖고 있다. `target`은 노드를 참조하고, 노드의 인덱스를 사용하고, `path`에 애니메이션 될 노드에 대한 속성을 정의한다. 샘플러로 부터 온 값은 이 속성의 값으로 들어가게 된다.

In the example above, there are two channels for the animation. Both refer to the same node. The path of the first channel refers to the `translation` of the node, and the path of the second channel refers to the `rotation` of the node. So all objects (meshes) that are attached to the node will be translated and rotated by the animation, as shown in Image 7b.   
위 예제에서는, 두개의 애니메이션 채널이 있다. 두 채널 모두 같은 노드를 참조한다. 첫번째 채널의 `path`는 노드의 `translation`을 참조하고, 두번째 채널은 `rotation`을 참조한다. 따라서, 이 노드에 연결되어 있는 모든 객체 (meshes)는 이 애니메이션에 따라 이동하고 회전하게 된다. 그림 7b 참조.

<p align="center">
<img src="images/animationChannels.png" /><br>
<a name="animationChannels-png"></a>Image 7b: Animation channels. - 그림 7b: 애니메이션 채널들
</p>

## Interpolation - 보간

The above example only covers `LINEAR` interpolation. Animations in a glTF asset can use three interpolation modes :    
위 예제에서는 `LINEAR` 보간만을 다루었으나, glTF 자산에서는 다음 3개의 보간 모드를 사용할 수 있다: 

 - `STEP`
 - `LINEAR`
 - `CUBICSPLINE`
 
### Step - 계단형 보간

The `STEP` interpolation is not really an interpolation mode, it makes objects jump from keyframe to keyframe *without any sort of interpolation*. When a sampler defines a step interpolation, just apply the transformation from the keyframe corresponding to `previousTime`.

`STEP` 보간은 계단형 보간으로 실제 보간을 하지는 않는다. 즉, *어떤 종류의 보간도 하지 않고* 키프레임 값으로 바로 점프를 한다. 샘플러가 계단형 보간으로 정의 되어 있으며, 현재 시각에 해당하는 `previousTime`의 키프레임 값이 변환에 적용된다. 

### Linear - 선형 보간

Linear interpolation exactly corresponds to the above example. The general case is :    
선형 보간은 위 예에서 사용된 것과 같다. 일반 코드는 : 

Calculate the `interpolationValue`:    
`interpolationValue` 계산 방법:

```
    interpolationValue = (currentTime - previousTime) / (nextTime - previousTime)
```

For scalar and vector types, use a linear interpolation (generally called `lerp` in mathematics libraries). Here's a "pseudo code" implementation for reference

스칼라 값이나 벡터 타입의 경우에는 선형 보간을 사용한다. (수학 라이브러리에는 일반적으로 `lerp`로 불리운다) 다음은 참고하기 위한 구현 "슈도 코드"이다. 

```
    Point lerp(previousPoint, nextPoint, interpolationValue)
        return previousPoint + interpolationValue * (nextPoint - previousPoint)
```

In the case of rotations expressed as quaternions, you need to perform a spherical linear interpolation (`slerp`) between the previous and next values:   
만일 회전이 쿼터니언으로 표현되어 있는 경우, 두 값을 구형선형보간(`slerp`)을 통해 보간 해야 한다.

```
    Quat slerp(previousQuat, nextQuat, interpolationValue)
        var dotProduct = dot(previousQuat, nextQuat)
        
        //make sure we take the shortest path in case dot Product is negative
        if(dotProduct < 0.0)
            nextQuat = -nextQuat
            dotProduct = -dotProduct
            
        //if the two quaternions are too close to each other, just linear interpolate between the 4D vector
        if(dotProduct > 0.9995)
            return normalize(previousQuat + interpolationValue(nextQuat - previousQuat))

        //perform the spherical linear interpolation
        var theta_0 = acos(dotProduct)
        var theta = interpolationValue * theta_0
        var sin_theta = sin(theta)
        var sin_theta_0 = sin(theta_0)
        
        var scalePreviousQuat = cos(theta) - dotproduct * sin_theta / sin_theta_0
        var scaleNextQuat = sin_theta / sin_theta_0
        return scalePreviousQuat * previousQuat + scaleNextQuat * nextQuat
```

This example implementation is inspired from this [Wikipedia article](https://en.wikipedia.org/wiki/Slerp)    
이 구현 예는 [위키 문서](https://en.wikipedia.org/wiki/Slerp)를 참고하였다. 

### Cubic Spline interpolation - 3차 스플라인 보간

Cubic spline interpolation needs more data than just the previous and next keyframe time and values, it also need for each keyframe a couple of tangent vectors that act to smooth out the curve around the keyframe points.  
3차 스플라인 보간에는 바로 직전 키프레임과 다음 키프레임보다 많은 데이터가 필요하다. 여기에는 각 키프레임에 몇개의 탄젠트 값이 필요한데 이를 이용하여 키프레임 값들을 곡선으로 부드럽게 연결하는 보간이 만들어 진다.

These tangent are stored in the animation channel. For each keyframe described by the animation sampler, the animation channel contains 3 elements :    
이 탄젠트 값은 애니메이션 채널에 저장된다. 애니메이션 샘플러에 의해 정의된 각 키프레임이 정의되며, 애니메이션 채널은 다음 3개 요소를 갖는다 :   

 - The input tangent of the keyframe - 키프레임의 입력 탄젠트
 - The keyframe value - 키프레임 값
 - The output tangent - 출력 탄젠트 
 
 The input and output tangents are normalized vectors that will need to be scaled by the duration of the keyframe, we call that the deltaTime    
 입력과 출력 탄젠트는 정규화된 벡터로 키프레임의 기간으로 스케일링 되어 있어야 한다. 이를 deltaTime으로 부른다. 
 
 ```
     deltaTime = nextTime - previousTime
 ```
 
 To calculate the value for `currentTime`, you will need to fetch from the animation channel :    
`currentTime` 값을 계산하기 위해 애니메이션 채널로 부터 값을 가져와야 한다. 
 
 - The output tangent direction of `previousTime` keyframe
 - `previousTime` 키프레임의 출력 탄젠트 방향 
 - The value of `previousTime` keyframe
 - `previousTime` 키프레임의 값
 - The value of `nextTime` keyframe
 - `nextTime` 키프레임의 값
 - The input tangent direction of `nextTime` keyframe
 - `nextTime` 키프렘의 입력 탄젠트 방향
 
*note: the input tangent of the first keyframe and the output tangent of the last keyframe are totally ignored*   
*주의: 첫번째 키프레임의 입력 탄젠트 마지막 키프레임의 출력 탄젠트 값은 완전히 무시된다* 
 
To calculate the actual tangents of the keyframe, you need to multiply the direction vectors you got from the channel by `deltaTime` 
키프렘에서 실제 탄젠트 값을 계산하기 위해서는, `deltaTime`으로 채널로부터 얻어진 방향을 곱해서 얻어진다.

```
    previousTangent = deltaTime * previousOutputTangent
    nextTangent = deltaTime * nextInputTangent
```
 
The mathematical function is described in the [Appendix C](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#appendix-c-interpolation) of the glTF 2.0 specification.   
glTF 2.0 표준의 [Appendix C](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#appendix-c-interpolation)에 관련된 수식이 설명되어 있다. 

Here's a corresponding pseudocode snippet :   
이에 해당하는 코드 조각은 다음과 같다 : 

```
    Point cubicSpline(previousPoint, previousTangent, nextPoint, nextTangent, interpolationValue)
        t = interpolationValue
        t2 = t * t
        t3 = t2 * t
        
        return (2 * t3 - 3 * t2 + 1) * previousPoint + (t3 - 2 * t2 + t) * previousTangent + (-2 * t3 + 3 * t2) * nextPoint + (t3 - t2) * nextTangent;
```

이전: [Simple Animation](gltfTutorial_006_SimpleAnimation.md) | [Table of Contents](README.md) | 다음: [Simple Meshes](gltfTutorial_008_SimpleMeshes.md)
