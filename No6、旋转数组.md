# 没做出来

太多干扰了，把简单问题给复杂化了。

# 常规做法

这就是常见的寻找非递减的元素啊！！！

```c++
class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) {
        if(rotateArray.empty())
            return 0;
        if(rotateArray.size()==1)
            return rotateArray[0];
        for (int i=1; i<rotateArray.size(); i++ )
        {
            if(rotateArray[i]<rotateArray[i-1])  //非递减判断   很常见   居然没想出来  醉了
                return rotateArray[i];
        }
        return rotateArray[0];
    }
};
```

# 二分法 很巧妙  无法参透

```c++
int minNumberInRotateArray(vector<int> rotateArray) {
    if (rotateArray.size() == 0) return 0;
    int low = 0, high = rotateArray.size() - 1;
    while (low + 1 < high) {
        int mid = low + (high - low) / 2;
        if (rotateArray[mid] < rotateArray[high]) high = mid;  //说明右边有序，那就向左边走
        else if (rotateArray[mid] == rotateArray[high]) high = high-1;// 这种情况跟是特例只能一个一个的判断   就很巧妙   没办法参透  无法判断有序无序  无论如何rotateArray[high]
        else {
            low = mid;  //左边无序 向右走
        }
    }
    return min(rotateArray[low], rotateArray[high]);  //这东西如何想出来，很神奇   
    
    //一般来说 rotateArray[high];是小的 因为 higt这一侧值（右侧）是偏小的 而且始终在往更小的地方变化
    // [1,2,2,2,2,2]     return  rotateArray[high]; 就不行    必须 return min(rotateArray[low], rotateArray[high]);
}
```

记住你的目的是找到无序的地方。

