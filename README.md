# 360LayoutAnalysis

[English](./README_EN.md)

## 一、背景

在当今数字化时代，文档版式分析是信息提取和文档理解的关键步骤之一。文档版式分析，也称为文档图像分析或文档布局分析，是指从扫描的文档图像中识别和提取文本、图像、表格和其他元素的过程。这项技术在自动化文档处理、电子数据交换、历史文档数字化等领域有着广泛的应用。

传统的文档版式分析模型往往难以准确区分文档中的段落和其他布局元素，这限制了文档信息的进一步处理和利用，而深度学习和模式识别技术的发展为文档版式分析带来了新的机遇，通过训练数据集，可以提高模型对文档结构的理解能力，但高质量的标注数据集是训练有效模型的基础。

在文档版式分析中，精细化的标注非常有必要，其中：段落的标注尤其关键，因为它直接影响到文本的语义理解和信息提取。当前，在版式分析领域，据我们了解，在论文场景中，以往的开源数据集如：CDLA（A Chinese document layout analysis），缺乏对段落信息的标注；在研报场景中的版式分析模型还相对空缺。

因此，为了解决这一问题，我们通过人工标注的方式对论文文档进行细粒度标签改造以及数据优化，并构建起研报场景细粒度版式分析数据集，最好利用这些标注数据集，训练了多个全新的中文文档版式分析模型，在**封闭测试集上表现优异**。

2024-06-15，我们优先开源了面向**论文**和**研报**两个场景的版面分析轻量化模型权重及相应的标签体系，旨在能够识别文档中的段落边界等信息，并准确区分文本、图像、表格、公式等其他元素，最终推动产业发展。

2024-06-28，新增**英文论文场景、通用场景**两个新版式分析模型，开源版式分析模型达到4个。

2024-09-06，新增**中文教材场景**版式分析模型，开源版式分析模型达到5个。

主要特点：

1)涵盖中文论文、英文论文、中文研报、教材4个垂直领域及1个通用场景模型；

2)轻量化推理快速【基于yolov8训练，单模型6.23MB】；

3)中文论文场景包含段落信息【CLDA不具备段落信息，我们开源独有】；

4)中文研报场景/通用场景/教材场景【基于数万级别高质量数据训练，我们开源独有】

**如果有更多的使用体验，欢迎向我们反馈。**

## 二、使用

