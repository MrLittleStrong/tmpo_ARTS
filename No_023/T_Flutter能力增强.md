# Flutter能力增强

### 背景

看完了闲鱼技术的能力增强环节，记录下知识点

### 知识点

1. 插件扩展Plugin

2. 基于外接纹理的同层渲染。

   背景：做群视频时，多路视频画面在渲染时分别上屏消耗了过多的CPU和GPU资源，所以思考能否在一个vsync节点去做统一上屏。

   过程：flutter提供了texture外接纹理的方式，生成的LayerTree最底层是flutter的textureLayer，在绘制这个textureLayer时先调用external_texture，获取CVpixelBuffer，通过OpenGL的方法来创建OpenGL的Texture，最后将OpenGL texture封装为SKImage，用Skia进行绘制。

   问题：Flutter直接使用外接纹理还是有问题，会走GPU-CPU-GPU的路径，还是有性能瓶颈，所以考虑直接GPU内解决，不走CPU。这里他们将EAGLContext进行share。

   疑问：1、使用OpenGL方法性能还是不能最大化，metal上如何优化。2、EAGLContext会进行copy，内存管理是有风险的，处理不好会有内存泄漏等问题。

3. 多媒体能力扩展。——主要是做了个相册

   由于使用原生相册在低端机上会有内存压力导致app被系统干掉，所以使用同层渲染，使用纹理来搞，极大降低内存，从而减少内存压力。

   解决了**分页加载、大图展示和GPU内存泄漏问题**

4. 富文本能力。

   主要设计思路是先计算出来布局，在特殊的图片和emoji等位置使用多个空字符来占位，再通过stack的方式把图片附在占位的位置上。

   但是问题是，这样一来就没办法选择文字了，因为会把空字符也选中。图片也是必须指定宽度和高度

   
