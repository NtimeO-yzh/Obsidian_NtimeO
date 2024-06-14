# [Anchor](https://anchorsemi.com/)软件功能分析
## Application
### **D2DB-PM**. [Die to Database Pattern Monitor](https://anchorsemi.com/Products/D2DB-PM/). 图像/数据库模式监控器
As feature sizes shrink well past the limits of optical resolution, the interest and investment in high resolution imaging tools grow significantly. It is not unusual for advanced fabs to employ twenty or more SEM tools and ten or more E-Beam inspection tools. The images they generate are vital to the understanding of yield limiting factors.  

Hundreds of thousands of high resolution images are generated every week – and in some cases every day. But these images are significantly underutilized. Their rich information content is typically reduced to a single class code number, and roughly 50% of the images are classified as SEM Non-Visual (SNV) and discarded.  

When images are reduced to a single class code and half the images are thrown away, there is a huge loss in the potential value of both the images and the imaging tools.  

D2DB-PM not only provides a solution to the problem of image underutilization, but it goes much further by taking Line Monitoring to the next level with the integration of advanced image processing and die-to-database technologies that provide new dimensions and new insights into the yield enhancement workflow. It studies every image in great detail – even if it is an SNV image – and then uploads all of the information into a central database where a historical record of that information is built. The historical record opens a wealth of possibilities for new yield enhancement applications.
#### Features
1. Thorough Analysis of Each Image  彻底分析每张图像
> 通过比对版图设计和原始图像的数据（轮廓数据？）进行分析
2. ==Printed Pattern Database==  印刷图案数据库
> 根据用户定义的规则，提取分析后图像中的不同模式，自动建立数据库
3. Inline Real-Time Pattern Monitor  实时模式监视
> 每次检查和SEM审查周期后，会对该周期内的图像产生一份分析报告，筛查有问题的模式，以便迅速采取措施
4. Seamless Integration into Production  与生产过程无缝集成
> 在不改变现有记录和跟踪计划，不影响生产周期，不产生额外开销的情况下，便捷地集成到现有流程中
5. Zero Waste Policy — Maximize ROI of SEM  零浪费政策--实现 SEM 投资回报率最大化
> 不仅仅把图像简化成类代码，充分提取图像信息（不产生非视觉图像？）

#### Benefits
### **PCYM**. [Pattern Centric Yield Manager](https://anchorsemi.com/Products/page-2/). 模式中心良率管理方法
Each layer of interest in the CAD Layout is decomposed systematically into a collection of unique constituent patterns of a specified maximum size. A poly layer, for instance, will be fully decomposed into its unique constituent patterns.

Decomposition is performed using a set of parametric search rules, which means that only patterns of interest are extracted. This eliminates don’t care patterns that would otherwise burden the database with too many nuisance patterns. When a layer is fully decomposed into its constituent patterns of interest, the result is an abbreviated representation of the layer. We call this the Design Decomposition Database.

Parametric search rules can specify both generic line space and line width patterns, or highly specific, but parametrized patterns. An example of a specific but parametrized pattern is a tip-to-line. There can be many variations of tip-to-line depending on the (a) gap between tip and line, (b) width of the tip, (c) width of the line, (d) run length of the tip, (e) run length of the line, etc. All of these \[items (a) through (e)] are parameters of the specific pattern known as tip-to-line.

Parametric rules allow us to locate all variations of tip-to-line based on flexible constraints applied to all parameters (a) through (e). Creating such a rule is both intuitive and efficient by using the Parametric Rule Editor.
#### Features
1. Design Decomposition Database  设计分解数据库
> PCYM begins by creating the all-important **Design Decomposition Database**. This is an integral part of the infrastructure that enables values-added downstream applications.
2. Pattern Risk Scoring  模式风险评级
> Patterns in the Design Decomposition Database are scored on the basis of any and all available information sources. These include simulation sources such as OPC simulation, statistical or geometrical sources (design signatures), and empirical sources such as pattern fidelity information extracted from real-silicon SEM images.  
  When all patterns are scored (or ranked), we can:  
	1. Perform **data mining** to find weakest and strongest patterns.
	2. **Trend** Analysis: How do the pattern scores change over time?
	3. Create optical inspection and E-Beam inspection **care areas**.
	4. Perform improved defect sampling for SEM review.
	5. Provide feedback to OPC / Lithography teams.
