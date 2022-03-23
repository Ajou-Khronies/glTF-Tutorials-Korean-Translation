이전: [A Minimal glTF File](gltfTutorial_003_MinimalGltfFile.md) | [Table of Contents](README.md) | 다음: [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md)

# Scenes and Nodes - 장면과 노드

## Scenes - 장면

There may be multiple scenes stored in one glTF file. The `scene` property indicated which of these scenes should be the default scene that is displayed when the asset is loaded. Each scene contains an array of `nodes`, which are the indices of the root nodes of the scene graphs. Again, there may be multiple root nodes, forming different hierarchies, but in many cases, the scene will have a single root node. The most simple possible scene description has already been shown in the previous section, consisting of a single scene with a single node:

하나의 glTF 파일 안에는 여러개의 장면이 들어 있을 수 있다. `scene` 속성에서는 여러 장면 중에 어는 장면이 기본(default) 장면이 될지를 지정해서 자산이 로딩될때 디스플레이될 장면을 결정한다. 각각의 장면은 `nodes`의 배열이 있어 여기에 장면 그래프들의 루트 노드들의 인덱스들이 들어 있다. 다시, 다수의 루트 노드들이 다른 계층을 구성할 수 있는데, 많은 경우 하나의 장면은 하나의 루트 노드를 갖는다. 가장 간단한 장면 서술은 이전 절에 본 예제와 같이 하나의 장면에 하나의 노드가 있는 경우이다. 

```javascript
  "scene": 0, 
  "scenes" : [
    {
      "nodes" : [ 0 ]
    }
  ],

  "nodes" : [
    {
      "mesh" : 0
    }
  ],
```


## Nodes forming the scene graph - 장면 그래프를 구성하는 노드(Node)

Each [`node`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-node) can contain an array called `children` that contains the indices of its child nodes. So each node is one element of a hierarchy of nodes, and together they define the structure of the scene as a scene graph.  

각각의 [`node`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-node)는 `children`이라 불리우는 배열을 갖을 수 있으며, 여기에는 자녀 노드들의 인덱스를 포함하고 있다. 따라서 각 노드는 노드의 계층의 한 요소가 되며, 함께 모여 장면 그래프로서 장면의 구조를 정의하게 된다.

<p align="center">
<img src="images/sceneGraph.png" /><br>
<a name="sceneGraph-png"></a>Image 4a: The scene graph representation stored in the glTF JSON. - 그림 4a: glTF JSON 내에 저장된 장면 그래프 표현. 
</p>

Each of the nodes that are given in the `scene` can be traversed, recursively visiting all their children, to process all elements that are attached to the nodes. The simplified pseudocode for this traversal may look like the following:

`scene`내에 주어진 각각의 노드들은 탐색이 가능하고, 재귀적으로 자녀 노드에 대한 방문을 할 수 있으며, 노드들에 첨부되어 있는 모든 요소를 처리한다. 이러한 탐색을 위한 단순화된 슈도 코드는 다음과 같이 나타낼 수 있다. 

```
traverse(node) {
    // Process the meshes, cameras, etc., that are
    // attached to this node - discussed later
    processElements(node);

    // Recursively process all children
    for each (child in node.children) {
        traverse(child);
    }
}
```

In practice, some additional information will be required for the traversal: the processing of some elements that are attached to nodes will require information about *which* node they are attached to. Additionally, the information about the transforms of the nodes has to be accumulated during the traversal.

실제로는, 탐색에는 일부 추가적인 정보가 요구된다. 노드에 첨부되어 있는 일부 요소의 처리에는 *어떤* 노드에 첨부되어 있는지에 대한 정보가 필요하다. 추가적으로, 노드의 변환(transforms)에 대한 정보가 탐색과정에서 누적되어 적용되어야 한다. 


### Local and global transforms - 로컬 및 글로벌 변환

Each node can have a transform. Such a transform will define a translation, rotation, and/or scale. This transform will be applied to all elements attached to the node itself and to all its child nodes. The hierarchy of nodes thus allows one to structure the translations, rotations, and scalings that are applied to the scene elements.

각 노드는 변환을 갖고 있을 수 있다. 이러한 변환은 이동(translate), 회전(rotation), 신축(scale)을 정의한다. 이 변환은 노드 자신과 노드와 자식노드에 첨부되어 있는 모든 요소에 적용될 것이다. 노드의 계층 구조는 따라서 구조적으로 이동, 회전, 신축을 장면 요소에 적용할 수 있게 된다. 

#### Local transforms of nodes - 노드의 로컬 변환 

There are different possible representations for the local transform of a node. The transform can be given directly by the `matrix` property of the node. This is an array of 16 floating point numbers that describe the matrix in column-major order. For example, the following matrix describes a scaling about (2,1,0.5), a rotation about 30 degrees around the x-axis, and a translation about (10,20,30):

