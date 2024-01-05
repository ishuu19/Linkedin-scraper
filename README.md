# LinkedIn Marketplace Investor Scraper

This Python script utilizes Selenium to scrape LinkedIn search results for individuals associated with the keyword "marketplace platform investor." The information extracted includes the name, title, location, summary, and profile link of each person. Here's a breakdown of the script:

## How to Use

1. **Set Up ChromeDriver:** Ensure you have the ChromeDriver executable (`chromedriver.exe`) in the specified path (`chromedriver_path`). Download the appropriate version from the [ChromeDriver website](https://sites.google.com/chromium.org/driver/) based on your Chrome browser version.

    ```python
    chromedriver_path = 'chromedriver.exe'
    driver = webdriver.Chrome(executable_path=chromedriver_path)
    ```

2. **LinkedIn Search URL:** The `base_url` variable contains the base LinkedIn search URL. Customize the `geoUrn` and `keywords` parameters based on your search criteria.

    ```python
    base_url = "https://www.linkedin.com/search/results/people/?geoUrn=%5B%22101355337%22%2C%22102890883%22%2C%22102713980%22%2C%22103291313%22%5D&keywords=marketplace%20platform%20investor&origin=FACETED_SEARCH&sid=SG"
    ```

3. **CSV File Setup:** The script creates a CSV file named `linkedin_marketplace_investor_results.csv` and writes headers for the data fields: Name, Title, Location, Summary, and Profile_Link.

    ```python
    with open('linkedin_marketplace_investor_results.csv', mode='w', encoding='utf-8', newline='') as csv_file:
        fieldnames = ['Name', 'Title', 'Location', 'Summary', 'Profile_Link']
        writer = csv.DictWriter(csv_file, fieldnames=fieldnames)
        writer.writeheader()
    ```

4. **Scrape LinkedIn Results:** The script iterates through LinkedIn search result pages, extracting information for each individual and writing it to the CSV file.

    ```python
    page = 1
    while True:
        url = f"{base_url}&page={page}"
        driver.get(url)
        time.sleep(20)

        try:
            results = driver.find_elements(By.CLASS_NAME, 'reusable-search__result-container')

            if not results:
                print(f"No results found on page {page}")
                break

            for result in results:
                # Extract information for each result and write to CSV
                # ...

        except Exception as outer_err:
            print(f"Error on page {page}: {outer_err}")
            break

        page += 1
    ```

5. **Extract and Write Data:** For each LinkedIn search result, the script extracts the name, title, location, summary, and profile link, and writes this information to the CSV file.

    ```python
    for result in results:
        try:
            # Extract information from each result
            # ...

            # Write data to CSV
            writer.writerow({
                'Name': name_text,
                'Title': title_text,
                'Location': location_text,
                'Summary': summary_text,
                'Profile_Link': profile_link
            })

        except Exception as inner_err:
            print(f"Error extracting information from result: {inner_err}")
    ```

6. **Error Handling:** The script includes error handling to gracefully handle unexpected errors and print relevant information.

    ```python
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    ```

7. **Close WebDriver:** The Selenium webdriver is closed in the `finally` block to ensure proper cleanup.

    ```python
    finally:
        driver.quit()
    ```
## After scraping the data, you need to clean it because the NAME column contains extra information.
#### To do that:

```python
Copy code
# Read the csv file
df = pd.read_csv('linkedin_marketplace_investor_results.csv')
# To clean the NAME column
df["Name"] = df["Name"].str.split("\n").str[0]
```


Feel free to customize the script according to your specific requirements. Happy scraping!
