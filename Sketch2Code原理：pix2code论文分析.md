﻿﻿## 关于Sketch2Code原理：pix2code论文研究
### 预备知识1：RNN
循环神经网络的主要用途是处理和预测序列数据，在全连接神经网络或卷积神经网络中，网络结果都是从输入层到隐含层再到输出层，层与层之间是全连接或部分连接的，但每层之间的结点是无连接的。考虑这样一个问题，如果要预测句子的下一个单词是什么，一般需要用到当前单词以及前面的单词，因为句子中前后单词并不是独立的。循环神经网络的来源就是为了刻画一个序列当前的输出与之前信息的关系。从网络结果上来说，RNN会记忆之前的信息，并利用之前的信息影响后面的输出。也就是说，RNN的隐藏层之间的结点是有连接的，隐藏层的输入不仅包括输入层的输出，还包含上一时刻隐藏层的输出。典型的RNN结构如下图所示，对于RNN来说，一个非常重要的概念就是时刻，RNN会对于每一个时刻的输入结合当前模型的状态给出一个输出。

### 预备知识2：解释器模式（简单解释器原理）
给定一个语言，定义他的一种简单文法表示，并且定义一个解释器，这个解释器使用该表示来解释语言句子。将语言当中的句子表示为一个抽象语法树来对语言进行解释。我们可以使用C++或者PYTHON轻易的实现一个解释器模型。具体参考GOF书INTERPRETER模式。
**解决的问题：如果一种特定类型的问题发生的频率足够高，那么可能就值得将该问题的各实例表述为一个句子。**

### 预备知识3:激活函数