노드의 로컬 변환에 대하여 서로 다른 여러가지 표현 방법이 존재한다. 변환은 노드에 `matrix` 속성을 이용해 직접적으로 줄 수 있다. 이 행렬은 16개의 부동소수점 숫자로 구성된 배열로 표현되며, 행 우선 (column-major) 순서로 되어 있다. 예를 들면, 다음 행렬은 (2,1,0.5) 만큼 신추가고, x-축을 기준으로 30도 회전하고, (10,20,30) 만큼 이동한 변환을 나타낸다. 

```javascript
"node0": {
    "matrix": [
        2.0,    0.0,    0.0,    0.0,
        0.0,    0.866,  0.5,    0.0,
        0.0,   -0.25,   0.433,  0.0,
       10.0,   20.0,   30.0,    1.0
    ]
}    
```

The matrix defined here is as shown in Image 4b.

여기 정의된 행렬을 수식으로 나타내면 그림 4b와 같다. 

<p align="center">
<img src="images/matrix.png" /><br>
<a name="matrix-png"></a>Image 4b: An example matrix. - 그림 4b - 행렬 예
</p>


The transform of a node can also be given using the `translation`, `rotation`, and `scale` properties of a node, which is sometimes abbreviated as *TRS*:  

노드에 대한 변환은 노드의 속성으로서, `translation`, `rotation`, `scale`을 사용하여 표현할 수 있다. 자주 *TRS* 로 약자로 표시된다.

```javascript
"node0": {
    "translation": [ 10.0, 20.0, 30.0 ],
    "rotation": [ 0.259, 0.0, 0.0, 0.966 ],
    "scale": [ 2.0, 1.0, 0.5 ]
}
```

Each of these properties can be used to create a matrix, and the product of these matrices then is the local transform of the node:

각 속성은 행렬을 만드는데 사용되고, 이 행렬들의 곱을 하면 노드에 대한 로컬 변환이 된다. 

- The `translation` just contains the translation in x-, y-, and z-direction. For example, from a translation of `[ 10.0, 20.0, 30.0 ]`, one can create a translation matrix that contains this translation as its last column, as shown in Image 4c.
- `translation`은 x-, y-, z-방향의 이동을 정의한다. 예를 들면 `[ 10.0, 20.0, 30.0 ]` 이동을 하면, 그림 4c와 같이 마지막 행의 값들에 이 벡터 값이 들어가게 된다. 

<p align="center">
<img src="images/translationMatrix.png" /><br>
<a name="translationMatrix-png"></a>Image 4c: A translation matrix. - 그림 4c: 이동 행렬
</p>


