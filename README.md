# Bilibili 评论情感分析项目

## 项目简介

本项目旨在通过Python及其相关数据处理和可视化库，对Bilibili平台的评论数据进行情感分析、数据预处理、可视化展示等操作，以挖掘评论中的情感倾向和热点话题等信息。

## 环境配置

### 硬件环境

- 本项目对硬件要求不高，普通的个人电脑即可运行。推荐配置：
  - CPU：Intel Core i5 或以上
  - 内存：8GB 或以上
  - 硬盘：至少 50GB 可用空间

### 软件环境

- **操作系统**：Windows 10 或以上，推荐使用 Windows 11
- **Python**：Python 3.12.4
- **依赖库**：
  - pandas：1.4.2
  - numpy：1.21.5
  - seaborn：0.11.2
  - jieba：0.42.1
  - wordcloud：1.8.1
  - matplotlib：3.5.1
  - scikit-learn：（如果使用机器学习方法进行情感分析时需要）

### 安装步骤

1. **安装Python**：
   - 访问 [Python官网](https://www.python.org/downloads/)，下载并安装Python 3.12.4。
   - 安装过程中，确保将Python添加到系统环境变量中。

2. **安装依赖库**：
   - 打开命令行工具（如cmd或PowerShell），运行以下命令安装所需的依赖库：
     ```bash
     pip install pandas==1.4.2 numpy==1.21.5 seaborn==0.11.2 jieba==0.42.1 wordcloud==1.8.1 matplotlib==3.5.1 scikit-learn
     ```
   - 如果安装过程中遇到网络问题，可以尝试使用国内的镜像源，例如：
     ```bash
     pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pandas==1.4.2 numpy==1.21.5 seaborn==0.11.2 jieba==0.42.1 wordcloud==1.8.1 matplotlib==3.5.1 scikit-learn
     ```

3. **安装Git**（如果需要从GitHub克隆项目）：
   - 访问 [Git官网](https://git-scm.com/downloads)，下载并安装Git。

4. **克隆项目**（如果从GitHub获取项目）：
   - 打开命令行工具，运行以下命令克隆项目到本地：
     ```bash
     git clone https://github.com/your-username/your-repo-name.git
     ```
   - 替换`your-username`和`your-repo-name`为你的GitHub用户名和项目仓库名称。

## 使用说明

### 数据准备

1. **评论数据文件**：
   - 将Bilibili评论数据保存为CSV格式，文件名为`my_bilibili_comment.csv`，并将其放置在`D:/bilibili_comment/`目录下。
   - 确保CSV文件包含以下列：评论内容、评论时间、评论者名称、评论者id、评论者主页、评论者性别、评论者头像、评论者等级、点赞数、回复数、视频总评论数、视频链接、一级评论id、评论类型。

2. **停用词文件**：
   - 准备一个包含常见中文停用词的文件，文件名为`stopwords.txt`，每行一个词。
   - 将该文件放置在项目的根目录下。

### 运行代码

1. **打开命令行工具**：
   - 打开命令行工具（如cmd或PowerShell）。

2. **切换到项目目录**：
   - 切换到项目的根目录，例如：
     ```bash
     cd path/to/your-project-directory
     ```

3. **运行Python脚本**：
   - 运行以下命令执行Python脚本：
     ```bash
     python bilibili_comment_analysis.py
     ```
   - 如果脚本名称不是`bilibili_comment_analysis.py`，请替换为实际的脚本文件名。

4. **查看结果**：
   - 运行脚本后，程序将输出数据预处理的结果、情感分析的结果以及各种可视化图表。
   - 生成的可视化图表将直接显示在屏幕上，部分结果也会打印在命令行中。

### 代码功能说明

1. **数据预处理**：
   - 读取CSV文件中的评论数据。
   - 查看数据集的基本信息，包括前几行数据、列名和数据类型、统计信息、唯一值数量、缺失值数量。
   - 删除含有缺失值的行。
   - 查看数值型列的异常值情况，并删除异常值。
   - 转换评论时间为日期格式，并提取评论日期。
   - 保存预处理后的数据为新的CSV文件`bilibili_comments_cleaned.csv`。

2. **情感分析**：
   - 使用简单的关键词匹配规则对评论进行情感分析，判断评论的情感倾向为积极、消极或中性。
   - 统计情感倾向的分布，并绘制情感分布饼图和柱状图。

3. **评论时间趋势分析**：
   - 按天统计评论数量，并绘制评论时间趋势折线图，观察评论热度的起伏变化。

4. **用户评论行为分析**：
   - 统计用户评论数量分布，并绘制用户评论数量分布柱状图，展示评论数量最多的前20个用户的评论贡献。

5. **热点话题分析**：
   - 使用中文分词工具`jieba`对评论内容进行分词。
   - 去除停用词和特殊符号。
   - 生成热点话题词云图，直观呈现评论中出现频率较高的热点词汇。

6. **用户属性分析**：
   - 统计评论者性别分布，并绘制评论者性别分布饼图。
   - 统计评论者等级分布，并绘制评论者等级分布柱状图。

7. **互动指标分析**：
   - 绘制点赞数、回复数、视频总评论数的分布直方图，并添加核密度估计（KDE）曲线，展示这些指标的分布范围和集中趋势。

## 注意事项

1. **数据隐私**：
   - 请确保你使用的评论数据符合相关法律法规和平台的使用条款，不要涉及侵犯他人隐私或版权的内容。

2. **文件路径**：
   - 根据你的实际文件存放位置，修改代码中的文件路径，例如`D:/bilibili_comment/my_bilibili_comment.csv`和`stopwords.txt`的路径。

3. **Python版本**：
   - 本项目基于Python 3.12.4开发，建议使用相同版本的Python运行代码。如果你使用的是其他版本的Python，可能会遇到兼容性问题。

4. **依赖库版本**：
   - 请确保安装的依赖库版本与项目中使用的版本一致，以避免潜在的兼容性问题。

5. **可视化图表**：
   - 如果在运行过程中遇到可视化图表显示不正常的情况，可以尝试调整`plt.rcParams`中的字体设置，或者更新`matplotlib`库到最新版本。

6. **情感分析规则**：
   - 本项目使用的是简单的关键词匹配规则进行情感分析，这种方法相对简单但可能存在一定的局限性。你可以根据实际情况调整情感分析规则，或者尝试使用更先进的自然语言处理模型进行情感分析。

## 项目结构

```
bilibili_comment_analysis/
│
├── bilibili_comment_analysis.py  # 主程序脚本
├── my_bilibili_comment.csv        # Bilibili评论数据文件
├── stopwords.txt                  # 停用词文件
├── bilibili_comments_cleaned.csv  # 预处理后的数据文件（运行后生成）
└── README.md                      # 项目说明文档
```




希望这份`README.md`文档对你有所帮助！如果你还有其他需求或需要进一步修改，请随时告诉我。











































