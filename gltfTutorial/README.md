# glTF Tutorial

작성 Marco Hutter, [@javagl](https://github.com/javagl)

번역 Hwanyong Lee, [@hwan-ajou](https://github.com/Ajou-Khronies/) - 아주대학교 크로니스 동아리

This tutorial gives an introduction to [glTF](https://www.khronos.org/gltf), the GL transmission format. It summarizes the most important features and application cases of glTF, and describes the structure of the files that are related to glTF. It explains how glTF assets may be read, processed, and used to display 3D graphics efficiently.

이 튜토리얼은 [glTF](https://www.khronos.org/gltf) (GL Transmission Format)를 소개하기 위한 것으로, glTF의 가장 주요한 기능들과 응용 사례를 요약하였고, glTF와 관련된 파일들의 구조를 설명하고 있습니다. 또한, glTF 자산이 읽혀지고, 처리되고, 3D 그래픽스에 효과적으로 디스플레이되기 위한 방법을 설명하고 있다. 

Some basic knowledge about [JSON](https://json.org/), the JavaScript Object Notation, is assumed. Additionally, a basic understanding of common graphics APIs, like OpenGL or WebGL, is required. This tutorial focuses on [glTF version 2.0](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html), where support for *Physically Based Rendering* was introduced, but the other concepts that are explained here are similar to how they had been implemented in [glTF version 1.0](https://github.com/KhronosGroup/glTF/tree/main/specification/1.0). 

[JSON](https://json.org/) (JavaScript Object Notation)에 대해 기본 지식을 갖고 있다고 가정하고 있습니다. 추가적으로, WebGL, OpenGL과 같은 일번적인 그래픽스 API에 대한 기초 이해가 필요합니다. 이 튜토리얼은 [glTF version 2.0](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html)에 초점이 맞추어져 있으며, glTF 2.0에서는 *물리 기반 렌더링 (Physically Based Rendering, PBR)* 지원 합니다. 물리기반 렌더링 외에 다른 개념은 [glTF version 1.0](https://github.com/KhronosGroup/glTF/tree/main/specification/1.0) 과 비슷하다. 


- [Introduction](gltfTutorial_001_Introduction.md)
- [Basic glTF Structure](gltfTutorial_002_BasicGltfStructure.md)
- [Example: A Minimal glTF File](gltfTutorial_003_MinimalGltfFile.md)
- [Scenes and Nodes](gltfTutorial_004_ScenesNodes.md)
- [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md)
- [Example: A Simple Animation](gltfTutorial_006_SimpleAnimation.md)
- [Animations](gltfTutorial_007_Animations.md)
- [Example: Simple Meshes](gltfTutorial_008_SimpleMeshes.md)
- [Meshes](gltfTutorial_009_Meshes.md)
- [Materials](gltfTutorial_010_Materials.md)
- [Example: A Simple Material](gltfTutorial_011_SimpleMaterial.md)
- [Textures, Images, and Samplers](gltfTutorial_012_TexturesImagesSamplers.md)
- [Example: A Simple Texture](gltfTutorial_013_SimpleTexture.md)
- [Example: An Advanced Material](gltfTutorial_014_AdvancedMaterial.md)
- [Example: Simple Cameras](gltfTutorial_015_SimpleCameras.md)
- [Cameras](gltfTutorial_016_Cameras.md)
- [Example: A Simple Morph Target](gltfTutorial_017_SimpleMorphTarget.md)
- [Morph Targets](gltfTutorial_018_MorphTargets.md)
- [Example: Simple Skin](gltfTutorial_019_SimpleSkin.md)
- [Skins](gltfTutorial_020_Skins.md)


**Acknowledgements:**

- Patrick Cozzi, Cesium, [@pjcozzi](https://twitter.com/pjcozzi)
- Alexey Knyazev, [@lexaknyazev](https://github.com/lexaknyazev)
- Sarah Chow, [@slchow](https://github.com/slchow)