- 权重下载地址：[🤗LINK](https://huggingface.co/qihoo360/360LayoutAnalysis)
    



| 模型权重                                                     | 场景         | 标签类别                                                     | 开源时间   |
| ------------------------------------------------------------ | ------------ | ------------------------------------------------------------ | ---------- |
| [paper-8n.pt](https://huggingface.co/qihoo360/360LayoutAnalysis/blob/main/paper-8n.pt) | 中文论文场景 | Text：正文（段落）<br/>Title：标题<br/>Figure：图片<br/>Figure caption：图片标题<br/>Table：表格<br/>Table caption：表格标题<br/>Header：页眉<br/>Footer：页脚<br/>Reference：注释<br/>Equation：公式 | 2024-06-15 |
| [report-8n.pt](https://huggingface.co/qihoo360/360LayoutAnalysis/blob/main/report-8n.pt) | 中文研报场景 | Text：正文（段落）<br/>Title：标题<br/>Figure：图片<br/>Figure caption：图片标题<br/>Table：表格<br/>Table caption：表格标题<br/>Header：页眉<br/>Footer：页脚<br/>Toc：目录 | 2024-06-15 |
| [publaynet-8n.pt](https://huggingface.co/qihoo360/360LayoutAnalysis/blob/main/publaynet-8n.pt) | 英文论文场景 | text：正文<br/>title：标题<br/>list：列表<br/>table：表格<br/>figure：图片 | 2024-06-28 |
| [general6-8n.pt](https://huggingface.co/qihoo360/360LayoutAnalysis/blob/main/general6-8n.pt) | 通用场景     | 元素：名称<br/>Text：正文<br/>Title：标题<br/>Figure：图片<br/>Table：表格<br/>Equation：公式<br/>Caption：表/图标题 | 2024-06-28 |
| [jiaocai-8n.pt](https://huggingface.co/qihoo360/360LayoutAnalysis/blob/main/jiaocai-8n.pt) | 教材场景     | 目录：catalogue<br/>页脚：footer<br/>脚注：footnote<br/>章标题：chapter title<br/>小节标题：subsection title<br/>小标题：subhead<br/>段落：paragraph<br/>无序列表：unordered list<br/>表格：table<br/>图片：figure<br/>代码：code<br/>页眉：header<br/>页码：page number<br/>索引标签：index<br/>节标题：section title<br/>标题：headline<br/>其他标题：other title<br/>公式：formula<br/>有序列表：ordered list<br/>表注：table caption<br/>图注：figure caption<br/>代码注释：code caption | 2024-09-06 |





- 使用方式：

  开源权重使用`yolov8`进行训练，预测方式如下：

  ```python
  from ultralytics import YOLO
  
  image_path = ''  # 待预测图片路径
  model_path = ''  # 权重路径
  model = YOLO(model_path)
  
  result = model(image_path, save=True, conf=0.5, save_crop=False, line_width=2)
  print(result)
  
  print(result[0].names)         # 输出id2label map
  print(result[0].boxes)         # 输出所有的检测到的bounding box
  print(result[0].boxes.xyxy)    # 输出所有的检测到的bounding box的左上和右下坐标
  print(result[0].boxes.cls)     # 输出所有的检测到的bounding box类别对应的id
  print(result[0].boxes.conf)    # 输出所有的检测到的bounding box的置信度
  ```

  

## 三、版面分析

### 3.1 论文场景

- 标签类别

  | 元素           | 名称         |
  | -------------- | ------------ |
  | Text           | 正文（段落） |
  | Title          | 标题         |
  | Figure         | 图片         |
  | Figure caption | 图片标题     |
  | Table          | 表格         |
  | Table caption  | 表格标题     |
  | Header         | 页眉         |
  | Footer         | 页脚         |
  | Reference      | 注释         |
  | Equation       | 公式         |

- 示例

<div align="center">
    <img src="./case/paper/1.jpg" width="50%" height="50%">
    <img src="./case/paper/2.jpg" width="50%" height="50%">
</div>




### 3.2 研报场景

- 标签类别

  | 元素           | 名称         |
  | -------------- | ------------ |
  | Text           | 正文（段落） |
  | Title          | 标题         |
  | Figure         | 图片         |
  | Figure caption | 图片标题     |
  | Table          | 表格         |
  | Table caption  | 表格标题     |
  | Header         | 页眉         |
  | Footer         | 页脚         |
  | Toc            | 目录         |

  

- 示例

  <div align="center">
      <img src="./case/report/1.jpg" width="50%" height="50%">
      <img src="./case/report/2.jpg" width="50%" height="50%">
  </div>


### 3.3 publaynet

- 标签类别

  | 元素   | 名称 |
  | ------ | ---- |
  | text   | 正文 |
  | title  | 标题 |
  | list   | 列表 |
  | table  | 表格 |
  | figure | 图片 |

  

- 示例

  <div align="center">
      <img src="./case/publaynet/case1.jpg" width="50%" height="50%">
      <img src="./case/publaynet/case2.jpg" width="50%" height="50%">
  </div>

### 3.4 通用版式

- 标签类别

  | 元素     | 名称 |
  | -------- | ---- |
  | Text     | 正文 |
  | Title    | 标题 |
  | Figure   | 图片 |
  | Table    | 表格 |
  | Equation | 公式 |
  | Caption  | 表/图标题 |



## License

This project utilizes certain datasets and checkpoints that are subject to their respective original licenses. Users must comply with all terms and conditions of these original licenses.The content of this project itself is licensed under the [Apache license 2.0](./LICENSE.txt).



## 许可证

本仓库源码遵循开源许可证Apache 2.0。360LayoutAnalysis模型开源模型支持商用，若需将本模型及衍生模型用于商业用途，请通过邮箱([360ailab-nlp@360.cn](mailto:360ailab-nlp@360.cn))联系进行申请， 具体许可协议请见[《360LayoutAnalysis模型开源模型许可证》](./360LayoutAnalysis开源模型许可证.txt)。
