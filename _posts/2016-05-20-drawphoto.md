---
layout: post
published: true
title:
- Python 50行代码实现图画转字符画
tags:
- Python
categories:
- Python
description: 
- 50行代码实现图画转字符画,简易实现。
---
代码来自[实验楼](https://www.shiyanlou.com/courses/370),添加了点注释。感觉好好玩，放上来。  

```python
from PIL import Image

ascii_char = list("$@B%8&WM#*oahkbdpqwmZO0QLCJUYXzcvunxrjft/\|()1{}[]?-_+~<>i!lI;:,\"^`'. ")


# 将256灰度映射到70个字符上
def get_char(r,b,g):
    length = len(ascii_char)
    gray = int(0.2126 * r + 0.7152 * g + 0.0722 * b)#得到灰度值，范围为[0-255], 0为黑，255为白
    unit = (256.0 + 1)/length#这里为什么是257而不是255，不知道诶，求解答
    # s = gray/255 颜色越重的，值越小，范围为[0-1]
    # s*length 颜色从深到浅分别为[0-length]
    return ascii_char[int(gray/unit)]

if __name__ == '__main__':
    IMG = '1.JPG'
    im = Image.open(IMG)
    im = im.resize((80,80), Image.NEAREST)

    txt = ""

    for i in range(80):
        for j in range(80):
            txt += get_char(*im.getpixel((j, i)))
        txt += '\n'

    print(txt)
    #因为字符的高比宽长，所以呈现的字符画会被拉长
```