现实生活当中所有问题并不全都是线性可分的，因此在深度学习当中我们一般引入激活函数来对网络的输出进行相关的处理。每一层的输出通过这些激活函数之后，就变得比以前复杂很多，从而提升了神经网络模型的表达能力。
![SIGMA函数](https://github.com/OneSilverBullet/SoftwareEngineering/blob/master/%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%E5%9B%BE%E7%89%87/sigma%E5%87%BD%E6%95%B0.png)

![TANH函数](https://github.com/OneSilverBullet/SoftwareEngineering/blob/master/%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%E5%9B%BE%E7%89%87/tan%E5%87%BD%E6%95%B0.png)


### 预备知识3：LSTM
目的：为了解决，是为了解决长期以来问题而专门设计出来的。LSTM 同样是这样的结构，但是重复的模块拥有一个不同的结构。不同于单一神经网络层，这里是有四个，以一种非常特殊的方式进行交互。
![核心思想](https://github.com/OneSilverBullet/SoftwareEngineering/blob/master/%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%E5%9B%BE%E7%89%87/LSTM.jpg)

![核心思想2](https://github.com/OneSilverBullet/SoftwareEngineering/blob/master/%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%E5%9B%BE%E7%89%87/LSTM.png)


核心思想：LSTM的关键在于细胞的状态整个(绿色的图表示的是一个cell)，和穿过细胞的那条水平线。细胞状态类似于传送带。直接在整个链上运行，只有一些少量的线性交互。信息在上面流传保持不变会很容易。
![核心思想3](https://github.com/OneSilverBullet/SoftwareEngineering/blob/master/%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%E5%9B%BE%E7%89%87/LSTM1.jpg
)

LSTM通过三个这样的本结构来实现信息的保护和控制。这三个门分别输入门、遗忘门和输出门。

1.遗忘门
第一步是决定从细胞状态中丢弃什么信息。这个决定通过一个称为忘记门层完成。该门会读取ht−1和xt，输出一个在 0到 1之间的数值给每个在细胞状态 Ct−1 中的数字。1 表示“完全保留”，0 表示“完全舍弃”。
![核心思想4](https://github.com/OneSilverBullet/SoftwareEngineering/blob/master/%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%E5%9B%BE%E7%89%87/LSTM2.jpg)

2.输入门
决定让多少新的信息加入到 cell 状态 中来。实现这个需要包括两个 步骤：首先，一个叫做“input gate layer ”的 sigmoid 层决定哪些信息需要更新；一个 tanh 层生成一个向量，也就是备选的用来更新的内容，C^t 。在下一步，我们把这两部分联合起来，对 cell 的状态进行一个更新。
![输入门](https://github.com/OneSilverBullet/SoftwareEngineering/blob/master/%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%E5%9B%BE%E7%89%87/LSTM3.jpg)

![输入门](https://github.com/OneSilverBullet/SoftwareEngineering/blob/master/%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%E5%9B%BE%E7%89%87/LSTM4.jpg)

3.输出门
最终确定输出什么值。这个输出将会基于细胞状态，但是也是一个过滤后的版本。首先，运行一个 sigmoid 层来确定细胞状态的哪个部分将输出出去。接着，把细胞状态通过 tanh 进行处理（得到一个在-1到1之间的值）并将它和sigmoid门的输出相乘，最终我们仅仅会输出确定输出的那部分。
![输出门](https://github.com/OneSilverBullet/SoftwareEngineering/blob/master/%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%E5%9B%BE%E7%89%87/LSTM5.jpg)

### 基本想法

从一个GUI屏射图片当中生成对应平台的GUI代码，这种
目标和使用语言去描述对应的图片这种最近比较火热的
深度学习研究有着异曲同工之妙。一般来说，我们可以将
这个任务抽象成三个问题：

1.计算机视觉任务：如何理解给定的图片。

比如对应的object的表示，他们的特点，位置以及样式等等的问题。

2.语言模型任务：如何生成正确的语言。

3.通过利用使用计算机视觉模型的带潜在的变量配合
语言模型来生成正确的语言。
![pix2code](https://github.com/OneSilverBullet/SoftwareEngineering/blob/master/%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%E5%9B%BE%E7%89%87/pix2code%E6%A8%A1%E5%9E%8B.png)
### 视觉模型
视觉模型使用的是CNN模型，更确切的说是VGG网络。
作者使用CBB来进行无监督学习，将输入的图片和
固定长度的向量进行映射。
```
image_model = Sequential()
image_model.add(Conv2D(16, (3, 3), padding='valid', activation='relu', input_shape=(256, 256, 3,)))
image_model.add(Conv2D(16, (3,3), activation='relu', padding='same', strides=2))
image_model.add(Conv2D(32, (3,3), activation='relu', padding='same'))
image_model.add(Conv2D(32, (3,3), activation='relu', padding='same', strides=2))
image_model.add(Conv2D(64, (3,3), activation='relu', padding='same'))
image_model.add(Conv2D(64, (3,3), activation='relu', padding='same', strides=2))
image_model.add(Conv2D(128, (3,3), activation='relu', padding='same'))
image_model.add(Flatten())
image_model.add(Dense(1024, activation='relu'))
image_model.add(Dropout(0.3))
image_model.add(Dropout(0.3))
```
这里使用的网络进行训练前，对图片进行如下处理：
* 将图片处理为256*256像素。
* 图片每一个像素都进行normalized在喂给CNN之前。

这篇文章当中使用的卷积核尺寸是，3 * 3的感受域，同时其步长为1。作者使用CNN对图片进行无监督学习，同时，
使用CNN将IMAGE映射到一个固定长度的向量。


### 语言模型

下面的是项目当中使用的DSL：一个简单的专业领域语言。
模型将一张图片转换为DSL格式之后，再进行更加深入的处理。

注意其DSL模型非常简单，便于缩减其搜索空间的同时还可以减少其
词汇量表的大小。通过这种手段，其语言模型可以
在离散输入的情况进行热编码。避免了单词嵌入这种
消耗大量时间空间的运算。
```
, { } small-title text quadruple row btn-inactive btn-orange btn-green btn-red double <START> header btn-active <END> single
```

### 解码
pix2code模型使用无监督训练。输入一个图片I与文本序列X（是当前图片的DSL）。将正确的Xt作为目标label。首先使用CNN将图片输出成一个向量p。
然后将输入x使用LSTM语言模型进行编码，编码形成一个中间表示q。然后将p与q两个向量进行串联，形成单一特征向量r,将这个向量继续输入到下一个LSTM当中。第二个LSTM模型解释了前面的LSTM与CNN的特征输出。从而将IMAGE的特征输出与DSL的特征输出联系了起来。
当前架构允许整个pix2code模型进行端到端梯度下降优化，来根据时间预测一个DSL序列。
![DECODE](https://github.com/OneSilverBullet/SoftwareEngineering/blob/master/%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%E5%9B%BE%E7%89%87/Decoder.png)



### 实验
训练序列的长度T对于建立长期依赖关系很重要。经过实证实验，将用于训练的DSL输入文件用一个大小为滑动窗口进行分割成48：我们展开递归神经网络48步，这种方法是最好的。对于每一个标记，在该模型同时具有输入图像和上下文序列T = 48的标记。

因为根本没有相应的数据集来进行训练，也就是IMAGE与DSL简码之间的数据集，因此作者是用来自己的数据集进行解释。（表示怀疑）就是表中描述的三个数据集。合成器指的是可以使用随机用户界面生成器合成的GUI配置的最大个数。实例指的是合成的(GUI截图，GUI代码)文件对的数量。样本指的是不同的图像序列对的数量。实际上，训练和采样一次只做一个令牌，方法是向模型输入一个图像和一个固定大小T。
![训练数据集](https://github.com/OneSilverBullet/SoftwareEngineering/blob/master/%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%E5%9B%BE%E7%89%87/%E8%AE%AD%E7%BB%83%E6%95%B0%E6%8D%AE%E9%9B%86.png)

### 实验结果

![训练结果](https://github.com/OneSilverBullet/SoftwareEngineering/blob/master/%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%E5%9B%BE%E7%89%87/%E5%AE%9E%E9%AA%8C%E7%BB%93%E6%9E%9C.png)

具体使用的损失函数如下：

![损失函数](https://github.com/OneSilverBullet/SoftwareEngineering/blob/master/%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%E5%9B%BE%E7%89%87/%E6%8D%9F%E5%A4%B1%E5%87%BD%E6%95%B0.png)


### 关于Sketch2code项目的编译器原理
pix2code这篇文章除了关于整个训练模型的训练模型，还提出了关于DSL的设计。
在Sketch2Code这个项目当中，设计了关于整个程序的DSL语言(也就是生成的GUI文件)。
其中如何将这些文件转换为正常的HTML代码，我对这方面原理进行了研究。这也是限制
Sketch2Code项目功能的一大原因。

DSL的特点是，简单而且特点明显。这注定不能够使用DSL去表达复杂的界面展示。从而
限制了整个程序生成代码的质量。之前有个大兄弟在GITHUB询问Sketch2Code的开发者
是否前端会失业。我想如果他研究一下这个代码就会发现这种顾虑是杞人忧天。

那么DSL为什么不做成非常复杂的模型呢？因为这不仅仅存在极大的效率问题，同时
还会增加前面训练模型的复杂度。这限制了DSL的扩张。如果DSL过于复杂，就只能够使用
“语法分析程序生成器”这种东西来完成。

那么我研究Sketch2Code如何进行代码转换呢，其中涉及一个设计模式：INTERPRETER解释器
模式。通过构建语法树，我们可以轻松实现不同语言之间的转换，当然是简单语言。

#### INTERPRETER 解释器
* 定义：给定一种语言，定义它的一种文法表示，并且定义一个解释器，这个解释器
使用语法树来解释语言句子。

Interpreter这种设计模式在GOF书当中给出的适用性如下所示：
* 当有一个语言需要解释执行，并且你可以把该语言中的句子表示为一个抽象的语法树时，可使用解释器模式．而当存在以下情况时，该模式的效果最好。
* 该文法简单，对于复杂的文法，文法的类层次变得庞大而无法管理．此时，语法分析程序生成器这样得工具时更好得选择。它们无需构建抽象语法树即可解释表达式，这样可以节省空间而且还可以节省时间。
* 效率不是一个关键的问题，最高效的解释器通常不是通过直接解释语法分析树实现的，而是首先把他们转换成另外一种形式．例如：正则表达式通常被转换成状态机。但即使在这种情况下，转换器仍可用解释器模式实现，该模式仍是有用的。


而在Sketch2code的项目环境下，这种设计模式恰恰满足所有的实用性。因此
我猜想这是作者使用这种设计模式的原因之一。

这种设计模式的成员具体如下：
* AbstractExpression:声明抽象解释。该接口为抽象语法树中所有节点所共享。
* TerminalExpression:实现与文法终结符相关的操作。
* NonterminalExpression:每个符号都维护一个AbstractExpression。其中递归
调用解释操作。

我们可以通过INTERPRETER来生成一些简单的程序。在这个基础上，我使用C++开始对
INTERPRETER模式进行一个实现，从而复现Sketch2code当中代码编译器的原理。我
将会设计一个简单的程序模型来说明语法树编译的原理。而这个简单的道理对于
Sketch2Code这个简单的程序已经非常足够了。

'''

    def compile(self, generated_gui):
        dsl_file = generated_gui
        #Parse fix
        dsl_file = dsl_file[1:-1]
        dsl_file = ' '.join(dsl_file)
        dsl_file = dsl_file.replace('{', '{8').replace('}', '8}8')
        dsl_file = dsl_file.replace(' ', '')
        dsl_file = dsl_file.split('8')
        dsl_file = list(filter(None, dsl_file))
        current_parent = self.root
        for token in dsl_file:
            token = token.replace(" ", "").replace("\n", "")
            if token.find(self.opening_tag) != -1:
                token = token.replace(self.opening_tag, "")
                element = Node(token, current_parent, self.content_holder)
                current_parent.add_child(element)
                current_parent = element
            elif token.find(self.closing_tag) != -1:
                current_parent = current_parent.parent
            else:
                tokens = token.split(",")
                for t in tokens:
                    element = Node(t, current_parent, self.content_holder)
                    current_parent.add_child(element)

        output_html = self.root.render(self.dsl_mapping)
        if output_html is None: return "HTML Parsing Error"

        return output_html
'''

如同上面的源码所述，依据当前的生成的GUI文件创建新的HTML文件。
其源码的基本思想与Interpreter如出一辙，使用递归的手法构建语法
生成树，从而可以快速的将DSL文件转换为相应的HTML文件。