#### Benefits
1. Apply geometric signatures, simulation results, and empirical data to assess the process marginality or risk of each pattern.  
2. Compare and contrast a new tape-out against a well-established design to identify risk factors in the new tape-out.
3. Apply pattern risk scores when generating care areas for both optical and E-Beam inspection, and SEM review plans.
### **PCML**: [Pattern Centric Machine Learning](https://anchorsemi.com/Products/page-3/). 模式中心的机器学习
Each layer of interest in the CAD Layout is decomposed systematically into a collection of unique constituent patterns of a specified maximum size. A poly layer, for instance, will be fully decomposed into its unique constituent patterns.  

Decomposition is performed using a set of parametric search rules, which means that only _patterns of interest_ are extracted. This eliminates _don’t care_ patterns that would otherwise burden the database with too many nuisance patterns. When a layer is fully decomposed into its constituent patterns of interest, the result is an abbreviated representation of the layer. We call this the **Design Decomposition Database**.  

Parametric search rules can specify both _generic_ line space and line width patterns, or highly _specific_, but parametrized patterns. An example of a specific but parametrized pattern is a tip-to-line. There can be many _variations_ of tip-to-line depending on the (a) gap between tip and line, (b) width of the tip, (c) width of the line, (d) run length of the tip, (e) run length of the line, etc. All of these [items (a) through (e)] are _parameters_ of the _specific_ pattern known as tip-to-line.  

Parametric rules allow us to locate all _variations_ of tip-to-line based on flexible constraints applied to all _parameters_ (a) through (e). Creating such a rule is both intuitive and efficient by using the _Parametric Rule Editor_.
#### Features
1. Pattern Centric Machine Learning  
> Pattern Centric Machine Learning bridges the _Printed Pattern Database_ and the _Design Decomposition Database_ to enable entirely new opportunities for yield learning and process optimization.   
>
  Because SEM review is a time consuming operation, only a relatively small number of images are captured on each inspected wafer. This does not cover all of the pattern variety that exists in the _Design Decomposition Database_, which means there is no direct empirical data for most of the patterns in that database, and hence we are somewhat handicapped in our ability to assess their process risk.  
  >
  But machine learning provides a next-best solution.  
  >
  Recent advancements in machine learning algorithms allow a computer to build an inference or predictive model itself using appropriate training data. Training data consists of numerous and diverse examples of inputs and their real-world outputs. The _Printed Pattern Database_ is an ideal repository of labeled training data because it contains numerous examples of the reference pattern and the printed pattern. Deviations between the reference and printed pattern are used to _label_ each training set. If the deviation is insignificant, the pair of reference (input) and printed (output) patterns are labeled as an example of a 'good' pattern. Conversely, if the deviation is significant, the pair of reference and printed patterns is labeled as an example of a 'bad' pattern.  
  >
  The _Printed Pattern Database_ therefore provides an _automatically_ labeled set of training data for use by supervised machine learning algorithms.  
  Moreover, new SEM images that are continuously being captured by the fab are also being added to the _Printed Pattern Database_. This dynamic environment allows the machine learning system to learn continuously and therefore improve its prediction accuracy. As the accuracy of the machine improves over time, the system moves closer to an _expert_ system.

#### Benefits:  

PCML builds a machine learning model from the data contained in the _Printed Pattern Database_ and makes pattern risk predictions on nearly all of the patterns contained in the much larger _Design Decomposition Database_. These predicted scores are expected to be more reliable that purely statistical or geometrical signatures.
### **PWA**. [Process Window Analyzer](https://anchorsemi.com/Products/page-5/). 进程窗口分析器
The conventional method of analyzing Focus/Exposure Modulations (FEM) is by performing a high-sensitivity wafer inspection followed by a large SEM Review in which tens of thousands of SEM images are captured and analyzed for the presence of _hard defects_. The conventional method does not take _pattern fidelity_ into account and therefore cannot track or report the subtle deviations that occur on each pattern across each modulation. Subtle deviations – pattern _fidelity_ variations – are playing an increasingly significant role in parametric yield loss. Establishing a lithography process window that takes into account pattern fidelity (not just pattern defectivity) leads to a more robust result.

