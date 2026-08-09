## HUST-SEEE Beamer 模板

本模板按照 `华中科技大学高压大功率特种电源研究组PPT模板` 的视觉风格绘制，适合课题组开题、中期、组会、进展汇报和学术报告使用。整体风格为白底、深蓝页眉横线、底部蓝色页脚栏、左上角标准彩色校徽，以及提纲页的浅蓝渐变圆角条。

### 功能特性

- 默认按 16:9 宽屏示例编写，也支持随 `aspectratio` 调整页面比例。
- 内置首页、正文页、章节目录页、参考文献页和致谢页样式。
- 自动根据 `\section{...}` 生成总目录，并在章节开始前插入当前章节高亮目录页。
- 正文页自动使用当前章节名作为页眉标题，也支持逐页自定义 frame 标题。
- 内置图、表、公式、代码、算法伪代码、语义重点块和参考文献展示环境，便于统一学术汇报排版。
- 内置项目符号列表和编号列表：标记形状统一，颜色与字号默认继承所在正文或语义块，也可逐处覆盖。
- 示例主文件提供双栏图文、并列子图和时间计划/甘特式进度页面，可直接复制后替换为实际内容。
- 正文内容环境支持 `compact`、`dense` 与字号/行距/留白微调参数，便于处理长标题、4:3 页面和内容较多的页。
- 使用 `biblatex` + `biber` 管理参考文献，普通 `\cite` 保持行内格式，上标引用使用 `\uppercite`。

### 快速开始

1. 进入模板目录：

   ```bash
   cd HUST-SEEE-Beamer
   ```

2. 编辑首页信息：

   ```tex
   \title[汇报主题]{汇报主题}
   \author{汇报人：xxx}
   \institute{华中科技大学高压大功率特种电源研究组}
   \date{20xx年x月x日}
   ```

   方括号中的短标题会用于正文页页脚，建议保持简洁，避免长标题挤占页脚空间。

3. 使用 XeLaTeX 与 Biber 编译：

   ```bash
   latexmk -xelatex HUST_SEEE_Beamer_Template.tex
   ```

4. 若不需要参考文献，可删除正文中的 `\cite{...}`、`\uppercite{...}`、`refsegment` 和 `\HUSTReferencesFrame`，再直接用 XeLaTeX 编译。

### 文件说明

- `HUST_SEEE_Beamer_Template.tex`：示例主文件，包含首页、目录、正文、图表公式、代码算法、参考文献和致谢页。
- `hustseee.sty`：模板样式文件，定义页面背景、字体、页眉页脚、目录、引用、代码高亮和内容 block。
- `HUST_SEEE_Beamer_Template.bib`：示例参考文献数据库，示例主文件通过 `\HUSTAddBibResource{...}` 显式读取。
- `res/figs/标准彩色校徽文件.png`：左上角与首页使用的校徽资源。
- `res/fonts/STZhongsong.ttf`、`res/fonts/SimHei.ttf`、`res/fonts/msyh.ttf`、`res/fonts/TimesNewRoman.ttf`：模板字体文件。
- `res/ref/ieee-transactions-on-power-electronics.csl`：IEEE TPEL 引用样式参考文件；模板实际使用 `biblatex` 的 IEEE 风格实现。

### 基本写作流程

建议保留示例主文件的整体结构，只替换标题信息、章节标题和每页内容：

```tex
\begin{document}

\HUSTTitleFrame

\begin{frame}{汇报提纲}
  \HUSTOutline
\end{frame}

\section{课题研究背景}
\begin{frame}
  ...
\end{frame}

\HUSTReferencesFrame
\HUSTThanksFrame

\end{document}
```

正文中用 `\section{...}` 组织一级汇报提纲；每个 `section` 开始前会自动插入当前章节高亮的目录页。除非需要特殊控制，一般不需要手动创建章节过渡页。

若需要从空文件开始，最小主文件可写为：

