# 完全体的 View 矩阵推导

直接讲 View 构成的文章不少，但讲解“为什么会这么构成”的却不多，本文从变换和矩阵的角度讲解：View 矩阵是什么、有什么用，以及一步步的推导。

## 是什么，有什么用

设想一个场景：在一块空地上，你看到有一个立方体，于是你假设一个右手坐标系。如图：

<img src="https://www.qiniu.cregskin.com/202203292234557.png" alt="image-20220329223431491" style="zoom:33%;" />

左瞧瞧右看看，你拿出相机，想挑一个位置拍张照片，于是你选好了： 相机位于点 eye，看向观测目标点 center (这里是 (0, 0, 0))

<img src="https://www.qiniu.cregskin.com/202203292241434.png" alt="image-20220329224105411" style="zoom:33%;" />

在相机视角下，有另一套以自身为原点的坐标轴，基向量分别为 x' y' z'，相机朝向 -z' 方向：

<img src="https://www.qiniu.cregskin.com/202203292248040.png" alt="image-20220329224815016" style="zoom:33%;" />

View 就是世界空间(你作为旁观者观测到的场景)下的坐标，到观测空间(摄像机观察到的场景)坐标的一种变换。



## 怎么来的

体会一下这句话：**将物体从世界空间变换到观测空间，等价于将相机从观测空间变换到世界空间**。如果不理解可以接着往下看。

研究如何变换物体比较麻烦，将相机从观测空间的 (0, 0, 0) 变换到世界空间的 eye 却很简单。我们用矩阵描述整个过程：

1.   M_rotation：旋转相机，让 x' y' z' 与原 x y z 轴的方向重合

2.   M_translation：摄像机从 (0, 0, 0) 位置平移到 eye

很容易写出：

<img src="https://www.qiniu.cregskin.com/202203301149637.png" alt="image-20220330114925608" style="zoom:50%;" />

<img src="https://www.qiniu.cregskin.com/202203301149734.png" alt="image-20220330114940711" style="zoom:50%;" />

<img src="https://www.qiniu.cregskin.com/202203301152022.png" alt="image-20220330115232998" style="zoom:50%;" />

将整个将摄像机从观测空间原点，变换到世界空间 eye 的矩阵取逆，应用到世界空间坐标下，就可以将世界空间坐标转化成观测空间坐标！

<img src="https://www.qiniu.cregskin.com/202203301152849.png" alt="image-20220330115242830" style="zoom:50%;" />

也就是：

<img src="https://www.qiniu.cregskin.com/202203301156222.png" alt="image-20220330115637199" style="zoom:50%;" />

让世界空间坐标左乘这个矩阵，就能得出观测空间坐标。但**这并不是完全体的 View 矩阵**！

回想一下最开始的假设：摄像机位于世界空间的 eye 位置，观测目标点 (0, 0, 0)。如果，观测目标点并不是原点呢？上边的矩阵就不成立了！

>   一些图形学 API 约定俗成让观测目标点是 (0, 0, 0)，才会有上面版本的 View 矩阵广泛流传。

相比之前的版本，完全体的 View 矩阵就是把观测目标点 center 考虑在内。重新梳理下我们对相机采用的变换：

1.   M_translation1：将相机从 (0, 0, 0) 平移到 center
2.   M_rotation：旋转相机，让 x' y' z' 与原 x y z 轴的方向重合
3.   M_translation2：将相机从 center 平移到 eye

<img src="https://www.qiniu.cregskin.com/202203301222122.png" alt="image-20220330122206101" style="zoom:50%;" />

分别写出这三个矩阵：

<img src="https://www.qiniu.cregskin.com/202203301225307.png" alt="image-20220330122534283" style="zoom:50%;" />

<img src="https://www.qiniu.cregskin.com/202203301223733.png" alt="image-20220330122325708" style="zoom:50%;" />

<img src="https://www.qiniu.cregskin.com/202203301224530.png" alt="image-20220330122448466" style="zoom:50%;" />

<img src="https://www.qiniu.cregskin.com/202203301224417.png" alt="image-20220330122428353" style="zoom:50%;" />

取逆：

<img src="https://www.qiniu.cregskin.com/202203301228767.png" alt="image-20220330122810740" style="zoom:50%;" />

这就是完全体的 View 矩阵了！当观测目标点 center = (0, 0, 0) 时，正好是上一个版本的 View 矩阵！



>   至于完全体 View 表示的三步变换能不能变顺序，emmmmm，我这里没有 3D 绘图软件，读者发挥发挥想象力吧！