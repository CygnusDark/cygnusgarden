---
layout: post
title: Add Two numbers
author: 细雪
header-style: text
lang: en
published: true
categories: algorithm
tags:
  - algorithm
---

# Carry
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode head = null, tail = null;  
        int carry = 0;
        while (l1 != null || l2 != null) {
            int n1 = l1 != null ? l1.val : 0;
            int n2 = l2 != null ? l2.val : 0;
            int sum = n1 + n2 + carry;
            if (head == null) {
                head = tail = new ListNode(sum % 10);
            } else {
                tail.next = new ListNode(sum % 10);
                tail = tail.next;
            }
            carry = sum / 10;
            if (l1 != null) {
                l1 = l1.next;
            }
            if (l2 != null) {
                l2 = l2.next;
            }
        }
        if (carry > 0) {
            tail.next = new ListNode(carry);
        }
        return head;
    }
}
```


```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        return digui(l1, l2, 0);
    }

    public ListNode digui(ListNode l1, ListNode l2, int add) {
        if (l1 == null && l2 == null && add == 0) {
            return null;
        }
        ListNode next1 = null;
        ListNode next2 = null;
        int sum = add;
        add = 0;
        if (l1 != null) {
            next1 = l1.next;
            sum += l1.val;
        }
        if (l2 != null) {
            next2 = l2.next;
            sum += l2.val;
        }
        add = sum >= 10 ? 1 : 0;
        sum = sum >= 10 ? sum - 10 : sum;
        if (l1 == null && l2 == null) {
            return new ListNode(sum);
        } else if (l1 != null) {
            l1.val = sum;
            l1.next = digui(next1, next2, add);
            return l1;
        } else {
            l2.val = sum;
            l2.next = digui(next1, next2, add);
            return l2;
        }
    }
}
```