```tex
% !TeX program = xelatex
\documentclass[10pt,aspectratio=169]{beamer}

\usepackage[UTF8]{ctex}
\usepackage{hustseee}
\usepackage{amsmath,amsfonts,amssymb,bm}
\usepackage{graphicx,hyperref,url}
\usefonttheme[onlymath]{serif}

\title[汇报主题]{汇报主题}
\author{汇报人：xxx}
\institute{华中科技大学高压大功率特种电源研究组}
\date{20xx年x月x日}

\begin{document}
\HUSTTitleFrame

\begin{frame}{汇报提纲}
  \HUSTOutline
\end{frame}

\section{课题研究背景}
\begin{frame}
\begin{HUSTRequirementFrame}
\HUSTRequirementHeading{基本要求：}\\
1. \quad 阐明课题研究背景。\\
2. \quad 突出课题研究意义。
\end{HUSTRequirementFrame}
\end{frame}

\HUSTThanksFrame
\end{document}
```

### 编译方式

推荐使用包含 XeLaTeX、Biber 和 latexmk 的 TeX 发行版，并从 `HUST-SEEE-Beamer` 目录内编译，确保 `res/` 下的字体、校徽和参考样式资源能被正确找到。

示例主文件使用 `\documentclass[10pt,aspectratio=169]{beamer}`。模板会根据当前 beamer 页面比例重新计算页面宽度和主要装饰位置；如需 4:3，可改为：

```tex
\documentclass[10pt,aspectratio=43]{beamer}
```

16:9 是推荐比例，4:3 下会保持同样的视觉元素和相对位置，但可用横向空间更少，长标题、宽表格和复杂图片需要相应压缩。

```bash
latexmk -xelatex HUST_SEEE_Beamer_Template.tex
```

若手动编译，需要按以下顺序运行，以生成引用与参考文献：

```bash
xelatex HUST_SEEE_Beamer_Template.tex
biber HUST_SEEE_Beamer_Template
xelatex HUST_SEEE_Beamer_Template.tex
xelatex HUST_SEEE_Beamer_Template.tex
```

清理辅助文件可使用：

```bash
latexmk -c HUST_SEEE_Beamer_Template.tex
```

### 字体与资源

样式文件会自动优先识别下列字体文件：

- 华文中宋：`res/fonts/STZhongsong.ttf` 或 `res/fonts/STZHONGS.TTF`
- 黑体：`res/fonts/SimHei.ttf` 或 `res/fonts/simhei.ttf`
- 微软雅黑：`res/fonts/Yahei.ttf`、`res/fonts/msyh.ttf` 或 `res/fonts/msyh.ttc`
- Times New Roman：`res/fonts/times.ttf` 或 `res/fonts/TimesNewRoman.ttf`
- 参考文献英文字体：优先使用 `res/fonts/Arial.ttf` 或系统 Arial，若不存在则使用默认无衬线字体。

若之后替换字体，只需把对应字体文件放入 `res/fonts/`，重新编译即可。校徽默认使用 `res/figs/标准彩色校徽文件.png`；若只替换文件内容，可以保持文件名不变；若想使用不同文件名或路径，可在导言区加入：

```tex
\HUSTSetLogo{res/figs/my-logo.png}
```

Logo 路径应使用相对路径，不建议使用绝对路径。

### 首页、目录与致谢页

首页和致谢页是与正文页不同的特殊页面，建议使用模板内置宏，而不是在主文件中手动叠加背景和页脚设置：

```tex
\HUSTTitleFrame
...
\HUSTThanksFrame
```

这两个宏会自动取消正文页左上角校徽、页眉短横线等装饰，并将校徽置于页面上方居中位置。首页标题后方会生成横贯整页的蓝色底纹，底纹高度约为标题文字高度的 1.3 倍，并可随多行标题自适应；汇报人、单位和日期保持固定位置，不会随标题行数下移。首页还会取消底部蓝色页脚栏，避免页脚标题与大标题重复。致谢页保留页脚页码以保持汇报页序连续。

