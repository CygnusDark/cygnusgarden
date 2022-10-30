---
layout: post
title: Lowest Common Ancestor
author: 细雪
header-style: text
lang: en
published: true
categories:  Algorithm
tags:
  - Algorithm
---

``` java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == p || root == q || root == null) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        // 如果left 和 right都不为空，说明此时root就是最近公共节点
        // 如果left为空，right不为空，就返回right，说明目标节点是通过right返回的，反之依然。
        if (left != null && right != null) return root;
        if (left == null) return right;
        return left;
    }
}
```