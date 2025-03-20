# Bash

Mình tổng hợp khá nhiều các câu lệnh và một chút về lập trình Bash trong này. Không nhất thiết chỉ là Bash mà còn là các câu lệnh khác trong Terminal nữa ^^!

### Loop đọc từ tệp txt

```bash
while read p; do
  yt-dlp "$p" -o
done <tmp.txt
```

### Gộp toàn bộ các tệp .txt thành một tệp .txt

```bash
# Nếu không bao gồm sub-folders
cat ./* > merged-file
# Nếu có
find . ! -path ./merged-file -type f -exec cat {} + > merged-file
```

### Tải trang từ Wayback Machine về

```bash
wget -rc --accept-regex '.*https://loda.me/articles.*' "https://web.archive.org/web/20230321225500/https://loda.me/articles"
```