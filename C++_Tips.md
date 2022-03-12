# new

```
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}   //注意到c++中struct是有构造函数的
 * };
 */
 
TreeNode* treeNode = new TreeNode(pre[0]);   //这个new的方法也是需要学习的    调用了构造函数


//基本用法
int *a = new int[5];
class A {...}   //声明一个类 A
A *obj = new A();  //使用 new 创建对象
delete []a;
delete obj;
```

