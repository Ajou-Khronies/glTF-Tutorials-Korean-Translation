Previous: [Introduction](gltfTutorial_001_Introduction.md) | [Table of Contents](README.md) | Next: [A Minimal glTF File](gltfTutorial_003_MinimalGltfFile.md)


# The Basic Structure of glTF - glTF의 기본 구조

The core of glTF is a JSON file. This file describes the whole contents of the 3D scene. It consists of a description of the scene structure itself, which is given by a hierarchy of nodes that define a scene graph. The 3D objects that appear in the scene are defined using meshes that are attached to the nodes. Materials define the appearance of the objects. Animations describe how the 3D objects are transformed (e.g., rotated or translated) over time, and skins define how the geometry of the objects is deformed based on a skeleton pose. Cameras describe the view configuration for the renderer.

glTF의 핵심은 JSON 파일입니다. 이 파일은 3D 장면의 전체 컨텐츠를 기술합니다. 이 파일은 장면 그래프를 정의하는 노드의 계층 구조로 되어 있는 장면 구조 자체를 기술합니다. 3D 물체(Object)는 장면내에 노드로 첨부된 메쉬(Mesh)로 정의합니다. 재질(materials)은 물체의 외양을 정의하고, 애니메이션(Animation)은 3D 물체가 시간에 따라서 어떻게 변환(예, 회전, 이동)되는지를 정의하며, 스킨(skins)은 골격 자세 (Skeleton pose)에 따라서 물체가 어떻게 변형되는지를 정의합니다. 카메라(Camera)는 렌더러의 시점을 기술하게 됩니다.  

## The JSON structure - JSON 구조

The scene objects are stored in arrays in the JSON file. They can be accessed using the index of the respective object in the array:

scene 객체는 JSON 파일에 배열로 저장됩니다. 배열의 각 객체의 인덱스를 사용해서 접근할 수 있습니다.

```javascript
"meshes" : 
[
    { ... }
    { ... }
    ...
],
```

These indices are also used to define the *relationships* between the objects. The example above defines multiple meshes, and a node may refer to one of these meshes, using the mesh index, to indicate that the mesh should be attached to this node:

이들 인덱스는 객체간의 *관계 (relationship)* 를 정의하는데에도 사용됩니다. 예를 들면, 여러개의 메쉬들을 정의하는 에서 보듯이, 노드가 메쉬 인덱스를 사용해서 메쉬를 참조할 수 있고, 이를 통해 메쉬가 이 노드에 첨부될 수 있습니다.  

```javascript
"nodes": 
[
    { "mesh": 0, ... },
    { "mesh": 5, ... },
    ...
]
```

