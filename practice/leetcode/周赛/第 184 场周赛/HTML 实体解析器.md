# HTML 实体解析器

「HTML 实体解析器」 是一种特殊的解析器，它将 HTML 代码作为输入，并用字符本身替换掉所有这些特殊的字符实体。

HTML 里这些特殊字符和它们对应的字符实体包括：

双引号：字符实体为 &quot; ，对应的字符是 " 。
单引号：字符实体为 &apos; ，对应的字符是 ' 。
与符号：字符实体为 &amp; ，对应对的字符是 & 。
大于号：字符实体为 &gt; ，对应的字符是 > 。
小于号：字符实体为 &lt; ，对应的字符是 < 。
斜线号：字符实体为 &frasl; ，对应的字符是 / 。
给你输入字符串 text ，请你实现一个 HTML 实体解析器，返回解析器解析后的结果。

示例 1：
```
输入：text = "&amp; is an HTML entity but &ambassador; is not."
输出："& is an HTML entity but &ambassador; is not."
解释：解析器把字符实体 &amp; 用 & 替换
```
示例 2：
```
输入：text = "and I quote: &quot;...&quot;"
输出："and I quote: \"...\""
```
示例 3：
```
输入：text = "Stay home! Practice on Leetcode :)"
输出："Stay home! Practice on Leetcode :)"
```
示例 4：
```
输入：text = "x &gt; y &amp;&amp; x &lt; y is always false"
输出："x > y && x < y is always false"
```
示例 5：
```
输入：text = "leetcode.com&frasl;problemset&frasl;all"
输出："leetcode.com/problemset/all"
```

提示：

1 <= text.length <= 10^5
字符串可能包含 256 个ASCII 字符中的任意字符。
```
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/html-entity-parser
```

# 代码

```cpp
class Solution {
public:
    unordered_map<string, char> mp;
    string entityParser(string text) {
        mp["&quot;"] = '\"';
        mp["&apos;"] = '\'';
        mp["&amp;"] = '&';
        mp["&gt;"] = '>';
        mp["&lt;"] = '<';
        mp["&frasl;"] = '/';
        string tmp, ans;
        for(auto x : text){
            if(x == '&'){
                ans += tmp;
                tmp.clear();
                tmp.push_back('&');
            }else if(tmp.size()){
                tmp.push_back(x);
                if(mp.find(tmp) != mp.end()){
                    ans.push_back(mp[tmp]);
                    tmp.clear();
                }
            }else ans.push_back(x);
        }
        ans += tmp;
        return ans;
    }
};
```

# 思路

+ 如果直接循环替换字符串中的特定字符串，复杂度最坏为$O(n^2)$（假设字符串中全是特殊的字符实体）不符合预期

所以将使用另一个字符串来辅助进行，模拟读入

当读入到 `&`时，就将之后的字符加入到判断字符串 tmp 中， 一旦发现是特殊的字符实体，则将其替换，拼接到 ans 字符串中。否则直到结束，或者遇到下一个`&`，将tmp中的内容拼接到ans中。

最终复杂度$O(n)$