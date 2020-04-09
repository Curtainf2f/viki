# 幸运N串-研发
```
时间限制：C/C++ 1秒，其他语言2秒

空间限制：C/C++ 64M，其他语言128M
```
小A很喜欢字母N，他认为连续的N串是他的幸运串。有一天小A看到了一个全部由大写字母组成的字符串，他被允许改变最多2个大写字母（也允许不改变或者只改变1个大写字母），使得字符串中所包含的最长的连续的N串的长度最长。你能帮助他吗？


输入描述:
输入的第一行是一个正整数T（0 < T <= 20），表示有T组测试数据。对于每一个测试数据包含一行大写字符串S（0 < |S| <= 50000，|S|表示字符串长度）。

数据范围：

20%的数据中，字符串长度不超过100；

70%的数据中，字符串长度不超过1000；

100%的数据中，字符串长度不超过50000。

输出描述:
对于每一组测试样例，输出一个整数，表示操作后包含的最长的连续N串的长度。

输入例子1:
```
3
NNTN
NNNNGGNNNN
NGNNNNGNNNNNNNNSNNNN
```

输出例子1:
```
4
10
18
```

# 代码

```cpp
#include<bits/stdc++.h>
using namespace std;

const int N = 2e5+5;

int main(){
    int t;
    scanf("%d", &t);
    while(t --){
        string s;
        cin >> s;
        int l = 0, r = 0, x = 0;
        int ans = 0, len = 0;
        while(r < s.size()){
            if(s[r] == 'N') r ++;
            else if(x < 2) r ++, x ++;
            else if(s[l] == 'N') l ++;
            else l ++, x --;
            ans = max(ans, r - l);
        }
        cout << ans << endl;
    }
    return 0;
}
```

# 思路

双指针，复杂度$O(n)$