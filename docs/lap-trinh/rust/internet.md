# Internet
Mấy cái xung quanh việc tương tác với Internet sử dụng Rust.

### Encode URL

Dùng cái này: `open = "5.0.1"`

```rust
let encoded_query =
        percent_encoding::utf8_percent_encode(&args.query, NON_ALPHANUMERIC).to_string();

    for site in config.sites {
        let url = site.url.replace("%s", &encoded_query);
        println!("{}:\t{}", site.name, url);
        // Phần này là dùng "open" rồi, hơi ngoài lề chút
        open::that(url).context("could not open the URL")?;
    }
```

### Request

Sử dụng ureq.

```rust
pub fn fetch(url: &str, refer_site: &str) -> Result<Response, ureq::Error> {
    let user_agent = "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36";
    
    let ureq_agent: Agent = ureq::AgentBuilder::new()
    .timeout_read(Duration::from_secs(30))
    .build();

    let body = ureq_agent.get(url)
        .set("User-Agent", user_agent)
        .set("Referer", refer_site)
        .call();
    body
}
```

### Lấy thông tin từ API

```rust
#[derive(Debug, Serialize, Deserialize, Default)]
struct Fact {
    id: String,
    text: String,
    source: String,
    source_url: String,
    language: String,
    permalink: String,
}

const FACT_BASE_URL: &str = "https://uselessfacts.jsph.pl";
const FACT_RANDOM: &str = "/api/v2/facts/random?language=en";
const FACT_DAILY: &str = "/api/v2/facts/today?language=en";

fn get_fact(url: &str) -> Result<Fact, Box<dyn std::error::Error>> {
    let resp = reqwest::blocking::get(url)?.json::<Fact>()?;
    Ok(resp)
}

fn get_random_fact() -> Result<Fact, Box<dyn std::error::Error>> {
    let url = format!("{}{}", FACT_BASE_URL, FACT_RANDOM);
    get_fact(&url)
}

fn get_daily_fact() -> Result<Fact, Box<dyn std::error::Error>> {
    let url = format!("{}{}", FACT_BASE_URL, FACT_DAILY);
    get_fact(&url)
}

fn get_help() -> Result<Fact, Box<dyn std::error::Error>> {
    let mut fact = Fact::default();
    fact.text = String::from("Usage: fact [t]oday | [r]andom");
    Ok(fact)
}

fn print_fact(fact: Fact) {
    println!("Fact: {}", fact.text);
    println!("Source: {}", fact.source_url);
}
```



### Mở trình duyệt

```rust
pub fn show_news(new: &NewsLink) -> Result<(), Box<dyn std::error::Error>> {
    let link = &new.link;
    let response = ureq::get(link).call()?;
    let url = response.get_url();

    // if is a video of youtube
    if url.contains("youtube") {
        if webbrowser::open(new.link.as_str()).is_ok() {}
        return Ok(());
    }

    let view_select = Select::new(
        "What view do you like to do?",
        vec!["Web", "Terminal", "Ia Draft"],
    )
    .with_help_message("Enter the view of the new")
    .prompt()
    .unwrap_or("Cancel");

    if view_select == "Cancel" {
        manage_exit("No view provided")
    }

    let view = View::from_str(view_select).expect("failed to parse view");
    match view {
        View::Terminal => {
            let html = response.into_string()?;
            let markdown = get_markdown_content(&html);

            if markdown.is_empty() {
                println!("Content of new cannot be loaded in terminal");
                println!("Opening browser instead");
                if webbrowser::open(new.link.as_str()).is_ok() {}
            } else {
                generate_view(markdown.as_str()).expect("failed to generate a markdown view");
            }
        }
        View::Web => {
            webbrowser::open(new.link.as_str())?;
        }
        View::Ia => {
            let html = response.into_string()?;
            let markdown = get_markdown_content(&html);
            get_resume(&markdown)?;
        }
    }
```