#### Features

1. Process Window Analyzer
> Anchor’s computational system redefines and reinvents FEM/PWQ analysis in the following ways:  
> 
> The computational system checks every SEM image for the presence of die-to-database defects. Some of these defects are not detectable using conventional die-to-die or die-to-golden die techniques. Multiple defects can be detected on a single image.   
> 
> The computational system _measures_ every feature of interest in every SEM image (massive metrology) to generate pattern uniformity statistics for each pattern of interest. This enables pattern _fidelity_ analysis.  
>
  The computational system tracks the uniformity of like patterns across each modulation to generate Bossung Curves automatically for hundreds or thousands or tens of thousands of patterns. These Bossung Curves supplement – not replace – conventional CD-SEM analysis because accuracy of measurements from Review SEM is limited. Nevertheless, these Bossung Curves are produced more quickly and cover a significantly wider set of patterns. They provide valuable early feedback.  
>
  The combination of (a) better defect detection, (b) pattern uniformity/fidelity analysis, and (c) generation of Bossung Curves for a wide set of patterns results in the reinvention of PWQ/FEM analysis.

#### Benefits
Automatically locates all patterns-of-interest in all SEM images across all Focus/Exposure modulations (automatic _target_ selection), measures the essential features of those patterns across modulations, and presents the information in flexible ways, including Bossung curves.  

By performing this analysis across a wide set of diverse patterns, it becomes possible to study patterning behavior across modulations in a significantly more comprehensive and time-efficient manner.  

The knowledge gained from this analysis can be fed back to OPC/Litho and used to establish tighter process windows.
## Toolkits
### **D2DB-Image Explorer**
#### Features
1. Annotation and Markup Removal  删除原始图像的注释和标记
        ![[Pasted image 20230903153443.png|200]]    ![[Pasted image 20230903153504.png|200]]
2. Contour Extraction  轮廓提取
> 图像-->轮廓数据-->mask，有关参数可以自动计算也可以手动设置

		![[Pasted image 20230903153929.png|200]]    ![[Pasted image 20230903153957.png|200]]

3. Contour Alignment  轮廓对齐
> mask与版图进行对齐

					![[Pasted image 20230903154235.png|200]]
4. Contour Search  轮廓搜索
> 在整个版图或者某个区域，搜索所有匹配的模式（包括旋转和翻转）
5. Defect Detection  缺陷检测
> 对齐后便可进行比对检测缺陷，主要包括
	• Hard Open (break) or Partial Open (necking).  
	• Hard Short (bridge) or Partial Short.  
	• Line End Pullback.


					![[Pasted image 20230903154849.png|200]]

#### Benefits

1. Set of Linux functions that provide leading-edge image processing capabilities.
2. Customers can embed these capabilities into their own automation workflows.
3. Vendor-neutral: D2DB-Image Explorer works with images from multiple SEM and E-Beam tool brands.
### **ADD**. Advanced Defect Discovery.
Advanced Defect Discovery is a workflow for generating defect sample plans for SEM Review that takes into account optical patch images, machine learning, and optionally the PCYM ranked database.  

Review SEMs have been used for decades to compensate for the lack of resolution on optical inspection tools. Optical inspectors scan the wafer for defects, but their _patch images_ are too pixelated and cannot be used to determine the type of each defect. A clear and detailed image of the defect is necessary to determine its type, its shape, its causal mechanism, and its impact to yield (killer versus non-killer).  