首页、正文页左上角和致谢页默认共用 `\HUSTLogoPath` 指向的校徽文件；使用 `\HUSTSetLogo{...}` 后三处会同步替换。

目录页使用 beamer 标准的 `\section{...}` 生成目录信息，并用自定义样式绘制圆圈编号和渐变胶囊底纹：

```tex
\begin{frame}{汇报提纲}
  \HUSTOutline
\end{frame}
```

如需关闭自动章节目录页，可在导言区加入：

```tex
\HUSTDisableSectionOutline
```

目录样式会根据章节数量自动调整字号和行距；5 节以内保持原 PPT 的大字号风格，6 节以上自动压缩以适应页面。较长章节标题会在目录胶囊内自动换行，但仍建议目录标题尽量短。

### 页眉与页脚

正文页若不显式填写 frame 标题，会自动使用当前 `\section{...}` 作为页眉标题，例如：

```tex
\section{课题研究背景}
\begin{frame}
  ...
\end{frame}
```

若某一页需要自定义页眉标题，使用 `\begin{frame}{自定义标题}`；若某一页不需要页眉标题，使用 `\begin{frame}{}`。页眉标题支持自动换行，但建议控制在一到两行内。

普通正文页默认采用顶部对齐，使正文紧随页眉横线下方开始，避免 Beamer 默认垂直居中造成标题与正文之间留白过大。若个别纯文字页面确实需要垂直居中，可为该页显式使用 `\begin{frame}[c]{标题}`。

页脚中部自动使用首页 `\title[短标题]{完整标题}` 中的短标题；若未设置短标题，beamer 会回退使用完整标题。因此长题目建议始终填写短标题，例如：

```tex
\title[高压脉冲电源控制]{面向高重复频率工况的高压大功率脉冲电源控制策略研究}
```

### 常用页面环境

模板提供两类正文内容环境：

- `HUSTRequirementFrame`：适合“基本要求”“小结”“文字提纲”等大字号文字页，默认正文 28pt，左右留白较宽。
- `HUSTContentFrame`：适合图、表、公式和图文混排页，默认正文 22pt，便于放置结果展示内容。

模板内置页内标题宏，用于拉开“小标题—正文”的视觉层级：

- `\HUSTRequirementHeading{...}`：文字要求页/小结页标题，32pt 黑体蓝色加粗。
- `\HUSTContentHeading{...}`：内容展示页中的页内标题，26pt 黑体蓝色加粗。

这两类环境都支持可选参数。为满足正文最小字号不低于 22pt 的要求，`compact` 和 `dense` 只压缩行距、页边距并向上下边界扩展正文区域，不会把正文字号降到 22pt 以下；如需手动指定字号，建议不要低于 22pt：

```tex
\begin{HUSTRequirementFrame}[compact]
  ...
\end{HUSTRequirementFrame}

\begin{HUSTContentFrame}[dense]
  ...
\end{HUSTContentFrame}

\begin{HUSTContentFrame}[fontsize=22,baselineskip=28,top=-.08in,bottom=-.06in]
  ...
\end{HUSTContentFrame}
```

`HUSTRequirementFrame` 还可额外设置 `left` 和 `right` 控制左右留白：

```tex
\begin{HUSTRequirementFrame}[fontsize=24,baselineskip=32,left=.07\paperwidth,right=.04\paperwidth]
  ...
\end{HUSTRequirementFrame}
```

默认值与快捷模式如下，字号和行距单位均为 pt：

