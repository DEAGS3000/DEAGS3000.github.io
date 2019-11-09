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

P.S. 在cocos的测试用例中可以看到，有一种通过GID object创建图块的方式。不知可不可行。

下面是我的采用第3种方法的实现：

`DEAGS3000/cocos2dx-animated-tile-support <https://github.com/DEAGS3000/cocos2dx-animated-tile-support>`_