Because of the relatively slow throughput of Review SEM tools, it is necessary to sample a subset of the defects that were detected by the optical inspector. If a poor subset is sampled, little if anything is learned. Fabs generally expect the sampled subset to (a) contain as many known defects of interest (DOI) as possible while also (b) discovering new defect types. It may seem straightforward to generate a sample plan that addresses both needs, but these are in fact competing requirements. If the sample plan is biased too much around (a), it will lose its ability to discover new defect types (b), and vice-versa.
#### Features
1. Advanced Defect Discovery
> Given a budget of N sample points for SEM review, Advanced Defect Discovery applies machine learning techniques to generate a balanced sample plan while providing users the ability to bias the algorithm a little in either direction. Balancing the sample plan means choosing defects from the original population whose backgrounds are similar to known high-risk patterns (to increase cap rate of known DOI) while also choosing defects whose backgrounds are dissimilar to known good and bad patterns (to improve the chances of discovering new defect types).  
  >
  This sampling methodology would not be possible without the Printed Pattern Database (known good and bad patterns), the Design Decomposition Database (collection of all patterns), and Machine Learning.

#### Benefits
ADD uses all available information from each wafer inspection (KLARF and patch images) and applies its own image processing and machine learning algorithms to generate an effective sample plan for SEM Review.
### **I2DC**. Image to Design Contour.
I2DC is a specialized tool for the OPC / Lithography engineer who needs to compare simulation with real silicon to analyze the OPC workflow and perform adjustments as needed.  

I2DC works with a collection of overlapping high resolution images to generate a physical layout from the assembled set of images. The principal workflow proceeds as follows:  


- Specify a set of images that have some overlap.
- I2DC will stitch the images into a single panoramic image.
- Micron markers and textual annotations that happen to be present on the image can be removed.
- Some input images contain artifacts that need to be ‘touched up’ prior to contour extraction. I2DC provides a set of built-in functions for image editing.
- Image processing algorithms are subsequently applied to extract contours while differentiating between current and previous layers on the image. The software supports differentiation of mixed data into separate layers.
- The rough and jagged contours are then converted to _physical layout_ format, which means straight lines, 90-degree corners, and various allowed angles (45°, 135°, 30°, 60°, 120°, 150°).
- An optional _unification_ step looks for duplicate shapes and builds a hierarchical layout instead of a strictly flat one.
#### Features
1. Image Stitching  图像拼接
> 将一组有重叠的图像拼接起来

		 ![[Pasted image 20230903160033.png|400]]
2. Annotation Removal  删除标记和注释
3. Image Editing  图像编辑
> 图像预处理？

					![[Pasted image 20230903160412.png|200]]
5. Layer Separation  分层
> 若图像中的两层有足够明显的灰度差别，可将其提取到单独的层中

			![[Pasted image 20230903160541.png|400]]
6. Image to Contour to Layout Conversion  图像-->轮廓-->版图的转换
> 根据轮廓提取结果，使用直线和允许的角度生成物理版图
## Technology
### **Computational System**
#### Introduction
#### Step

### **Parametric Rule Editor**
#### Introduction
Flexible pattern search and full-chip pattern segmentation are becoming increasingly important tools for effective yield learning and yield enhancement.  

**Flexible Pattern Search**  
A typical physical layout (GDS/OASIS) consists of patterns and subtle to substantial _variations_ of the patterns. Often it is necessary to search for a pattern while specifying precise constraints for finding variations of the pattern in a controlled, deterministic way.  

**Full-Chip Pattern Segmentation**  
Advanced inline inspection tools from the leading vendors have provided _array mode_ inspection and _random mode_ inspection. Array mode inspection significantly enhances sensitivity to defects in mem
ory arrays. Because noise characteristics within an array are fairly uniform throughout the array, the _threshold_ setting can be lowered compared with the threshold in random logic regions. However, even random logic regions can be segmented on the basis of uniform pattern characteristics such as line orientation, line density, pattern complexity, and so on, to generate _micro care areas_. Segmenting the logic regions into homogeneous subregions allows each subregion to be thresholded individually for best sensitivity and noise rejection.  

Anchor’s Parametric Rule Editor (PRE) provides an elegant and user-friendly solution for both of these applications. It is a groundbreaking product that provides supreme flexibility with unmatched ease-of-use.  

PRE can be used to build simple or complex search rules on patterns that are either imported from a file or drawn by hand using polygon-editing functions built right into the product. Exact constraints for a number of geometric properties such as line width and line space can be specified, with full support for multiple layers.  

