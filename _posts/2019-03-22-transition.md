---
title: transition
date: 2019-03-22 12:31:26
tags:
---

## 从右往左入 从左往右出

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

## 从下往上入 从上往下出

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

## 稍微放大入 淡关闭

```css

.の-enter-active {
  animation: の-in .5s;
}
.の-leave-active {
  animation: の-out .3s;
}
@keyframes の-in {
  0% {
    transform: scale(1.185);
    opacity: 0;
  }
  100% {
    transform: scale(1);
    opacity: 1;
  }
}
@keyframes の-out {
  0% {
    transform: scale(1);
    opacity: 1;
  }
  100% {
    transform: scale(0.85);
    opacity: 0;
  }
}

```