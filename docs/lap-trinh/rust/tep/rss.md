```rust
fn main() {
    let matches = Command::new("tinpub-rs")
        .arg(
            Arg::new("source")
                .short('i')
                .long("source")
                .help("Which source do you wanna fetch news from")
                .required(true),
        )
        .get_matches();
        
    let feed = FeedConfig {
        name: String::from("untitled"),
        url: config_path.clone()
    };

    
}

fn handle_feed(feed: &FeedConfig, client: &Client) -> Vec<FetchedItem> {
    println!("Fetching {}", feed.name);

    let rss_result = fetch_rss(&feed.url, client);
    let mut src_items: Vec<FetchedItem> = Vec::new();

    let rss_channel = match rss_result {
        Ok(channel) => channel,
        Err(_) => {
            println!("Failed to load RSS feed");
            return src_items;
        },
    };

    for item in rss_channel.into_items() {
        let title_result = item.title();
        
        let title = match title_result {
            Some(title) => title,
            None => {
                println!("No title found");
                return src_items;
            },
        };

        let item_url = item.link().unwrap();
        let item_desc = item.clone().description.unwrap();

        let item_content: String = match item.clone().content {
            Some(content) => content,
            None => String::from(""),
        };

        let fetched_item = FetchedItem {
            name: String::from(title),
            url: String::from(item_url),
            desc: item_desc.to_string(),
            content: item_content.to_string()
        };

        println!("Fetching post: {}[{:?}]", title, item_url);

        src_items.push(fetched_item)

        // if download_filter.is_match(title) {}
    }
    src_items
}

fn fetch_rss(url: &str, client: &Client) -> Result<Channel, Box<dyn std::error::Error>> {
    println!("Fetching URL {}", url);
    let response = client.get(url).send()?;
    let status = response.status();
    println!("Response status: {}", status);
    if status.is_success() {
        let text = response.text()?;

        let rss_result = Channel::read_from(text.as_bytes());
        if let Ok(channel) = rss_result {
            Ok(channel)
        } else {
            let error = rss_result.err().unwrap();
            println!("Error parsing RSS feed: {:?}", error);
            Err(Box::new(error))
        }
    } else {
        println!("Error fetching RSS feed");
        Err(Box::new(response.error_for_status().err().unwrap()))
    }
}
```