Want to check if there is a single-via of a specified width with specified coverage within a specified distance from the end of line that is of a specified width and has a minimum specified run length? No problem. This example combines a via layer and an interconnect layer, yet it is a relatively simple example of the flexibility offered by PRE.  

In this section we walk through a sample use case to demonstrate some of the capabilities of the Parametric Rule Editor.


#### Step
1. The Physical Failure Analysis (PFA), Diagnostics, or Yield Enhancement group just handed you a SEM image of a potential ==weak pattern==. You suspect the small metal island that is surrounded by pattern on three sides. You decide to search for this pattern on the full chip, but you pause for a moment to consider the following:   
	- It would be more meaningful if there was a via connecting this metal island. You want to avoid ‘floating metal’ that is of no consequence.  
	- You feel that there might be several  _variations_ of this pattern that might be quite relevant. You decide that this pattern _and its variants_ are ‘Patterns of Interest’ or POI that should be searched.
							  ![[Pasted image 20230902165244.png|200]]
2. Although we can use Image Explorer’s contour extraction feature to lift this pattern out of the SEM image, we note that the pattern is simple enough to be drawn manually within the PRE GUI. ==We don’t need an exact rendering because we will specify numeric constraints==.  
  
   Within a few seconds we use the PRE GUI to draw the pattern on the right.
                         ![[Pasted image 20230903095052.png|200]]

3. Because we want to ensure that the metal island contains a via (otherwise it would be a ‘floating metal’ or dummy structure), we also draw a via.  
  
   Now the base pattern is complete and we move on to specifying the necessary constraints.
							   ![[Pasted image 20230903095346.png|200]]
	
4. The process of defining constraints begins with identifying the key geometric properties. In this case we want to ensure that the metal island is flanked on three sides, that it contains a via, and that the via is located within a specified distance from the line end.  
  
   This can be achieved by examining the 4 properties illustrated on the right.
									![[Pasted image 20230903095449.png]]
5. Now we specify the numeric constraints for our 4 variables. We decide, in this case, that the island should be flanked on top within 50nm, on the side within 70nm, and on the bottom within 200nm. We also decide that the via should be located within 50nm of the line end.
								  ![[Pasted image 20230903095737.png]]
6. Putting all of this together, the PRE GUI will look something like this. We are now ready to run the parametric pattern search…
					![[Pasted image 20230903100024.png]]
7. The parametric pattern search engine applies the user’s constraints on the entire die or any subarea. The results can be viewed in several ways. A common view is the Tile View that shows all of the unique patterns that were found and allows the user to iterate through each of the instances of those patterns across the die.  
  
   On the left we can see 17 variations of the pattern. Each of the 17 variations may have hundreds or thousands of instances. Note the subtle variations in each of these 17 patterns. Note also the variation in the location of the via.
						 ![[Pasted image 20230903100101.png]]
8. Because we defined variables to track top edge distance, side edge distance, bottom edge distance, and via edge distance, we can view the distribution of values for each of these variables.  
  
   For example, the gallery on the left shows the 3 different values of **side edge distance** that were found. These values (0.05um, 0.06um, and 0.065um) can be plotted on a histogram to show how many instances had a side distance of 0.05um, how many had a distance of 0.06um, etc. Filtering and sampling operations are also provided to narrow down the population into one or more subsets.
						   ![[Pasted image 20230903100140.png]]
### **Image Processing**
#### Introduction
High resolution images have always played an important role in yield learning, but at today’s feature sizes these images are taking on a whole new significance. Investment in SEM and E-Beam tools has been increasing, and the throughput of these tools has been increasing as well.  

Anchor’s seminal product was a Post-RET Verification (PRV) tool that employs lithography models to simulate printed wafer images and identify weak patterns. This is an image-intensive application that demonstrates Anchor’s longstanding expertise in image processing technology.  

