---
layout: post
title: Vector, Covector, and Riemannian Metric
date: 2023-10-23 09:22:00-0400 
description: introduction to vector and covector
tags: Tensor 
categories: Math Graphics
featured: true
---

## Why Differentiating Row Vector and Column Vector?

One of the most basic operations we learned in our first linear algebra courses must be the "dot product" $$ u^Tv $$. However, have you wondered why we differentiate the "row" vector and "column" vector here while they seem both are just the same kind of "vector"? Moreover, why do we place the "row" vector on the left (otherwise the result is a matrix other than a scalar)? Is it just a trick to conform to the definition of matrix multiplication?

The answer is that the "row" vector and the "column" vector are different objects. These two kinds of objects are just the "covector" and "vector" we are going to discuss.

## Vector

We can specify a point $$ p $$ in the space using standard Cartesian coordinate $$ x^i; i = 0 \cdots n $$, or using a kind of "local" coordinate $$ u^i; i = 0 \cdots n $$ which may be curvilinear (e.g. polar coordinate in 2D Euclidian space).

The coordinates change as the point change.

## Covector

## Connecting Vector and Covector: Riemannian Metric