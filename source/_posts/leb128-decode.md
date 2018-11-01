---
title: 记一次手动解码「LEB128」压缩数据
date: 2018-05-21 16:17:02
thumbnail: https://i.loli.net/2018/10/28/5bd59c0021e77.png
tags:
- 算法
---
几天前因为一个项目需要用到「Google」的[Protocol Buffers](https://developers.google.com/protocol-buffers/)，在项目页找了一圈也没有提供 `Bash` 版本的编码器和解码器，于是想到按照官方文档给出的数据结构手动解码 {% ruby 二进制数据流|Binary Data Stream %}。
<!-- more -->

## 解码流程

1. 假设接收到的数据为 `0xDADBFE01`
2. 首先转为二进制，每8位bit位为一组，得到 `(1101 1010) (1101 1011) (1111 1110) (0000 0001)`
3. 每组数据除去首位，得到 `(101 1010) (101 1011) (111 1110) (000 0001)`
4. 左右调换组的顺序，得到 `(000 0001) (111 1110) (101 1011) (101 1010)`
5. 最后将二进制转回十进制即可得到 {% ruby 整型|Integer %} `4173274`

## 写成脚本大概这样

```bash
#/bin/zsh
_BIN="0x01FEDBDA"
_A=$(($_BIN >> 24 & 0x7f))
_B=$((($_BIN & 0x00ff0000) >> 16 & 0x7f))
_C=$((($_BIN & 0x0000ff00) >> 8 & 0x7f))
_D=$(($_BIN & 0x000000ff & 0x7f))
_OUT=$(($_A * (1 << 21) + $_B * (1 << 14) + $_C * (1 << 7) + $_D))
echo "OUT:" $_OUT
```

## 参考资料
+ [LEB128](https://en.wikipedia.org/wiki/LEB128)
+ [php-protobuf](https://github.com/allegro/php-protobuf/)
+ [Encoding | Google Developers](https://developers.google.com/protocol-buffers/docs/encoding)
