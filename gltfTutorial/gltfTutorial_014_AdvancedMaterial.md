이전: [Simple Texture](gltfTutorial_013_SimpleTexture.md) | [Table of Contents](README.md) | 다음: [Simple Cameras](gltfTutorial_015_SimpleCameras.md)

# An Advanced Material - 고급 재질

The [Simple Texture](gltfTutorial_013_SimpleTexture.md) example in the previous section showed a material for which the "base color" was defined using a texture. But in addition to the base color, there are other properties of a material that may be defined via textures. These properties have already been summarized in the [Materials](gltfTutorial_010_Materials.md) section:

이전 절에서 [Simple Texture](gltfTutorial_013_SimpleTexture.md) 예제에서는 "기본색 (base color)"을 텍스처를 사용하여 정의하였다. 하지만, 기본색 외에도 텍스처를 사용해서 정의할 수 있는 재질 속성들이 있다. 이들 속성에 대해서는 이미 [Materials](gltfTutorial_010_Materials.md)절에서 간단히 요약하였다. 

- The *base color*, - 기본색
- The *metallic* value, - 금속성 값
- The *roughness* of the surface, - 표면의 거칠기 
- The *emissive* properties, - 발광 속성
- An *occlusion* texture, and - 어쿨루션 텍스처 
- A *normal map*. - 법선벡터 맵


The effects of these properties cannot properly be demonstrated with trivial textures. Therefore, they will be shown here using one of the official Khronos PBR sample models, namely, the [WaterBottle](https://github.com/KhronosGroup/glTF-Sample-Models/tree/master/2.0/WaterBottle) model. Image 14a shows an overview of the textures that are involved in this model, and the final rendered object:

이들 특성의 효과는 간단한 텍스처로는 보여줄 수 없다. 그래서 여기에서는 공식 Khronos PBR 샘플 모델에 있는 예제인 [WaterBottle](https://github.com/KhronosGroup/glTF-Sample-Models/tree/master/2.0/WaterBottle) 모델이다. 그림 14a는 이 모델에 관련된 텍스처를 사용한 렌더링 결과를 보여준다. 

<p align="center">
<img src="images/materials.png" /><br>
<a name="cameras-png"></a>Image 14a: An example of a material where the surface properties are defined via textures. - 그림 14a: 텍스처로 정의된 표면 속성으로 렌더링된 재질의 예제
</p>

Explaining the implementation of physically based rendering is beyond the scope of this tutorial. The official Khronos [glTF Sample Viewer](https://github.com/KhronosGroup/glTF-Sample-Viewer) contains a reference implementation of a PBR renderer based on WebGL, and provides implementation hints and background information. The following images mainly aim at demonstrating the effects of the different material property textures, under different lighting conditions.

물리 기반 렌더링의 구현을 설명하는 것은 이 튜토리얼의 범위를 벗어난다. 크로노스 그룹의  [glTF Sample Viewer](https://github.com/KhronosGroup/glTF-Sample-Viewer)에는 WebGL에 기반한 PBR 렌더링의 참조 구현이 제공된다. 이를 사용하여 구현 방법에 아이디어를 얻거나 배경 정보를 얻을 수 있다. 다음 그림은 여러 다른 조명 조건에서 여러 재질 속성 텍스처가 어떤 효과를 만드는지를 보여주기 위해 만들어졌다. 

Image 14b shows the effect of the roughness texture: the main part of the bottle has a low roughness, causing it to appear shiny, compared to the cap, which has a rough surface structure.

이미지 14b는 텍스처 거칠기의 효과를 보여준다: 병의 주요 부분은 낮은 거칠기로 되어 있고, 반짝이는 느낌이 강하다, 대조적으로 뚜껑 부분은 거칠 표면 구조를 갖고 있다.

<p align="center">
<img src="images/advancedMaterial_roughness.png" /><br>
<a name="advancedMaterial_roughness-png"></a>Image 14b: The influence of the roughness texture. - 그림 14b - 거칠기 텍스처의 영향
</p>

Image 14c highlights the effect of the metallic texture: the bottle reflects the light from the surrounding environment map.

그림 14c 금속성 텍스처의 하일라이트 효과: 병이 주변의 환경 맵에서 오는 빛을 반사한다. 

<p align="center">
<img src="images/advancedMaterial_metallic.png" /><br>
<a name="advancedMaterial_metallic-png"></a>Image 14c: The influence of the metallic texture. - 그림 14c: 급속성 텍스처의 영향
</p>

Image 14d shows the emissive part of the texture: regardless of the dark environment setting, the text, which is contained in the emissive texture, is clearly visible.  
그림 14d는 텍스처의 발광 부분을 보여준다. 어두운 환경 설정에도 불구하고, 발광 텍스처를 갖고 있는 문자 부분은 명확하게 볼 수 있다. 



<p align="center">
<img src="images/advancedMaterial_emissive.png" /><br>
<a name="advancedMaterial_emissive-png"></a>Image 14d: The emissive part of the texture. - 그림 14d : 텍스처의 발광 부분
</p>

Image 14e shows the part of the bottle cap for which a normal map is defined: the text appears to be embossed into the cap. This makes it possible to model finer geometric details on the surface, even though the model itself only has a very coarse geometric resolution.

그림 14e는 병 뚜껑 부분에 법선(normal) 매핑이 정의된 결과를 보여준다. 뚜껑의 글자 부분이 볼록하게 나타난 것과 같은 렌더링되었다. 모델 자체의 기하 해상도는 매우 낮더라도 표면에 모델의 기하 모델의 정밀한 조정할 수 있도록 해 준다.

<p align="center">
<img src="images/advancedMaterial_normal.png" /><br>
<a name="advancedMaterial_normal-png"></a>Image 14e: The effect of a normal map. - 그림 14e: 법선 매핑의 효과 
</p>

Together, these textures and maps allow modeling a wide range of real-world materials. Thanks to the common underlying PBR model - namely, the metallic-roughness model - the objects can be rendered consistently by different renderer implementations.

이들 텍스처와 맵이 함께 실제 세계의 재질 모델링을 폭넓게 가능하게 해 준다. 이러한 금속성-거칠기 모델로 명명된 PBR 모델로 인해 불체들은 서로 다른 렌더러의 구현에서도 일관성있는 렌더링을 가능하게 해 준다.  



이전: [Simple Texture](gltfTutorial_013_SimpleTexture.md) | [Table of Contents](README.md) | 다음: [Simple Cameras](gltfTutorial_015_SimpleCameras.md)