| 环境 | 模式 | 字号 | 行距 | 顶部/底部留白 | 其他 |
| --- | --- | ---: | ---: | --- | --- |
| `HUSTRequirementFrame` | 默认 | 28 | 39 | `.08in` / — | 左 `.08625\paperwidth`，右 `.0525\paperwidth` |
| `HUSTRequirementFrame` | `compact` | 24 | 32 | `.03in` / — | 左 `.070\paperwidth`，右 `.040\paperwidth` |
| `HUSTRequirementFrame` | `dense` | 22 | 29 | `.02in` / — | 左 `.0525\paperwidth`，右 `.035\paperwidth` |
| `HUSTContentFrame` | 默认 | 22 | 30 | `-.08in` / `-.06in` | 上下扩展正文可用区域 |
| `HUSTContentFrame` | `compact` | 22 | 29 | `-.10in` / `-.08in` | 更紧凑 |
| `HUSTContentFrame` | `dense` | 22 | 27 | `-.12in` / `-.10in` | 最大化正文区域 |

文字要求页示例：

```tex
\begin{frame}
\begin{HUSTRequirementFrame}
\HUSTRequirementHeading{基本要求：}\\
1. \quad 阐明课题研究背景，突出课题研究意义。\\
2. \quad 目标导向明确，主线清晰，自然衔接研究主题。
\end{HUSTRequirementFrame}
\end{frame}
```

内容展示页示例：

```tex
\begin{frame}
\begin{HUSTContentFrame}
  \begin{itemize}
    \item 适合放置图表、公式、实验结果和简短说明。
    \item 建议每页只突出一个核心结论。
  \end{itemize}
\end{HUSTContentFrame}
\end{frame}
```

### 项目符号与编号列表

标准 `itemize` 和 `enumerate` 可直接使用，主题只统一标记形状，不固定正文颜色和字号：

- `itemize` 依次使用实心圆、空心圆和方块表示三级层次；
- `enumerate` 使用简洁的 `1.`、`2.`、`3.` 数字标记；
- 字号继承所在的 `HUSTContentFrame`、局部 `\fontsize` 或语义块；
- 颜色继承 Beamer 的 `local structure`。在 `HUSTKeyPointBlock`、`HUSTAlertBlock` 和 `HUSTConclusionBlock` 中会分别跟随青色、红色和绿色。

```tex
\begin{itemize}
  \item 一级结论
  \begin{itemize}
    \item 二级说明
  \end{itemize}
\end{itemize}

\begin{enumerate}
  \item 故障检测
  \item 电流限制
  \item 故障后恢复
\end{enumerate}
```

需要统一控制列表间距时，推荐使用 `HUSTBulletList` 和 `HUSTNumberList`。它们同样默认继承颜色与字号：

```tex
\begin{HUSTBulletList}[compact]
  \item 电流型限流
  \item 电压型限流
\end{HUSTBulletList}

\begin{HUSTNumberList}[compact,start=2]
  \item 第二阶段
  \item 第三阶段
\end{HUSTNumberList}
```

列表提供三种间距模式：`normal`、`compact` 和 `loose`；兼容别名 `large` 等同于 `loose`。也可局部设置缩进、间距、颜色和字号：

```tex
\begin{HUSTBulletList}[
  leftmargin=1.4em,
  itemsep=.02in,
  color=HUSTAlertRed,
  fontsize=17,
  baselineskip=21
]
  \item 仅当前列表使用显式样式
\end{HUSTBulletList}
```

一般不建议同时指定颜色和字号；只有当列表需要脱离所在页面或信息框的视觉层级时再显式覆盖。

### 图、表、公式、代码与算法样式

模板已内置图、表、公式、代码和算法伪代码展示样式，可在正文页中配合 `HUSTContentFrame` 使用。`HUSTFigureBlock`、`HUSTTableBlock`、`HUSTEquationBlock`、`HUSTCodeBlock`、`HUSTAlgorithmBlock` 的标题均为可选参数；不写标题时不会生成标题区，写标题时标题会作为内容上方的 26pt 居中标题显示。

图形示例：

```tex
\begin{frame}
\begin{HUSTContentFrame}
\begin{HUSTFigureBlock}[图形样式示例]
  \begin{figure}
    \centering
    \includegraphics[width=.78\linewidth]{res/figs/example.png}
    \caption{控制策略下输出电压响应曲线示意图}
  \end{figure}
\end{HUSTFigureBlock}
\end{HUSTContentFrame}
\end{frame}
```

