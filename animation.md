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
定位器偏移在模块空间中指定。定位器的例子有 "lead"；定位器用于显示指引将附加到何处。

### enable_attachables
实体是否可以装备。这允许实体渲染装甲(true)。

`"enable_attachables": true`

### spawn_egg
这设置刷怪蛋的 材质。
#### 使用十六进制值来决定底部颜色和上部颜色:
* base_color:底部颜色
* overlay_color:顶部颜色
```
"spawn_egg": {
  "base_color": "#53443E",
  "overlay_color": "#2E6854"
}
```

#### 指定一个材质:
一个指定材质的刷怪蛋的例子。刷怪蛋储存在材质包文件夹里的JSON，"items_texture"当这个材质有多个名称时，你可以用(texture_index)指数选其中一个。如果没有特别指定的指数，你可以假设第一个材质的指数为0.

```
"spawn_egg": {
  "texture": "spawn_egg",
  "texture_index": 2
}

```
都挺简单的，不是吗？下面的内容就稍微有点难度了:]

## 数据驱动动画
这可能需要一点数学知识)
### 概要
该方案应遵循当前的Minecraft JSON方案
* 字段应为小写并使用下划线（无空格）。
* 定义目录和subtree中的所有JSON文件都将被动画系统读入并解释。
* 对于十月技术预览，您需要启用世界上的“使用测试游戏”选项。