Today, Anchor understands the importance of applying image processing technology to the production Fab, and has developed products such as [D2DB-Image Explorer](https://anchorsemi.com/Products/Image_Explorer/ "Image Explorer") and [D2DB-Pattern Monitor](https://anchorsemi.com/Products/D2DB-PM/ "D2DB-PM").  

In this section we examine some of the image processing capabilities offered by Anchor across various products.  

There is a wealth of information contained in high resolution images, but the information is not fully exploited, and many images such as SEM Non-Visual (SNV) are simply discarded, resulting in a significant reduction in the return on investment of expensive imaging tools.  

Some of Anchor’s image processing capabilities for the Fab include:  


1. Pattern (contour) extraction.
2. Contour to layout conversion.
3. Contour alignment to design.
4. Contour search.
5. Layout to contour conversion.
6. Measurement.
7. Defect detection.
8. Image stitching.


These are described in the sections that follow.

#### Step
1. Image Processing begins with an image! When working with high resolution images, the first step is **contour extraction**, but this is more challenging than it seems. Contour extraction algorithms are governed by several parameters, much like recipe parameters in an inspection tool. If these parameters are not tuned properly, the resulting contour may be unusable.
							![[Pasted image 20230903100445.png|200]]
2. Because image quality varies widely from wafer to wafer, layer to layer, tool to tool, and even day to day, it would simply be impractical to tune contour extraction parameters manually. That is why Anchor has developed **automatic parameter setup**, which examines every image individually and customizes contour extraction parameters for each and every image. For the (relatively clean) SEM image above, the resulting contour (with automatic parameter setup) is shown here.
							![[Pasted image 20230903100529.png|200]]
3. Once a contour has been extracted, it is often necessary to find that contour in the physical layout or design file (GDS / OASIS) and align the two together. Despite line edge roughness and corner rounding in all extracted contours, Anchor’s alignment engine is able to match that with the straight lines and perfect corners of a Manhattan layout. The resulting **alignment** and overlay is shown here.  
  
   Contour alignment is designed for use cases where a starting location is known, such as a defect coordinate. Defect coordinates contain some amount of stage and measurement error, which means that the coordinate itself cannot be trusted as the true location of the defect. By aligning the image to design, the true coordinate can be determined.
							![[Pasted image 20230903100635.png|200]]
4. Whereas contour alignment is designed to eliminate coordinate error and therefore corrects for x and y shifts within a small distance from a starting point, **Contour Search** makes no _a priori_ assumptions, and is designed to operate on the full chip or any subregion. It compensates for rotation and flip in addition to x and y shift. A sample result is shown here.
					![[Pasted image 20230903100716.png|300]]
5. Anchor provides specialized tools for **Contour-to-Layout** conversion and vice-versa.  
  
   Given an image, contour extraction is first performed to lift the pattern out of the image. The resulting contour is then converted to resemble Manhattan geometry, as shown here.
							![[Pasted image 20230903100820.png|200]]
6. ==**Layout-to-Contour conversion** is illustrated here, but it is taken one step further to create a pseudo-SEM image from the contour. Several parameters are available to control background noise, line edge intensity, and other properties of the simulated SEM image.==
							![[Pasted image 20230903100919.png|200]]
7. If a suitably small pixel size is used, usable measurements can be made of line widths and line spaces. Anchor provides two measurement methods in its [D2DB-Pattern Monitor](https://anchorsemi.com/Products/D2DB-PM/ "D2DB-PM") product. The first method is **Contour Based Measurement**, which is highly dependent on the contour extraction recipe settings that were used. Nevertheless, it is a often a good-enough method when the actual value is not as important as the overall trend between multiple measurements.
				![[Pasted image 20230903101431.png|400]]
1. The alternative to Contour Based Measurement is **Intensity Based Measurement**. This requires a little more input from the user, which allows for greater fine-tuning of the measurement. In the figure to the left, we can see a SEM image of a small line. The graph shows the grayscale values as we trace a line from the middle-left to the middle-right of the SEM image. We can see that there are several possible values for the width of the line, as shown by the outer and inter slopes. The user can specify the location of the ‘gauge’ along these slopes as shown.
				![[Pasted image 20230903101418.png|400]]
1. Once a contour has been extracted and aligned to design, deviations between the two can be calculated and classified. Anchor’s [D2DB-Pattern Monitor](https://anchorsemi.com/Products/D2DB-PM/ "D2DB-PM") and [D2DB-Image Explorer](https://anchorsemi.com/Products/Image_Explorer/ "Image Explorer") products provide 6 types of **defect detection** and classification, with more types being developed.
				![[Pasted image 20230903101356.png|400]]
1. Image stitching is the process of combining multiple images with overlapping fields to produce a single seamless panoramic image. This can be done with images as well as with extracted contours that have been converted to layout form.
 ![[Pasted image 20230903101332.png|600]]
### **NanoScope PRV**
#### Introduction
NanoScope PRV™ is a model-based, pattern-centric full chip Post-RET/OPC verification software solution that features network based computing performance, accurate lithography process modeling, comprehensive hotspot inspection, analysis, and process window limiting pattern extractions.  

Armed with Anchor's patented pattern-centric technology, PRV offers features and capabilities that target mask error detection, correction, and data management. The distributed computing architecture produces faster turnaround time for large designs at advanced technology nodes. It runs on generic UNIX / Linux based servers with an option of operating in either batch or GUI mode.  

NanoScope PRV also provides comprehensive inspection capability for full process window verification covering all design layers supported by NanoScope Modeler to ensure accurate hotspot detection. Some examples of these inspection functions include:  


- Line width and space CD-based error detection.
- Break and bridge detection.
- Hole size and area coverage.
- Gate CD uniformity assessment.
- Line-end pull back and end cap length detection.
- SRAF printability assessment.
- Side-lobe detection.

  

**Pattern grouping capabilities simplify OPC hotspot review and fix.**  
Anchor's unique pattern grouping technology is among the key analytical functions proven to be critical in today's chip manufacturing environment that faces increasing hotspot counts and design related defects. Because hotspot review and analysis operations can be time consuming and prone to human error if done manually, pattern grouping becomes a key enabler for hotspot analysis when it is necessary to first reduce the large number of defects by grouping them into fewer and more manageable number of groups.  

Anchor currently offers grouping in the categories listed below. All of them can be tailored to handle such issues as pattern resize, jogs, center-shift and error mark shift.  


- **Exact** pattern grouping. An example of grouping with center-shift is discussed below.
- **Similar** pattern grouping (pixel based and topology based).
- Anchor proprietary pattern **signature** grouping.
#### Step
1. **Exact pattern grouping with center-shift.**
   Exact_ pattern group with center-shift has gained popularity for its efficiency and effectiveness. A center-shift radius can be user-defined. The radius is highly dependent on the feature size and affects both the number of groups generated and the number of members in each group.  
  
   **Edge and jog tolerances.**  
   Two patterns can be grouped together if threshold values of pattern size difference or jog size are defined by the user.
		![[Pasted image 20230903103224.png|500]]
2. Taking defect coordinate uncertainty into consideration during grouping, each pattern can be shifted within a user-defined distance in either direction to look for exact matches.
		![[Pasted image 20230903103320.png|500]]
3. Pattern grouping results can be viewed in a user-friendly _tile view_. In this example, 77,929 patterns were consolidated into 11 unique groups, 9 of which are shown here.   
				![[Pasted image 20230903103351.png|400]]
4. Continuing on the topic of viewing capabilities, NanoScope PRV provides a GUI-based analyzer that allows users to review results through HTML / Browser summary reports. This avoids the need to open separate EDA software and permits more people across the organization to view the results.
				![[Pasted image 20230903103501.png|400]]
5. Pattern hierarchy analysis (cell extraction) can help users identify the hotspot source and assist with the OPC optimization process.
				![[Pasted image 20230903103555.png|400]]
6. Through the NanoScope PRV GUI, users can perform Boolean operations, contour simulation, and pattern comparison using the appropriate lithography model validated by wafer SEM image.
				![[Pasted image 20230903103624.png|400]]
7. In this last example, we see that NanoScope PRV identified a fatal Poly-Diffusion bridging condition. The silicon confirmation is shown in the second graphic.
						![[Pasted image 20230903103707.png|300]]
