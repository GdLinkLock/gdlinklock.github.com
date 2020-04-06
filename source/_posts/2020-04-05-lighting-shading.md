---
title: lighting shading
date: 2020-04-05 09:57:29
tags: graphic
---

> “Everything we know is only some kind of approximation.”     - Richard Feynman.

##### Illumination(物理学范畴)

The transport of energy from light sources to surfaces & points

- Category
  - direct  vs indirect
  - empirical vs physically based
  - local vs global
- Two components
  - light source
    - propertices
      - color
      - position
      - direction
      - shape
      - attenuation
    - light type
      - ambient light sources
      - directional light sources
      - point light sources
      - pot light rsources
      - area light sources
  - surface properties
    - color
    - position
    - orientation
    - micro-structure

#####   Lighting(物理学范畴)

The process of computing the luminous intensity  at a particular 3-D point, usually on a surface.
Interaction between materials and light sources.

Category

  - Lambert lighting
  - Phone lighting
    - empiracal ,include there components:ambient、 diffuse、specular
  - Blinn Phone

#####   Shading(图形学范畴)

The process of assigning colors to pixels. Which determines  the color of a pixel.

Category

- Flat shading

  Calculates illumination at a single point for each polygon

- Gouraud shading
    The normal vector at vertex V is calculated as the average of the surface normals for each polygon sharing that vertex

- Phone shading
    linearly interpolating the surface normal across the facet, applying the Phong lighting model at every pixel

##### 参考链接

<https://web.csl.cornell.edu/courses/cs5620/lectures/05_Illumination_and_Shading.pdf>

<http://cs.boisestate.edu/~alark/cs464/lectures/Shading.pdf>

<http://web.cse.ohio-state.edu/~shen.94/581/Site/Slides_files/illumination.pdf>