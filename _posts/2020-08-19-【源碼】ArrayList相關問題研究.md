---
layout:     post
title:      ArrayList源碼問題研究
subtitle:   
date:       2020-08-19
author:     BY Jady
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - 源碼
    - Java
    - 集合
--- 

# 源碼中的部分疑問點研究


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

因爲Collection的toArray()方法，并不總是返回Object[]，也可能會返回Object的子類數組；
例如Arrays.ArrayList的toArray()就是返回實際類型的。
即：new ArrayList(Arrays.asList(1,2,3)).toArray()返回的是Integer[]

- 手動擴容方法ensureCapacity的作用

主要是多次大批量添加數據前，提前擴充到需要的大小，防止多次擴容影響效率。

- 擴容方法重點

```
    private void grow(int minCapacity) {
        int oldCapacity = elementData.length;
        // 新容量是老容量的1.5倍，向下取整
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        // 如果老容量的1.5倍不夠用，就用最小需要的容量
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        // MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8 如果沒超過這個數，直接使用
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            //超過的int值極限了
            if (minCapacity < 0) 
                throw new OutOfMemoryError();
            // 使用預留的幾位長度
            return (minCapacity > MAX_ARRAY_SIZE) ? Integer.MAX_VALUE : MAX_ARRAY_SIZE;
        // 拷貝到新長度的數組中並替代舊數組
        elementData = Arrays.copyOf(elementData, newCapacity);
    }

```

- indexOf，lastIndexOf，contains方法，使用的是遍歷進行equals方法判斷查找，null除外

- toArray(T[] a)方法儅傳入數組長度小於當前列表對應數組時，會返回同長度數組；若大於，多餘位置為null；

***
