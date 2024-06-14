- <font color="#f79646">目的:</font> 捕获 目标层上 指定位置的模式
- 用途: 就是在进行 **Pattern Match** 之前, 需要现有<font color="#ff0000">原始的 Pattern</font>.那么原始的 Pattern 是怎么来的?就是通过捕获目标层上指定位置的Pattern来的.捕获的模式越精确,模式数越少,每个模式下面对应的shape会越多.
# Usage
DFM PATTERN CAPTURE  
	{{**LAYER_HOTSPOT hotspot_layer**  
		**PATTERN_HALO {halo | x_halo y_halo}** \[FROM_EXTENTS | FROM_EDGES]  
		\[MAX_SEARCH value \[SEARCH_LEVEL EXACT]]  
		\[MAX_LENGTH max_length]}  
	|{**LAYER_PATTERN_MASK pattern_mask_layer**}}  
	{**LAYER_TARGET target_layer** \[LAYER_NUMBER layer_number] \[DC]  
		\[layer_halo] \[FROM_EXTENTS | FROM_EDGES]  
		\[DENSITY_PROP \[PROP_NAME dens_prop_name]]  
		\[VERTEX_PROP \[PROP_NAME prop_name]\[KEEP_ORIGINAL_VERTEX]]  
	}...  
	\[LAYER_EMPTY empty_name \[DC]] ...  
	\[LAYER_REGION region_source_layer region_type region_target_layer]...  
	\[OUTFILE filename \[<u>OVERWRITE</u> | APPEND]  
		\[PATTERN_ATTR attr_name attr_value \[INTEGER | FLOAT | STRING]]… ]  
	\[OUTPUT_LAYER {<u>DEFAULT</u> | BBOX | BBOX10}]  
	~~\[EXCEPTIONS_ONLY]~~  
	\[LAYER_PATTERN_MARKER marker_spec marker_name] ...  
	~~\[PATTERN_MARKER_METHOD marker_method]~~  
	\[MERGE_MARKERS]  
	\[EXTENT_TRACING]  
	\[PATTERN_NAME_PREFIX name \[NUM_START value]]   
	\[PATTERN_NAME_PROP name_str_prop]  
	\[PATTERN_CGLOBAL cglobal]  
	\[LAYER_CGLOBAL layer_cglobal cglobal_target_layer] ...  
	\[MATCH_ORIENT | MATCH_ROTATION]  
	\[MATCH_TYPE]  
	\[PATTERN_ORIENT pattern_orient_option]  
	\[MATCH_ORIENTATIONS match_orientations_option]]  
	\[CAPTURE_ORIENT orient_prop_name]  
	\[PATTERN_PROP pattern_prop_name prop_value \[FLOAT | INTEGER]] ...   
	\[OUTPUT_PROP]  
	\[INDEX_PROP index_prop_name]  
	\[SET_PROP prop_name \[<u>MIN</u> | MAX]]  
	\[DUP_COUNT dup_prop_name]  
	\[PATTERN_KEY name] ...  
	\[COMMENT "comment_string"]   
# Arguments
## LAYER_HOTSPOT hotspot_layer
A keyword set that specifies to capture patterns centered at the <span style="background:#affad1">shapes</span> on the hotspot_layer, which must be an original or derived polygon layer. This keyword set is required if LAYER_PATTERN_MASK is not specified. The default pattern capture area is defined by the PATTERN_HALO keyword.  
It is possible to specify both LAYER_HOTSPOT and LAYER_PATTERN_MASK. In this case the pattern capture area is determined by the shapes on the pattern_mask_layer, while the shapes on the hotspot_layer that are within the <span style="background:#affad1">pattern extent</span> <span style="background:#affad1">are instantiated as marker shape(s) for the captured pattern.</span>
> [!question]
> 1. 既然是 Hotspot,那么可以理解 Hotspot 是一个点,但是说 Hotspot 是一个形状就不是很理解了.
> 2. pattern extent值的是什么,谁的 pattern
> 3. are instantiated as marker shape(s) for the captured pattern如何去理解
### PATTERN_HALO
A required keyword set when LAYER_HOTSPOT is specified. The PATTERN_HALO keyword set defines the halo region, which is <span style="background:#affad1">centered on the hotspot_layer shapes</span> by default. <font color="#ff0000">The captured patterns are composed of shapes on the target_layer(s) within the halo region.</font> See Figure 4-93. The halo size is specified in user units.
> [!question] 
> 不规则 shape 的 center 怎么确定

