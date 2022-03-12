[toc]

# 方法一

使用DFS搜索

这个题目的灵性在于建立标记矩阵，防止无尽的递归检索。

```c++
class Solution {
    int x_min;
    int x_max;
    int y_min;
    int y_max;
    int size_m;
    int size_n;
    int target;
    bool result;
    vector<vector<int> > array;
    vector<vector<bool> > array_flag;
public:
    //todo: dfs优化  还是要把判断函数弄出来   方便在其他地方利用
    void DFS(int x, int y)
    {
        if(result)
            return;
    
        if(x<0||y<0||x>=size_m||y>=size_n)
            return;
        
        //if((x<=x_min&&y<=y_min)||(x>=x_max&&y>=y_max)) 不行
        //    return;
        
        //if((x<=x_min&&y<=y_min)&&(x>=x_max&&y>=y_max)) //可以 多了几种处理的可能 多个方向   多了几个方向   进入矩形交叉区域
       //     return;   //这东西不加也能跟过！！！
        
        if(array_flag[x][y])
            return;
        if(array[x][y]==target)
        {
            result=true;
            array_flag[x][y]=true;
            return;
        }
        if(array[x][y]<target)
        {
            array_flag[x][y]=true;
            x_min = max(x,x_min);
            y_min = max(y,y_min);
            DFS(x+1, y+1);  //斜方向
            if(result)
                return;
            DFS(x, y+1);  //方向
            
            if(result)
                return;
            DFS(x-1, y+1);  //方向
            if(result)
                return;
            DFS(x+1, y-1);  //方向
            if(result)
                return;
            DFS(x+1, y);  //方向
            if(result)
                return;
        }
        if(array[x][y]>target)
        {
            array_flag[x][y]=true;
            x_max = min(x,x_max); 
            y_max = min(y,y_max);
            
            DFS(x-1, y-1);  //斜方向
            if(result)
                return;
            DFS(x, y-1);  //斜方向
            if(result)
                return;
            DFS(x+1, y-1);  //斜方向
            if(result)
                return;
            DFS(x-1, y+1);  //斜方向
            if(result)
                return;
            DFS(x-1, y);  //斜方向
            if(result)
                return;
        }
      
    }
        
    bool Find(int target, vector<vector<int> > array) {
        this->target=target;
        result = false;
        if(array.empty())
            return result;
        if(array[0].empty())
            return result;
        x_min = -1;
        x_max = array.size();
        size_m =array.size();
        y_min = -1;
        y_max = array[0].size();
        size_n = array[0].size();
        vector<vector<bool> > array_flag(x_max,vector<bool>(y_max,false));
        this->array_flag = array_flag;
        this->array=array;
        DFS(0, 0);
        return result;
    }
};
```

## 改进版本

```c
class Solution {
    int size_m;
    int size_n;
    int target;
    bool result;
    vector<vector<int> > array;
    vector<vector<bool> > array_flag;
public:
    //dfs优化  还是要把判断函数弄出来   方便在其他地方利用
    void DFS(int x, int y)
    {
        if(result)
            return;
    
        if(x<0||y<0||x>=size_m||y>=size_n)
            return;
        
        if(array_flag[x][y])
            return;
        if(array[x][y]==target)
        {
            result=true;
            array_flag[x][y]=true;
            return;
        }
        if(array[x][y]<target)
        {
            array_flag[x][y]=true;
            DFS(x+1, y+1);  //斜方向
            if(result)
                return;
            DFS(x, y+1);  //方向
            
            if(result)
                return;
            DFS(x-1, y+1);  //方向
            if(result)
                return;
            DFS(x+1, y-1);  //方向
            if(result)
                return;
            DFS(x+1, y);  //方向
            if(result)
                return;
        }
        if(array[x][y]>target)
        {
            array_flag[x][y]=true;
            DFS(x-1, y-1);  //斜方向
            if(result)
                return;
            DFS(x, y-1);  //斜方向
            if(result)
                return;
            DFS(x+1, y-1);  //斜方向
            if(result)
                return;
            DFS(x-1, y+1);  //斜方向
            if(result)
                return;
            DFS(x-1, y);  //斜方向
            if(result)
                return;
        }
      
    }
        
    bool Find(int target, vector<vector<int> > array) {
        this->target=target;
        result = false;
        if(array.empty())
            return result;
        if(array[0].empty())
            return result;
        size_m =array.size();
        size_n = array[0].size();
        vector<vector<bool> > array_flag(size_m,vector<bool>(size_n,false));
        this->array_flag = array_flag;
        this->array=array;
        DFS(0, 0);
        return result;
    }
};
```

# 方法二

因为确定一旦行列确定了，其实就能确定一个矩阵中的元素。

搜索也可以从行列出发，行扫描  列扫描   如果矩阵中不存在某个元素 则 行列编号都到底边界

行从最小，列从最大，开始扫描。

使用行中最大的值可以排除该行

使用列中最小的值可以排除该列

同理列中最大，行中最小也是可以的。

```c++
class Solution {
    
public:
    bool Find(int target, vector<vector<int> > array) {      
        if(array.empty())
            return false;
        if(array[0].empty())
            return false;
        int m,n;
        m = array.size();
        n = array[0].size();
        int x=0,y=n-1;
        for(int i=1; i<=m+n; i++)
        {
            if(x>=m||y>=n||x<0||y<0)   //注意这里  矩阵一定要最大值和最小值都考虑
                  return false;
            if(array[x][y]==target)
                return true;
            if(array[x][y]>target)
            {
                y--;
               
                continue;
            }
            if(array[x][y]<target)
            {
                x++;
                continue;
            }
           
        }
        return false;
    }
};
```

