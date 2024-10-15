# README: Homework 2 - Considering Bias in Data

## Overview

This project explores bias in Wikipedia articles using data on political figures from different countries. The goal is to combine data from Wikipedia with country population data to assess the coverage and quality of articles, and perform a bias analysis based on per-capita metrics. The article quality is evaluated using the ORES machine learning model via the [LiftWing API](https://wikitech.wikimedia.org/wiki/Machine_Learning/LiftWing).

## Repository Structure

```bash
.
├── LICENSE                     # License file for the project
├── README.md                   # This README file
├── data                        # Data folder containing the CSV files used/generated
│   ├── politicians_by_country_AUG.2024.csv               # Source data of politicians
│   ├── politicians_with_ores_predictions.csv            # Data with ORES quality predictions for each article
│   ├── population_by_country_AUG.2024.csv               # Source data of population statistics by country
│   └── wp_politicians_by_country.csv                    # Final combined data with articles, quality scores, and population
├── ores_errors_log.txt         # Log file with articles for which ORES scores couldn't be retrieved
├── wp_article_analysis.ipynb   # Jupyter notebook performing the analysis
├── wp_countries-no_match.txt   # List of countries not matched between the Wikipedia and population datasets
```

## Data Sources

Data sources 1 and 2 were given to me by the teaching staff of DATA 512

1. **`politicians_by_country_AUG.2024.csv`**:
   - **`name`**: The name of the politician.
   - **`url`**: The Wikipedia URL of the politician's article.
   - **`country`**: The country to which the politician belongs.

2. **`population_by_country_AUG.2024.csv`**:
   - **`Geography`**: The name of the country or region.
   - **`Population`**: The population of the country or region (in millions). Some rows include regions such as "AFRICA", "ASIA", etc., and should not match directly with countries.

3. **`politicians_with_ores_predictions.csv`**:
   - **`name`**: The name of the politician.
   - **`url`**: The Wikipedia URL of the politician's article.
   - **`country`**: The country of the politician.
   - **`revision_id`**: The Wikipedia revision ID for the latest edit of the article.
   - **`quality_prediction`**: The ORES-predicted quality class for the article (e.g., FA, GA, B, C, Start, Stub).

4. **`wp_politicians_by_country.csv`**:
   - **`country`**: The name of the country.
   - **`region`**: The region where the country is located (e.g., AFRICA, EUROPE).
   - **`population`**: The population of the country (in millions).
   - **`article_title`**: The title of the politician's Wikipedia article.
   - **`revision_id`**: The Wikipedia revision ID for the article.
   - **`article_quality`**: The ORES-predicted quality class for the article.

## Intermediary Files

1. **`ores_errors_log.txt`**: Contains a list of articles for which ORES scores could not be retrieved. This file logs the names of articles that resulted in errors during the ORES API request.
   
2. **`wp_countries-no_match.txt`**: Lists the countries that could not be matched between the Wikipedia dataset and the population dataset. These countries did not have a population entry corresponding to their Wikipedia country name.

## Jupyter Notebook

The notebook `wp_article_analysis.ipynb` contains all the code for processing the data, making API requests to retrieve ORES quality scores, calculating per-capita statistics, and analyzing the bias in the data. The notebook outputs various tables and rankings, including:

- Top 10 and Bottom 10 countries by article coverage (articles per capita).
- Top 10 and Bottom 10 countries by high-quality article coverage (high-quality articles per capita).
- Geographic regions ranked by total articles and high-quality articles per capita.

## Instructions

1. Cloning the Repository

   To clone this repository to your local machine, follow these steps:

    - Open a terminal window.
    - Navigate to the directory where you want to clone the repository.
    - Run the following command:

    ```bash
    git clone https://github.com/your-username/data-512-homework_2.git
    ```
    
    - Once the repository is cloned, navigate into the project folder:


    ```bash
    cd data-512-homework_2
    ```

    Make sure to replace your-username with your actual GitHub username if applicable. You can then explore the code, datasets, and notebooks provided in this project.

2. **Installation**:
   - Ensure that `pandas` and `requests` Python libraries are installed.
   - Run the Jupyter notebook (`wp_article_analysis.ipynb`) to reproduce the analysis and generate the required output files.

3. **API Credentials**:
   - You will need an [ORES API](https://ores.wikimedia.org) access token to run the notebook and retrieve quality scores for the articles. Update the token in the relevant section of the notebook.

4. **Important Documentation**:
   * ORES API Documentation: [ORES API documentation](https://ores.wikimedia.org)
   * LiftWing Documentation: [LiftWing ORES API Documentation](https://wikitech.wikimedia.org/wiki/Machine_Learning/LiftWing/Usage)

## Research Implications

_Through this project, I gained a deeper understanding of how biases can have big affects in large datasets like Wikipedia articles and how these biases can impact the overall analysis. One key takeaway was the uneven distribution of articles and their quality across different countries and regions, which was very suprising. I was primarly surprised to see significant differences in article coverage per capita, with smaller countries or less globally influential regions often having disproportionately fewer articles. On the complete other hand, some small countries, due to particular editorial interest, had a very high article count per capita. This finding reflects potential systemic bias in how content is created and maintained on Wikipedia, where editorial activity tends to focus more on certain countries and topics, potentially influenced by the demographics and interests of Wikipedia contributors._

* What biases did you expect to find in the data (before you started working with it), and why?

   _Before starting this analysis, I expected to find that more developed nations or nations with more significant global influence would have higher article coverage. I also anticipated that wealthier nations would have higher-quality articles due to the availability of resources and higher engagement from users in those regions. These assumptions were largely confirmed, but I did not expect some smaller nations to have such high article counts per capita, indicating editorial preferences rather than strictly population-driven factors._

* What (potential) sources of bias did you discover in the course of your data processing and analysis?

   _During the course of this analysis, one major source of bias I identified was related to the limitations of the ORES system and the reliance on article revision IDs. For some articles, quality scores could not be retrieved, which may have impacted the results with every run through the entire code. Additionally, the population data being in millions created some granularity issues when working with smaller countries. The manual adjustments to populations in cases like Monaco and Tuvalu (where I had to go and look up their populations on WikiPedia and manually enter them is) were another source of potential bias, though necessary to ensure accuracy. Furthermore, the discrepancies in how countries are grouped into regions, especially when country names didn't match perfectly, also introduced a layer of bias._

* How might a researcher supplement or transform this dataset to potentially correct for the limitations/biases you observed?

   _To address these limitations, researchers could supplement the dataset by seeking alternative sources of population data to ensure accuracy, particularly for smaller countries. Additionally, diversifying the contributors to Wikipedia, promoting participation from underrepresented regions, or using tools like bots to improve content coverage in less populated or developing countries could help mitigate some of the biases. An expansion of the dataset to include more nuanced regional data or exploring external databases for a more complete set of articles would also help reduce biases in coverage and article quality._


## Known Issues or Limitations

1. **Missing ORES Scores:**
   - In some cases, the ORES quality prediction for certain articles may not be retrieved. These cases are logged in the `ores_errors_log.txt` file, and an error rate for missing scores is calculated in the analysis. The error rate in this project was approximately 0.11%.
   
2. **Population Data Adjustments:**
   - Some countries in the population dataset, such as Monaco and Tuvalu, were found to have incorrect population values. These were manually adjusted based on data from Wikipedia.
   
3. **Manual Region Assignments:**
   - Countries were assigned to regions based on population data. If a country did not match exactly between the datasets, it was excluded from the final analysis. The excluded countries are logged in the `wp_countries-no_match.txt` file.
   
4. **API Limitations:**
   - The Wikimedia ORES and PageInfo APIs have request rate limits. Although the requests were throttled to avoid exceeding the limit, heavy usage of the API in a short time could lead to temporary restrictions.
   
5. **Population in Millions:**
   - The population data provided is in millions. For smaller populations, such as Tuvalu, precision may be lost when dealing with small article counts relative to population size.
   
6. **Outdated Population Data:**
   - The population data used in this project may not reflect the most current statistics, and updates to the population figures may be required in future analyses.


## License and Data Attribution
* The code in this repository is licensed under the MIT License.
* The data use adheres to the [Wikimedia Foundation Terms of Use](https://foundation.wikimedia.org/wiki/Terms_of_Use).

## Acknowledgments
This project built upon the sample code developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons CC-BY license](https://creativecommons.org/).

This project was developed with the assistance of [ChatGPT](https://openai.com/chatgpt), which helped format code, structure the README, and organize the data and analysis in an efficient and clear manner. All content has been verified and tested by the author.