The following image (adapted from the [glTF concepts section](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#concepts)) gives an overview of the top-level elements of the JSON part of a glTF asset:

다음 이미지는 (adapted from the [glTF concepts section](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#concepts)) glTF 자산의 최상위 레벨의 요소들을 개략적으로 보여 줍니다. 

<p align="center">
<img src="images/gltfJsonStructure.png" /><br>
<a name="gltfJsonStructure-png"></a>Image 2a: The glTF JSON structure - 그림 2a: glTF JSON 구조 
</p>


These elements are summarized here quickly, to give an overview, with links to the respective sections of the glTF specification. More detailed explanations of the relationships between these elements will be given in the following sections.

이들 요소를 간단히 설명하고, glTF 표준 문서의 해당 섹션으로 링크를 추가하였습니다. 다음 절에서 상세한 내용과 요소들간의 관계에 대해서 설명할 것입니다. 

- The [`scene`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-scene) is the entry point for the description of the scene that is stored in the glTF. It refers to the `node`s that define the scene graph.
- [`scene`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-scene) 은 장면을 정의 하는 진입점으로 glTF에 저장되어 있습니다. 장면 그래프를 정의하는 `node`를 참조하게 됩니다.
- The [`node`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-node) is one node in the scene graph hierarchy. It can contain a transformation (e.g., rotation or translation), and it may refer to further (child) nodes. Additionally, it may refer to `mesh` or `camera` instances that are "attached" to the node, or to a `skin` that describes a mesh deformation.
- [`node`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-node)는 장면 그래프 계층 내에 단 하나의 노드로 존재합니다. 노드에는 변환(transformation, 예, 회전, 이동)과 (자식) 노드들을 참조할 수 있습니다. 추가적으로, `mesh` 또는 `camera` 인스턴스를 참조해서 노드에 "첨부(attached)"하거나, `skin`을 통해 메쉬의 변형을 정의할 수 있습니다. 
- The [`camera`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-camera) defines the view configuration for rendering the scene.
- [`camera`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-camera)는 장면의 렌더링을 위한 시점(view) 설정을 정의합니다.
- A [`mesh`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh) describes a geometric object that appears in the scene. It refers to `accessor` objects that are used for accessing the actual geometry data, and to `material`s that define the appearance of the object when it is rendered.
- [`mesh`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-mesh)는 장면에 나타날 기하 객체를 정의합니다. `accessor` 객체를 참조하여 실제 기하 데이터에 접근할 수 있고, `material`로 객체의 렌더링 될때의 외양을 정의할 수 있습니다. 
- The [`skin`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-skin) defines parameters that are required for vertex skinning, which allows the deformation of a mesh based on the pose of a virtual character. The values of these parameters are obtained from an `accessor`.
- [`skin`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-skin)은 버텍스 스키닝 (vertex skinning)에 필요한 패러매터를 정의하는데, 메쉬를 가상 캐릭터의 자세에 따라 변형할 수 있도록 해 줍니다. 이 패러매터의 값들은 `accessor`에서 가져오게 됩니다.
- An [`animation`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-animation) describes how transformations of certain nodes (e.g., rotation or translation) change over time.
- [`animation`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-animation)는 특정 노드에 대한 시간에 따른 변환을 정의합니다. (예: 회전, 이동)
- The [`accessor`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-accessor) is used as an abstract source of arbitrary data. It is used by the `mesh`, `skin`, and `animation`, and provides the geometry data, the skinning parameters and the time-dependent animation values. It refers to a [`bufferView`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-bufferview), which is a part of a [`buffer`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-buffer) that contains the actual raw binary data.
- [`accessor`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-accessor)는 임의(arbitrary)의 데이터 소스에 대한 추상화에 사용됩니다. `mesh`, `skin`, 과 `animation`에서 사용되어 기하 데이터, 스키닝 패러매터, 시간에 종속적인 애니메이션 값을 정의하는데 사용됩니다. `accessor`는 [`bufferView`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-bufferview)를 참조하며, `bufferView`는 실제 원시(raw) 바이너리 데이터를 갖고 있는 [`buffer`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-buffer)의 일부분입니다.
- The [`material`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-material) contains the parameters that define the appearance of an object. It usually refers to `texture` objects that will be applied to the rendered geometry.
- [`material`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-material)는 객체의 외양에 대한 정의를 하기 위한 패러매터를 갖고 있습니다.  `texture` 객체를 참조하여 형상을 렌더링하는데 적용되도록 할 수 있습니다.
- The [`texture`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-texture) is defined by a [`sampler`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-sampler) and an [`image`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-image). The `sampler` defines how the texture `image` should be placed on the object.
- [`texture`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-texture) 는 [`sampler`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-sampler)와 [`image`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-image)를 이용해서 정의됩니다. `sampler`는 텍스처 `image`가 물체에 어떻게 자리잡을 지를 정의하게 됩니다.   


## References to external data - 외부 데이터에 대한 참조

The binary data, like geometry and textures of the 3D objects, are usually not contained in the JSON file. Instead, they are stored in dedicated files, and the JSON part only contains links to these files. This allows the binary data to be stored in a form that is very compact and can efficiently be transferred over the web. Additionally, the data can be stored in a format that can be used directly in the renderer, without having to parse, decode, or preprocess the data.  

기하 형상이나 3D 오브젝트의 텍스처와 같은 바이너리 데이터는 일반적으로 JSON 파일에 포함하지 않습니다. 대신에, 독자적인 파일에 저장한 후 JSON 에서는 이들 파일에 대한 링크만을 포함하게 됩니다. 이를 통해서 바이너리 데이터가 간결하고 웹을 통해 전송될때 효율적으로 저장할 수 있습니다. 추가적으로, 데이터는 렌더러에서 직접 사용되는 포맷으로 저장될 수 있는데 이를 통해 파싱, 디코딩, 전처리가 필요없도록 할 수 있습니다. 

<p align="center">
<img src="images/gltfStructure.png" /><br>
<a name="gltfStructure-png"></a>Image 2b: The glTF structure - 그림 2b: glTF 구조
</p>

As shown in the image above, there are two types of objects that may contain such links to external resources, namely `buffers` and `images`. These objects will later be explained in more detail.

위 그림에서 보는 것과 같이, 두 유형의 객체가 외부 리소스롤 링크로 포함될 수 있습니다. 여기에는 `buffers`와 `images`가 있는데, 이들 객체는 뒤에 상세히 설명할 예정입니다. 


## Reading and managing external data - 외부 데이터의 읽기와 관리

Reading and processing a glTF asset starts with parsing the JSON structure. After the structure has been parsed, the [`buffer`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-buffer) and [`image`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-image) objects are available in the top-level `buffers` and `images` arrays, respectively. Each of these objects may refer to blocks of binary data. For further processing, this data is read into memory. Usually, the data will be be stored in an array so that they may be looked up using the same index that is used for referring to the `buffer` or `image` object that they belong to. 

glTF 자산의 읽기와 관리의 첫 단계는 JSON 구조를 파싱하는 것입니다. 구조가 파싱된 후, [`buffer`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-buffer) 와 [`image`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-image) 객체가 `buffers` 와 `images` 배열의 최상위 계층에서 사용할 수 있게 됩니다. 각각의 객체들은 바이너리 데이터의 블록으로 참조할 수 있습니다. 추가 처리를 위해서 데이터는 메모리로 들어오게 됩니다. 통상적으로, 데이터는 배열에 저장되어 `buffer` 또는  `image` 객체에 해당하는 같은 인덱스를 통해 참조할 수 있습니다. 


## Binary data in `buffers` - `buffers`내의 바이너리 데이터

A [`buffer`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-buffer) contains a URI that points to a file containing the raw, binary buffer data:

[`buffer`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-buffer)는 원시, 바이너리 데이터를 포함하고 있는 파일을 가리캐는 URI를 갖게 됩니다. 

```javascript
"buffer01": {
    "byteLength": 12352,
    "type": "arraybuffer",
    "uri": "buffer01.bin"
}
```

This binary data is just a raw block of memory that is read from the URI of the `buffer`, with no inherent meaning or structure. The [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md) section will show how this raw data is extended with information about data types and the data layout. With this information, one part of the data may, for example, be interpreted as animation data, and another part may be interpreted as geometry data. Storing the data in a binary form allows it to be transferred over the web much more efficiently than in the JSON format, and the binary data can be passed directly to the renderer without having to decode or pre-process it. 

이 바이너리 데이터는 메모리의 원시 블록으로서 `buffer`의 URI로부터 읽게 됩니다. 이때 상속 개념이나 구조에 대한 정보는 포함되지 않습니다.  [Buffers, BufferViews, and Accessors](gltfTutorial_005_BuffersBufferViewsAccessors.md) 절에서는 어떻게 원시 데이터가 자료형과 데이터 레이아웃 정보를 갖게 만드는지를 설명하고 있습니다. 이 정보로 부터, 데이터의 일부분은 예를들면, 애니메이션 데이터로, 다른 부분은 기하 데이터로 해석 될 수 있습니다. 데이터를 바이너리 형식으로 저장함으로써 JSON 포맷으로 전송하는 것에 비해 훨씬 효율적으로 전송할 수 있으며, 바이너리 데이터가 직접적으로 렌더러에 전달되므로 디코드나 전처리가 필요하지 않게 됩니다. 


## Image data in `images` - 

An [`image`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-image) may refer to an external image file that can be used as the texture of a rendered object:

[`image`](https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#reference-image)는 외부 이미지 파일을 참조할 수 있습니다. 이를 통해 렌더링 객체의 텍스처로 사용할 수 있습니다.

```javascript
"image01": {
    "uri": "image01.png"
}
```

The reference is given as a URI that usually points to a PNG or JPG file. These formats significantly reduce the size of the files so that they may efficiently be transferred over the web. In some cases, the `image` objects may not refer to an external file, but to data that is stored in a `buffer`. The details of this indirection will be explained in the [Textures, Images, and Samplers](gltfTutorial_016_TexturesImagesSamplers.md) section. 

참조는 URI로 주어지는데, 일반적으로 PNG나 JPG 파일을 가리키게 됩니다. 이들 포맷은 파일크기를 크게 줄여주는 압축 방식으로서 웹을 통해 전송할때 효율성을 제공해 주게 됩니다. 경우에 따라서는, `image` 객체가 외부 파일을 참조하지 않으나, `buffer`에 데이터가 저장되어 있을 수 있습니다. 이런 우회 방법에 대한 설명은 [Textures, Images, and Samplers](gltfTutorial_016_TexturesImagesSamplers.md) 절에서 상세하게 설명할 것입니다.


## Binary data in data URIs - 데이터 URI 내의 바이너리 데이터 

Usually, the URIs that are contained in the `buffer` and `image` objects will point to a file that contains the actual data. As an alternative, the data may be *embedded* into the JSON, in binary format, by using a [data URI](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs).

일반적으로, URI는 `buffer`와 `image` 객체를 포함하고 있고 실제 데이터를 갖고 있는 파일을 가리키게 됩니다. 이와는 다르게 데이터가 JSON 에 바이너리 포맷으로 *임베딩(embedding)* 되어 있을 수 있는데, 이때에는 [data URI](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs)를 사용합니다.

Previous: [Introduction](gltfTutorial_001_Introduction.md) | [Table of Contents](README.md) | Next: [A Minimal glTF File](gltfTutorial_003_MinimalGltfFile.md)
