---
title: transition
date: 2019-03-09 12:31:26
tags:
---

## 从右往左打开 从左往右关闭

```
<transition name="の"></transition>

```


```css

.の-enter-active, .の-leave-active {
  transition: all .4s ease;
}
.の-enter, .の-leave-to /* .の-leave-active below version 2.1.8 */ {
  transform: translate3d(100%, 0, 0);
  opacity: 0;
}

```

## 从下往上打开 从上往下关闭

```css

.の-enter-active,.の-leave-active {
  transition: all .4s ease;
}
.の-enter,.の-leave-to {
  transform: translate3d(0, 100%, 0);
  opacity: 0;
}

```

## 淡入淡出

```css

.の-enter-active, .の-leave-active {
  transition: opacity .5s
}
.の-enter, .の-leave-active {
  opacity: 0
}

```