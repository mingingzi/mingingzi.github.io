---
published: true
layout: post
title:      "How to Make a Flow-map in Houdini"
subtitle: ' Flow the way you want!'
date: 2018-02-13T17:00:00.000Z
author: MagentaM
header-img: img/in-post/2-19-bg.jpg
tags:
  - Houdini
  - VFX
---
## General Idea

1. Prepare a mesh with uv that you want your flowmap work for, generate a default vector field.
2. Manipulate the vector field using **curves**/**obstacle**/**combs**.
3. Translate the vector field into a color map, and bake it out.

## Network Overview
![flowmap_graph](http://mingingzi.github.io/img/in-post/flowmap.jpg)

Result:
![flowmap](http://mingingzi.github.io/img/in-post/Flowmap-result.jpg)

## Notice

In subnetwork _flowmaptocolor_, the code for _ConverttoTangentSpace_ should be corrected as follow, since the worldToTangent matrix should be transposed:
~~~
matrix3 worldToTangent = 1; 

v@v = normalize(v@v);
v@tangent = cross(v@normal, v@binormal);

worldToTangent.xx = v@tangent.x; 
worldToTangent.xy = v@normal.x; 
worldToTangent.xz = v@binormal.x;  

worldToTangent.yx = v@tangent.y; 
worldToTangent.yy = v@normal.y; 
worldToTangent.yz = v@binormal.y;  

worldToTangent.zx = v@tangent.z; 
worldToTangent.zy = v@normal.z; 
worldToTangent.zz = v@binormal.z;  

v@Cd = (normalize(v@v * worldToTangent) +1) * .5;
v@Cd.y = v@Cd.z; 
v@Cd.z = 1; 
~~~

In Houdini, all vectors are treated as column vector, and are multiplied from left to right.

Traditionally, mul(A, B) = mul(transpose(B), A). However, in Houdini, v@v * worldToTangent = worldToTangent * v@v. Since v@v is treated as a column vector, it will always be the righter matrix in traditional math convention. Simply switching the order in multiplication doesn't do the transpose work.

Official documentation can be found here:http://www.sidefx.com/docs/houdini11.0/hom/hou/Matrix4