![[support/img/Pasted image 20231028200104.png|500]]
- **halo** — The width of the halo region is 2 * halo. By default the halo is measured from the center of the hotspot_layer shape.
- **x_halo y_halo** — The halo region is 2 * x_halo in the x-direction, and 2 * y_halo in the y-direction.
- **FROM_EXTENTS** — An optional keyword that specifies to measure the halo from the bounding box of the hotspot_layer shape.
- **FROM_EDGES** — An optional keyword that specifies to measure the halo from the edges of the hotspot_layer shape. Non-Manhattan hotspot shapes are ignored and a warning is issued. The keyword cannot be used with MAX_SEARCH or with independent x_halo and y_halo values.
### MAX_SEARCH
An optional keyword set that specifies to perform <font color="#ff0000">auto-centering</font> of the pattern location if value is greater than zero. The default is to not perform auto-centering. MAX_SEARCH is only valid when capturing patterns with the LAYER_HOTSPOT method.  
Auto-centering primarily uses shapes on the <span style="background:#affad1">first listed target_layer </span>that do not have the DC keyword.  
- **value** — Defines a square search region centered on the hotpot shape and with a width of 2\*value. <font color="#ff0000">An internal algorithm uses target layer edges and vertices within the search region to optimize the location of the pattern center.</font> If any target layer edges or vertices exist within the hotspot_layer shape, they contribute to auto-centering regardless of the MAX_SEARCH value. If no target layer edges are found within the search region, the pattern center is not adjusted. The use of MAX_SEARCH may increase the number of duplicate patterns that are found. When specified, the MAX_SEARCH value is recommended to be at least half the halo size set with PATTERN_HALO.
- **SEARCH_LEVEL EXACT** — Specifies to use <u>an improved search algorithm</u> that attempts to optimize the number of duplicate patterns that are identified, thus reducing the total number of patterns that are captured. Compared to the case when SEARCH_LEVEL EXACT is not specified, the search finds more duplicate patterns and increases runtime.  
  SEARCH_LEVEL EXACT also enables auto-centering when only edges (no vertices) are present within the search range. BCM patterns are not auto-centered unless SEARCH_LEVEL EXACT is specified.  
  When SEARCH_LEVEL EXACT is specified an error is issued if the MAX_SEARCH value is greater than the halo size.  
  MAX_LENGTH is not allowed with SEARCH_LEVEL EXACT. The MAX_LENGTH keyword is ignored if used and a warning is issued.  
