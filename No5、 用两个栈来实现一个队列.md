# 自己的方法一

```c++
class Solution
{
public:
    void push(int node) {
        this->stack1.push(node);
    }

    int pop() {
        int item;
        if(this->stack2.empty())
        {
           while(stack1.size()!=1)
           {
               stack2.push(stack1.top());
               stack1.pop();
           }
           item = stack1.top();
           stack1.pop();
           return item;
        }
        item = this->stack2.top();
        this->stack2.pop();
        return item;
    }

private:
    stack<int> stack1;
    stack<int> stack2;
};
```

# 别人的方法  经过测试一般般  还不如我自己写的呢

```c++
public:
    void push(int node) {
        stack1.push(node);
    }

    int pop() {
        while(stack1.size() != 1){
            stack2.push(stack1.top());
            stack1.pop();

        }
        int value = stack1.top();
        stack1.pop();
        while(!stack2.empty()){   //我觉得不需要复原   
            stack1.push(stack2.top());
            stack2.pop();
        }

        return value;        
    }

private:
    stack<int> stack1;//保存元素
    stack<int> stack2;//辅助栈
};

```

