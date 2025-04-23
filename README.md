# web_crawler
 An intelligent, scalable web crawler that sniffs out product pages across e-commerce sites with speed and intelligence.

This project has the basic implementation to showcase as POC.

 Here's a high level diagram of the scaled implementation:

![design](https://raw.githubusercontent.com/reverie-ss/web_crawler/refs/heads/main/site_crawler.svg)

 There are 3 components to this project:
1. **Crawler Engine**: The core component responsible for fetching and parsing web pages. It uses a queue-based system to manage URLs and ensures efficient crawling while respecting robots.txt rules.

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


