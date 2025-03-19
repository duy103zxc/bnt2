# EPUB

### Tạo EPUB

```rust

pub fn gen_epub(
    book: NovelContent,
    book_language: &str,
    is_vertical: &bool,
) -> epub_builder::Result<()> {
    let output = utils::gen_file(&book.title, "epub").unwrap();

    let epub_mode = css_gen(is_vertical);
    let mut binding = epub_builder::EpubBuilder::new(epub_builder::ZipLibrary::new()?)?;
    let gen_epub = binding
        .metadata("author", book.author.trim())?
        .metadata("title", &book.title)?
        .epub_direction(epub_mode.epub_direction)
        .metadata("lang", book_language)?
        .metadata("generator", "Buukuru")?
        .add_metadata_opf(epub_builder::MetadataOpf {
            name: String::from("primary-writing-mode"),
            content: epub_mode.p_writing_mode,
        })
        .epub_version(epub_builder::EpubVersion::V30)
        .stylesheet(epub_mode.epub_css.as_bytes())?;

    let generated_content = gen_content(gen_epub, book);
    generated_content
        .inline_toc()
        .generate(output)
        .expect("Error generating .epub file");
    Ok(())
}
```


### Tạo XHTML

### XHTML Generator

Phần 1:

```rust
fn to_xhtml(content: &Vec<String>, chap_title: &str) -> String {
    // Start
    let start = format!(
        r##"<?xml version='1.0' encoding='utf-8'?>
<html xmlns="http://www.w3.org/1999/xhtml" lang="ja" xml:lang="ja">
    <head>
        <title>{}</title>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
        <link rel="stylesheet" type="text/css" href="stylesheet.css"/>
    </head>
    <body>"##,
        chap_title
    );
    let chapter_title = format!("\n\t<h2>{}</h2>\n", chap_title);
    let mut file: String = start.to_string() + chapter_title.as_str();

    for line in content {
        match line.as_str() {
            "" => {}
            _ => {
                file += format!("\t\t<p>{}</p>\n", &line.trim()).as_str();
            }
        }
    }
    file += "\n\t</body>\n</html>";
    // End
    file
}
```


Phần 2:

```rust
pub fn compress_xhtml(html_input: &str) -> String {
    format!(r##"<?xml version='1.0' encoding='utf-8'?>
<html xmlns="http://www.w3.org/1999/xhtml" lang="ja" xml:lang="ja">
    <head>
        <title>Untitled</title>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
        <link rel="stylesheet" type="text/css" href="stylesheet.css"/>
    </head>
    <body>{}{}"##, html_input, "\n\t</body>\n</html>")
}
```

### CSS for English and Japanese

```rust
fn css_gen(is_vertical: &bool) -> EpubMode {
    let vertical_css = r##"@page {
  margin-bottom: 5pt;
  margin-top: 5pt;
}

html {
  -webkit-writing-mode: vertical-rl;
  -moz-writing-mode: vertical-rl;
  -ms-writing-mode: vertical-rl;
  writing-mode: vertical-rl;
}

p {
  display: block;
  height: auto;
  text-indent: inherit;
  width: auto;
  margin: 0;
  padding: 0;
}"##;
```