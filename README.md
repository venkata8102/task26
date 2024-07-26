imdb_search/
    ├── pages/
    │   └── search_page.py
    ├── tests/
    │   └── test_imdb_search.py
    ├── conftest.py
    └── requirements.txt
    from selenium.webdriver.common.by import By 
from selenium.webdriver.support.ui import WebDriverWait 
from selenium.webdriver.support import expected_conditions as EC 
from selenium.webdriver.support.ui import Select

class SearchPage: 
    def __init__(self, driver): 
        self.driver = driver 
        self.url = 'https://www.imdb.com/search/name'

    # Locators 
    first_name_locator = (By.ID, 'name') 
    birth_place_locator = (By.ID, 'birth_place') 
    birth_month_locator = (By.ID, 'birth_monthday') 
    birth_year_locator = (By.ID, 'birth_year') 
    gender_locator = (By.ID, 'gender') 
    search_button_locator = (By.XPATH, '//button[@type="submit"]')

    def open(self): 
        self.driver.get(self.url)

    def set_first_name(self, name): 
        element = WebDriverWait(self.driver, 10).until( 
            EC.presence_of_element_located(self.first_name_locator) 
        ) 
        element.send_keys(name)

    def set_birth_place(self, place): 
        element = WebDriverWait(self.driver, 10).until( 
            EC.presence_of_element_located(self.birth_place_locator) 
        ) 
        element.send_keys(place)

    def set_birth_month(self, month): 
        element = WebDriverWait(self.driver, 10).until( 
            EC.presence_of_element_located(self.birth_month_locator) 
        ) 
        select = Select(element) 
        select.select_by_visible_text(month)

    def set_birth_year(self, year): 
        element = WebDriverWait(self.driver, 10).until( 
            EC.presence_of_element_located(self.birth_year_locator) 
        ) 
        select = Select(element) 
        select.select_by_value(year)

    def set_gender(self, gender): 
        element = WebDriverWait(self.driver, 10).until( 
            EC.presence_of_element_located(self.gender_locator) 
        ) 
        select = Select(element) 
        select.select_by_visible_text(gender)

    def click_search(self): 
        element = WebDriverWait(self.driver, 10).until( 
            EC.element_to_be_clickable(self.search_button_locator) 
        ) 
        element.click() 
        import pytest 
from selenium import webdriver 
from pages.search_page import SearchPage

@pytest.fixture(scope='module') 
def driver(): 
    driver = webdriver.Chrome(executable_path='/path/to/chromedriver')  # Update the path 
    yield driver 
    driver.quit()

def test_imdb_search(driver): 
    search_page = SearchPage(driver) 
    search_page.open()

    # Fill the form 
    search_page.set_first_name('Leonardo') 
    search_page.set_birth_place('Los Angeles') 
    search_page.set_birth_month('November') 
    search_page.set_birth_year('1974') 
    search_page.set_gender('Male')

    # Perform search 
    search_page.click_search()

    # Wait for the search results to be displayed 
    results_locator = (By.CLASS_NAME, 'lister-item') 
    WebDriverWait(driver, 10).until( 
        EC.presence_of_all_elements_located(results_locator) 
    )

    # Validate: You can add more assertions as needed 
    assert len(driver.find_elements(*results_locator)) > 0 
