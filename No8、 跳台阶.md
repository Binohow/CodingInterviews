# 方法一：DFS

```c++
class Solution {
    int counter {0};
public:
    void DFS(int n)
    {
        if(n==0 || n==1)
        {
            counter++;
            return;
        }
        DFS(n-1);
        if(n>=2)    //算是减枝了
            DFS(n-2);
    }
    int jumpFloor(int number) {
  
           DFS(number);
        return counter;
    }
};
```

# 别人的方法：递归

```c++
int jumpFloor(int number) {
    if(number==1) return 1;   //巧妙
    if(number==2) return 2;   //巧妙
    return jumpFloor(number-1) + jumpFloor(number-2);  //这个状态等于 之前两个状态加起来    
}
```

这个方法简单是简单，也很高雅， 但是涉及太多变量，反而效率比较低。

从上边这个递归方程很容易看出来，这其实就是斐波拉契数列，所以必然有斐波拉契数列的非递归解法

## 