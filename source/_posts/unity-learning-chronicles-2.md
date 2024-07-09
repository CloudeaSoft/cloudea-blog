---
title: Unity学习记录 - 2024.07.09
date: 2024-07-09 23:11:03
categories:
tags:
---

记录一下最近遇到的问题

## 1. 包版本兼容性导致的错误

在[John Lemon's Haunted Jaunt: 3D Beginner](https://learn.unity.com/project/john-lemon-s-haunted-jaunt-3d-beginner)教程中，使用到了URP与Post Processing包，通过它们来配置抗锯齿与Bloom等后期处理效果。但实际在按照教程步骤操作后，无法正常显示后期处理效果。因系统自动安装使用的包版本较新(我这边是URP14.0和PP3.4)，而[在URP8.x后，不再兼容Post Processing Stack v2 package](https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@8.0/manual/integration-with-post-processing.html#:~:text=URP%20is%20not%20compatible%20with%20the%20post%2Dprocessing%20version%202%20package.)。

本问题花了好久才排查出来，虽然是不遵守教程要求导致的，但也能算是不错的经验积累。