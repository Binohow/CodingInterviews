# 基础知识

[(4 条消息) 已知前序遍历序列和中序遍历序列，确定唯一的一棵二叉树和后序遍历序列\_俱往矣 - CSDN 博客\_先序遍历和中序遍历确定唯一二叉树](https://blog.csdn.net/Gakki_wpt/article/details/92805382)

# 方法一：自己的解法，递归法   

不断的分割数组，直到左右数组边界错位（左边大于右边）。

```c++
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
    int pre_index = 0;
    int size;
    bool flag_end = false;
    vector<int> pre;
    vector<int> vin;
public:
    int find_index_vin(int target, int L, int R)
    {
        int index;
        for(int i=L; i<=R; i++)
        {
            if(vin[i]==target)
            {
                return i;
            }
        }
        return index;
    }
    TreeNode* DFS(int L, int R)
    {
        if(flag_end)     //经过这个测试，没有这个变量减枝也是可以的。
            return NULL;
        
        if(pre_index>=size) 
        {
            flag_end = true; 
            return NULL ;
        }
          
        if(L<0||R>=size)
        {
            return NULL;
        }
        if(L>R)
        {
            return NULL;
        }
        TreeNode * treenode=(TreeNode *)malloc(sizeof( TreeNode));   
        
        if(L==R)
        {
            treenode->left = NULL;
            treenode->right = NULL;
            treenode->val = vin[L];
        }
        
        int root_index_vin = find_index_vin(pre[pre_index], L, R);
        treenode->val = vin[root_index_vin];
        pre_index++;
        //左子树
        //提前判断有无左子树
        if(L<0||(root_index_vin-1)<0||((root_index_vin-1)<L)) //不存在左子树
        {
            treenode->left=NULL;
        }
        else
        {
            treenode->left = DFS(L, root_index_vin-1);
        }
        
        
        if((root_index_vin+1)>=size||R>=size||((root_index_vin+1)>R)) //不存在右子树
        {
            treenode->right=NULL;
        }
        else
        {
            treenode->right = DFS(root_index_vin+1,R);
        }
        
        return treenode;
    }
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {
        if(pre.empty()||vin.empty())
            return NULL;
        this->size = pre.size();
        this->pre = pre;
        this->vin = vin;
        return  DFS(0, pre.size()-1);
    }
};
```

这个写法本质上是C语言的写法，没有发挥C++ STL的威力。

## 简化边界版本   去除一些不必要的约束   边界条件写得仔细点总是没有错

```c++
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
    int pre_index = 0;
    int size;
    vector<int> pre;
    vector<int> vin;
public:
    int find_index_vin(int target, int L, int R)
    {
        int index;
        for(int i=L; i<=R; i++)
        {
            if(vin[i]==target)
            {
                return i;
            }
        }
        return index;
    }
    TreeNode* DFS(int L, int R)
    {
           
        if(L<0||R>=size) //边界区间的判断
        {
            return NULL;
        }
        if(L>R)  //子区间的交叉判罚  巧妙点
        {
            return NULL;
        }
        TreeNode * treenode=(TreeNode *)malloc(sizeof( TreeNode));   
        
        if(L==R)
        {
            treenode->left = NULL;
            treenode->right = NULL;
            treenode->val = vin[L];
        }
        
        int root_index_vin = find_index_vin(pre[pre_index], L, R);
        treenode->val = vin[root_index_vin];
        pre_index++;
        //左子树
        treenode->left = DFS(L, root_index_vin-1);
        treenode->right = DFS(root_index_vin+1,R);

        return treenode;
    }
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {
        if(pre.empty()||vin.empty())
            return NULL;
        this->size = pre.size();
        this->pre = pre;
        this->vin = vin;
        return  DFS(0, pre.size()-1);
    }
};
```



# 方法二  STL版本                                                                                                                                                                                                                                                                                                                                                                                                                                                                  

很是简洁  尤其是==边界处理==，简直了，恰到好处，学不来，学不来

需要首先熟悉二叉树先序遍历与中序遍历的规则。 先找到 preorder 中的起始元素作为根节点，在 inorder 中找到根节点的索引 mid；那么，preorder [1:mid + 1] 为左子树，preorder [mid + 1:] 为右子树；inorder [0:mid] 为左子树，inorder [mid + 1:] 为右子树。递归建立二叉树。

```c++
TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {
         if (pre.size() == 0 || vin.size() == 0) {
            return NULL;                                
        }
        TreeNode* treeNode = new TreeNode(pre[0]);   //这个new的方法也是需要学习的
        int mid = distance(begin(vin), find(vin.begin(), vin.end(), pre[0])); //mid是inorder数组中的根节点坐标 mid+1是包括根节点在内的元素个数 
        vector<int> left_pre(pre.begin() + 1, pre.begin() + mid + 1);   //这个构造函数输入的可能是左开右闭区间[ )  需要测试，经过测试确实如此。   去除根节点的左子树的全部内容 发生了拷贝构造
        vector<int> right_pre(pre.begin() + mid + 1, pre.end());  //划分集合  后半部分  右子树
        vector<int> left_in(vin.begin(), vin.begin() + mid);   //去除根节点 根节点左侧  
        vector<int> right_in(vin.begin() + mid + 1, vin.end()); //去除根节点  根节点右侧

        treeNode->left = reConstructBinaryTree(left_pre, left_in); 
        treeNode->right = reConstructBinaryTree(right_pre, right_in);
        return treeNode;
    }
```

==vector<int> left_pre(pre.begin() + 1, pre.begin() + mid + 1);发生了拷贝构造，这是关键啊==

巧妙利用，这些构造情况

```c++
	vector<int> c{1,2,3,4,5};
    vector<int> f(c.begin()+1, c.begin()+1); //合法
	printf("f.size()=%d\n",f.size());   //输出结果为0

	vector<int> g(c.begin() + 2, c.begin() + 1);  //不合法运行时错误
	printf("f.size()=%d\n", g.size());


	vector<int> h(c.begin() + 6, c.begin() + 6);  //不合法
	printf("h.size()=%d\n", h.size());

	vector<int> o(c.begin() + 5, c.begin() + 5); //等价于c.end()  合法 成功创建
	printf("o.size()=%d\n", o.size());
```

