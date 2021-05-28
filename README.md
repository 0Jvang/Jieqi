# 揭棋

这是一个揭棋AI项目，提供命令行对战UI和基于概率/pvs算法的AI。

本项目基于bupticybee大佬的项目二次开发: https://github.com/bupticybee/elephantfish，并严格遵守bupticybee设置的GPL License: https://github.com/bupticybee/elephantfish/blob/master/LICENSE.md。

## v1/随机走子(Random Policy Test): 
`python elephantfish_pvs_v1_fixed.py`

## v4/加入了吃暗车的价值估计，可以根据明车的数量判断暗车的价值: 

`python elephant_pvs_v4_fixed.py`

## 20210529/加入了很多逻辑:

### 1. 空头炮

1. 对于空头炮, 之前的版本只考虑了两种情况: 第一，炮从不在中线的位置走到了中线空头的位置，加分; 第二，炮从中线空头的位置离开了中线，扣分。但我遗漏了另外两种情况。炮沿着中线移动，也有可能从非空头变成空头，或者空头变为非空头，唯一的可能就是打掉对方的子。这两种情况在之前的版本中疏漏了，导致之前的AI经常出现"弃空头"的问题。此外，忽视了空头炮和对方老将的距离。距离越远或者有车配合，抽射能力更强。

### 2. 沉底炮

2. 对于沉底炮，之前的版本盲目打分过高。实际上，揭棋中的沉底炮确实更有威胁，因为没有连环士/连环相的威胁，很容易被捉暗士，沉底车，或捉暗相闷宫，防不胜防。此外，沉底炮在底线牵制诸多暗子，同时瞄着炮碾丹砂等手段，而象棋中沉底炮牵制子力价值较小的士象。然而，如果对方在底线暗子数量有限，则沉底炮未必有很大威胁。

### 3. 暗士

3. 对于翻动暗士，之前的版本(v4)对剩下的兵卒数量有粗略的估计，如果剩余兵卒数量过多，则不鼓励翻动暗士。然而，在这个版本中，如果对方同侧暗车已出，且暗士所在的肋道上己方车的数量比对方多，则鼓励翻士出将助攻。如果对方暗车未出，或相应肋道上己方车的数量小于等于对方车的数量，则仍然沿用兵卒逻辑。

### 4. 暗炮进四
4. 鼓励暗炮进四压制对手。

### 5. 暗炮打马
5. 在己方车未完全出动的情况下，如果对方暗炮进四翻出的不是车或者兵，以及不存在边路兵顶马的情形，则将暗炮打车设为禁着。

### 6. 暗炮进六:
6. 如果对方的暗马未启动，且暗相位置有东西，则鼓励暗炮进六压制暗马，其鼓励分数正比于本方所剩兵卒的数量的数学期望。

### 7. 长将/长捉
7. 不允许电脑走入重复局面，避免了长将。

### 8. pypy
8. 支持pypy3运行程序。pypy使用JIT编译技术，规避了GIL。

运行方法: `python elephantfish_pvs_20210529_fixed.py`。

## 注:
带有`fixed`的版本永久固定，如果您发现有bug，欢迎提Issues，我会在新的版本中改正。您可以使用pypy3(不支持pypy2)运行我的代码，只需要将`python`替换成`pypy3`即可。为了您的方便，我在`getpip_pypy3`文件夹中提供了pypy3的getpip代码。

## 后续:
如果您对我的拙作感兴趣，可以联系我:

邮箱 miaosi2018@gmail.com
或者QQ:
2904661326@qq.com。

我个人认为，MCTS也许可以解决揭棋的问题。虽然揭棋是非完美博弈，且信息集合很大，但这些信息集可以被划分为有限的等价类(换句话来说，很多状态是等价的)。例如，暗相是马暗士是兵，抑或暗相是兵暗相是马，其实并没有太多的区别，都被掩盖在强子(车炮)的光环之下。因此，揭棋这个NFA也许可以近似为一个DFA，但我并不确定。
