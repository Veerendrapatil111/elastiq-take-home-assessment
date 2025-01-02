# QA Selenium Automation with Python

## Objective
Create a Selenium automation script in Python to validate search functionality on the **Selenium Playground** website.

> [!NOTE]
> **Deliverables:**
> 1. A Python script (`qa_selenium_test.py`) that:
>    - Navigates to the [Selenium Playground Table Search Demo](https://www.lambdatest.com/selenium-playground/table-sort-search-demo).
>    - Locates and interacts with the search box to search for "New York".
>    - Validates that the search results show **5 entries out of 24 total entries**.
> 2. A brief **README** explaining the approach and how to run the script.
> 3. Any additional setup instructions (e.g., local environment, dependencies, drivers etc).

> [!TIP]
> Use Python's `pytest` framework to structure your test cases.

> [!IMPORTANT]
> - **Environment Setup:** Follow good coding practices and ensure the script is compatible with the latest stable Selenium version.
> - **Browser Compatibility:** Test with at least one major browser (e.g., Chrome, Firefox).

> [!CAUTION]
> - **Assertions:** Ensure all validations use robust assertion statements.
> - **Code Quality:** Follow PEP8 standards for Python code.
>
> - import time
import pytest
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from webdriver_manager.chrome import ChromeDriverManager

@pytest.fixture(scope="module")
def driver():
    # Setup: Initialize the WebDriver (Chrome in this case)
    driver = webdriver.Chrome(ChromeDriverManager().install())
    driver.get("https://www.lambdatest.com/selenium-playground/table-sort-search-demo")
    yield driver
    # Teardown: Quit the driver after tests are completed
    driver.quit()

def test_search_functionality(driver):
    # Locate the search input box using its XPath and perform search for "New York"
    search_box = driver.find_element(By.XPATH, "//input[@type='search']")
    
    # Perform search for "New York"
    search_box.clear()  # Clear any pre-existing text
    search_box.send_keys("New York")
    search_box.send_keys(Keys.RETURN)  # Press Enter to trigger the search
    
    # Wait for the table to update the search results (allowing some time for the results to appear)
    time.sleep(2)
    
    # Validate the number of rows shown in the search results
    rows = driver.find_elements(By.XPATH, "//table[@id='example']/tbody/tr")
    assert len(rows) == 5, f"Expected 5 rows, but got {len(rows)} rows."
    
    # Additionally, validate the total number of entries shown in the table header
    total_entries = driver.find_element(By.XPATH, "//div[@class='dataTables_info']").text
    assert "24" in total_entries, f"Expected total entries to be 24, but found: {total_entries}"

if __name__ == "__main__":
    pytest.main()


**Good luck!**
