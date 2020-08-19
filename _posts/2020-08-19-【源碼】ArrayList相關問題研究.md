---
layout:     post
title:      ArrayList源碼問題研究
subtitle:   
date:       2020-08-19
author:     BY Jady
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - 源碼
    - Java
    - 集合
--- 

> 源碼中的部分疑問點研究


-  傳入集合的構造方法爲什麽進行了一次Object的轉換
```
 public ArrayList(Collection<? extends E> c) {
        elementData = c.toArray();
        if ((size = elementData.length) != 0) {
            // 爲什麽進行了一次類型的轉換？
            if (elementData.getClass() != Object[].class)
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }
```

