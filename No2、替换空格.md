# 方法一  正则表达式

```c++
#include<regex>
class Solution {
   //这里放置变量要小心  可能会报错
public:
	void replaceSpace(char *str,int length) {
        string str_line(str);   //根据字符串指针初始化string
        regex reg("( )");       //正则表达式的空格表示方法
        // regex reg(" ");       //正则表达式的空格表示方法  这样也是可以的
        // regex reg("\\s");    //这个也是可以的
        //  regex reg("\s");    //这个是不可以的
        str_line=regex_replace(str_line,reg,"%20"); //正则表达式替换
        strcpy(str, str_line.c_str());   //注意学习string的char *的提取
	}
};
```