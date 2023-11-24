链表常见操作
- reverse
- recursive


基于基本操作可以处理很多问题
[[24. Swap Nodes in Pairs]]
[[25. Reverse Nodes in k-Group]]




## Reverse Frist N
递归解法
```python
    def reverseFirstN(self, head: Optional[ListNode], n: int) -> ListNode:
        if n == 1:
            return head
        new_head = self.reverseFirstN(head.next, n-1)
        tail = head.next
        right_head = tail.next

        tail.next = head
        head.next =right_head

        return new_head
```