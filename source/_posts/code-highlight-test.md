---
title: 测试新的 Hexo 代码块结构
date: 2018-05-20 21:40:06
tags:
- 测试
---
#### 目前代码块结构Code
```html
<figure class="highlight code" data-language="Html">
  <pre class="language-html">
    <span class="line">...</span>
    <span class="line">...</span>
    <span class="line">...</span>
  </pre>
</figure>
```

#### JavaScript
```javascript
$(function() {
  hljs.initHighlighting();
  $('pre code').each(function() {
    var lines = $(this).text().split('\n').length - 1;
    var $numbering = $('<ul/>').addClass('pre-numbering');
    $(this)
      .addClass('has-numbering')
      .parent()
      .append($numbering);
    for (i = 1; i <= lines; i++) {
      $numbering.append($('<li/>'));
    }
  });
});
```

#### CSS

```css
.post-content figure.highlight pre {
  font-size: 1.5rem;
  line-height: 3.09rem;
  letter-spacing: 0.2rem;
  font-family: "Consolas, Monaco, Menlo, sans-serif";
}
```

#### Diff
```diff
+ 鸟宿池边树，僧敲月下门
- 鸟宿池边树，僧推月下门
```


#### Java
```java
public static void main(String[] args) {}
```

#### C
```c
void main(int argc, char *argv[]) {}
```

#### CPP
```cpp
string &operator+(const string& A, const string& B) {}
```

#### Bash
```bash
echo "hello GitHub"
```
