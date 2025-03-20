# HTML

### Lấy các nội dung từ các Selector

```rust
pub fn fetcher(
    base_url: String,
    id: &str,
    list_selector: &str,
    item_selector: &str,
    element: &str
) -> Vec<String> {
    let given_url = base_url + id;
    let body = http::get_body_from_url(&given_url).unwrap();
    let document = Html::parse_document(&body);
    // Logging
    println!("Đang tải chương truyện với URL là: {}", &given_url);
    // Selector
    let list = Selector::parse(list_selector).unwrap();
    let item = Selector::parse(item_selector).unwrap();
    let body = Selector::parse("body").unwrap();

    let content_matcher = match document.select(&list).next() {
        Some(doc) => {
            doc.select(&item)
        },
        None => {
            document.select(&body).next().unwrap().select(&item)
        },
    };

    content_matcher.into_iter()
    .filter_map(|f| f.value().attr(element).map(|elem| elem.to_string())).collect()
}
```


### Cái gì đó không rõ
```rust
/// Displays formatted html that fits a CSS selector.
pub fn display_article(
    contents_selector: scraper::Selector,
    title_selector: scraper::Selector,
    request: scraper::Html,
) {
    let mut paragraphs = request
        .select(&contents_selector)
        .map(|x| from_read(x.inner_html().as_bytes(), 190))
        .collect::<Vec<String>>()
        .join("\n");

    paragraphs = filters::remove_references(paragraphs);
    paragraphs = filters::remove_square_brackets(paragraphs);
    paragraphs = filters::remove_links(paragraphs);

    let title = request
        .select(&title_selector)
        .map(|x| from_read(x.inner_html().as_bytes(), 50))
        .collect::<Vec<String>>()
        .join("");

    let article = ArticleDisplay {
        title: title,
        contents: paragraphs,
    };

    ui::ArticleDisplay::new(article).unwrap();
}

```


### Code that cleans the article from HTML leftovers.

```rust
use regex::Regex;

pub fn remove_square_brackets(text: String) -> String {
    let square_bracket_regex =
        Regex::new(r"[(?P<link>[a-zA-Z0-9\(\)\-\,[:space:]&=]+)]").unwrap();
    square_bracket_regex.replace_all(&text, "$link").to_string()
}

pub fn remove_references(text: String) -> String {
    let reference_regex = Regex::new(r"[+([0-9]+)]+").unwrap();
    reference_regex.replace_all(&text, "").to_string()
}

pub fn remove_links(text: String) -> String {
    let link_regex = Regex::new(r": [/?/[a-zA-Z0-9-%:/(/)_.//&=]+/]+\n").unwrap();
    let note_regex =
        Regex::new(r": #cite_note-[0-9-]+\n|: #cite_note-[a-zA-Z_-]+-[0-9-]+\n").unwrap();

    let mut new_text = link_regex.replace_all(&text, "").to_string();
    new_text = note_regex.replace_all(&new_text, "").to_string();

    new_text
}
```


### Chọn các elements trong HTML bằng select.rs

```rust
pub fn html_to_dict_term(file_path: &str) -> Vec<DictTerm> {
    let html_content = fs::read_to_string(file_path).expect("Should have been able to read the file");
    let document = Document::from(html_content.as_str());

    document.find(Name("idx:entry")).map(|element| {
        let dict_term = element.find(Name("idx:orth")).next().unwrap().attr("value").unwrap();
        DictTerm {
            term: dict_term.to_owned(),
            definition: element.text().replace("\"", "-")
        } 
    }).collect::<Vec<_>>()
    
}
```