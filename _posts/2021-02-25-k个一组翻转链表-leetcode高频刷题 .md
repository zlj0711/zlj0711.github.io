---
layout:     post
title:      k个一组翻转链表
subtitle:   leetcode高频刷题
date:       2021-02-25
author:     zlj
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - leetcode
    - 高频刷题
    - 链表
---

上一次面试也看过这个题解

主要首先要把链表一次翻转的函数写清楚，也就是pre cur tmp三个指针 tmp用来存一下cur->next

注意在这道题，因为不能断开和后面的部分 所以pre的初始值应该是tail->next

还要注意的是这道题中使用了pair的用法

tie是直接获取pair返回值的方法

代码如下

```c++c++
class Solution {
public:
    pair<ListNode*, ListNode*> myReverse(ListNode* head, ListNode* tail){
        ListNode* tmp;
        ListNode* pre;
        ListNode* cur;
        pre = tail->next;
        cur = head;
        tmp = nullptr;
        while(pre != tail){
            tmp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = tmp;
        }
        return make_pair(tail, head);
    }
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode* hair = new ListNode(0);
        hair->next = head;
        ListNode* pre = hair;
        while(head){
            ListNode* tail = pre;
            for(int i = 0;i < k;i ++){
                tail = tail->next;
                if(!tail)
                    return hair->next;
            }
            ListNode* nex = tail->next;
            tie(head, tail) = myReverse(head, tail);
            pre->next = head;
            tail->next = nex;
            pre = tail;
            head = tail->next;
        }  
        return hair->next;    
    }
};
```

