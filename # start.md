# Minecraft addons 从入门到精通
大部分抄袭自minecraft wiki:)
此教程包含如何在Minecraft中创建新的客户端实体定义（New client entity definitions）的流程。

## 概要
架构应遵循当前的Minecraft JSON范例：
* 字段应为小写并使用下划线（无空格）
* 字段值应有所指向（浮点数应该只是浮点数，而不是字符串）
* “实体”中的所有JSON文件。目录是实体在世界中加载所必需的

对于十月技术预览，您需要启用“使用实验游戏”。世界上的选项在资源包中使用此功能。
## 开始
我姑且认定你的addons水平在1.2版本，并且熟悉json语法;

这个教程会让一切都变得简单，相信我XD(笔者在写教程时还未看过文档)

编辑器选择：

我建议你使用
[Atom](www.atom.io) ; [SublimeText](www.sublimetext.com)

语法高亮：[Addon-Syntax-Highlighter](https://github.com/jocopa3/Minecraft-Addon-Syntax-Highlighter.git)

## 新实体创建
首先，创建一个新文件夹并将其命名为“entity”。在资源包的根目录内，在实体文件夹中创建一个JSON文件并为其命名。JSON文件需要格式版本和minecraft:client_entity信息。

对于10月技术预览版，格式版本需要为1.8.0.-beta.1。 这允许在试用游戏时使用客户端实体定义JSON。

下面是猪的客户端实体定义JSON。
```
{   
  "format_version": "1.8.0-beta.1",   
  "minecraft:client_entity": {
    "description": {
      "identifier": "minecraft:pig",
      "materials": { 
        "default": "pig"
      },
      "textures": {
        "default": "textures/entity/pig/pig",
        "saddled": "textures/entity/pig/pig_saddle"
      },
      "geometry": {
        "default": "geometry.pig"
      },
      "animations": {
        "setup": "animation.pig.setup",
        "walk": "animation.quadruped.walk",
        "look_at_target": "animation.common.look_at_target",
        "baby_transform": "animation.pig.baby_transform"
      },
      "animation_controllers": [
        {
          "setup": "controller.animation.pig.setup" 
        },
        {
          "move": "controller.animation.pig.move"
        },
        {
          "baby": "controller.animation.pig.baby"
        }
      ],
      "render_controllers": [ "controller.render.pig" ],
      "locators": {
        "lead": { 
          "head": [ 0.0, 14.0, -6.0 ]
        }
      },
      "spawn_egg": {
        "texture": "spawn_egg",
        "texture_index": 2
      }
    }
  }
}
```
我们来解释一下每个对象的含义：
### min_engine_version(format_version)
如果存在，玩家可以设置允许解析JSON所需的最小版本。将定义中的版本与构建顶级资源包的引擎版本进行比较。如果定义的min_engine_version比该包的引擎版本新，则不解析该定义。

### minecraft:client_entity
包含实体的描述。在描述中，您可以设置有关实体的许多内容。通常，此文件定义了生物需要的资源，并为每个资源提供友好名称，使其他定义文件可以使用。

多个定义文件可以使用相同的标识符，在这种情况下，只会加载其中一个定义。与顶级资源包的引擎版本相比，解析具有相同或接近且不大于min_engine_version的定义; 将不会解析具有相同标识符的所有其他定义。

当用旧资源包覆盖新资源包时，这对于继续支持较旧版本的实体非常有用，同时在所有其他情况下也支持较新版本的实体。
### Identifier
Identifier用于向服务器注册实体。在客户端实体定义JSON中，标识设置实体的外观（Materials, textures,  geometry etc.）。JSON中的Behavior Pack中的标识赋予实体行为。	

### materials, textures and geometry
玩家可以在这一部分选择实体的材料、材质、模型。玩家可以设置一个或多个生物的材料、材质、模型。玩家必须设置它们的名字。这些名字会被Render Controllers JSON使用。玩家可以从原版材质中并且自定义材质。这些材质应该被放在Resource Pack文件夹中正确的位置。

### animations
玩家可以改动画名字。这些动画名字会被JSON的动画控制器使用。玩家可以通过更改Minecraft原版材质来创造自己的动画。自定义动画应该被储存在Resource Pack文件夹中的相对应位置。

### animation_controllers
动画控制器决定了动画应该在什么时候处理。每个控制器罗列一个或多个动画状态。玩家可以更改动画处理器的名字。名字应该与其他生物动画控制器的名字有所不同。玩家可更改Minecraft原版材质来创造自己的动画控制器。自定义动画控制器应该被储存在Resource Pack文件夹中的相对应位置。

### Particle
允许玩家指定一个key来引用颗子的长名称。 如果存在这些，则在生成实体时创建粒子。自定义的key应该与其他生物动画控制器的名字有所不同。玩家可更改Minecraft原版材质来创造自己的粒子。自定义粒子应该被储存在Resource Pack文件夹中的相对应位置。

### render_controllers
指定'渲染控制器'的名称。此名称需要与Render Controllers文件夹中对应的JSON的名称匹配。玩家可更改Minecraft原版材质来创造自己的'渲染控制器'。自定义'渲染控制器'应位于资源包根目录的textures文件夹中。

### locators
定位器偏移在模块空间中指定。定位器的例子有 "lead&quot；定位器用于显示指引将附加到何处。

### enable_attachables
实体是否可以装备。这允许实体渲染装甲(true)。

`"enable_attachables": true`

### spawn_egg
这设置刷怪蛋的 材质。
#### 使用十六进制值来决定底部颜色和上部颜色:
```
"spawn_egg": {
  "base_color": "#53443E",
  "overlay_color": "#2E6854"
}
```
