## :not()伪类的使用

``` css
/* 边框巧用 */
.item:not(:last-child) {
  border: 1px solid #ececec;
}

/* 视频静音 */
.vedio:not(:mute) {
  display: none;
}
```

## 伪元素
- :first-line 对块元素起作用，作用是选择第一行文本，与换行无关
- :first-letter 对所有元素起作用，对文本的一个字母起作用。
- :before 添加元素，在选择的元素之前
- :after 添加元素，在选择的元素之后
