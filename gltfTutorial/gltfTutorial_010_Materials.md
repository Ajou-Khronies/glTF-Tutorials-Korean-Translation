이전: [Meshes](gltfTutorial_009_Meshes.md) | [Table of Contents](README.md) | 다음: [Simple Material](gltfTutorial_011_SimpleMaterial.md)

# Materials - 재질

## Introduction - 개요

The purpose of glTF is to define a transmission format for 3D assets. As shown in the previous sections, this includes information about the scene structure and the geometric objects that appear in the scene. But a glTF asset can also contain information about the *appearance* of the objects; that is, how these objects should be rendered on the screen.

glTF의 목적은 3D 자산에 대한 전송 포맷을 정의하는 것이다. 이전 절에서 살펴본 바와 같이, 여기에는 장면 구조와 기하 객체에 대한 정보가 포함되며, 장면에서 어떻게 나타날지를 정의한다. 하지만 glTF 자산은 물체의 *외양(appearance)* 에 대한 정보를 포함할 수 있다. 즉, 장면에서 이 물체를 어떻게 렌더링 될지 정의하게 된다.  

There are different possible representations for the properties of a material, and the *shading model* describes how these properties are processed. Simple shading models, like the [Phong](https://en.wikipedia.org/wiki/Phong_reflection_model) or [Blinn-Phong](https://en.wikipedia.org/wiki/Blinn%E2%80%93Phong_shading_model), are directly supported by common graphics APIs like OpenGL or WebGL. These shading models are built on a set of basic material properties. For example, the material properties involve information about the color of diffusely reflected light (often in the form of a texture), the color of specularly reflected light, and a shininess parameter. Many file formats contain exactly these parameters. For example, [Wavefront OBJ](https://en.wikipedia.org/wiki/Wavefront_.obj_file) files are combined with `MTL` files that contain this texture and color information. Renderers can read this information and render the objects accordingly. But in order to describe more realistic materials, more sophisticated shading and material models are required.

재질의 특성을 표현하는 여러가지 방법들이 존재하며, *쉐이딩 모델(shading model)* 은 이러한 재질 특성이 어떻게 처리되는지를 설명한다. [Phong](https://en.wikipedia.org/wiki/Phong_reflection_model) 또는 [Blinn-Phong](https://en.wikipedia.org/wiki/Blinn%E2%80%93Phong_shading_model)과 같은 간단한 쉐이딩 모델은 기존의 OpenGL 또는 WebGL 과 같은 그래픽스 API에서 지원된다. 이들 쉐이딩 모델들은 기초적인 재질 속성 세트로 구성된다. 예를 들면, 재질 특성에는 산란 반사에 대한 광원의 색상 (경우에 따라서는 텍스처 형태) 또는 전반사 광에 대한 생상, shiness 패러매터 등으로 정의한다. 많은 파일 포맷들은 이러한 패러매터를 갖고 있다. 예를 들면 [Wavefront OBJ](https://en.wikipedia.org/wiki/Wavefront_.obj_file) 파일 포맷은 `MTL` 파일에 텍스처와 색상 정보를 갖고 있다. 렌더러는 이 정보를 읽어 물체를 렌더링하게 된다. 하지만, 좀더 실감나는 재질 표현을 위해, 좀더 복잡한 쉐이딩과 재질 모델이 요구된다. 

## Physically-Based Rendering (PBR) - 물리기반 렌더링

To allow renderers to display objects with a realistic appearance under different lighting conditions, the shading model has to take the *physical* properties of the object surface into account. There are different representations of these physical material properties. One that is frequently used is the *metallic-roughness-model*. Here, the information about the object surface is encoded with three main parameters:

여러 다른 조명 조건에서 실제감 있는 외양을 렌더링하여 디스플레이하기 위해서는, 쉐이딩 모델이 물체 표면의 *물리적* 성질들을 사용해야 한다. 물리적 재질 특성을 표현하는 여러가지 방법들이 존재한다. 이들 중 널리 사용되는 방식이 *금속성-거칠기-모델 metallic-roughness-model* 이다. 여기서는 물체 표면의 정보를 다음 3개의 패러매터로 인코딩 한다. 

- The *base color*, which is the "main" color of the object surface.
- *기본 색 base color* 는 물체 표면의 "기본(main)" 색상을 정의한다.
- The *metallic* value. This is a parameter that describes how much the reflective behavior of the material resembles that of a metal.
- *금속성(metallic)* 값 패러매터는 금속 재질으 반사 특성을 정의한다. 
- The *roughness* value, indicating how rough the surface is, affecting the light scattering.
- *거칠기 (roughness)* 값은 표면의 거칠기를 정의하여, 빛이 산란하는 효과를 정의한다.

The metallic-roughness model is the representation that is used in glTF. Other material representations, like the *specular-glossiness-model*, are supported via extensions.

금속성-거칠기(metallic-roughness) 모델은 glTF에서 사용된다. *specular-glossiness-model* 와 같은 다른 재질 표현 방법은 확장판(extension)을 통해 지원된다. 

The effects of different metallic- and roughness values are illustrated in this image:

여러가지 금속성- 과 거칠기 값에 따른 효과를 렌더링한 결과는 다음 그림과 같다. 

<p align="center">
<img src="images/metallicRoughnessSpheres.png" /><br>
<a name="metallicRoughnessSpheres-png"></a>Image 10a: Spheres with different metallic- and roughness values.
</p>

The base color, metallic, and roughness properties may be given as single values and are then applied to the whole object. In order to assign different material properties to different parts of the object surface, these properties may also be given in the form of textures. This makes it possible to model a wide range of real-world materials with a realistic appearance.

기본색, 메탈릭, 거칠기 특성은 하나의 값으로 주어지고 전체 물체에 적용된다. 물체의 표면의 다른 부분에 다른 재질을 지정하기 위해서는 이 특성들이 텍스처 형태로 주어져야 한다. 이를 통해 실제 세계의 재질을 실감나는 외양으로 표현하는 것이 가능해 진다. 

Depending on the shading model, additional effects can be applied to the object surface. These are usually given as a combination of a texture and a scaling factor:    
쉐이딩 모델에 따라서, 추가적인 효과를 물체의 표면에 적용할 수 있다. 이는 텍스처와 스캐일링 팩터의 조합으로 주어진다.

- An *emissive* texture describes the parts of the object surface that emit light with a certain color.
- *emissive* 텍스처는 물체의 표면 일부가 특정한 색의 빛을 내는 효과를 만들어 준다.
- The *occlusion* texture can be used to simulate the effect of objects self-shadowing each other.
- *occlusion* 텍스처는 자체 그림자를 서로 물체에 만들어주는 효과를 시뮬레이션 해 준다. 
- The *normal map* is a texture applied to modulate the surface normal in a way that makes it possible to simulate finer geometric details without the cost of a higher mesh resolution.
- *normal map* 은 표면의 법선 벡터에 변화를 주는 텍스처로 메쉬의 해상도를 높이지 않고도 표면의 미세한 디테일을 표현하는 것을 시뮬레이션 해 줄 수 있다.  

glTF supports all of these additional properties, and defines sensible default values for the cases that these properties are omitted.   
glTF는 이들 추가 특성을 지원하고, 이 특성 값들이 생략된 경우 적용될 디폴트 값들을 정의하고 있다.  

The following sections will show how these material properties are encoded in a glTF asset, including various examples of materials:   
다음 절에서는 재질 특성이 glTF 자산에서 어떻게 인코딩되어 있는지를 설명하고, 재질에 대한 다양한 예제를 갖고 있다.

- [A Simple Material](gltfTutorial_011_SimpleMaterial.md) - 단순한 재질
- [Textures, Images, and Samplers](gltfTutorial_012_TexturesImagesSamplers.md) that serve as a basis for defining material properties - 재질 특성을 정의하는 가장 기반으로 적용
- [A Simple Texture](gltfTutorial_013_SimpleTexture.md) showing an example of how to use a texture for a material - 재질에 텍스처를 사용하는 방법 
- [An Advanced Material](gltfTutorial_014_AdvancedMaterial.md) combining multiple textures to achieve a sophisticated surface appearance for the objects - 여러개의 텍스처를 결합하여 물체의 복잡한 표면 재질을 만드는 방법 


이전: [Meshes](gltfTutorial_009_Meshes.md) | [Table of Contents](README.md) | 다음: [Simple Material](gltfTutorial_011_SimpleMaterial.md)
