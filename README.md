# Vegetation-Studio-Instanced-Indirect
This includes the files needed to upgrade a vegetation shader to the instanced indirect implementation of Vegetation Studio

<b>ADDING INSTANCED INDIRECT SUPPORT TO SHADERS</b>

Vegetation Studio can use instanced indirect rendering on vegetation. This allows the vegetation instanced to be rendered direct from a compute buffer on the GPU allowing for larger batches than the 1023 max of normal rendering. In addition to this there is a final compute shader pass before rendering that does GPU fristum culling and LOD selection.

You can modify most shaders to support Vegetation Studios instanced indirect implementation with a cginc include file and 3 lines of code in the shader.  Copy the VS_Indirect.cginc file to the same folder as the shader and add the following lines to the shader. This will set up the shader to call the setup function in the include file where each instance is loading the instance info.

Adding this to a shader does not break normal use of the shader. The setup function is only called when used with the InstancedIndirect API.

The VS_indirect.cginc file is located in the Shader subfolder of VegetationStudio. If someone wants to include this file with a asset store asset they are free to do this.

Currently the instanced indirect rendering is only working on single submesh models. I am working on support for multi submesh.

<i>
#pragma instancing_options procedural:setup</br>
#pragma multi_compile GPU_FRUSTUM_ON __</br>
#include "VS_indirect.cginc"</br></i>
</br>

If the shader is a grass and plant shader you can change the first #pragma to this. That will add a function that scales in/out the grass at the vegetation distance for a smoother transition.

<i>#pragma instancing_options procedural:setupScale</i>
</br>

If you are updating Speedtree shaders or other shaders that already has the instancing_options pragma add the procedural:setup to the existing #pragma

<i>#pragma instancing_options assumeuniformscaling lodfade maxcount:50 procedural:setup</i>
</br>
<b>AMPLIFY SHADER EDITOR</b>

If you are using Amplify Shader Editor to make the shader you can add the same settings there. Copy the VS_indirect.cginc to the same folder as the shader and set the include and pragmas in the shader editor like this.

<p align="center">
  <img src="https://www.awesometech.no/wp-content/uploads/2017/11/Image-257.png" width="350"/>
</p>
