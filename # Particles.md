# Particles
## 文档：
[click here](./particles.md)
## 例子：
[click here](./particles_example.md)
## 开始
我们在资源包根目录创建一个'particle'文件夹，用于存放粒子文件
### 基本结构
粒子效果由基本渲染参数和一组组件组成,基本渲染参数需要出现在版本之后并包含材质和纹理。
```
"format_version": "1.8.0",
"particles": {
  "minecraft:my_particle_effect": {
    "basic_render_parameters": {
      "material": <string>
      "texture": <string>
    }
    "curves": {
      "<molangvar>": {
        "type": type,
        "nodes": [<float / molang>, <float / molang>, <float / molang>, <float / molang>],
        "input": <float / molang>, 
        "horizontal_range": <float / molang>
      }
    }
    "components": {}
  }
}
```
#### format_version　
版本，必须
### particles
#### namespace:name
* 粒子被命名时应该使用命名空间
* 所有粒子效果必须具有名称空间作为其标识符的一部分:"namespace:name"
* 此名称是用于从其他位置引用粒子的名称，例如命令
#### basic_render_parameters
所谓的＇基本渲染器＇
##### material 
对于material，有两种material可供选择:
* particles_alpha
* particles_opaque
##### texture 
对于材质，粒子系统可以使用Minecraft所有能被读取的粒子
#### components
粒子系统是基于组件的。
这意味着粒子效果是通过一组组件组成的。
为了使效果能够做某事，你需要添加一个处理效果方面的组件。例如，发射器需要有生命周期的规则，因此发射器将（通常）有一个或多个处理生命周期职责的生命周期组件

用于发射器和发射的粒子。

Mojang的想法是可以在以后添加新组件，并且可以组合组件（在有意义的地方）以获得不同的行为。粒子可能具有用于移动的动态组件，以及用于处理与地形的交互的碰撞组件

将组件视为告诉粒子系统您希望发射器或粒子做什么，而不是暴露粒子参数列表并要求对这些参数进行争论以获得所需的行为。
#### curves
插值，输入从0到1，输出基于所描述的曲线。结果是一个MoLang变量，可以在组件中的MoLan表达式中使用。对于每个粒子，每帧评估一次曲线
#### type
* linear
* bezier
* catmull_rom
#### nodes
值：Vector [a, b, c, d]

控制曲线的节点为0,0.25,0.5和1.0。每个节点都可以作为float或MoLang表达式给出
#### input
要使用的输入值，可以是float或MoLang表达式
#### horizontal_range
输入范围映射在0和此值之间。可以是float或MoLang表达式