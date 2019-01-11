
#### 我想写一遍文章

    文档有三个类型：文章(article)，报告（report），书（book）
    中文文章是ctexart，中文字体是UTF8
    说以代码是\documentclass[UTF8]{ctexart} {} 不许写，[]可以不写
    
#### 我开始写一篇文章

    说的方式就是用“宏包”，有了宏包才能 写公式，做表格，放图片
    代码：\usepackage{package1, package1 ,...}
    常用有mathtools，amsmath，graphicx，array
    例如：\usepackage{mathtools,amsmath,graphicx,array}


#### 我正在写文档

    首先建立一个文档环境，这个环境里面只放文档内容，宏包和文档类型都放在外面

    \begin{环境名字}
    你想放在环境中的内容
    \end{环境名字}

    段与段之间要空行

#### 字体的命令和环境

    加粗：\textbf{内容}
    字号：\normalsize \large \Large \LARGE \huge \Huge
    字号大小在一个段内或者{}之间生效
    居中：\begin{center} 内容\end{center}

####标题作者日期

    \title{标题名}
    \author{作者名}
    \date{日期}
    \maketitle

####章节划分
    一级章节\section{章节名}
    二级章节\subsection{章节名}
    三级章节\subsubsection{章节名}

####添加目录
    \tableofcontents
    \newpage  %目录单独站一页

#####添加摘要
    \begin{abstract}
    摘要内容
    \end{abstract}

####类表环境
    \begin{enumerate}
    \item 第一个东西
    \item 第二个东西
    \end{enumerate}

#### 公式写作
    \begin{equation}  % equation* 该部分不编号
    公式
    \end{equation} %这个环境内公式不可以换行
    
    \[  %效果同上，没有编号
    ]\
    
    \begin{align}  %  align* 该部分不编号
    &公式 \\   %  &为对齐位置，可以用\notag 取消该行公式编号
    &公式
    \end{align}  %这个环境内公式能够换行，都可以为公式自动编号

    $公式$ %行内公式
   
   
#### 常见公式
    幂 y^{x}
    下标 x_{下标}
    公式 \frac{分子}{分母}
    求和号 \sum^{上限}_{起始}
    积分号 \int^{上限}_{下限]
    微分号 \mathrm{d}
    
#### 希腊字母和函数

    \alpha
    \beta
    \delta
    \gama
    \pi
    \epsilon
    \rho
    \sigma

    \sin{x}
    \cos{x}
    \tan{x}
    \ln{x}
    \sqrt{x}


#### 矩阵环境  %宏包mathtools

    方括号
    \begin{bmatrix}
    1&2&3\\
    4&5&7\\
    \end{bmatrix}

    大括号
    \begin{cases}
    &x^2
    &x^2+x
    \end{cases}

    %输出\  ,\verb|\|


#### 表格 tabular

    \begin{tabular}{|l|c|r|}   % 竖线
      \hline                      %横行
      指标1&指标2&指标3 \\
      \hline
      居左&居中&居右 \\
      \hline
    \end{tabular}


    三线表  %宏包booktable
    \begin{table}[!htbp]
      \centering
      \caption{题目}
      \begin{tabular}{cccc}
          \toprule        %三条线
          &指标一&指标二& 指标三 \\
          \midrule
          方案一 & 1 & 0.5 & 100 \\
          方案二 & 1 & 0.5 & 100 \\
          \bottomrule
      \end{tabular}
    \end{table}


#### 图片

    \begin{figure}[!htbp]
        \centering
        \includegraphics[height=12cm]{name.png}  %和tex文件同目录
        \capton{标题}
    \end{figure}


#### 标签
    \label{#1,#2}  %第一个参数表示类型，eq 公式， tab 表格 fig 图片
                    %第二个参数表示名字

    \begin{equation}        
    F=ma \label{eq:NiuDun}
    \end{equation}     

    \ref{eq:NiuDun}   %显示NiuDun公式的编号







