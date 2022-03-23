[Table of Contents](README.md) | Next: [Basic glTF Structure](gltfTutorial_002_BasicGltfStructure.md)





# Introduction to glTF using WebGL - WebGL을 이용하는 glTF 소개

An increasing number of applications and services are based on 3D content. Online shops offer product configurators with a 3D preview. Museums digitize their artifacts with 3D scans and allow visitors to explore their collections in virtual galleries. City planners use 3D city models for planning and information visualization. Educators create interactive, animated 3D models of the human body. Many of these applications run directly in the web browser, which is possible because all modern browsers support efficient rendering with WebGL.

3D 컨텐츠에 기반한 응용과 서비스가 크게 증가하고 있다. 온라인 상점에서는 3D 미리보기 기능을 이용해 상품에 대한 설정을 변경해 볼 수 있다. 박물관에서는 유물을 3D 스캔하여 방문자들이 가상 갤러리에서 관람할 수 있도록 해 준다. 교사들은 대화형, 애니메이션이 들어 있는 인체의 3D 모델을 사용하여 교육한다. 이들 응용중 상당 수는 웹 브라우저에서 바로 실행할 수 있다. 이것이 가능한 이유는 최신 웹 브라우저들이 WebGL을 사용해 효과적으로 렌더링을 할 수 있기 때문이다. 

<p align="center">
<img src="images/applications.png" /><br>
<a name="applications-png"></a>Image 1a: Screenshots of various websites and applications showing 3D models. - 그림 1a: 3D 모델을 보여주는 다양한 웹사이트와 응용의 스크린 샷
</p>

Demand for 3D content in various applications is constantly increasing. In many cases, the 3D content has to be transferred over the web, and it has to be rendered efficiently on the client side. But until now, there has been a gap between the 3D content creation and efficient rendering of that 3D content in the runtime applications.

다양한 응용에서 3D 컨텐츠에 대한 수요는 꾸준히 증가해 왔다. 많은 경우, 3D 컨텐츠는 웹 환경으로 전환되었고, 클라이언트 측에서 효과적으로 렌더링되고 있습니다. 하지만, 현재까지도 3D 컨텐츠 제작과 런타임 응용에서의 3D 컨텐츠의 효육적인 렌더링간에는 간격이 있다.  

## 3D content pipelines - 3D 컨텐츠 파이프라인

