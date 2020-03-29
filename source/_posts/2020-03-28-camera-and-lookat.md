---
title: camera-and-lookat
date: 2020-03-28 11:14:07
tags:
typora-copy-images-to: 2020-03-28-camera-and-lookat
typora-root-url: 2020-03-28-camera-and-lookat
---

> 约定
>
> - 右手坐标系
> - 列向量

### 如何定位相机

定位摄像机需要3个向量

- 相机位置 pos
- 观察位置 target
- 相机上向量 up

![1585404714096](/1585404714096.png)

### 定义FPS相机

Position、Target、Up、Pitch、Yaw

因为不需要翻滚，所以这里没有定义Roll

### 如何移动相机

```c++
vec3 Front=Target-Position;	 	//计算相机前进方向
```

相机前进和后退：

```c++
Position += Front;			 //相机前进
Position -=	Front；			//相机后退
```

相机左右移动：

```c++
vec Right = cross(Front,v3c(0,1,0));	 //重新计算Right，因为Front会因为旋转而改变
Position += Right;			//相机右移
Position -= Right；			//相机左移
```

### 如何旋转相机

![1585456963733](/1585456963733.png)

```
front.y = sin(pitch);
front.x = cos(pitch) * cos(yaw);
front.z = cos(pitch) * sin(yaw);
Front=normalize(front)

mat4 view = lookat(Position,Position+Front,Up);
```

### 推导LookAt

首先，明确两个matrix

- camera transform matrix将camera当做世界中的一个物体来看，用于定位相机位置和朝向；记为 M<sub>tm </sub>
- view matrix，用于将world space转换到view space，记为 M<sub>view</sub>

其中：

 M<sub>tm</sub>=M<sub>T</sub> * M<sub>R</sub>

相机作为世界中的一个物体，在确认位置和朝向时；操作的顺序是先旋转再平移；因为我们使用的列向量，所以上述记法M<sub>R</sub>在右侧。也就是说，从右到左进行计算。

下图(px,py,pz)表示相机位置，r1-r9表示3x3的旋转矩阵。

![1585443958345](/1585443958345.png)

M<sub>view</sub>=M<sub>tm</sub><sup>-1</sup>=M<sub>R</sub><sup>-1</sup> * M<sub>T</sub><sup>-1</sup>=M<sub>R</sub><sup>T</sup> * M<sub>T</sub><sup>-1</sup>

因为旋转矩阵是正交矩阵，正交矩阵的逆等于其转置，因此

M<sub>view</sub>=M<sub>R</sub><sup>T</sup> * M<sub>T</sub><sup>-1</sup>

![1585444860584](/1585444860584.png)



![1585405367107](/1585405367107.png)

下面我们计算right、up、forward三个向量。

```c++
vec3 worldup=(0.f,1.f,0.f);

vec3 forward = normalize(pos-target);			//zaxis,注意forward指向z正向
vec3 right = normalize(cross(worldup,forward));		//xaxis
vec3 up = normalize(cross(forward,right));		//yaxis
```

由上我们求得了3个正交基向量，代入上面的公式，可得到view matrix

```c++
|	right.x		right.y		right.z		-dot(right,pos)		|
|	up.x		up.y		up.z		-dot(up,pos)		|
|	forward.x	forward.y	forward.z	-dot(forward,pos)	|
|	0		0		0		1			|
```

实现LookAtRH

```c++
mat4 LookAtRH(vec3 position,vec3 target,vec3 wolrdup)
{
    vec3 forward = normalize(position-target);
    vec3 right 	 = normalize(cross(nomalize(worldup),forward));
    vec3 up	 = normalize(forward,right);
    
    mat4 view(1.0f);
    view[0][0] = right.x;	view[1][0] = right.y;	view[2][0] = right.z;	view[3][0] = -dot(right, position);
    view[0][1] = up.x;		view[1][1] = up.y;	view[2][1] = up.z;	view[3][1] = -dot(up, position);
    view[0][2] = forward.x;	view[1][2] = forward.y;	view[2][2] = forward.z;	view[3][2] = -dot(forward, position);
    view[0][3] = 0;		view[1][3] = 0;		view[2][3] = 0; 	view[3][3] = 1;

    return view;
}
```