表格示例：

```tex
\begin{HUSTTableBlock}[表格样式示例]
\begin{table}
\centering
\caption{不同方案关键指标对比}
\HUSTLatinFont\HUSTSerifFont\fontsize{22}{29}\selectfont
\begin{tabular}{lccc}
\toprule
方案 & 峰值效率 & 输出纹波 & 动态恢复时间 \\
\midrule
传统 PI 控制 & 92.1\% & 3.8\% & 4.6 ms \\
预测优化控制 & 96.2\% & 1.6\% & 2.2 ms \\
\bottomrule
\end{tabular}
\end{table}
\end{HUSTTableBlock}
```

公式示例：

```tex
\begin{HUSTEquationBlock}[公式样式示例]
\HUSTLatinFont\HUSTSerifFont\fontsize{22}{30}\selectfont
\begin{equation}
  J = \sum_{k=0}^{N-1}\left(\lVert \bm{x}_k-\bm{x}^{\ast}_k\rVert_Q^2
  + \lVert \bm{u}_k\rVert_R^2\right)
\end{equation}
\end{HUSTEquationBlock}
```

算法伪代码示例：

```tex
\begin{HUSTAlgorithmBlock}[算法伪代码示例]
\begin{lstlisting}[language={}]
Input: reference trajectory r, measured state x
Initialize: horizon N, weights Q/R
Predict state sequence
Solve constrained optimization
Apply first control increment
Output: switching command
\end{lstlisting}
\end{HUSTAlgorithmBlock}
```

代码片段示例：

```tex
\begin{HUSTCodeBlock}[代码片段示例]
\begin{lstlisting}[language=Python]
import numpy as np

def rms_error(reference, measured):
    error = np.asarray(reference) - measured
    return np.sqrt(np.mean(error ** 2))
\end{lstlisting}
\end{HUSTCodeBlock}
```

### 语义 Block 与组合页面

除按内容类型划分的 `HUSTFigureBlock`、`HUSTTableBlock` 等环境外，模板还提供三类语义 Block，适合在图文混排页中突出“重点—风险—结论”：

- `HUSTKeyPointBlock`：用于关键发现、核心观点、主要贡献。
- `HUSTAlertBlock`：用于风险提示、约束条件、待验证问题。
- `HUSTConclusionBlock`：用于本页结论、下一步建议、阶段性判断。

基本用法如下：

```tex
\begin{frame}{重点结论 Block 示例}
\begin{HUSTContentFrame}
\begin{HUSTKeyPointBlock}{关键发现}
预测优化控制可同时压低纹波并缩短恢复时间。
\end{HUSTKeyPointBlock}
\vspace{.06in}
\begin{HUSTAlertBlock}{需要关注}
器件结温、采样延迟仍需实验验证。
\end{HUSTAlertBlock}
\vspace{.06in}
\begin{HUSTConclusionBlock}{本页结论}
下一阶段联合优化控制参数与保护策略。
\end{HUSTConclusionBlock}
\end{HUSTContentFrame}
\end{frame}
```

双栏图文页适合“左图右结论”的方法说明、实验平台介绍和单个结果解释。建议左栏放结构图、平台图或波形图，右栏只保留 2--3 条关键判断：

```tex
\begin{frame}{双栏图文示例}
\begin{HUSTContentFrame}
\begin{columns}[T,onlytextwidth]
\begin{column}{.52\textwidth}
\begin{HUSTFigureBlock}[实验平台与信号链路]
  \includegraphics[width=.9\linewidth]{res/figs/example.png}
\end{HUSTFigureBlock}
\end{column}
\begin{column}{.44\textwidth}
\begin{HUSTKeyPointBlock}{页面写法}
\begin{itemize}
  \item 左侧放图，右侧放关键判断。
  \item 结论先行，避免堆叠段落。
\end{itemize}
\end{HUSTKeyPointBlock}
\end{column}
\end{columns}
\end{HUSTContentFrame}
\end{frame}
```

