---
layout: post
title: The Right Side View of a Binary Tree 
author: 细雪
header-style: text
lang: en
published: true
categories:  算法
tags:
  - 算法
---

``` java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if (root == null) return list;
        // LinkedList 实现了 Queue 接口
        LinkedList<TreeNode> queue  = new LinkedList<>();
        queue.add(root);
        // 遍历每一层
        while (!queue.isEmpty()) {
            // 获取本层最后一个元素
            list.add(queue.getLast().val);
            int size = queue.size();
            // 将队列中的本层元素替换为下层的元素
            for (int i = 0;i < size;i++) {
                TreeNode node = queue.getFirst();
                if (node.left != null) queue.add(node.left);
                if (node.right != null) queue.add(node.right);
                queue.removeFirst();
            }
        }
        return list;
    }
}
```