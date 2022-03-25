이전: [Simple Material](gltfTutorial_011_SimpleMaterial.md) | [Table of Contents](README.md) | 다음: [Simple Texture](gltfTutorial_013_SimpleTexture.md)

# Textures, Images, and Samplers - 텍스처, 이미지, 샘플러

Textures are an important aspect of giving objects a realistic appearance. They make it possible to define the main color of the objects, as well as other characteristics that are used in the material definition in order to precisely describe what the rendered object should look like.

텍스처는 물체에 실재감있는 외양을 만드는데 중요한 역할을 한다. 텍스처로 물체의 주 색상(main color)를 정의 할 수 있을 뿐만 아니라, 재질을 정교하게 지정하여 물체의 렌더링된 외양을 만들어 줄 수 있는 다른 특성을 정의할 수 있도록 해 준다.

A glTF asset may define multiple [`texture`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-texture) objects, which can be used as the textures of geometric objects during rendering, and which can be used to encode different material properties. Depending on the graphics API, there may be many features and settings that influence the process of texture mapping. Many of these details are beyond the scope of this tutorial. There are dedicated tutorials that explain the exact meaning of all the texture mapping parameters and settings; for example, on [webglfundamentals.org](https://webglfundamentals.org/webgl/lessons/webgl-3d-textures.html),  [open.gl](https://open.gl/textures), and others. This section will only summarize how the information about textures is encoded in a glTF asset.

glTF 자산은 복수의 [`texture`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-texture) 객체를 정의할 수 있다. 이 객체는 기하 물체의 렌더링하는데 텍스처로 사용되고, 여러가지 다른 재질 속성을 인코딩하는데에 사용된다. 그래픽스 API에 따라 텍스처 매핑의 처리에 영향을 주는 다양한 기능과 설정이 많이 있을 수 있다. 이러한 상세한 내용은 이 튜토리얼의 범위에서 벗어난다. 여기에 특화된 튜토리얼들이 있어, 텍스처 매핑의 패러메터와 설정들의 많은 부분을 설명하고 있다. 예를 들면   [webglfundamentals.org](https://webglfundamentals.org/webgl/lessons/webgl-3d-textures.html),  [open.gl](https://open.gl/textures) 등이 있다. 이 절에서는 glTF 자산에서 텍스처를 인코딩하는 방법에 대한 정보를 간략히 소개한다. 

There are three top-level arrays for the definition of textures in the glTF JSON. The `textures`, `samplers`, and `images` dictionaries contain  [`texture`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-texture),  [`sampler`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#_texture_sampler), and [`image`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-image) objects, respectively. The following is an excerpt from the [Simple Texture](gltfTutorial_013_SimpleTexture.md) example, which will be presented in the next section:

glTF JSON에는 텍스처를 정의하는 3개의 최상위 레벨의 배열이 있다. `textures`, `samplers`, `images` 딕셔너리들에는 [`texture`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-texture),  [`sampler`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#_texture_sampler),  [`image`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-image) 객체를 갖고 있다. 다음은 [Simple Texture](gltfTutorial_013_SimpleTexture.md) 예제의 일부를 발췌한 것으로 다음절에서 설명된다.  

```javascript
"textures": [
  {
    "source": 0,
    "sampler": 0
  }
],
"images": [
  {
    "uri": "testTexture.png"
  }
],
"samplers": [
  {
     "magFilter": 9729,
     "minFilter": 9987,
     "wrapS": 33648,
     "wrapT": 33648
   }
],
```

The `texture` itself uses indices to refer to one `sampler` and one `image`. The most important element here is the reference to the `image`. It contains a URI that links to the actual image file that will be used for the texture. Information about how to read this image data can be found in the section about [image data in `images`](gltfTutorial_002_BasicGltfStructure.md#image-data-in-images).

`texture` 자신은 하나의 `sampler`와 하나의 `image`를 참조하는 인덱스를 사용한다. 여기에서 가장 중요한 요소는 `image`에 대한 참조이다. 여기에는 URI를 갖고 있어 실제 이미지 파일에 대한 링크를 갖고 있어, 텍스처로 사용된다. 이미지 데이터를 어떻게 읽는지에 대한 정보는  [image data in `images`](gltfTutorial_002_BasicGltfStructure.md#image-data-in-images)에서 찾을 수 있다.

The next section will show how such a texture definition may be used inside a material.    
다음 절에서는 이러한 텍스처 정의가 재질에서 어떻게 사용되는지를 보여 준다. 


이전: [Simple Material](gltfTutorial_011_SimpleMaterial.md) | [Table of Contents](README.md) | 다음: [Simple Texture](gltfTutorial_013_SimpleTexture.md)