并列子图页适合展示“传统方案 vs 本文方案”“仿真 vs 实验”“工况 A vs 工况 B”。若使用真实图片，可用两个 `minipage` 组织子图，并保证子图标注不低于 22pt：

```tex
\begin{HUSTFigureBlock}[方案对比结果]
\begin{figure}
\centering
\begin{minipage}{.46\linewidth}
  \centering
  \includegraphics[width=\linewidth]{res/figs/result-a.png}
  （a）传统控制
\end{minipage}\hfill
\begin{minipage}{.46\linewidth}
  \centering
  \includegraphics[width=\linewidth]{res/figs/result-b.png}
  （b）预测优化控制
\end{minipage}
\caption{不同控制策略下输出响应对比}
\end{figure}
\end{HUSTFigureBlock}
```

时间计划页适合开题、中期和组会汇报，用于说明“已完成—进行中—计划开展”的里程碑。示例主文件中给出了 TikZ 甘特式进度条；实际使用时建议只保留 3--5 个阶段，保证所有标注不低于 22pt，不要把完整项目管理表格压缩到一页。

排版建议：

- **图形**：保留白底，不额外添加装饰性外框、阴影或大面积底纹；坐标轴、图例、标注应清晰可读。
- **表格**：优先使用 `booktabs` 三线表，仅保留 `\toprule`、`\midrule`、`\bottomrule`，避免竖线和密集网格线。
- **公式**：单行公式使用 `equation`，多行推导使用 `align`；公式中的函数、文字下标和单位使用 `\mathrm{}` 或 `\text{}`。
- **代码/算法**：使用 `lstlisting`，默认代码字号为 22pt 且不显示行号；长代码优先删减到关键逻辑，避免完整源码挤入一页。
- **题注编号**：图表题注默认编号，便于在正文中引用；题注文字应简洁说明对象和条件。
- **字体**：正文、图内标注、表格、代码和算法伪代码均不低于 22pt；参考文献可适当更小。
- **Block 标题**：内容类型 Block 的可选标题为 26pt，不是题注；题注仍用 `\caption{...}`。无标题时写 `\begin{HUSTFigureBlock}`，需要标题时写 `\begin{HUSTFigureBlock}[标题]`，表格、公式、代码和算法同理。
- **语义 Block**：`HUSTKeyPointBlock`、`HUSTAlertBlock`、`HUSTConclusionBlock` 必须填写标题；标题为 24pt，正文为 22pt，正文建议控制在 1--3 行。
- **组合页面**：双栏图文、并列子图和进度页应服务于一个核心结论；如果解释超过三点，优先拆成多页。
- **大小控制**：图像用 `\includegraphics[width=.8\linewidth]{...}`，TikZ 用 `scale` 或 `x/y` 比例；表格用 `\tabcolsep`、`\arraystretch` 或拆分列控制大小，谨慎使用会压低字号的整体缩放。

示例主文件中“已开展的研究工作”章节已给出图、表、公式、算法、代码、语义 Block、双栏图文、并列子图和时间计划页面，可直接复制后替换为实际结果。

### 字号层级

当前模板字号单位均为 pt，正文相关元素不低于 22pt，并通过标题加大形成层级：

