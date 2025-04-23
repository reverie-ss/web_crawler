# web_crawler
 An intelligent, scalable web crawler that sniffs out product urls across e-commerce sites. Add list of domains and it will pull out all product pages in it.

This project has the basic implementation to showcase as POC.

 Here's a high level diagram of the scaled implementation:

![design](https://raw.githubusercontent.com/reverie-ss/web_crawler/refs/heads/main/site_crawler.svg)

 There are 3 components to this project:
1. **Crawler Engine**: The core component responsible for fetching and parsing data. It uses a queue-based system to manage URLs and ensures efficient crawling.

2. **URL Trainer**: It uses labelled dataset to develop a model which can be used to classify urls. This model adds intelligence so that it can be scaled to 100s of other websites. 


3. **Storage Module**: Stores the extracted data in a structured format, such as a database or JSON files, for easy access and further processing.

## URL Trainer

The important issue of scaling such an application to other websites is its ability to filter product urls across 100s of other websites.
We can start with as simple as finding patterns in the url, like `[/product/, /item/, /p/]`.
But this approach constantly needs to be updated with every website which do not follow any of the given pattern

Instead of using a `Rule-based` approach, this project uses machine learning for better scalability.



| Approach        | Pros | Cons                              |
|------------------|--------------|-----------------------------------------|
| `Rule-based`  |  Fast       | Hard to maintain, low recall            |
| `Machine Learning`    | Scalable       | Overhead of training model and labelling             |


The feature extraction was done using `TF-IDF` as we can generalize the classification by dividing urls into smaller parts and figuring out the frequency.
Data from 3 different ecommerce websites was labbeled and vectorized to train using `XGBoost`. This produced a model with very accurate results.

## Crawler Engine
The Crawler Engine primarily searches for the `sitemap.xml` file of the e-commerce website. Once located, it recursively processes the sitemap index to extract all available URLs.

If the `sitemap.xml` file is unavailable or inaccessible, the crawler employs browser automation using Playwright to dynamically fetch URLs.



# Result
The model was trained on a labelled dataset of 16000+ URLs.
The model was then tested with all the URLs from TataCLiq and the results were amazing.
```
Scraped URLs from TataCliq: 8,11,745

False Positives: 4

False negatives: 0
```

# Product URLs

The crawler engine was run on three domains:
1. https://www.westside.com/
2. https://virgio.com/
3. https://www.tatacliq.com/

Total URLs collected from the above three domains: `8,28,292`

The collected URLs went through the model created earlier and predicted product URLS.
Total product URLs: `8,24,761`

The result is stored in `product_urls.json`

## Installation
1. Clone the repository:
    ```bash
    git clone https://github.com/reverie-ss/web_crawler.git
    ```
2. Navigate to the project directory:
    ```bash
    cd web_crawler
    ```
3. Install dependencies:
    ```bash
    pip install -r requirements.txt
    ```

## Usage
Since these are jupyter notebooks, it can be used directly without running any server.

### Crawl and Predict

1. Add list of urls in `sitemap_crawler.ipynb`
2. Run the crawler to fetch all urls
3. Classify the urls using the model at the end of notebook

### Label and Train
1. Navigate to the models directory
2. Prepare data by labelling or use the existing data
3. Run the URL classifier trainer
4. Save the model in order to use it for prediction


