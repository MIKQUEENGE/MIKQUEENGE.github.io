---
title: JavaScript实现双向链表
toc: false
date: 2018-10-07 10:11:36
categories:
- Web
tags:
- JavaScript
- 双向链表
---

- append(element): 添加元素到链表尾部
- insert(position,element): 向双向链表中某个位置插入元素
- removeAt(position): 移除双向链表中某个位置的元素
- getHead(): 获取双向链表的头部
- getTail(): 获取双向链表的尾部
- isEmpty(): 检查双向链表是否为空，为空则返回true
- size(): 返回双向链表长度

```js
   function DoublyLinkedList() {
    var Node = function (element) {
        this.element = element;
        this.next = null;
        this.prev = null;
    }

    var length = 0;
    var head = null;
    var tail = null;
    
    this.append = function (element) {
        var node = new Node(element);

        if (head === null) {
            head = node
            tail = node
        } else {
            tail.next = node;
            node.prev = tail;
            tail = node;
        }
        length++;
        return true;
    }


    /**
     * 向双向链表中某个位置插入元素
     * 
     * @param {any} position 要插入的位置
     * @param {any} element 要插入的元素
     * @returns 插入成功或失败
     */
    this.insert = function (position, element) {
        var node = new Node(element),
            current = head,
            previous,
            index = 0;

        if (position < 0 && position > length) {
            return false;
        }

        if (position === 0) {
            node.next = head
            head.prev = node
            head = node
        } else if (position === length) {
            tail.next = node;
            node.prev = tail;
            tail = node;
        } else {
            while (index++ < position) {
                previous = current
                current = current.next;
            }
            previous.next = node;
            node.prev = previous;
            node.next = current;
            current.prev = node;
        }
        length++;
        return true;
    }

    /**
     * 移除双向链表中某个位置的元素
     * 
     * @param {any} position 要移除元素的位置
     * @returns 移除成功，返回移除的元素
     */
    this.removeAt = function (position) {
        var previous,
            current = head,
            index = 0;
        if (position < 0 && position >= length) {
            return false;
        }

        if (position === 0) {
            head = current.next;
            head.prev = null;
        } else if(position === length - 1) {
             current = tail;
             tail = current.prev;
             tail.next = null;
        } else {
            while (index++ < position) {
                previous = current
                current = current.next;
            }
            previous.next = current.next;
            current.next.prev = previous;
        }
        length--;
        return current.element;
    }

    this.getHead = function () {
        return head.element;
    }

    this.isEmpty = function () {
        return length === 0
    }

    this.getTail = function () {
        return tail.element;
    }

    this.size = function () {
        return length
    }
}
```