| 元素 | 字号 | 说明 |
| --- | ---: | --- |
| 首页标题 | 52 | 首页蓝色标题栏文字 |
| 首页作者 | 36 | 汇报人信息 |
| 首页单位/日期 | 28 | 单位与日期 |
| 正文页眉标题 | 36 | frame 标题/自动章节标题 |
| 页脚短标题/页码 | 22 | 非正文辅助信息 |
| 目录 5 节以内 | 40 | 章节标题与圆圈编号 |
| 目录 6--7 节 | 32 / 34 | 章节标题 / 圆圈编号 |
| 目录 8--10 节 | 24 / 27 | 章节标题 / 圆圈编号 |
| 目录 10 节以上 | 22 / 24 | 章节标题 / 圆圈编号 |
| `HUSTRequirementFrame` 默认正文 | 28 | 要求页、大段文字页 |
| `HUSTRequirementFrame[compact]` | 24 | 内容较多但仍保持强调 |
| `HUSTRequirementFrame[dense]` | 22 | 最紧凑正文模式 |
| `\HUSTRequirementHeading{...}` | 32 | 要求页页内小标题 |
| `HUSTContentFrame` 正文 | 22 | 图文混排页正文 |
| `\HUSTContentHeading{...}` | 26 | 内容页页内小标题 |
| 内容类型 Block 可选标题 | 26 | 图/表/公式/代码/算法块标题 |
| 语义 Block 标题 | 24 | 关键发现/提醒/结论标题 |
| 语义 Block 正文 | 22 | 关键发现/提醒/结论正文 |
| 图表题注 | 22 | `\caption{...}` 编号题注 |
| 代码/算法 | 22 | `lstlisting` 默认字号 |
| 页内参考文献 | 11 | 引用辅助信息，可用 `fontsize=...` 调整 |
| 参考文献页 | 11 | 参考文献列表，允许小于正文 |

### 引用与参考文献

模板默认使用 `biblatex` 的 IEEE 数字引用风格，视觉上对应 `res/ref/ieee-transactions-on-power-electronics.csl` 所指向的 IEEE parent style。`.bib` 文件不需要与主 `.tex` 同名；只需在导言区显式添加实际使用的参考文献库即可，例如：

```tex
\HUSTAddBibResource{HUST_SEEE_Beamer_Template.bib}
\HUSTAddBibResource{references.bib}
```

如果复制模板后把主文件改名为 `main.tex`，无需同步改名 `.bib` 文件，只要把 `\HUSTAddBibResource{...}` 中的路径改为实际文件即可。

模板不再重定义 `\cite`：普通 `\cite{key}` 保持 `biblatex` 的默认行内数字引用格式；需要句末上标引用时，使用模板提供的 `\uppercite{key}` 或兼容别名 `\HUSTCite{key}`。

```tex
方法细节见文献 \cite{sample-tpel-2024}。
该方法适合句末补充来源\uppercite{sample-tpel-2024}。
```

如确实希望整篇文档都把 `\cite` 改为上标格式，可在导言区主动调用：

```tex
\HUSTUseSuperscriptCites
```

可根据汇报需要选择三种参考文献展示方式。

**方式一：页内参考文献 block（推荐）**

某页有引用时，可用 `refsegment` 包住该页内容，并在正文合适位置调用 `\HUSTFrameReferences`。该 block 无标题条，直接显示当前 `refsegment` 中引用过的文献；它会占用正常正文空间，不会覆盖页脚或正文内容。默认页底模式会取消 Beamer 自带的底部弹性空白，并在正文与引用之间保留弹性间距：引用块下边缘贴近蓝色页脚，上边缘作为正文区域的下边界。页内参考文献默认为 RGB `(127,127,127)` 灰色，正文式与兼容式页内参考文献均使用约 `95%` 页宽，使左右边缘更靠近页面边界。

```tex
\begin{frame}
\begin{refsegment}
\begin{HUSTRequirementFrame}
参考某项工作\uppercite{sample-tpel-2024}。
\end{HUSTRequirementFrame}
\HUSTFrameReferences
\end{refsegment}
\end{frame}
```

页内参考文献支持可选参数控制字号、行距和最多显示条数：

```tex
\HUSTFrameReferences[fontsize=10,baselineskip=12,maxitems=2]
```

其中 `maxitems=0` 表示不限制条数。默认页底模式下 `afterskip=0pt`，引用块会直接落在正文区域底部、紧邻蓝色页脚。可用较小的正值将引用略微上移；负值可能侵入页脚区域，一般不建议使用：

