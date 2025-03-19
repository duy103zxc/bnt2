# Chơi đùa với System

### Mở URL & Path với ứng dụng

```rust
open::that("https://rust-lang.org");
open::with("https://rust-lang.org", "firefox");

// Hoặc các đồng bào có thể dùng Path của ứng dụng để mở cũng oke, chưa thử
```


### Kiểm tra đuôi tệp

```rust
pub fn check_img_format(filename: &PathBuf) -> bool {
    static IMAGE_FORMAT: &[&'static str; 9] = &["jpg", "jpeg", "jpe", "jfif", "jfi", "jif", "png", "bmp", "gif"];
    filename.extension().and_then(OsStr::to_str).is_some_and(|ext| IMAGE_FORMAT.contains(&ext))
}
```


### Tạo tệp
```rust
pub fn gen_file(name: &str, book_format: &str) -> Result<File, std::io::Error> {
    OpenOptions::new()
        .write(true)
        .create(true)
        .append(true)
        .open(format!("{}.{}", name, book_format))
}
```

### Đọc nội dung từ tệp

```rust
pub fn read_from_file(file: &str) -> String {
    let mut input = String::new();
    let mut f = File::open(file).expect("File not found!");
    f.read_to_string(&mut input).expect("Something went wrong reading the file!");
    input
}
```

### Viết nội dung vào tệp
```rust
pub fn write_to_file() -> Result<(), anyhow::Error> {
    let filename = gen_file("tep", "txt");
    let mut f = BufWriter::new(f);
    f.write_all(gen_str(chapter.name, &chapter.content).as_bytes())
        .expect("Can't write to the file");
    f.flush().unwrap();
    Ok(())
}
```


### Truy cập vào các ứng dụng khác

```rust
use std::io::ErrorKind;
use std::path::PathBuf;
use std::process::Command;
use std::{env, fs, io};

pub trait ProgramOpener {
    fn open_editor(&self, file_path: &str) -> io::Result<()>;
    fn open_pager(&self, file_path: &str) -> io::Result<()>;
}

#[derive(Default)]
pub struct ProgramAccess;

impl ProgramOpener for ProgramAccess {
    fn open_editor(&self, file_path: &str) -> io::Result<()> {
        self.open_with_fallback(file_path, "EDITOR", "vi")
    }

    fn open_pager(&self, file_path: &str) -> io::Result<()> {
        self.open_with_fallback(file_path, "PAGER", "less")
    }
}

impl ProgramAccess {
    fn open_with_fallback(&self, file_path: &str, env_var: &str, fallback: &str) -> io::Result<()> {
        let program = env::var(env_var)
            .map(PathBuf::from)
            .or_else(|_| self.get_if_available(fallback))?;

        // Make sure file exists
        fs::metadata(file_path)?;
        Command::new(program).arg(file_path).status().map(|_| ())
    }

    fn get_if_available(&self, program: &str) -> io::Result<PathBuf> {
        which::which(program).map_err(|err| std::io::Error::new(ErrorKind::NotFound, err))
    }
}
```