### 开始
我们先去拿一份新的行为包作为参照:
```
mkdir addons && cd addons 
wget https://aka.ms/behaviorpacktemplate
```
打印一下目录：
```
addons
├── Documentation_Addons.html
├── Documentation_Animations.html
├── Documentation_Particles.html
├── entities
│   ├── area_effect_cloud.json
│   ├── armor_stand.json
│   ├── arrow.json
│   ├── bat.json
│   ├── blaze.json
│   ├── boat.json
│   ├── cat.json
│   ├── cave_spider.json
│   ├── chest_minecart.json
│   ├── chicken.json
│   ├── command_block_minecart.json
│   ├── cow.json
│   ├── creeper.json
│   ├── dolphin.json
│   ├── donkey.json
│   ├── dragon_fireball.json
│   ├── drowned.json
│   ├── egg.json
│   ├── elder_guardian.json
│   ├── ender_crystal.json
│   ├── ender_dragon.json
│   ├── enderman.json
│   ├── endermite.json
│   ├── ender_pearl.json
│   ├── evocation_illager.json
│   ├── eye_of_ender_signal.json
│   ├── fireball.json
│   ├── fireworks_rocket.json
│   ├── fishing_hook.json
│   ├── fish.json
│   ├── ghast.json
│   ├── guardian.json
│   ├── hopper_minecart.json
│   ├── horse.json
│   ├── husk.json
│   ├── iron_golem.json
│   ├── lightning_bolt.json
│   ├── lingering_potion.json
│   ├── llama.json
│   ├── llama_spit.json
│   ├── magma_cube.json
│   ├── minecart.json
│   ├── mooshroom.json
│   ├── mule.json
│   ├── ocelot.json
│   ├── panda.json
│   ├── parrot.json
│   ├── phantom.json
│   ├── pig.json
│   ├── player.json
│   ├── polar_bear.json
│   ├── pufferfish.json
│   ├── rabbit.json
│   ├── salmon.json
│   ├── sheep.json
│   ├── shulker_bullet.json
│   ├── shulker.json
│   ├── silverfish.json
│   ├── skeleton_horse.json
│   ├── skeleton.json
│   ├── slime.json
│   ├── small_fireball.json
│   ├── snowball.json
│   ├── snow_golem.json
│   ├── spider.json
│   ├── splash_potion.json
│   ├── squid.json
│   ├── stray.json
│   ├── thrown_trident.json
│   ├── tnt.json
│   ├── tnt_minecart.json
│   ├── tripod_camera.json
│   ├── tropicalfish.json
│   ├── turtle.json
│   ├── vex.json
│   ├── villager.json
│   ├── vindicator.json
│   ├── witch.json
│   ├── wither.json
│   ├── wither_skeleton.json
│   ├── wither_skull_dangerous.json
│   ├── wither_skull.json
│   ├── wolf.json
│   ├── xp_bottle.json
│   ├── zombie_horse.json
│   ├── zombie.json
│   ├── zombie_pigman.json
│   └── zombie_villager.json
├── loot_tables
│   ├── chests
│   │   ├── abandoned_mineshaft.json
│   │   ├── buriedtreasure.json
│   │   ├── desert_pyramid.json
│   │   ├── dispenser_trap.json
│   │   ├── end_city_treasure.json
│   │   ├── igloo_chest.json
│   │   ├── jungle_temple.json
│   │   ├── monster_room.json
│   │   ├── nether_bridge.json
│   │   ├── shipwreck.json
│   │   ├── shipwrecksupply.json
│   │   ├── shipwrecktreasure.json
│   │   ├── simple_dungeon.json
│   │   ├── spawn_bonus_chest.json
│   │   ├── stronghold_corridor.json
│   │   ├── stronghold_crossing.json
│   │   ├── stronghold_library.json
│   │   ├── underwater_ruin_big.json
│   │   ├── underwater_ruin_small.json
│   │   ├── village_blacksmith.json
│   │   ├── village_two_room_house.json
│   │   └── woodland_mansion.json
│   ├── empty.json
│   ├── entities
│   │   ├── armor_set_chain.json
│   │   ├── armor_set_diamond.json
│   │   ├── armor_set_gold.json
│   │   ├── armor_set_iron.json
│   │   ├── armor_set_leather.json
│   │   ├── armor_stand.json
│   │   ├── bat.json
│   │   ├── blaze.json
│   │   ├── boat.json
│   │   ├── cat_gift.json
│   │   ├── cat.json
│   │   ├── cave_spider.json
│   │   ├── chicken.json
│   │   ├── clownfish.json
│   │   ├── cow.json
│   │   ├── creeper.json
│   │   ├── dolphin.json
│   │   ├── drowned_equipment.json
│   │   ├── drowned.json
│   │   ├── drowned_ranged_equipment.json
│   │   ├── elder_guardian.json
│   │   ├── enderman.json
│   │   ├── endermite.json
│   │   ├── evocation_illager.json
│   │   ├── fish.json
│   │   ├── ghast.json
│   │   ├── giant.json
│   │   ├── guardian.json
│   │   ├── horse.json
│   │   ├── iron_golem.json
│   │   ├── llama.json
│   │   ├── magma_cube.json
│   │   ├── mooshroom.json
│   │   ├── mooshroom_shear.json
│   │   ├── ocelot.json
│   │   ├── panda.json
│   │   ├── panda_sneeze.json
│   │   ├── parrot.json
│   │   ├── phantom.json
│   │   ├── pig.json
│   │   ├── pig_saddled.json
│   │   ├── polar_bear.json
│   │   ├── pufferfish.json
│   │   ├── rabbit.json
│   │   ├── salmon_large.json
│   │   ├── salmon_normal.json
│   │   ├── sea_turtle.json
│   │   ├── sheep.json
│   │   ├── sheep_sheared.json
│   │   ├── sheep_shear.json
│   │   ├── shulker.json
│   │   ├── silverfish.json
│   │   ├── skeleton_gear.json
│   │   ├── skeleton_horse.json
│   │   ├── skeleton.json
│   │   ├── slime.json
│   │   ├── snowman.json
│   │   ├── spider.json
│   │   ├── squid.json
│   │   ├── stray.json
│   │   ├── tropicalfish.json
│   │   ├── vex_gear.json
│   │   ├── vindication_illager.json
│   │   ├── vindicator_gear.json
│   │   ├── witch.json
│   │   ├── wither_boss.json
│   │   ├── wither_skeleton_gear.json
│   │   ├── wither_skeleton.json
│   │   ├── wolf.json
│   │   ├── zombie_equipment.json
│   │   ├── zombie_horse.json
│   │   ├── zombie.json
│   │   ├── zombie_pigman_gear.json
│   │   └── zombie_pigman.json
│   ├── equipment
│   │   └── low_tier_items.json
│   └── gameplay
│       ├── fishing
│       │   ├── fish.json
│       │   ├── jungle_fish.json
│       │   ├── jungle_junk.json
│       │   ├── junk.json
│       │   └── treasure.json
│       ├── fishing.json
│       └── jungle_fishing.json
├── manifest.json
├── pack_icon.png
├── spawn_rules
│   ├── bat.json
│   ├── blaze.json
│   ├── cat.json
│   ├── chicken.json
│   ├── cod.json
│   ├── cow.json
│   ├── creeper.json
│   ├── dolphin.json
│   ├── donkey.json
│   ├── drowned.json
│   ├── enderman.json
│   ├── ghast.json
│   ├── guardian.json
│   ├── horse.json
│   ├── husk.json
│   ├── llama.json
│   ├── magma_cube.json
│   ├── mooshroom.json
│   ├── ocelot.json
│   ├── panda.json
│   ├── parrot.json
│   ├── phantom.json
│   ├── pig.json
│   ├── polar_bear.json
│   ├── pufferfish.json
│   ├── rabbit.json
│   ├── salmon.json
│   ├── sheep.json
│   ├── skeleton.json
│   ├── slime.json
│   ├── spider.json
│   ├── squid.json
│   ├── stray.json
│   ├── tropicalfish.json
│   ├── turtle.json
│   ├── witch.json
│   ├── wither_skeleton.json
│   ├── wolf.json
│   ├── zombie.json
│   └── zombie_pigman.json
└── trading
    ├── armorer_trades.json
    ├── butcher_trades.json
    ├── cartographer_trades.json
    ├── cleric_trades.json
    ├── farmer_trades.json
    ├── fisherman_trades.json
    ├── fletcher_trades.json
    ├── leather_worker_trades.json
    ├── librarian_trades.json
    ├── shepherd_trades.json
    ├── tool_smith_trades.json
    └── weapon_smith_trades.json

```
行为包总体变化不大，就增加了一个'spawn_rules'目录，还送文档官方真是贴心呢XD

