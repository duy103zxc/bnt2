# Một số câu lệnh tải video hay bằng `yt-dlp`

### Chỉnh sửa tên tệp (Output)
Khi tải video Youtube chẳng hạn, thường thì nó sẽ trả về kiểu `Tiêu đề video [id của video].webm` chẳng hạn. Nó khá là khó chịu =)) (Ít nhất với mình).

```
yt-dlp -o "%(title)s.%(ext)s" [link ở đây]
```
Thường là mình để mặc định thế này, chỉ có tiêu đề với đuôi tệp thôi.

### Tải cả cái Podcast
Lên trên [Podcastindex.org](https://podcastindex.org/) và tìm Podcast bạn muốn tải. Ví dụ mình sẽ tải Podcast có đường dẫn là [https://podcastindex.org/podcast/366334](https://podcastindex.org/podcast/366334).

Bạn chỉ cần bấm **Copy RSS** rồi ném vào trong `yt-dlp` là xong:

```
yt-dlp https://feeds.simplecast.com/dfh_verV
```

Rồi nó sẽ tải toàn bộ cho bạn.


### Danh sách các lệnh tổng hợp
Tải một danh sách phát trên Youtube, độ phân giải 480p, có phụ đề Tiếng Anh với Output tên đẹp đẹp xíu.

```
yt-dlp -S res:480 --write-subs --write-auto-subs --sub-lang=en --convert-subs=srt -o "%(title)s.%(ext)s" [Cho link ở đây]
```

Giải thích thêm:

- `-S res:480`: Độ phân giải 480p.
- `--write-subs`
- `--write-auto-subs`: Tải sub tự Youtube tạo 
- `--sub-lang=en`: Chọn ngôn ngữ phụ đề
- `--convert-subs=srt`: Chuyển đổi sang định dạng `.srt`, mặc định `yt-dlp` sẽ dùng `.vtt`
- `-o "%(title)s.%(ext)s"` - Tên tệp đẹp
