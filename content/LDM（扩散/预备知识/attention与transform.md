# 一.Attention
关键词： token ，embedding
参考：
[Attention、Transformer公式推导和矩阵变化_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1q3411U7Hi/?spm_id_from=333.337.search-card.all.click&vd_source=1de3c2786f2973d155248cd390d724b0)
## 矩阵计算
### 整体
softmax后得到的是**attention score**：
![[Pasted image 20241007191755.png]]
每个q都会与每个k做内积得到a‘，第一个数字“1”表示以第一个x1为例来算分别的内积。

最后得分：
![[Pasted image 20241007191656.png]]
$y^{1}$是表示是最后得分。 a'相当于**v的权重**（attention score）。


### 计算
计算Q矩阵：
![[Pasted image 20241007192533.png]]

计算a’矩阵：
![[Pasted image 20241007193402.png]]
a‘的**每一列**表示的某一个向量的**q**的全部**attention score**值。

具体计算最后得分（y2为例）：
![[Pasted image 20241007194457.png|950]]
最后一个式子是矩阵乘法的样式。

**最终的矩阵形式**：
![[Pasted image 20241007194818.png]]
**即**：
![[Pasted image 20241007194907.png]]

### 总结
1. 计算得到QKV
![[Pasted image 20241007195622.png|975]]

2. $K^{T}Q$得到A，（attention score~），并且softmax。
![[Pasted image 20241007195745.png]]

3. 最终得到Y
![[Pasted image 20241007200013.png]]


## 加入位置编码
![[Pasted image 20241007200526.png]]$$\begin{aligned}&\mathrm{PE(pos,2i)}=\sin(\mathrm{pos/10000^{2i/d_{model}}})\\&\mathrm{PE(pos,2i+1)=cos(pos/10000^{2i/d_{model}})}\end{aligned}$$
**说明：**
pos是向量的位置，d_model是向量x的纬度。
例如pos=2，dmodel=4：
![[Pasted image 20241007201252.png]]


# 二.transformer

![[Pasted image 20241007205610.png|325]]
                                             图-编解码
## 编码器
1. Input Embedding-->词变成词向量
2. 进行位置编码
3. self-attention
![[Pasted image 20241007204315.png|525]]
	attention层一般是输入多少向量，输出就是多少向量。

4. Add&Norm层
![[Pasted image 20241007204527.png|525]]
	residual残差     ，layer normalization层标准化      的操作。
	
**residual残差：**
![[Pasted image 20241007204821.png|400]]
输出向量与输入向量相加。（向量的长度一致）
**tip：** 当然除了最开始的Input embedding与最后的linear操作会改变向量长度。

**标准化**：
![[Pasted image 20241007205136.png|325]]

5. Feed Forward层
相当于全连接层

## 解码器
进行位置编码
![[Pasted image 20241007205407.png]]

编码器输出与输入一样个数的向量。
解码器是自回归的，前一个输出会作为后一个输入。

1. 输入  Embedding
![[Pasted image 20241007205746.png|275]]
2. 进行位置编码

3. masked attention    （引入的transformer的一大特性--**自回归**）
      首先只有一个向量<bos.>:经过output embedding和positional encoding后来到这里。
![[Pasted image 20241007210437.png]]
通过bos->得到第一个“永”；
第二阶段通过解码器结合编码器的信息和后面的bos和永的信息来预测得到“不”。  ······

**具体实现： masked）**
就是把后面不想关注的向量对应的attention score设置为负无穷大就行。
![[Pasted image 20241007210844.png]]
归一化后： 
![[Pasted image 20241007211005.png]]
最后得到y：
![[Pasted image 20241007211328.png]]
得到输出后面两个向量为0
4. Add&Norm

5. **cross attention**     （不太一样和前面）
混合编码器和解码器的信息：
解码器生成的q ，查找编码器生成的k，一起计算cross attention   score，softmax后，将编码器的向量v按权相加，得到cross attention的结果。
以q1为例：
![[Pasted image 20241007212210.png]]


5. Add&Norm
6. feed Forward（全连接层）
7. Add&Norm
8. Linear
Linear（x）=Wx+b     转换向量的纬度：与输出的向量结果y的纬度是一致的。

9. softmax  得到输出的概率向量，每个向量的纬度代表一个中文字出现的概率大小。
![[Pasted image 20241007212921.png|225]]


# 三.补充
1. 
