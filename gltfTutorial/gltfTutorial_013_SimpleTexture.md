이전: [Textures, Images, and Samplers](gltfTutorial_012_TexturesImagesSamplers.md) | [Table of Contents](README.md) | 다음: [Advanced Material](gltfTutorial_014_AdvancedMaterial.md)

# A Simple Texture - 간단한 텍스처

As shown in the previous sections, the material definition in a glTF asset contains different parameters for the color of the material or the overall appearance of the material under the influence of light. These properties may be given via single values, for example, defining the color or the roughness of the object as a whole. Alternatively, these values may be provided via textures that are mapped on the object surface. The following is a glTF asset that defines a material with a simple, single texture:

이전 절에서 살펴본바와 같이, glTF 자산의 재질 정의에는 재질의 색상을 위한 여러 파라메터들 또는 조명의 영향을 받는 전체 외양에 대한 파라메터들을 갖고 있다. 이들 특성은 하나의 값으로 주어진다. 예를 들면, 물체의 색상 또는 거칠기를 정의하면 물체 전체에 적용된다. 이와는 달리, 물체의 표면에 매핑된 텍스처로 이러한 값들을 지정할 수 있다. 다음 glTF 자산 예제에서는 하나의 재질을 하나의 간단한 텍스처로 정의한다. 

```javascript
{
  "scene": 0,
  "scenes" : [ {
    "nodes" : [ 0 ]
  } ],
  "nodes" : [ {
    "mesh" : 0
  } ],
  "meshes" : [ {
    "primitives" : [ {
      "attributes" : {
        "POSITION" : 1,
        "TEXCOORD_0" : 2
      },
      "indices" : 0,
      "material" : 0
    } ]
  } ],

  "materials" : [ {
    "pbrMetallicRoughness" : {
      "baseColorTexture" : {
        "index" : 0
      },
      "metallicFactor" : 0.0,
      "roughnessFactor" : 1.0
    }
  } ],

  "textures" : [ {
    "sampler" : 0,
    "source" : 0
  } ],
  "images" : [ {
    "uri" : "testTexture.png"
  } ],
  "samplers" : [ {
    "magFilter" : 9729,
    "minFilter" : 9987,
    "wrapS" : 33648,
    "wrapT" : 33648
  } ],

  "buffers" : [ {
    "uri" : "data:application/gltf-buffer;base64,AAABAAIAAQADAAIAAAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAAAAAAAAAgD8AAAAAAACAPwAAgD8AAAAAAAAAAAAAgD8AAAAAAACAPwAAgD8AAAAAAAAAAAAAAAAAAAAAAACAPwAAAAAAAAAA",
    "byteLength" : 108
  } ],
  "bufferViews" : [ {
    "buffer" : 0,
    "byteOffset" : 0,
    "byteLength" : 12,
    "target" : 34963
  }, {
    "buffer" : 0,
    "byteOffset" : 12,
    "byteLength" : 96,
    "byteStride" : 12,
    "target" : 34962
  } ],
  "accessors" : [ {
    "bufferView" : 0,
    "byteOffset" : 0,
    "componentType" : 5123,
    "count" : 6,
    "type" : "SCALAR",
    "max" : [ 3 ],
    "min" : [ 0 ]
  }, {
    "bufferView" : 1,
    "byteOffset" : 0,
    "componentType" : 5126,
    "count" : 4,
    "type" : "VEC3",
    "max" : [ 1.0, 1.0, 0.0 ],
    "min" : [ 0.0, 0.0, 0.0 ]
  }, {
    "bufferView" : 1,
    "byteOffset" : 48,
    "componentType" : 5126,
    "count" : 4,
    "type" : "VEC2",
    "max" : [ 1.0, 1.0 ],
    "min" : [ 0.0, 0.0 ]
  } ],

  "asset" : {
    "version" : "2.0"
  }
}
```

The actual image that the texture consists of is stored as a PNG file called `"testTexture.png"` (see Image 15a).   
텍스처를 구성하는 실제 이미지의 PNG 파일 `"testTexture.png"`은 그림 15a와 같다.   

<p align="center">
<img src="images/testTexture.png" /><br>
<a name="testTexture-png"></a>Image 15a: The image for the simple texture example.
</p>

Bringing this all together in a renderer will result in the scene rendered in Image 15b.

이를 모두 가져와 렌더러는 장면을 렌더링한 결과는 그림 15b와 같다. 

<p align="center">
<img src="images/simpleTexture.png" /><br>
<a name="simpleTexture-png"></a>Image 15b: A simple texture on a unit square.
</p>


## The Textured Material Definition - 텍스처 재질 정의

The material definition in this example differs from the [Simple Material](gltfTutorial_011_SimpleMaterial.md) that was shown earlier. While the simple material only defined a single color for the whole object, the material definition now refers to the newly added texture:    
이 예제의 재질 정의는 앞서 보았던 [Simple Material](gltfTutorial_011_SimpleMaterial.md)과 다르다. 전체 물체에 하나의 색상으로 정의했던 것과는 달리 이 예제에서는 새롭게 추가된 첵스처로 재질을 정의한다. 

```javascript
"materials" : [ {
  "pbrMetallicRoughness" : {
    "baseColorTexture" : {
      "index" : 0
    },
    "metallicFactor" : 0.0,
    "roughnessFactor" : 1.0
  }
} ],
```

The `baseColorTexture` is the index of the texture that will be applied to the object surface. The `metallicFactor` and `roughnessFactor` are still single values. A more complex material where these properties are also given via textures will be shown in the next section.

`baseColorTexture`는 물체의 표면에 적용될 텍스처의 인덱스이다.   `metallicFactor` 와 `roughnessFactor`는 여기에서도 하나의 값으로 되어 있다. 이들 속성에도 텍스처를 정용하여 복잡한 재질을 만드는 방법은 다음 절에서 설명할 것이다.  

In order to apply a texture to a mesh primitive, there must be information about the texture coordinates that should be used for each vertex. The texture coordinates are only another attribute for the vertices defined in the `mesh.primitive`. By default, a texture will use the texture coordinates that have the attribute name `TEXCOORD_0`. If there are multiple sets of texture coordinates, the one that should be used for one particular texture may be selected by adding a `texCoord` property to the texture reference:

메쉬 프리미티브에 텍스처를 적용하기 위해서는 각 버텍스에 텍스처 좌표를 매핑하기 위한 정보가 반드시 있어야 한다. 텍스처 좌표는 버텍스의 추가 속성으로 `mesh.primitive`내에 정의되어 있어야 한다. 기본적으로 텍스처 좌표는 버텍스 속성의 이름이 `TEXCOORD_0`로 되어 있는 버텍스 속성을 사용한다. 만일 다수의 텍스처 좌표 세트가 있는 경우, `texCoord` 속성에 참조할 특정 텍스처를 선택해야 한다. 

```javascript
"baseColorTexture" : {
  "index" : 0,
  "texCoord": 2  
},
```

In this case, the texture would use the texture coordinates that are contained in the attribute called `TEXCOORD_2`.   
이 경우, 텍스처는 `TEXCOORD_2`라는 버텍스 속성에 들어 있는 텍스처 좌표를 사용한다. 


이전: [Textures, Images, and Samplers](gltfTutorial_012_TexturesImagesSamplers.md) | [Table of Contents](README.md) | 다음: [Advanced Material](gltfTutorial_014_AdvancedMaterial.md)
