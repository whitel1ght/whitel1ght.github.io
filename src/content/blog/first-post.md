---
title: 'Dynamic Pinia Modules'
description: 'Creaing Pinia modules dynamically'
pubDate: 'Jul 08 2022'
heroImage: '/pine.png'
---

It's easier to squeeze water out of a stone than to start writing a blog post. But here I am, writing my first and I hope you'll enjoy it. Or You might enjoy something completely different.

**TLDR;** this post explains bunch of stuff

So, let's start with something fairly simple today. Imagine working on a project where you have a bunch of **similar** pages, and you need to create a Pinia state for each one. While the state may vary slightly, the approach I'm going to explain works well in scenarios where you need to repeat the same module creation multiple times. Where might this happen, you ask? A typical case is an admin panel with several tables. These tables may display independent entities, but you need to save table state that is common to all of them. Every table has paging, and your store modules will contain valuable data about each table.
