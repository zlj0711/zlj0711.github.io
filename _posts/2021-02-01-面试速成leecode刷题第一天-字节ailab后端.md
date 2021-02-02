---
layout:     post
title:      面试速成leetcode刷题第一天
subtitle:   字节ailab后端
date:       2021-02-01
author:     zlj
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - leetcode
    - 面试速刷
    - 字节跳动
---

##### （一）无重复字符的最长字串

方法：滑动窗口

![image-20210201101709819](D:\PRE\zlj0711.github.io\_posts\image-20210201101709819.png)

![image-20210201102025221](D:\PRE\zlj0711.github.io\_posts\image-20210201102025221.png)

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int>rem;
        int start = 0;
        int end = 0;
        int length = 0;
        int result = 0;
        while(end < s.length()){
            char tmp = s[end];
            if(rem.find(tmp) != rem.end() && rem[tmp] >= start){
                start = rem[tmp] + 1;
                length = end - start;
            }
            rem[tmp] = end;
            end ++;
            length ++;
            result = max(result, length);
        }
        return result;    

    }
};
```



##### （二）k个一组翻转链表

（1）题干

（2）思路

![image-20210201140616229](D:\PRE\zlj0711.github.io\_posts\image-20210201140616229.png)

（3）代码

```c++
class Solution {
public:
    pair<ListNode*, ListNode*> myReverse(ListNode* head, ListNode* tail) {
        ListNode *pre =  tail->next;
        ListNode* cur = head;
        ListNode* tmp = NULL;
        while(pre != tail){
            tmp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = tmp;
        }
        return {tail, head};
    }

    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode* hair = new ListNode(0);
        hair->next = head;
        ListNode* pre = hair;
        while (head) {
            ListNode* tail = pre;
            for (int i = 0; i < k; ++i) {
                tail = tail->next;
                if (!tail) {
                    return hair->next;
                }
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



##### （三）三数之和

（1）题干

![image-20210201103959726](D:\PRE\zlj0711.github.io\_posts\image-20210201103959726.png)

（2）思路

![image-20210201105346343](D:\PRE\zlj0711.github.io\_posts\image-20210201105346343.png)

![image-20210201105416851](D:\PRE\zlj0711.github.io\_posts\image-20210201105416851.png)

![image-20210201105506210](D:\PRE\zlj0711.github.io\_posts\image-20210201105506210.png)

![image-20210201105644504](D:\PRE\zlj0711.github.io\_posts\image-20210201105644504.png)

![image-20210201105735508](D:\PRE\zlj0711.github.io\_posts\image-20210201105735508.png)

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int size = nums.size();
        if(size < 3) return{};
        vector<vector<int>> res;
        std::sort(nums.begin(), nums.end());
        for(int i = 0; i < size;i ++){
            if(nums[i] > 0) return res;
            if(i > 0 && nums[i] == nums[i - 1]) continue;
            int left = i + 1;
            int right = size - 1;
            while(left < right){
                if(nums[left] + nums[right] > -nums[i]){
                    right--;
                }
                else if(nums[left] + nums[right] < -nums[i]){
                    left ++;
                }
                else{
                    res.push_back(vector<int>{nums[i], nums[left], nums[right]});
                    left ++;
                    right --;
                    while(left < right && nums[left] == nums[left - 1])
                        left ++;
                    while(left < right && nums[right] == nums[right + 1])
                        right --;

                }                  
            }
        }
        return res;
    }
};
```



##### （四）接雨水

![image-20210201203435210](D:\PRE\zlj0711.github.io\_posts\image-20210201203435210.png)

![image-20210201203516518](D:\PRE\zlj0711.github.io\_posts\image-20210201203516518.png)

![image-20210201203545482](D:\PRE\zlj0711.github.io\_posts\image-20210201203545482.png)

![image-20210201204145675](D:\PRE\zlj0711.github.io\_posts\image-20210201204145675.png)

![image-20210201205128476](D:\PRE\zlj0711.github.io\_posts\image-20210201205128476.png)



```c++
class Solution {
public:
    int trap(vector<int>& height) {
        if(height.empty())
            return 0;
        int res = 0;
        int size = height.size();
        vector<int> left(size);
        vector<int> right(size);
        left[0] = height[0];
        for(int i = 1;i < size;i ++){
            left[i] = max(height[i], left[i - 1]);
        }
        right[size - 1] = height[size - 1];
        for(int i = size - 2; i >= 0;i --){
            right[i] = max(height[i], right[i + 1]);
        }
        for(int i = 1;i < size - 1;i ++){
            res += min(left[i], right[i]) - height[i];
        }
        return res;
    }
};
```



##### （五）字符串相加

![image-20210201211310343](D:\PRE\zlj0711.github.io\_posts\image-20210201211310343.png)

最后注意需要反转一下，才得到答案

```c++
class Solution {
public:
    string addStrings(string num1, string num2) {
        int i = num1.length() - 1;
        int j = num2.length() - 1;
        int add = 0;
        string res = "";
        while(i >= 0 || j >= 0 || add != 0){
            int x = i >= 0 ? num1[i] - '0' : 0;
            int y = j >= 0 ? num2[j] - '0' : 0;
            int result = x + y + add;
            res.push_back('0' + result % 10);
            add = result / 10;
            i --;
            j --;
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```

##### （六）二叉树的锯齿形层次遍历

（1）思路

![image-20210201190159097](D:\PRE\zlj0711.github.io\_posts\image-20210201190159097.png)

![image-20210201190112651](D:\PRE\zlj0711.github.io\_posts\image-20210201190112651.png)

用队列实现BFS按层遍历，定义双端队列存储当前层的值

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> res;
        if(!root)
            return res;
        queue<TreeNode> que;
        bool left = true;
        que.push(*root);
        while (!que.empty())
        {
            deque<int> dque;
            int n = que.size();
            for(int i = 0;i < n;i++){
                auto node = que.front();
                que.pop();
                if(left){
                    dque.push_back(node.val);
                }
                else{
                    dque.push_front(node.val);
                }
                if(node.left)que.push(*node.left);
                if(node.right)que.push(*node.right);
            }
            res.push_back(vector<int>{dque.begin(), dque.end()});
            left = !left;
        }
        return res;       
    }
};
```

##### 补充二叉树的层次遍历

用bfs

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        if(!root)
            return res;
        queue<TreeNode*>que;
        que.push(root);
        while(!que.empty()){
            int n = que.size();
            res.push_back(vector<int>());
            for(int i = 0;i < n;i++){
                auto node = que.front();
                que.pop();
                res.back().push_back(node->val);
                if(node->left)
                    que.push(node->left);
                if(node->right)
                    que.push(node->right);
            }
        }
        return res;      
    }
};
```



##### （七）买卖股票最佳时机

![image-20210202095129366](D:\PRE\zlj0711.github.io\_posts\image-20210202095129366.png)

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.empty())
            return 0;
        int n = prices.size();
        vector<vector<int>>dp(n, vector<int>(2, 0));
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        for(int i = 1;i < n;i ++){
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            dp[i][1] = max(dp[i - 1][1], -prices[i]);
        }
        return dp[n - 1][0];
    }
};
```



##### （八）反转链表

（1）思路

![迭代.gif](https://pic.leetcode-cn.com/7d8712af4fbb870537607b1dd95d66c248eb178db4319919c32d9304ee85b602-%E8%BF%AD%E4%BB%A3.gif)

（2）代码

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* pre = NULL;
        ListNode* cur = head;
        ListNode* tmp = NULL;
        while(cur != NULL){
            tmp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = tmp;
        }
        return pre;
    }
};
```

##### （九）两数之和

（1）题干

![image-20210201151429969](D:\PRE\zlj0711.github.io\_posts\image-20210201151429969.png)

（2）思路

![image-20210201151412454](D:\PRE\zlj0711.github.io\_posts\image-20210201151412454.png)

![image-20210201154719086](D:\PRE\zlj0711.github.io\_posts\image-20210201154719086.png)

![image-20210201154738372](D:\PRE\zlj0711.github.io\_posts\image-20210201154738372.png)

（3）代码

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> hashtable;
        for (int i = 0; i < nums.size(); ++i) {
            auto it = hashtable.find(target - nums[i]);
            if (it != hashtable.end()) {
                return {it->second, i};
            }
            hashtable[nums[i]] = i;
        }
        return {};
    }
};

```



##### （十）二叉树的右视图

（1）题干

![image-20210201213713210](D:\PRE\zlj0711.github.io\_posts\image-20210201213713210.png)

（2）思路

dfs

![image-20210201215306160](D:\PRE\zlj0711.github.io\_posts\image-20210201215306160.png)

bfs

![image-20210201215331704](D:\PRE\zlj0711.github.io\_posts\image-20210201215331704.png)

bfs的代码

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        if(!root)
            return {};
        vector<int> res;
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty()){
            int n = que.size();
            for(int i = 0;i < n;i ++){
                auto node = que.front();
                que.pop();
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
                if(i == n - 1)
                    res.push_back(node->val);
            }
        }
        return res;
    }
};
```

