# 方法一  insert一直在第0个位置插入

```c++
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        vector<int> result;
        for( ; head; head=head->next)
        {
            result.insert(result.begin(), head->val);   //注意传入的参数只能是迭代器类型    复杂度为0(N)
        }
        return result;
    }
};
```

整体复杂度$O(N^2)$

# 方法二：使用**reverse**

  reverse(result.begin(),result.end());  传入也是迭代器的值 复杂度O(N)

整体复杂度 $O(N+N)=O(2N)$

# 方法三：  return vector\<int>(result.rbegin(),result.rend());   妙啊