Auto-centering is useful when the process that produces the hotspot layer results in hotspot locations that vary from run to run. See “MAX_SEARCH and SEARCH_LEVEL EXACT Keywords for Pattern Auto-Centering” on page 513 for further discussion and illustrations.  
### MAX_LENGTH
An optional keyword set, where max_length is specified in user units. <u>When specified, hotspot shapes are divided into a number of smaller segments when encountering long hotspot shapes.</u> The default max_length value is 0, indicating no segmentation. If a hotspot shape exceeds max_length in length, width, or both, the shape is divided into equal hotspot segments, each being less than or equal to the max_length value. Each hotspot segment is then handled independently, according to MAX_SEARCH, PATTERN_HALO, and other keywords.  
MAX_LENGTH is not allowed with SEARCH_LEVEL EXACT. The MAX_LENGTH keyword is ignored when used with SEARCH_LEVEL EXACT and a warning is issued. MAX_LENGTH is not allowed with LAYER_PATTERN_MASK.  
In the following figure, MAX_LENGTH divides a hotspot into seven equal segments. <font color="#ff0000">This example uses MAX_SEARCH, so segment 2 and segment 7 do not cover the entire section of the drawn layer due to nearby geometries.</font>  
![[support/img/Pasted image 20231028204901.png]]
The resulting four patterns created using MAX_LENGTH are shown in the following figure. <span style="background:#affad1">The patterns show that the marker for the pattern is the portion of the hotspot shape captured per segment. The pattern corresponding to segments 3, 4, 5, and 6 show that the hotspot does not extend out to the extent, because the length of the segments is less than max_length.</span>  
![[support/img/Pasted image 20231028205510.png]]
> [!question]
> 意思就是,由于 2 和 7 段的周围存在的别的 shape,因此呢版图别的地方也有相似的,一使用 MAX_SEARCH 的话,就定心偏移了.之后绿色的那一段的意思就是这个长条 shape 的长度÷7 之后是小于 max_length 的,而这个 extent 是图中的每个 segment 的黑框,所以也有了这么个说法.
## LAYER_PATTERN_MASK 
A keyword set that specifies to capture patterns at the locations of shapes on pattern_mask_layer, which must be an original or derived polygon layer. The shapes on the layer must be Manhattan shapes in order to generate a pattern. The default capture area is defined by the shapes on pattern_mask_layer—the pattern consists of target_layer(s) shapes within the extent of the pattern_mask_layer shapes. This keyword must be specified if LAYER_HOTSPOT is not used.
PATTERN_HALO, MAX_SEARCH, and MAX_LENGTH generate an error if LAYER_PATTERN_MASK is specified.  
## LAYER_TARGET
A required keyword set that specifies a target layer name. The secondary keywords are order dependent and <font color="#ff0000">should be specified in the order shown.</font> <span style="background:#fdbfff"><font color="#ff0000">The polygons that exist on the target_layer within the bounds of the capture area are copied to the pattern layer</font></span>, where the capture area is determined by the LAYER_HOTSPOT, PATTERN_HALO, FROM_EXTENTS, FROM_EDGES, and LAYER_PATTERN_MASK keywords. This keyword set may be specified more than once. When specifying this keyword set more than once, the order of the target layers is preserved. When doing a pattern matching run, the matching layer <span style="background:#d3f8b6">order passed to the CMACRO call</span> must correspond to the captured order, otherwise no matches are detected. The order of the target layer names must stay consistent from run to run.  
> [!question] Order?
> order 是啥,是不是可以理解为marker 和 target 的顺序的匹配,但是不应该是用户指定的吗,软件怎么会知道这个顺序不对?还有这个 CMACRO 是什么东西.
### LAYER_NUMBER
**LAYER_NUMBER** layer_number — An optional keyword set that specifies the layer number that is assigned to <span style="background:#affad1">the pattern layer</span> in the saved pattern library.  
If not specified, original layers are assigned the layer number determined from the LAYER and LAYER MAP statements in the rule file.
### layer_halo
layer_halo — An optional value that defines a <span style="background:#affad1">per-layer</span> halo region. The layer_halo value is specified in user units and defines a square halo region centered on the hotspot_layer shapes and with a width of 2 * layer_halo. If layer_halo is specified, target_layer shapes must be within the per-layer halo region t<span style="background:#affad1">o be included in the pattern—the layer_halo overrides the PATTERN_HALO value for the specified target_layer.</span> The LAYER_HOTSPOT keyword must be specified if a per-layer layer_halo is used. If every target layer has a specified layer_halo value, then the PATTERN_HALO keyword set is optional. The pattern extent is determined by the largest halo value specified. See the figures within Table 4-28.  
If FROM_EXTENTS or FROM_EDGES is specified with PATTERN_HALO, the keyword is used when determining the per-layer halo region. If FROM_EXTENTS or FROM_EDGES is specified for a specific target_layer, the specified behavior is used for that target_layer.  
If OUTFILE is specified, the per-layer halo is saved as a per-layer custom extent in the saved pattern library.
![[support/img/Pasted image 20231102084030.png]]
> [!question] 如何理解这里的per-layer halo

Pattern Location and Capture Area
1. 搜索的逻辑:
	1. 首先看搜索的区域之中有没有 edge or vertex, 如果是两者都没有的话,就不会进行自动对齐. 如果只有边的话,将会把中心垂直或者水平的投射到 edge 之上, 如果投射的 center 所检索出来的 pattern 是重复的那么就用这个projected center, 如果还和以前一样的话就用原来的. 既有 edge 也有 vertex, 这个时候找的就是定点,把 search_region 之中的 vertex 作为 auto_centering 的结果,如果结果重复的话就当做新的 center,如果还是和以前一样得到了新的pattern 的话就用原来的 center.

# Description
基本的功能: 