3D content that is rendered in client applications comes from different sources and is stored in different file formats. The [list of 3D graphics file formats on Wikipedia](https://en.wikipedia.org/wiki/List_of_file_formats#3D_graphics) shows an overwhelming number, with more than 70 different file formats for 3D data, serving different purposes and application cases.  

3D 컨텐츠는 여러가지 다른 소스로 부터 만들어져, 여러가지 다른 파일 포맷으로 저장된 후 클라이언트 응용에서 렌더링된다. [list of 3D graphics file formats on Wikipedia](https://en.wikipedia.org/wiki/List_of_file_formats#3D_graphics) 에서 보는 바와 같이 그 종류가 매우 많다. 70개가 넘는 다양한 3D 데이터 파일 포맷이 목적과 응용 사례에 따라 다르게 사용되고 있다. 

For example, raw 3D data may be obtained with a 3D scanner. These scanners usually provide the geometry data of a single object, which is stored in [OBJ](https://en.wikipedia.org/wiki/Wavefront_.obj_file), [PLY](https://en.wikipedia.org/wiki/PLY_(file_format)), or [STL](https://en.wikipedia.org/wiki/STL_(file_format)) files. These file formats do not contain information about the scene structure or how the objects should be rendered.

예를 들면, raw 3D 데이터가 3D 스케너에 의해서 얻어지는 경우, 스캐너들은 단일 물체에 대한 기하 형상 데이터를 제공하는데 [OBJ](https://en.wikipedia.org/wiki/Wavefront_.obj_file), [PLY](https://en.wikipedia.org/wiki/PLY_(file_format)), 또는 [STL](https://en.wikipedia.org/wiki/STL_(file_format)) 파일 포맷을 주로 사용합니다. 이들 포맷은 장면(scene) 구조나, 물체가 어떻게 렌더링 되어야 하는지에 대한 정보를 포함하고 있지 않다. 

More sophisticated 3D scenes can be created with authoring tools. These tools allow one to edit the structure of the scene, the light setup, cameras, animations, and, of course, the 3D geometry of the objects that appear in the scene. Applications store this information in their own, custom file formats. For example, [Blender](https://www.blender.org/) stores the scenes in `.blend` files, [LightWave3D](https://www.lightwave3d.com/) uses the `.lws` file format, [3ds Max](https://www.autodesk.com/3dsmax) uses the `.max` file format, and [Maya](https://www.autodesk.com/maya) uses `.ma` files.

좀 더 복잡한 3D 장면은 저작 도구를 통해 제작될 수 있다. 이들 도구들은 장면의 구조를 편집할 수 있고, 조명과 카메라, 애니메이션, 장면에서 물체의 3D 형상에 대한 정보를 편집할 수 있습니다. 예를 들면 [Blender](https://www.blender.org/)는 장면을 `.blend` 파일로 저장하고, [LightWave3D](https://www.lightwave3d.com/)는 `.lws` 파일 포맷으로 저장하고, [3ds Max](https://www.autodesk.com/3dsmax)는 `.max` 파일로, [Maya](https://www.autodesk.com/maya)는 `.ma` 파일로 저장한다.

In order to render such 3D content, the runtime application must be able to read different input file formats. The scene structure has to be parsed, and the 3D geometry data has to be converted into the format required by the graphics API. The 3D data has to be transferred to the graphics card memory, and then the rendering process can be described with sequences of graphics API calls. Thus, each runtime application has to create importers, loaders, or converters for all file formats that it will support, as shown in [Image 1b](#contentPipeline-png).

이와 같은 3D 컨텐츠를 렌러링하기 위해서는, 런타임 응용은 여러개의 서로 다른 입력 파일 포맷을 읽을 수 있어야 한다. 장면 구조를 파싱하고, 3D 형상 데이터는 그래픽스 API가 요구하는 형태로 변경되어야 합니다. 이들 3D 데이터는 그래픽스 카드의 메모리로 전송되어야 하고, 일련의 그래픽스 API 명령으로 정의된 렌더링 처리를 수행하여야 한다. 따라서, 각각의 런타임 응용은 지원하는 파일 포맷에 대한 가져오기 기능, 로더 또는 변환기를 갖고 있어야 한다. [그림 1b](#contentPipeline-png)

<p align="center">
<img src="images/contentPipeline.png" /><br>
<a name="contentPipeline-png"></a>Image 1b: The 3D content pipeline today - 현재의 3D 컨텐츠 파이프라인.
</p>


## glTF: A transmission format for 3D scenes - 3D 장면을 위한 전송 포맷

The goal of glTF is to define a standard for representing 3D content, in a form that is suitable for use in runtime applications. The existing file formats are not appropriate for this use case: some of do not contain any scene information, but only geometry data; others have been designed for exchanging data between authoring applications, and their main goal is to retain as much information about the 3D scene as possible, resulting in files that are usually large, complex, and hard to parse. Additionally, the geometry data may have to be preprocessed so that it can be rendered with the client application.

glTF의 목표는 3D 컨텐츠를 표현하는 표준을 정의하는 것으로, 런타임 응용에 사용하기 적합한 형식으로 만드는 것입니다. 기존의 파일 포맷은 다음과 같은 이유로 적합하지 않다. 장면정보는 없이 기하 형상 데이터만 갖고 있는 경우가 있고, 여러 저작도구에서 데이터를 상호 교환하기 위한 목적으로 만들어진 포맷의 경우에는 3D 장면에 대한 정보를 유지하는 것이 가장 주요한 목적이기 때문에 일반적으로 파일 포맷이 너무 크고, 복잡하고 파싱이 어려운 경우가 많다. 또한, 클라이언트 응용에서 렌더링하기 위해서는 기하 데이터에 대한 전처리가 필요한 경우가 있다.  

None of the existing file formats were designed for the use case of efficiently transferring 3D scenes over the web and rendering them as efficiently as possible. But glTF is not "yet another file format." It is the definition of a *transmission* format for 3D scenes:

기존의 파일 포맷은 3D 장면을 웹을 통해 전송하고 최대한 효율적으로 렌더링하는데 적합하게 설계되지 않았습니다. 하지만 glTF "비슷한 또 하나의 파일 포맷"이 아니라, 3D 장면에 대한 *전송 (Transmission)* 포맷에 대한 정의이다. 

- The scene structure is described with JSON, which is very compact and can easily be parsed.
- The 3D data of the objects are stored in a form that can be directly used by the common graphics APIs, so there is no overhead for decoding or pre-processing the 3D data.

- 장면 구조는 JSON으로 정의하여 간결하고 쉽게 파싱할 수 있다. 
- 물체의 3D 데이터는 그래픽스 API들에서 직접 사용될 수 있는 형태로 저장합니다. 따라서 3D 데이터에 대한 전처리가 필요하지 않다. 

Different content creation tools may now provide 3D content in the glTF format. And an increasing number of client applications are able to consume and render glTF. Some of these applications are shown in [Image 1a](#applications-png). So glTF may help to bridge the gap between content creation and rendering, as shown in [Image 1c](#contentPipelineWithGltf-png).

여러 컨텐츠 저작 도구들이 glTF 포맷의 3D 컨텐츠를 지원하고 있다. 그리고 클라이언트 응용이 glTF 파일을 읽고 렌더링하는 경우가 증가하고 있다 이들 응용중 일부를 [그림 1a](#applications-png)에 표시하였다. 결론적으로 glTF 포맷은 컨텐츠 저작과 렌더링 사이의 간격을 메워줄 수 있다.  [그림 1c](#contentPipelineWithGltf-png)

<p align="center">
<img src="images/contentPipelineWithGltf.png" /><br>
<a name="contentPipelineWithGltf-png"></a>Image 1c: The 3D content pipeline with glTF. - 그림 1c: glTF로 구성된 3D 컨텐츠 파이프라인  
</p>

An increasing number of content creation tools provide glTF import and export directly. For example, the Blender Manual documents [how to import and export PBR materials](https://docs.blender.org/manual/en/latest/addons/import_export/scene_gltf2.html) using glTF.  Alternatively, other file formats can be used to create glTF assets, using one of the open-source conversion utilities listed in the [glTF Project Explorer](https://github.khronos.org/glTF-Project-Explorer/). The output of converters and exporters can be validated using the [Khronos glTF Validator](https://github.khronos.org/glTF-Validator/).

컨텐츠 저작도구 중 glTF의 가져오기 내보내기 기능을 지원하는 경우가 크게 증가하고 있다. 예를 들면 Blender 메뉴얼 문서 [how to import and export PBR materials](https://docs.blender.org/manual/en/latest/addons/import_export/scene_gltf2.html)를 보면 glTF를 활용하는 방법이 있다. 이와는 대조적으로 다른 파일 포맷을 이용해 glTF 자산을 만들 수 있다. [glTF Project Explorer](https://github.khronos.org/glTF-Project-Explorer/)에서 다양한 오픈 소스 변환 유틸리티들을 찾을 수 있다. 내부내기 기능과 변환기의 출력 결과를 검증하기 위해서는 [Khronos glTF Validator](https://github.khronos.org/glTF-Validator/)를 사용할 수 있다.

[Table of Contents](README.md) | Next: [Basic glTF Structure](gltfTutorial_002_BasicGltfStructure.md)
