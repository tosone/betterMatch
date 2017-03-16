# betterMatch

### Introduction
对输入的一段文字进行模糊匹配，对输入的字符串和目标字符串之间选取最相近的最高匹配度的部分。

### Usage

`betterMatch(Origin, Matches [, Whole [, Similarity [, Search]]])`

- `Origin` [String] 原始的字符串。
- `Matches` [Array] 希望匹配的关键词列表。
- `Whole` [Boolean] 是否进行全匹配，默认否，若不进行全部匹配，则当匹配到某个字符串的时候，符合要求就返回，返回的数据类型是 String 或者 null。若进行全匹配，那么会将所有的关键词列表匹配一遍，将所有符合条件的关键词以匹配度的精确程度排名组成一个数组返回，返回值为数组数组可能为空。
- `Similarity` [Number] 相似度，默认相似度超过 Origin 的字符串长度，就认为不满足条件。
- `Search` [Boolean] 是否进行搜索匹配。

### Detail
- 传入参数后首先会进行精确匹配，查看传入的文字是否含有可匹配的关键词。
- 精确匹配失败后，会进行模糊匹配的流程，首先将传入的文字转换成拼音，用目标串和匹配串进行拼音的模糊匹配，得到目标串中对匹配所有匹配串中最高匹配度部分的最大匹配度的匹配串。
- 模糊匹配失败后，会进行搜索匹配的流程，主要针对于目标串的长度非常短的情况，首先用目标串在匹配串中进行模糊匹配，得到模糊匹配度，再加上目标串和匹配串之间的长度差，最终得到匹配度。将小于设置的相似度的匹配串返回。
- 针对于目标串和匹配串的长度都非常短的情况，建议将相似度设置的小一点，避免匹配被串了。

``` javascript
console.log('画', ['花甲', '花架', '回家', '会瞎']);
console.log('单词匹配结果：', betterMatch('画家', ['花甲', '花架', '回家', '会瞎']));
// 单词匹配结果： 花甲
console.log('列表匹配结果：', betterMatch('画家', ['花甲', '花架', '回家', '会瞎'], true));
// 列表匹配结果： [ '花甲', '花架', '回家', '会瞎' ]

console.log('画', ['花甲', '花架', '回家', '会瞎']);
console.log('搜索匹配结果：', betterMatch('画', ['花甲', '花架', '回家', '会瞎'], true, 3, true));
// 搜索匹配结果： [ '花甲', '花架', '回家', '会瞎' ]
console.log('普通匹配结果：', betterMatch('画', ['花甲', '花架', '回家', '会瞎'], true));
// 普通匹配结果： []
```

### 匹配

|Match|Origin|Similarity|
|:---:|:---:|:---:|
|画家|花甲|0|
|画家|花架|0|
|画家|回家|1|
|画家|会瞎|2|
|画家|麾下|2|
|画家|慧霞|2|
|张叉|装叉|1|
|张|装叉|2|
