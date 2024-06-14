***
{% if title %}Title:: "{{title}}"{% endif %}
Authors:: {{authors}}{{directors}}
Publication:: "{{publicationTitle}}"
{% if date %}Date:: {{date | format("YYYY-MM-DD")}}{% endif %}
citekey:: {{citekey}}
tags:: {{hashTags}}, 
Pdf::{{pdfZoteroLink}}

***
## {{title}}

**引文目录:** {{bibliography}}

**来源:** {{url}}

**Zotero 位置:** 

**Tags :** {{hashTags}}, #zotero

>[!abstract]+
>*« {{abstractNote}} »*


#### Idea
|原文|Idea|链接|
|-|-|-|{% for annotation in annotations %}{% if annotation.colorCategory == "Red" %}
|{{annotation.annotatedText}}|{{annotation.comment}}|[zotero-Link](zotero://open-pdf/library/items/{% for t in attachments %}{% if loop.first %}{{t.itemKey}}{% endif %}{% endfor %}?{{annotation.page}}&annotation={{annotation.id}})|{% endif %}{% endfor %}
#### 不理解的
|原文|不懂的点|链接|
|-|-|-|{% for annotation in annotations %}{% if annotation.colorCategory == "Green" %}
|{{annotation.annotatedText}}|{{annotation.comment}}|[zotero-Link](zotero://open-pdf/library/items/{% for t in attachments %}{% if loop.first %}{{t.itemKey}}{% endif %}{% endfor %}?{{annotation.page}}&annotation={{annotation.id}})|{% endif %}{% endfor %}
#### 专有名词
|原文|翻译|链接|
|-|-|-|{% for annotation in annotations %}{% if annotation.colorCategory == "Gray" %}
|{{annotation.annotatedText}}|{{annotation.comment}}|[zotero-Link](zotero://open-pdf/library/items/{% for t in attachments %}{% if loop.first %}{{t.itemKey}}{% endif %}{% endfor %}?{{annotation.page}}&annotation={{annotation.id}})|{% endif %}{% endfor %}
#### 专业名词
|原文|翻译|链接|
|-|-|-|{% for annotation in annotations %}{% if annotation.colorCategory == "Orange" %}
|{{annotation.annotatedText}}|{{annotation.comment}}|[zotero-Link](zotero://open-pdf/library/items/{% for t in attachments %}{% if loop.first %}{{t.itemKey}}{% endif %}{% endfor %}?{{annotation.page}}&annotation={{annotation.id}})|{% endif %}{% endfor %}
#### 词汇
|原文|翻译|链接|
|-|-|-|{% for annotation in annotations %}{% if annotation.colorCategory == "Blue" %}
|{{annotation.annotatedText}}|{{annotation.comment}}|[zotero-Link](zotero://open-pdf/library/items/{% for t in attachments %}{% if loop.first %}{{t.itemKey}}{% endif %}{% endfor %}?{{annotation.page}}&annotation={{annotation.id}})|{% endif %}{% endfor %}
#### 长难句
|原文|翻译|链接|
|-|-|-|{% for annotation in annotations %}{% if annotation.colorCategory == "Yellow" %}
|{{annotation.annotatedText}}|{{annotation.comment}}|[zotero-Link](zotero://open-pdf/library/items/{% for t in attachments %}{% if loop.first %}{{t.itemKey}}{% endif %}{% endfor %}?{{annotation.page}}&annotation={{annotation.id}})|{% endif %}{% endfor %}
[[Thu/Research3_SMO/Note/bibliographic/bibliographic|bibliographic]]