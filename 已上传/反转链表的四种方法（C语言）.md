

@[toc]
# 反转链表
[206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

**示例 1：**

![img](https://img-blog.csdnimg.cn/img_convert/be580c2244815c873bf68798ab433f83.jpeg)

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

**示例 2：**

![img](https://img-blog.csdnimg.cn/img_convert/05b4e3914f52abda76109e1f18788aa8.jpeg)

```
输入：head = [1,2]
输出：[2,1]
```

**示例 3：**

```
输入：head = []
输出：[]
```

 

**提示：**

- 链表中节点的数目范围是 `[0, 5000]`
- `-5000 <= Node.val <= 5000`

 

# 方法一 原地反转
本题链表不带头结点
原地反转需要两个指针，以防`pre`指针操作后指向上一节点而丢失指向下一节点的指针
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/4cfc23eeb0a442a4bbd61848d3a6df29.png#pic_center)

`pre->next = cur->next`:1.连:第一步先将`pre`所在节点和`cur->next`所在节点连接起来,将当前节点 `cur` 从链表中移除，并把链表连接起来。
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/5136f0644f7648db97b895cfc1088429.png#pic_center)
`cur->next = head;`2.掉:将cur的next指向head，将cur插入到head的前面,以便将其放置到链表的前面，成为新的头节点。 ![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/2e4130d4722943db8d55c5e3086895f7.png#pic_center)

`head = cur;`3.接:更新head，使其指向当前的cur
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/83b42fe35912486ca46236fa4722ff89.png#pic_center)

`cur = pre->next;`4.移:更新cur，移动到下一个待反转的节点
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/48d391823bef4294aa27042a47de2764.png#pic_center)




```c
struct ListNode* reverseList(struct ListNode* head) {
    if (head == NULL) {
        return head;
    }
    struct ListNode* cur = head->next; 
    struct ListNode* pre = head; 
    while (cur) {
        pre->next = cur->next; 
        cur->next = head; 
        head = cur;   
        cur = pre->next; 
    }
    return head; 
}
```

# 方法二 迭代
`temp`保存`cur`的下一个节点，以防止反转时丢失链表信息。
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/b445ba4909d8462fa9a09733c1854e6f.png#pic_center)

`cur->next = pre;`将`cur`的下一个指针指向`pre`,实现反转。
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/837d20127836499db1dfc85394aba10b.png#pic_center)

更新`pre`为当前的`cur`,为下一次迭代做准备。
更新`cur`为保存的下一个节点`temp`,继续迭代。
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/0a9ae39ce90b4575ad197518a8180a2a.png#pic_center)
通过不断改变指针的指向将链表进行反转。`pre`充当新链表的头节点,当`cur`为`NULL`循环停止，返回`pre`

```c
struct ListNode* reverseList(struct ListNode* head) {
    struct ListNode* pre = NULL;
    struct ListNode* cur = head;
    while (cur != NULL) {
        struct ListNode* temp = cur->next;
        cur->next = pre;
        pre = cur;
        cur = temp;
    }
    return pre;
}
```



# 方法三 递归
我们可以通过迭代的方法来得到递归方法
观察reverse函数，
函数声明中`pre`指针指向的为NULL，`cur`指针指向的为`head`，正如递归中声明并初始化的`pre`和`cur`指针
递归结束条件为`cur`为`NULL`,返回`pre`
同样`temp`保存`cur`的下一个节点，以防止反转时丢失链表信息。
然后进行反转`cur->next = pre;`
`pre `为当前已反转部分的头节点，`cur`为当前待反转的节点。
然后调用递归，将`cur`作为新的`pre`传入下一层，将`temp`作为新的`cur`传入下一层。
实现了链表的递归反转
```c
struct ListNode* reverse(struct ListNode* pre, struct ListNode* cur) {
    if (!cur){
        return pre;
    }
    struct ListNode* temp = cur->next;
    cur->next = pre;
    //将cur作为pre传入下一层
    //将temp作为cur传入下一层，改变其指针指向当前cur
    return reverse(cur, temp);
}

struct ListNode* reverseList(struct ListNode* head) {
    return reverse(NULL, head);
}
```
# 方法四 新建链表法

`struct ListNode* newNode = (struct ListNode*)malloc(sizeof(struct ListNode));`
` newNode->val = head->val;`
新建一个链表，并初始化
`newNode->next = newHead;`将新节点插入到新链表的头部

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/5ad0b3ea079c4a37a7431f072762f6b6.png#pic_center)
`newHead = newNode;`
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/a041ed9d2e5344628a3be10e3dff44f7.png#pic_center)
` head = head->next;`
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/354cd83f3dc34effb093a8aaadfc3aee.png#pic_center)
遍历原链表，使用头插法新建链表，并不断移动`newHead`指针，当原链表头指针指向`NULL`最终返回`newHead`






```c

struct ListNode* reverseList(struct ListNode* head) {
    struct ListNode* newHead = NULL;
    while (head) {
        // 创建一个新节点
        struct ListNode* newNode = (struct ListNode*)malloc(sizeof(struct ListNode));
        newNode->val = head->val;        
        // 将新节点插入到新链表的头部
        newNode->next = newHead;
        newHead = newNode;
        // 移动到原链表的下一个节点
        head = head->next;
    }
    return newHead;
}
```
---
---

感谢您的阅读

