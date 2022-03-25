이전: [Meshes](gltfTutorial_009_Meshes.md) | [Table of Contents](README.md) | 다음: [Simple Material](gltfTutorial_011_SimpleMaterial.md)

# Materials - 재질

## Introduction - 개요

The purpose of glTF is to define a transmission format for 3D assets. As shown in the previous sections, this includes information about the scene structure and the geometric objects that appear in the scene. But a glTF asset can also contain information about the *appearance* of the objects; that is, how these objects should be rendered on the screen.

glTF의 목적은 3D 자산에 대한 전송 포맷을 정의하는 것이다. 이전 절에서 살펴본 바와 같이, 여기에는 장면 구조와 기하 객체에 대한 정보가 포함되며, 장면에서 어떻게 나타날지를 정의한다. 하지만 glTF 자산은 물체의 *외양(appearance)* 에 대한 정보를 포함할 수 있다. 즉, 장면에서 이 물체를 어떻게 렌더링 될지 정의하게 된다.  

There are different possible representations for the properties of a material, and the *shading model* describes how these properties are processed. Simple shading models, like the [Phong](https://en.wikipedia.org/wiki/Phong_reflection_model) or [Blinn-Phong](https://en.wikipedia.org/wiki/Blinn%E2%80%93Phong_shading_model), are directly supported by common graphics APIs like OpenGL or WebGL. These shading models are built on a set of basic material properties. For example, the material properties involve information about the color of diffusely reflected light (often in the form of a texture), the color of specularly reflected light, and a shininess parameter. Many file formats contain exactly these parameters. For example, [Wavefront OBJ](https://en.wikipedia.org/wiki/Wavefront_.obj_file) files are combined with `MTL` files that contain this texture and color information. Renderers can read this information and render the objects accordingly. But in order to describe more realistic materials, more sophisticated shading and material models are required.

재질의 특성을 표현하는 여러가지 방법들이 존재하며, *쉐이딩 모델(shading model)* 은 이러한 재질 특성이 어떻게 처리되는지를 설명한다. [Phong](https://en.wikipedia.org/wiki/Phong_reflection_model) 또는 [Blinn-Phong](https://en.wikipedia.org/wiki/Blinn%E2%80%93Phong_shading_model)과 같은 간단한 쉐이딩 모델은 기존의 OpenGL 또는 WebGL 과 같은 그래픽스 API에서 지원된다. 이들 쉐이딩 모델들은 기초적인 재질 속성 세트로 구성된다. 예를 들면, 재질 특성에는 산란 반사에 대한 광원의 색상 (경우에 따라서는 텍스처 형태) 또는 전반사 광에 대한 생상, shiness 패러매터 등으로 정의한다. 많은 파일 포맷들은 이러한 패러매터를 갖고 있다. 예를 들면 [Wavefront OBJ](https://en.wikipedia.org/wiki/Wavefront_.obj_file) 파일 포맷은 `MTL` 파일에 텍스처와 색상 정보를 갖고 있다. 렌더러는 이 정보를 읽어 물체를 렌더링하게 된다. 하지만, 좀더 실감나는 재질 표현을 위해, 좀더 복잡한 쉐이딩과 재질 모델이 요구된다. 

## Physically-Based Rendering (PBR) - 물리기반 렌더링

To allow renderers to display objects with a realistic appearance under different lighting conditions, the shading model has to take the *physical* properties of the object surface into account. There are different representations of these physical material properties. One that is frequently used is the *metallic-roughness-model*. Here, the information about the object surface is encoded with three main parameters:

- The *base color*, which is the "main" color of the object surface.
- The *metallic* value. This is a parameter that describes how much the reflective behavior of the material resembles that of a metal.
- The *roughness* value, indicating how rough the surface is, affecting the light scattering.

The metallic-roughness model is the representation that is used in glTF. Other material representations, like the *specular-glossiness-model*, are supported via extensions.

The effects of different metallic- and roughness values are illustrated in this image:

<p align="center">
<img src="images/metallicRoughnessSpheres.png" /><br>
<a name="metallicRoughnessSpheres-png"></a>Image 10a: Spheres with different metallic- and roughness values.
</p>

The base color, metallic, and roughness properties may be given as single values and are then applied to the whole object. In order to assign different material properties to different parts of the object surface, these properties may also be given in the form of textures. This makes it possible to model a wide range of real-world materials with a realistic appearance.

Depending on the shading model, additional effects can be applied to the object surface. These are usually given as a combination of a texture and a scaling factor:

- An *emissive* texture describes the parts of the object surface that emit light with a certain color.
- The *occlusion* texture can be used to simulate the effect of objects self-shadowing each other.
- The *normal map* is a texture applied to modulate the surface normal in a way that makes it possible to simulate finer geometric details without the cost of a higher mesh resolution.

glTF supports all of these additional properties, and defines sensible default values for the cases that these properties are omitted.

The following sections will show how these material properties are encoded in a glTF asset, including various examples of materials:

- [A Simple Material](gltfTutorial_011_SimpleMaterial.md)
- [Textures, Images, and Samplers](gltfTutorial_012_TexturesImagesSamplers.md) that serve as a basis for defining material properties
- [A Simple Texture](gltfTutorial_013_SimpleTexture.md) showing an example of how to use a texture for a material
- [An Advanced Material](gltfTutorial_014_AdvancedMaterial.md) combining multiple textures to achieve a sophisticated surface appearance for the objects


이전: [Meshes](gltfTutorial_009_Meshes.md) | [Table of Contents](README.md) | 다음: [Simple Material](gltfTutorial_011_SimpleMaterial.md)
