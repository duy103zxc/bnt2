# Regex
Rất nhiều regex hay dã man, và cũng nhanh khủng khiếp, nếu biết Pattern =))

### Match từ đây đến hết câu

```
[Phần bắt đầu của đoạn bạn cần match]\s*([^\n\r]*)
```

### Xóa toàn bộ các dòng bị trùng lặp

```
^(.*)(\r?\n\1)+$
```

### Match markdown links

```
\[(.+)\]\(([^ ]+?)( "(.+)")?\)
```
Extracted from [this post](https://www.michaelperrin.fr/blog/2019/02/advanced-regular-expressions). There are 4 capturing groups (surrounded by parenthesis) in that regex:

- Group 1 is the text of the link.
- Group 2 is the URL of the link.
- Group 3 is the optional title of the link including the double quotes (this group is necessary for the ? marker).
- Group 4 is the optional title of the link.
