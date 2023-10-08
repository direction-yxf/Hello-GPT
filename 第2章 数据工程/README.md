### 2.0.1 数据工程
&emsp;&emsp;本章节概述参考符尧的一篇大模型数据工程方法论文章An Initial Exploration of Theoretical Support for Language Model Data Engineering。  
研究数据工程的动机有以下三点：
1、最近大模型开源社区研究热点从model engineering转移到data engineering，大家开始意识到数据质量的重要性。
2、data engineering的理论还不太成熟，例如：好数据的准确定义是什么？，如何优化数据的结构组成？我们的优化目标是什么？
3、对data engineering进行理论分析可以帮助我们在正式跑实验前预测每个task最终的performance，openai在gpt4的技术报告中提到了这点，非常有意义。  

&emsp;&emsp;具体来说，我们讨论预训练和SFT数据优化的以下目标：
-预训练阶段数据优化：找到最优的混合比例+数据格式+数据课程=》使学习速度最大化
-指令微调阶段调整数据优化：寻找最少的query-resonse pairs（最少的训练数据）=》使用户偏好分布的覆盖范围最大。  

**Part 1 Pretraining**  
*（1）预训练能力评估指标：speed of grokking（获取某技能的速度）*  
&emsp;&emsp;在训练开始时，模型记忆了训练数据，但测试精度比较低并且没有变化。随着训练的进行有一个相变期，模型突然从记忆过渡到泛化，在测试集上显示出高
的准确率，学习过程的这种阶段性变化成为”grokking“。通常预训练模型评估指标：下一个单词预测损失，但是loss函数并不能反映其在具体下游任务上的性能表现，而
speed of grokking模型获得特定技能的速度是一个不错的选择替代指标。  
*（2）数据因素对speed of grokking的影响*  
&emsp;&emsp;分析mix ratio（数据混合比例）+ data format（数据格式） + data curriculum（数据教程）+数据规模对speed of grokking的影响。  
--Format of data(训练数据格式)对模型的影响。数据说明：plain(没有任何COT中间结果)、reverse（倒过来）、simplified scratchpad（提供部分中间COT
推理结果作为训练数据）、detailed scratchpad（提供详细的COT推理结果作为训练数据）；结论、利用越详细的COT中间结果来训练模型，模型学习的速度越快。  
--Cirriculum of data（数据课程：按照一定的课程顺序编排训练数据，使模型学到的效果最佳）。叠加多种类型的数据，按照一定顺序来训练模型，可能比只在单一
任务上的训练效果更好，收敛速度更快。  
--Mix ratio（各部分数据比例对模型的影响）。好的混合比例pile提高了模型的表现，使其有更好的学习曲线，让模型能给更快的从数据中进行学习。  
-- Caveat：model scaling（模型尺寸大小对数据工程的影响）。代码数据对小模型像7B模型的推理能力可能有一定帮助，但对大模型70B就没有帮助了。  
参考文献：  
[1] An Initial Exploration of Theoretical Support for Language Model Data Engineering  

**Part 2 InstructTuning**  
