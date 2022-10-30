---
layout: post
title: Binary Tree Inorder Traversal
author: 细雪
header-style: text
lang: en
published: true
categories: Algorithm
tags:
  - Algorithm
---

# 题目
## 二叉树的中序遍历
给定一个二叉树的根节点 root ，返回 它的 中序 遍历 。
输入：root = [1,null,2,3]
输出：[1,3,2]
   
# 解法
## 递归
``` java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        inorder(root, res);
        return res;
    }

    public void inorder(TreeNode root, List<Integer> res) {
        if (root == null) {
            return;
        }
        inorder(root.left, res);
        res.add(root.val);
        inorder(root.right, res);
    }
}
```

## 迭代
``` java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        Deque<TreeNode> stk = new LinkedList<TreeNode>();
        while (root != null || !stk.isEmpty()) {
            while (root != null) {
                stk.push(root);
                root = root.left;
            }
            root = stk.pop();
            res.add(root.val);
            root = root.right;
        }
        return res;
    }
}
```