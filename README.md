from selenium import webdriver
from selenium.webdriver.common.by import By
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.chrome.service import Service
from time import sleep



class TestGettop:
    browser = None

    def setup(self):
        # Install and start the browser
        driver_path = ChromeDriverManager().install()
        self.browser = webdriver.Chrome(service=Service(driver_path))

    def test_header_link(self):
        #click laptops link
        self.browser.get('https://gettop.us/')
        self.browser.find_element(By.XPATH, "//a[text()='Laptops' and @class='nav-top-link']").click()
        #Verify correct page opens:
        expected_result = 'HOME / LAPTOP'
        actual_result = self.browser.find_element(By.XPATH, "//nav[contains(@class, 'breadcrumbs')]").text
        assert actual_result == expected_result, f'Error Actual {actual_result} did not match expected {expected_result}'


    def test_add_to_cart(self):
        #open first product
        self.browser.get('https://gettop.us/product-category/laptop/')
        self.browser.find_element(By.XPATH,"//p[@class='name product-title']").click()
        #store product name and click add to cart
        product_name = self.browser.find_element(By.XPATH,"//h1[contains(@class, 'product-title')]").text
        self.browser.find_element(By.XPATH,"//button[@name='add-to-cart']").click()
        #Verify ]
        self.browser.get('https://gettop.us/cart/')
        cart_product_name = self.browser.find_element(By.XPATH,"//td[@class='product-name']//a").text
        assert cart_product_name == product_name


    def teardown(self):
        self.browser.quit()


