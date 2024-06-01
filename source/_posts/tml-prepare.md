---
title: 关于Terraria的Mod开发
categories: 技术
tags:
  - C#
  - Terraria
  - 游戏开发
cover: "/images/cover/tmodloader.jpg"
---

最近加入了 Terraria 的[流光无际 Mod](https://github.com/Solaestas/Everglow)制作组，参与模组的代码开发。之后会记录一些开发过程中遇到的问题与解决方法。

#### 开发环境与工具

.Net 8、Visual Studio 2022、tModLoader

#### 学习 tModLoader 的基本知识

谨遵“最好的教程就是官方文档”这句话，查阅 Github 上[教学引导](https://github.com/tModLoader/tModLoader/wiki/tModLoader-guide-for-developers#i-am-new-to-modding)，发现几个可供学习的地方：基本教程、示例模组、文档。

1. [基本教程](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Modding-Guide)

开头看着挺保姆的，照着走一遍下来~~基本能搞清楚大致的内容。~~只能做出一把剑！但确实能让我们初步了解一些开发过程，后面就都是较为宽泛的内容了。

2. [Example Mod](https://github.com/tModLoader/tModLoader/tree/stable/ExampleMod) (😍 神中神 😍)

该案例 Mod 中有大量物品的代码案例，我们可以从中学到很多东西。

使用以下命令拉取代码：

```bash
git clone https://github.com/tModLoader/tModLoader.git
```

打开 ExampleMod 文件夹中的解决方案文件`ExampleMod.sln`，查看解决方案。

绝大部分我们在游戏中接触并使用过的内容（像是武器、道具、NPC、Buff、坐骑...），都放在`Content`项目文件夹下，可以先在这个文件夹下学习。

3. 查阅 [tModLoader 文档](https://docs.tmodloader.net/docs/stable/annotated.html)

内容很多很杂，但确实是开发的核心。在新手期，一般会用到以下几个类：

- [Item](https://docs.tmodloader.net/docs/stable/class_item.html) + [ModItem](https://docs.tmodloader.net/docs/stable/class_mod_item.html)
- [Player](https://docs.tmodloader.net/docs/stable/class_player.html) + [ModPlayer](https://docs.tmodloader.net/docs/stable/class_mod_player.html)
- [Projectle](https://docs.tmodloader.net/docs/stable/class_projectile.html) + [ModProjectile](https://docs.tmodloader.net/docs/stable/class_mod_projectile.html)
