---
layout: post
title: Lombok Notebook
tags: Java tool note
category: blog
---

[Lombok Features Link](https://projectlombok.org/features/all)

<!--more-->

### @NonNull

For non-null fields check. Once it happens, NullPointerException will be thrown with the null parameter.

### @Data

All together now: A shortcut for `@ToString`, `@EqualsAndHashCode`, `@Getter` on all fields, and `@Setter` on all non-final fields, and `@RequiredArgsConstructor`!

### @NoArgsConstructor

constructors that take no arguments

### @RequiredArgsConstructor

one argument per **final / non-null** field

### @AllArgsConstructor

one argument for every field.