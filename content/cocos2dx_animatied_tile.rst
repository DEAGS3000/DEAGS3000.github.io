Cocos2d-x TiledMap Animation
##########################################################

:date: 2019-11-08 21:43
:modified: 2019-11-08 21:43
:tags: game, 游戏, engine, 引擎, C++
:category: 游戏
:authors: IllusiveMan
:summary: 关于如何在Cocos2d-x中支持Tiled的图块动画

为Cocos2d-x的TMXTiledmap类添加动画块支持，有如下三个方法：

1. 偷懒法。自行创建动画块放到合适位置。缺点是需要第二次解析地图文件，而且无法使用SpriteBatchNode的优化渲染。
2. 修改Atlas法。优点是可以使用优化渲染，但这种做法属于改贴图数据了。众所周知向显存传送贴图是性能开销的大头。不太可取。
3. 更新地图图层特定位置图块法。用的还是原来的贴图数据，只不过更改了绘制/选取位置。同样可以使用优化渲染。

P.S. 在cocos2dx的测试用例中可以看到，有一种通过GID object创建图块的方式。不知可不可行。

另外，图块动画的播放也有两种可选的实现方式：

1. 使用Cocos2dx自带的schedule系统。让Layer或Map的相关组件负责动画块的tick。
2. 自己创建一个专门用于tick的类，通过在update里面调用病记录经过的时间，决定换帧的时机。

实际上Cocos2d-x自己的ActionManager用的也是第2种方法。第一种方法可能会造成内存抖动。

另外，TMXTileAnimInfo对象的生命周期显然应当与TileSet同步。如果用户进行了修改tileset的操作，原来的动画信息应当失效。

下面是我的采用第3种方法的实现：

`DEAGS3000/cocos2dx-animated-tile-support <https://github.com/DEAGS3000/cocos2dx-animated-tile-support>`_

最后Tiled的作者bjorn建议我给cocos2d-x提交pr，直接让引擎native支持动画块。这里面也有许多坑，算是把我这些年来没好好用GitHub欠下的债给还上了。。。
