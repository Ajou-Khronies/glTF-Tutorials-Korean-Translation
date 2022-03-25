Previous: [Materials](gltfTutorial_010_Materials.md) | [Table of Contents](README.md) | Next: [Textures, Images, Samplers](gltfTutorial_012_TexturesImagesSamplers.md)

# A Simple Material - 간단한 재질

The examples of glTF assets that have been given in the previous sections contained a basic scene structure and simple geometric objects. But they did not contain information about the appearance of the objects. When no such information is given, viewers are encouraged to render the objects with a "default" material. And as shown in the screenshot of the [minimal glTF file](gltfTutorial_003_MinimalGltfFile.md), depending on the light conditions in the scene, this default material causes the object to be rendered with a uniformly white or light gray color.

이전 절에서 보았던 glTF 자산 예제에는 기초적인 장면 구조와 간단한 기하 형상 객체가 들어 있었다. 하지만, 객체의 외양에 대한 정보를 갖고 있지 않았다. 이러한 정보가 주어지지 않으면, 뷰어 프로그램은 "디폴트(default)" 재질로 렌더링 하는 것이 권장된다. [minimal glTF file](gltfTutorial_003_MinimalGltfFile.md)에서의 렌더링 결과 이미지에서 보는 것과 같이, 장면의 조명 조건에 맞추어, 디폴트 재질로 렌더링 하면 균일하게 밝은 회색으로 렌더링 된다. 

This section will start with an example of a very simple material and explain the effect of the different material properties.

이 절에서는 가장 간단한 재질 설정 예제로 재질 속성이 어떤 영향을 나타내는지를 보여줄 것이다. 

This is a minimal glTF asset with a simple material:    
다음은 간단한 재질을 표현하기위한 최소한의 glTF 자산이다. 

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
      "mesh" : 0
    }
  ],

  "meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1
        },
        "indices" : 0,
        "material" : 0
      } ]
    }
  ],

  "buffers" : [
    {
      "uri" : "data:application/octet-stream;base64,AAABAAIAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAACAPwAAAAA=",
      "byteLength" : 44
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
    }
  ],

  "materials" : [
    {
      "pbrMetallicRoughness": {
        "baseColorFactor": [ 1.000, 0.766, 0.336, 1.0 ],
        "metallicFactor": 0.5,
        "roughnessFactor": 0.1
      }
    }
  ],
  "asset" : {
    "version" : "2.0"
  }
}
```      

When rendered, this asset will show the triangle with a new material, as shown in Image 11a.   
위 재질로 렌더링 한 결과는 그림 11a와 같이 삼각형이 새로운 재질로 렌더링 되었다. 


<p align="center">
<img src="images/simpleMaterial.png" /><br>
<a name="simpleMaterial-png"></a>Image 11a: A triangle with a simple material.
</p>


## Material definition - 재질 정의


A new top-level array has been added to the glTF JSON to define this material: The `materials` array contains a single element that defines the material and its properties:

glTF JSON 파일에 최상위 노드의 배열로 재질을 정의한다. `materials` 배열에는 재질과 속성을 정의하는 단일 요소가 들어 있다. 

```javascript
  "materials" : [
    {
      "pbrMetallicRoughness": {
        "baseColorFactor": [ 1.000, 0.766, 0.336, 1.0 ],
        "metallicFactor": 0.5,
        "roughnessFactor": 0.1
      }
    }
  ],
```

The actual definition of the [`material`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-material) here only consists of the [`pbrMetallicRoughness`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-material-pbrmetallicroughness) object, which defines the basic properties of a material in the *metallic-roughness-model*. (All other material properties will therefore have default values, which will be explained later.) The `baseColorFactor` contains the red, green, blue, and alpha components of the main color of the material - here, a bright orange color. The `metallicFactor` of 0.5 indicates that the material should have reflection characteristics between that of a metal and a non-metal material. The `roughnessFactor` causes the material to not be perfectly mirror-like, but instead scatter the reflected light a bit.

실제 [`material`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-material)의 정의에 대해서는 표준 문서를 참조하기 바란다. 여기에서는 단순히 [`pbrMetallicRoughness`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-material-pbrmetallicroughness) 객체만을 사용하였고, *metallic-roughness-model* 의 속성들만 정의 하였다. (정의된 것을 제외한 다른 나머지 속성들은 따라서 디폴트 값으로 지정된다. 이에 대해서는 다음에 설명한다.) `baseColorFactor`는 red, green, blue, alpha 구성요소로 재질의 주요 색상을 표현한다. 이 예제에서는 밝은 오렌지 색상이다. `metallicFactor`가 0.5 가 의미하는 것은 재질이 반사 특성이 금속성과 비금속성 특성의 중간쯤 된다는 것을 의미한다. `roughnessFactor`가 0.1이 의미하는 것은 완전히 거울 같은 재질이 아니고, 반사광을 상당부분 산란 시킨다는 것을 의미한다. 

## Assigning the material to objects - 물체에 재질을 지정하는 방법

The material is assigned to the triangle, namely to the `mesh.primitive`, by referring to the material using its index:    
이 재질을 삼각형, 즉 `mesh.primitive`에 지정하기 위해서는,  `"material"` 에 재질 인덱스를 넣어 원하는 재질을 참조한다. 

```javascript
  "meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1
        },
        "indices" : 0,
        "material" : 0
      } ]
    }
```

The next section will give a short introduction to how textures are defined in a glTF asset. The use of textures will then allow the definition of more complex and realistic materials.

다음 절에서는 glTF 자산에서 텍스처를 정의하는지 살펴본다. 텍스처를 사용하여 좀더 복잡하고 실감나는 재질을 만들 수 있다.

이전: [Materials](gltfTutorial_010_Materials.md) | [Table of Contents](README.md) | 다음: [Textures, Images, Samplers](gltfTutorial_012_TexturesImagesSamplers.md)