```tex
\HUSTFrameReferences[afterskip=.03in]
```

若希望参考文献紧跟正文，而不是自动下推到页底附近，可使用 `inline` 模式；`beforeskip` 可控制正文与参考文献之间的额外间距：

```tex
\HUSTFrameReferences[inline,beforeskip=.08in]
```

**方式二：页内参考文献兼容命令**

旧版文档中的 `\HUSTOverlayFrameReferences` 仍可继续使用，但现在会自动转为与 `\HUSTFrameReferences` 相同的安全页底布局：参考文献占用正常正文空间，并通过弹性留白下推到页脚上方，不再悬浮覆盖正文。

```tex
\HUSTOverlayFrameReferences[fontsize=10,maxitems=2]
```

**方式三：最终参考文献页总 block**

若希望集中列出全文参考文献，可在致谢页之前加入。总参考文献页同样无标题条，正文参考文献采用黑色：

```tex
\HUSTReferencesFrame
\HUSTThanksFrame
```

不要额外写 `\section{参考文献}`，这样单独参考文献页不会计入 `\HUSTOutline` 生成的目录提纲。若文献很多，`\HUSTReferencesFrame` 会使用 `allowframebreaks` 自动续页。参考文献列表默认使用微软雅黑加 Arial，字号为 11pt。

也可以同时采用多种方式：正文页底部展示“本页引用”，文末再展示“全文参考文献”。示例主文件已在“国内外研究现状”页展示页内参考文献 block，并在致谢页前展示总参考文献页。

### 自定义建议

- **修改主题文字**：优先修改主文件的 `\title`、`\author`、`\institute`、`\date` 和各个 `\section`。
- **替换校徽**：可替换 `res/figs/标准彩色校徽文件.png` 的文件内容，也可用 `\HUSTSetLogo{...}` 指向新的相对路径。
- **替换字体**：把新的字体文件放入 `res/fonts/`，并在 `hustseee.sty` 的字体识别区域调整文件名。
- **新增图片**：建议放入 `res/figs/` 或自建相对路径目录，并在 `\includegraphics{...}` 中使用相对路径。
- **调整颜色**：可在 `hustseee.sty` 中修改 `HUSTBlue`、`HUSTDarkBlue`、`HUSTAgendaLeft`、`HUSTAgendaRight` 等颜色定义。

### 常见问题

- **必须使用 XeLaTeX 吗？** 建议使用 XeLaTeX。模板依赖 `ctex`、系统/本地字体和中文排版能力，使用 pdfLaTeX 往往无法正确处理中文字体。
- **为什么参考文献没有显示？** 请确认已用 `\HUSTAddBibResource{...}` 添加实际存在的 `.bib` 文件，并确认编译流程中运行过 `biber`。
- **为什么校徽或字体找不到？** 请从 `HUST-SEEE-Beamer` 目录内编译，并确认 `res/figs/` 与 `res/fonts/` 下的资源文件存在。
- **为什么新增章节没有出现在目录页？** 请使用 `\section{...}`，不要只写普通 frame 标题；目录只显示一级章节。
- **为什么图表过大或溢出页面？** 优先调整 `width`、字号、列距、行距或拆分内容；不要把过多结论堆在同一页。
- **为什么页眉标题不是想要的文字？** 无标题 frame 默认使用当前章节名；需要自定义时写 `\begin{frame}{自定义标题}`，需要空标题时写 `\begin{frame}{}`。

### 写作规范建议

- 每页突出一个核心结论，避免将完整论文段落直接搬到 PPT。
- 图、表、公式必须有清晰题注，关键变量、单位和实验条件要完整。
- 页面最小字号建议不低于 22pt；参考文献和必要脚注可适当更小。
- 章节顺序建议保持“研究背景—研究现状—已开展工作—仿真与实验—小结与计划”。
- 引用文献应服务于论证，不建议在一页中堆叠过多参考文献。
