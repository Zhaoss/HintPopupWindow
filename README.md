在某个版本更新时, UI要求弹出选项框类似于下面的效果:

![image](http://upload-images.jianshu.io/upload_images/2582948-a06b1a364aacd8c4.gif?imageMogr2/auto-orient/strip)

对, 就是qq这种弹出方式, 而UI给的效果图实际是这样的:

![image](http://upload-images.jianshu.io/upload_images/2582948-3695db8b621be484.gif?imageMogr2/auto-orient/strip)

恩, 我们UI的审美认为下面加一层模糊效果会比较好看, 我看了之后认为没什么问题, 就是多了一层模糊效果而已, 但做到后面才发现有的很大的坑在等我.
首先我做出来的自定义view是希望全局只要调一个方法就可以用的, 就像popupwindow那样, 所以我的思路是初始化整个布局, 然后以Toast的方式添加到屏幕最前端.
所以第一步: 初始化整个弹窗布局

![image](http://upload-images.jianshu.io/upload_images/2582948-689fa965e0c6d197.png?imageMogr2/auto-orient/strip)
![image](http://upload-images.jianshu.io/upload_images/2582948-e40648ed4d242ca1.png?imageMogr2/auto-orient/strip)

第二步: 这里比较重要了, 当点击button, 弹出选项框的时候, 具体做哪些事情

![image](http://upload-images.jianshu.io/upload_images/2582948-5fb944cbc7343f35.png?imageMogr2/auto-orient/strip)

到这里我们所有操作就都完成了 大家是不是感觉很简单, 嘿嘿嘿, 最难的坑其实是模糊图片那里, 因为我们是当用户点击弹出按钮的时候动态模糊的, 所以效率就很重要, 下面是我对activity视图bitmap的处理:

![image](http://upload-images.jianshu.io/upload_images/2582948-9cfdd0e016505849.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当用户点下按钮时,我们需要立刻就将模糊后的图片显示出来, 下面是我的模糊图片代码:

![image](http://upload-images.jianshu.io/upload_images/2582948-3d3ccbaeff83336d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

android里面的高斯模糊我大概总结了一下 基本有三种, 优缺点都有, 我用的是系统推荐的, 速度比较快,而且也简单, 但只能支持android版本17以上, 但现在手机用android4.2以下的估计也很少了.

第二种就是利用glide自定义类继承BitmapTransformation来实现在加载图片时模糊图片,但和第一种差不多,也要android版本17以上才能用
第三种就是用java层的代码, 手动算出像素值, 因为图片处理的代码逻辑都是用java实现的, 所以效率极差, 不推荐.
最后在说一下那个弹出蠕动的动画, 很简单20行代码就ok了, 我是用属性动画写的, 让弹窗view的宽和高的规模从0到1, 然后在从1到0.95, 这样就造成了一个弹出的动态效果, 很easy吧

![image](http://upload-images.jianshu.io/upload_images/2582948-3d3ccbaeff83336d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

大功告成出来的效果就是这样的啦

![image](http://upload-images.jianshu.io/upload_images/2582948-37d35360b9d249cf.gif?imageMogr2/auto-orient/strip)