我们再拿一份材质包：
```
mkdir textures && cd textures
wget https://aka.ms/resourcepacktemplate
```
列一下目录，列表[点这里](./textures.md)

资源包中添加了几个关于动画的新目录:
* animations
* animation_controllers
* entity
其他的目录有其他的作用，不要混淆了．

动画被指定为短名称+其全名。短名称用于animation controllers，而长名称用于动画文件。
### animations
动画都会以json格式储存在“animation”文件夹内,并由实体的json定义文件引用
### animation_controllers
动画控制器允许实体的参数来混合多个动画（例如：移动速度，旋转速度等）
### entity
引导文件
### 自定义动画
为了定义实体具有动画，必须将animations和animation controllers 都添加到entity的实体定义文件中：

可以说，entity目录的文件就是动作的'引导'，决定了哪些实体用哪些动画．
```
# cat ./entity/pig.json
{
  "format_version": "1.8.0-beta.1",
  "minecraft:client_entity": {
    "description": {
      "identifier": "minecraft:pig",
      "materials": { "default": "pig" },
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
        { "setup": "controller.animation.pig.setup" },
        { "move": "controller.animation.pig.move" },
        { "baby": "controller.animation.pig.baby" }
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
我把文件中目前不用到的内容删去之后：
```
# cat ./entity/pig＿removed.json
{
  "minecraft:client_entity": {
    "description": {
    //至少我们还要知道它是一只猪，对吧？
      "identifier": "minecraft:pig",
      //动画文件
      "animations": {
        "setup": "animation.pig.setup",
        "walk": "animation.quadruped.walk",
        "look_at_target": "animation.common.look_at_target",
        "baby_transform": "animation.pig.baby_transform"
      },
      //动画控制器
      "animation_controllers": [
        { "setup": "controller.animation.pig.setup" },
        { "move": "controller.animation.pig.move" },
        { "baby": "controller.animation.pig.baby" }
      ]
    }
  }
}
```
#### animations
在每帧的开始，骨架从其几何定义被重置为其默认姿势，然后按顺序依次应用动画。
```
# cat ./animations/pig.json
{
  "format_version": "1.8.0",
  "animations": {
    "animation.pig.setup.v1.0": {
      "loop": true,
      "bones": {
        "body": {
          "rotation": [ "90.0 - this", 0.0, 0.0 ]
        }
      }
    },
    "animation.pig.setup": {
      "loop": true,
      "bones": {
        "body": {
          "rotation": [ "-this", 0.0, 0.0 ]
        }
      }
    },
    "animation.pig.baby_transform": {
      "loop": true,
      "bones": {
        "head": {
          "scale": 2.0,
          "position": [ 0.0, 8.0, 4.0 ]
        }
      }
    }
  }
}
```
#### animation_controllers
玩家需要能够控制动画的播放方式，时间以及它们与其他动画的交互方式。动画控制器允许实体的参数来混合多个动画（例如：移动速度，旋转速度等）。
```
# cat ./animation_controllers/pig.json
{
  "format_version": "1.8.0-beta.1",
  "animation_controllers": {
    "controller.animation.pig.setup": {
      "states": {
        "default": {
          "animations": {
            "setup": {}
          }
        }
      }
    },
    "controller.animation.pig.baby": {
      "states": {
        "baby": {
          "parameters": [ "Entity.Flags.BABY" ],
          "animations": {
            "baby_transform": [
              {
                "0.0": 0.0,
                "1.0": 1.0
              }
            ]
          }
        }
      }
    },
    "controller.animation.pig.move": {
      "states": {
        "default": {
          "parameters": [ "Entity.Member.WalkSpeed" ],
          "animations": {
            "walk": [
              {
                "0.0": 0.0,
                "1.0": 1.0
              }
            ],
            "look_at_target": {}
          }
        }
      }
    }
  }
}
```
### animation文件的格式
这一节非常麻烦和复杂，wiki翻译到这里就停止了:(
```
"<animation_name>": { 
  "loop": <bool> 
  "blend_weight": <expression> 
  "animation_length": <float> 
  "override_previous_animation": <bool> 
  "bones": [
    {
       "<bone_name>": {                  

            "position": 1.0,                     
            "position": [1.0],                   
            "position": [1.0, 2.0, 3.0],          

            "rotation": 45.0,                     
            "rotation": [45.0],                  
            "rotation": [30.0, 0.0, 45.0],    
            "rotation": [0.373, 0.577, 0.687, 0.235],
            "scale": 2.0,                         
            "scale": [2.0],                       

            
            "rotation": {
                "0.0": [80.0, 0.0, 0.0],
                "0.1667": [-80.0, 0.0, 0.0],
                "0.333": [80.0, 0.0, 0.0]
            }

            
            "rotation": {
                "0.3": {                                                
                    "pre": [30.0, 0.0, 45.0],                            
                    "post": "180.{{code|0 * Math}}.Sin(Params.KeyFrameLerpTime)"  
                }
            }

            
            "rotation": {
                "0.0": [80.0, 0.0, 0.0],           
                "0.4": {
                    "pre": [80.0, 0.0, 0.0],      
                    "post": [0.0, 0.0, 0.0],      
                },
                "0.8": [-80.0, 0.0, 0.0]          
            }
        }
    }
]
```
非必须，都有默认值
* loop 是否循环
* blend_weight　这个动画与其他动画混合了多少
* animation_length　动画结束的时间
* override_previous_animation　在应用此动画之前，是否应将骨骼的动画姿势设置为绑定姿势，从而覆盖此点之前的任何动画？
#### bones
* bone_name 　对应geometry中的名称，像head,body.
动画有多种写法:

x = y = z = 1:
```
"position": 1.0
"position": [1.0]
```
x = 1.0 ; y = 2.0 ; z = 3.0
`"position": [1.0, 2.0, 3.0]`

x = y = z = 45 degrees
```
"rotation": 45.0
"rotation": [45.0]
```

x = 30 degrees ; y = 0 degrees ; z = 45 degrees
`"rotation": [30.0, 0.0, 45.0]`

x,y,z,w被设置对应的弧度：
`"rotation": [0.373, 0.577, 0.687, 0.235]`

scale = 2.0;
```
"scale": 2.0
"scale": [2.0]
```
上述任何一种值都适用于“pre”和“post”，而“pre”不必具有与“post”相同的格式．

关键帧数据如下所述
