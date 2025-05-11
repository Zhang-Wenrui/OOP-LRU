# OOP-LRU
记录了算是第一次自己进行的面向对象编程
写之前花了很久读题目，看了答案仍然觉得很模糊，找了张纸写了下思路，突然清晰了点
![b339bd758fb1d8ae21f4f4e1ba5a1b1](https://github.com/user-attachments/assets/45a1e89b-d2e6-449a-a6bc-2589b87ae100)

```python
class BListnode:
    def __init__(self,key=0,value=0):
        self.key=key
        self.value=value
        self.prev=None
        self.next=None
class LRUCache:
    def __init__(self, capacity: int):
        self.capacity=capacity
        self.cache={}
        self.head=BListnode()
        self.tail=BListnode()
        self.head.next=self.tail
        self.tail.prev=self.head

    def add_to_head(self,node):  
        
        node.prev=self.head
        node.next=self.head.next
        self.head.next.prev=node
        self.head.next=node
    def remove_node(self,node):
        node.prev.next=node.next
        node.next.prev=node.prev
    def move_node(self,node):  #存在对应key，只需移动
        self.remove_node(node)
        self.add_to_head(node)
        
    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        node=self.cache[key]  #找到双链表中对应节点
        self.move_node(node)  #移动这个节点
        return node.value   #返回节点对应value



    def put(self, key: int, value: int) -> None:
        if key in self.cache:   #key就在哈希表中，更新value即可
            node = self.cache[key]
            node.value = value
            self.move_node(node)
        else:
            node=BListnode(key,value)
            if len(self.cache) >= self.capacity:  #容量满了
                key_remove=self.tail.prev
                self.remove_node(key_remove)
                self.cache.pop(key_remove.key)
                self.cache[key] = node #加到双链表
                self.add_to_head(node)
            else:
                self.cache[key] = node #加到双链表
                self.add_to_head(node)


```
#problenm
##1.为什么在get方法中，node = self.cache[key]而不能直接node=key
![image](https://github.com/user-attachments/assets/dd8144b8-e28e-4c7d-b32f-58c7bf5a69e7)
##2.我理解 get 的过程是：如果 key 存在哈希表中，就通过 key 查找到对应的 value。但我不明白这个 value 是怎么和 DLinkedNode 关联的。硬要说的话，双链表中某个节点的值就是哈希表中那个键对应的值？
![image](https://github.com/user-attachments/assets/e5a186d6-970a-49b2-915c-a87b1fbd1fa0)
这里哈希表的value类似于一个指针，可以直接指向双链表中的某个节点