- The `rotation` is given as a [quaternion](https://en.wikipedia.org/wiki/Quaternion). The mathematical background of quaternions is beyond the scope of this tutorial. For now, the most important information is that a quaternion is a compact representation of a rotation about an arbitrary angle and around an arbitrary axis. It is stored as a tuple `(x,y,z,w)`, where the `w`-component is the cosine of half of the rotation angle. For example, the quaternion `[ 0.259, 0.0, 0.0, 0.966 ]` describes a rotation about 30 degrees, around the x-axis. So this quaternion can be converted into a rotation matrix, as shown in Image 4d.
- `rotation`은 [quaternion](https://en.wikipedia.org/wiki/Quaternion) 형태로 주어진다. 쿼터니언에 대한 수학적인 배경은 이 튜토리얼에서 다루지 않는다. 중요한 사실은, 지금까지 알려진 방법 중, 쿼터니언이 임의의 축에 대한 회전을 표현하는 가장 간편한 방법이라는 것이다. 4개의 튜플 값 `(x,y,z,w)` 으로 정의 되며, 여기에서 `w` 요소는 회전각의 절반에 해당하는 각도의 코사인 값이다. 예를 들면 쿼터니언  `[ 0.259, 0.0, 0.0, 0.966 ]` 은 x 축에 대한 30도 회전을 의미한다. 따라서 이 쿼터니언으로부터 회전 행렬을 그림 4d와 같이 변환할 수 있다. 

<p align="center">
<img src="images/rotationMatrix.png" /><br>
<a name="rotationMatrix-png"></a>Image 4d: A rotation matrix.
</p>


- The `scale` contains the scaling factors along the x-, y-, and z-axes. The corresponding matrix can be created by using these scaling factors as the entries on the diagonal of the matrix. For example, the scale matrix for the scaling factors `[ 2.0, 1.0, 0.5 ]` is shown in Image 4e.
- `scale`에는 크기 변환 비율을 각 x-, y-, z-축에 대한 값을 갖는다. 이에 해당하는 행렬은 대각선 행렬 요소에 순서대로 들어가게 된다. 예를 들면, 크기 변환 비율 `[ 2.0, 1.0, 0.5 ]` 에 대한 변환 행렬은 그림 4e와 같다.  

<p align="center">
<img src="images/scaleMatrix.png" /><br>
<a name="scaleMatrix-png"></a>Image 4e: A scale matrix.
</p>

When computing the final, local transform matrix of the node, these matrices are multiplied together. It is important to perform the multiplication of these matrices in the right order. The local transform matrix always has to be computed as `M = T * R * S`, where `T` is the matrix for the `translation` part, `R` is the matrix for the `rotation` part, and `S` is the matrix for the `scale` part. So the pseudocode for the computation is

노드에 대한 로컬 변환 행렬을 최종적으로 계산할 때에는 이들 행렬을 모두 곱하게 된다. 이때 곱하는 순서를 바를게 하는 것이 중요하다. 로컬 변환 행렬은 항상 `M = T * R * S` 순서로 계산되어야 한다. 여기에서 `T` 는 `translation`에 대한 행렬, `R` 은 `rotation`에 대한 행렬, and `S`는 `scale`에 대한 행렬을 의미한다. 따라서, 계산은 다음과 같은 순서로 실행되어야 한다. 

```
translationMatrix = createTranslationMatrix(node.translation);
rotationMatrix = createRotationMatrix(node.rotation);
scaleMatrix = createScaleMatrix(node.scale);
localTransform = translationMatrix * rotationMatrix * scaleMatrix;
```

For the example matrices given above, the final, local transform matrix of the node will be as shown in Image 4f.

위에 주어진 행렬에 대한 최종 로컬 변환 행렬은 그림 4f와 같다. 

<p align="center">
<img src="images/productMatrix.png" /><br>
<a name="produtMatrix-png"></a>Image 4f: The final local transform matrix computed from the TRS properties. - 그림 4f: TRS 속성으로 부터 계산된 최종 로컬 변환 행렬 
</p>

This matrix will cause the vertices of the meshes to be scaled, then rotated, and then translated according to the `scale`, `rotation`, and `translation` properties that have been given in the node.

이 행렬은 메쉬를 구성하는 버텍스들이 노드내의 주어진 `scale`, `rotation`, `translation` 속성에 따라서 크기를 변환하고, 회전하고, 이동하는데 사용된다. 

When any of the three properties is not given, the identity matrix will be used. Similarly, when a node contains neither a `matrix` property nor TRS-properties, then its local transform will be the identity matrix.

만일, 이들 이 세개의 속성이 주어지지 않은 경우, 단위 행렬 (identity matrix)가 사용된다. 유사하게, 노드가 `matrix` 또는 TRS 속성 중 아무것도 갖고 있지 않으면, 로컬 변환은 단위 행렬이 된다.  

#### Global transforms of nodes - 노드의 글로벌 변환

Regardless of the representation in the JSON file, the local transform of a node can be stored as a 4&times;4 matrix. The *global* transform of a node is given by the product of all local transforms on the path from the root to the respective node:

JSON 파일내의 표현과 무관하게, 노드의 로컬 변환은 4&times;4 행렬로 저장될 수 있다. 노드의 *글로벌(global)* 변환은 루트 노드에서 해당 노드에 까지 가는 경로의 모든 노드의 로컬 변환 행렬에 대한 곱으로 계산된다. 

    Structure:           local transform      global transform
    root                 R                    R
     +- nodeA            A                    R*A
         +- nodeB        B                    R*A*B
         +- nodeC        C                    R*A*C

It is important to point out that after the file was loaded these global transforms can *not* be computed only once. Later, it will be shown how *animations* may modify the local transforms of individual nodes. And these modifications will affect the global transforms of all descendant nodes. Therefore, when the global transform of a node is required, it has to be computed directly from the current local transforms of all nodes. Alternatively, and as a potential performance improvement, an implementation could cache the global transforms, detect changes in the local transforms of ancestor nodes, and update the global transforms only when necessary. The different implementation options for this will depend on the programming language and the requirements for the client application, and thus are beyond the scope of this tutorial.

여기에서 중요한 점은, 파일이 로딩되었을 때 글로벌 변환이 한번에 계산될 수 *없다* 는 것이다. 추후에 어떻게 *애니메이션(animation)* 이 각각의 노드의 로컬 변환을 변경할 수 있는지 설명할 것이다. 따라서, 노드에 대한 글로벌 변환이 필요한 경우, 관련된 모든 노드들의 로컬 변환으로 부터 직접 계산되어야 한다. 이와 다르게, 성능 향상을 위해서, 글로벌 변환을 캐싱 할 수 있다. 이를 위해서는 상위(ancestor) 노드들의 변화를 감지하고, 필요한 경우에만 글로벌 변환을 갱신하는 방법을 사용할 수 있다. 여러가지 다른 구현 방법이 있을 수 있다. 이는 프로그래밍 언어와 클라이언트 응용에서의 요구사항에 따라 매우 다르기 때문에, 이 튜토리얼에서는 다루지 않는다. 


이전: [A Minimal glTF File](gltfTutorial_003_MinimalGltfFile.md) | [Table of Contents](README.md) | Next: [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md)
