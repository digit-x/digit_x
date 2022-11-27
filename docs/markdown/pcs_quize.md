> Created on Tue Aug  9 09:04:22 2022  @author: Richie Bao-caDesign设计(cadesign.cn)

# 测验-练习

网络上python相关内容异常丰富，虽然线下解释器同样可以交互，但增加有评价、评分系统，更广泛社区参与的在线交互的方式会让学习过程变的更有乐趣。因此，在完成PCS后的测验练习部分采取两种途径的结合，一是自主命题测验；二是网络资源筛选练习测验。对于网络资源删选，可能会不定期更新资源，这出于多种原因：1是网络资源可能不稳定，某些资源有一天不存在；2是有更好的资源可替代，优化测验练习内容；3是更新测试练习内容结构等。

同时，在测验-练习部分，划分为必做部分和选做部分。必做部分为必须测验或练习的部分；选做部分为，根据自身情况，例如时间精力或者是否有必要而自行选择是否练习。

## A.部分-1-PCS_1-6

1. [__必做__]完成[CodingBat](https://codingbat.com/python)练习，包括：Warmup-1, Warmup-2, String-1,String-2,List-1,List-2,Logic-1,Logic-2。`free`

      时间节点：为完成PCS_6学完之前。

2. [__必做__]从[matplotli案例](https://matplotlib.org/stable/gallery/index.html)中选择一个图表类型，自选或自定义数据结构，并给出样例数据，定义可以自由配置所选图表样式的函数。结果形式参考`PCS_5_[5.5 函数定义综合实验-自定义箱型图打印样式]`，但不唯一。

      时间节点：PCS_5完成后开始练习。

3. [选做]可以自由跟随[Learn X](https://www.learnx.org/)练习。`free`


## B.部分-2-PCS_7

1. 在[pypi](https://pypi.org/)上注册自己的账户，用于包的发布；

2. 参考PCS_7，按组实验，每组建立一个数据分析的项目，项目内容自行根据自身情况设定，项目组成员应该都有贡献的模块或函数工具在其中。最低要求：包含至少3个子包，每个子包下有2-3个模块，每个模块下包含至少3组函数工具。需要有测试文件，位于`test`文件夹下，测试内容为：打包推送至`pypi`，并安装成功，测试模块函数，至少每个模块测试一组。


## C.部分-3-PCS_8-9

结合自己专业或相关应用，自行选择问题（待实现的功能），并定义为类。将自定义类作为模块置于`练习-B`部分的自定义包中。


## 综合

结合`A,B,C`部分练习，完善[pypi](https://pypi.org/)上的包功能，同时发布到[GitHub](https://github.com/)代码托管仓库中。自行扩充模块方法，完成一个比较系统的包，并用`markdown`书写包说明，包含模块方法功能的内容，实现的途径说明，和相关解释案例。

---

## 考核方式：

按组**在线公开答辩**。时间周期：`2022.11.15-2022.12.15`，每周至少答辩3组（可以同一时间段顺序答辩，亦可各自不同时间段分别答辩），由课程负责人和各组协调安排。具体答辩时间确定后，与任课老师确认。

* 答辩内容：

1. 期中（平时）部分（应已作为模块融入到期末Python包中，均为个人部分），为A-1，A-2。
2. 期末部分（由个人部分按组建立Python包，含期中），为B，C->综合。

> 小组各自组织好报告流程，确定不同成员汇报内容和确保衔接的流畅性。

* 具体提问内容：

1. 期中-`A-1-CodingBat练习`，答辩时，1. 个人报告是否全部完成，或完成情况；2. 教师从该练习中随机抽取一个练习提问；-->计入期中成绩（50分）。
2. 期中-`A-2-matplotli案例函数`，答辩时，1.个人报告，教师提问；-->计入期中成绩（50分）。
3. 期末，小组报告+自定义Python库（包）案例演示，教师提问（包含小组整体情况，和小组各成员成果); -->计入期末成绩（含总体成绩（50分） + 个人成绩（50分））

> 到课情况按教务要求占10分（10%）。最后综合得分按教务要求比例计算。

* 注意：

1. 未参加公开答辩，或没有完成相关部分，该部分记0分；
2. 答辩时，对自己所写代码一无所知，该部分记0分；
3. 答辩时，即时敲定成绩，无合理特殊原因（满足学校教务处要求）不再调整；
4. 任何质疑请与任课教师联系沟通。

* 代码文件与提交

1. 代码文件输出为PDF格式文档，开始行注明小组，小组成员信息（姓名，序号、班级等）和自定义Python库安装地址（例如`pip install toolkit4beginner`），并上传至教务处OR系统，由教师查看，根据答辩情况批阅。
2. 代码原始.py文件，如果OR系统不限制文件类型，仍需打包上传至OR系统。

## 评分标准

| 部分/内容  |  1-完成度 | 2-提问  |  3-PyPI | 4-到课  | 合计  | 小计 |  备注  |
|---|---|---|---|---|---|---|---|
|  平时：CodingBat部分 |  60分（76个，每个76/60分） |  15分（随机抽取一个提问） |   |   |  75分 |   |   |
| 平时：自定义matplotly函数  |   |15分   |   |   | 15分  |   |   |
| 平时：到课  |   |   |   |  10分 |  10分 |   |   |
|  平时合计 |   |   |   |   |   |  100分 |   |
|  期末 | 60分（每人至少1个包，该包含2-3个模块，每个模块下至少3个自定义函数（含测试文件），计每人至少6个函数或类）  |  30分（每人3个问题，一个问题10分） | 10分（是否传至PyPI，并可安装成功）  |   | 100分 |   |   |
| 期末合计  |   |   |   |   |  |  100分  |   |

> 总分=平时×50%+期末×50%

__CodingBat问答提取随机选择__

<gradio-app space="richiebao/codingbat_random_selection"></gradio-app>

__在线计算__

<gradio-app space="richiebao/scoring_criteria"></gradio-app>

__线下计算__

下载`JupyterLab`文件：<a href="./download/scoring_criteria.ipynb" title="scoring criteria" download>scoring criteria</a>

__计算代码__

`scoring_criteria`
```python
class ScoringCriteria:
    """
    Python数字设计编程基础————得分计算
    """
    def __init__(self):
        pass
    
    def score_calculation(self,num,num_total,score_total,decimal=3):
        return round(num*score_total/num_total,decimal)
    
    def usual_codingbat_completion(self,num_completed,num_total=76,score_total=60):
        score=self.score_calculation(num_completed,num_total,score_total)
        print(f"usual codingbat score: {score} (section score: {score_total}), total num: {num_total}")
        return score
    
    def usual_codingbat_QA(self,score,score_total=15):
        print(f"usual codingbat Q&A score: {score} (section score: {score_total})")
        return score
        
    def usual_func_QA(self,score,score_total=15):
        print(f"usual func Q&A score: {score} (section score: {score_total})")
        return score
    
    def usual_class_attendance(self,num,num_total=12,score_total=10):
        score=self.score_calculation(num,num_total,score_total)
        print(f"usual class attendence score: {score} （section score: {score_total}, total num: {num_total})")
        return score
        
    def final_completion(self,num,num_total=6,score_total=60):
        score=self.score_calculation(num,num_total,score_total)
        print(f"final completion score: {score} (section score: {score_total}, total num: {num_total}, total num: {num_total})")
        return score
        
    def final_QA(self,score,score_total=30):
        print(f"final Q&A score: {score} (section score: {score_total})")
        return score
    
    def final_pypi(self,score,score_total=10):
        print(f"final pypy score: {score} (section score: {score_total})")
        return score    

    def total_score(self,num_4_usual_codingbat_completion,score_4_usual_codingbat_QA,score_4_usual_func_QA,num_4_usual_class_attendance,num_4_final_completion,score_4_final_QA,score_4_final_pypi):
        usual_a=self.usual_codingbat_completion(num_4_usual_codingbat_completion)
        usual_b=self.usual_codingbat_QA(score_4_usual_codingbat_QA)
        usual_c=self.usual_func_QA(score_4_usual_func_QA)
        attendance=self.usual_class_attendance(num_4_usual_class_attendance)
        final_a=self.final_completion(num_4_final_completion)
        final_b=self.final_QA(score_4_final_QA)
        final_c=self.final_pypi(score_4_final_pypi)
        
        score_usual=round(usual_a+usual_b+usual_c+attendance,3)
        score_final=round(final_a+final_b+final_c,3)
        
        total_score=round(score_usual*0.5+score_final*0.5,3)
        print(f"usual score: {score_usual}; final score: {score_final}---->total score: {total_score}")
        return f"平时成绩={score_usual}\n期末成绩={score_final}\n总成绩={total_score}"
        
import gradio as gr

sc=ScoringCriteria()

demo=gr.Interface(
                fn=sc.total_score,
                inputs=[
                        gr.Slider(label="平时_CodingBat 完成数量",value=0,minimum=0,maximum=76, step=1),
                        gr.Slider(label="平时_CodingBat 问答分数",value=0,minimum=0,maximum=15, step=1),
                        gr.Slider(label="平时_自定义函数 问答分数",value=0,minimum=0,maximum=15, step=1),
                        gr.Slider(label="平时_到课次数",value=0,minimum=0,maximum=12, step=1),
                        gr.Slider(label="期末_定义函数（类）完成数量",value=0,minimum=0,maximum=6, step=1),
                        gr.Slider(label="期末_问答分数",value=0,minimum=0,maximum=30, step=1),
                        gr.Slider(label="期末_PyPI分数",value=0,minimum=0,maximum=10, step=1),
                        ],    
                outputs=["text"],
                title="Python数字设计编程基础————得分计算",
                description="包括平时成绩（占50%），含4个部分；期末成绩（占50%），含3个部分。总成绩为最终成绩。",
                cache_examples=False,
                )

demo.launch()
```

`random_choice_F_codingbat`
```python
def random_choice_F_codingbat():
    from random import choice

    codingbat_dict={"Warmup-1":12,"Warmup-2":9,"String-1":11,"String-2":6,"List-1":12,"List-2":6,"Logic-1":9,"Logic-2":7}
    key=choice(list(codingbat_dict.keys()))
    val=choice(list(range(1,codingbat_dict[key]+1)))
    print(f"codingbat selection: {key}_{val}")
           
    return f"codingbat selection: {key}_{val}"
    
random_choice_F_codingbat();

import gradio as gr
demo=gr.Interface(fn=random_choice_F_codingbat, inputs=None, outputs="text")
demo.launch() 
```