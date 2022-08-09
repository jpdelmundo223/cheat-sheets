# Selenium Cheat Sheet

### Installation
```bash
pip install selenium
```

### Basic example 
```python 
import os
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By

driver = webdriver.Chrome(executable_path=os.getenv('chromedriver', r'C:\Users\Downloads\chromedriver.exe'))

driver.get("https://www.facebook.com/")

email_or_phone_input = driver.find_element(By.CSS_SELECTOR, "input[class='inputtext _55r1 _6luy']")
email_or_phone_input.send_keys('username')

password_input = driver.find_element(By.CSS_SELECTOR, "input[class='inputtext _55r1 _6luy _9npi']")
password_input.send_keys('password')

login_button = driver.find_element(By.NAME, "login")
login_button.send_keys(Keys.ENTER)

driver.close()
```

### Element locators
#### `NAME`
```
<html>
 <body>
  <h1 name="welcome">Welcome</h1>
</body>
</html>
```

```python
...
from selenium.webdriver.common.by import By

...
element = driver.find_element(By.NAME, 'welcome')
```

#### `CLASS_NAME`
```
<html>
 <body>
  <h1 class="welcome">Welcome</h1>
</body>
</html>
```

```python
...
from selenium.webdriver.common.by import By

...
element = driver.find_element(By.CLASS_NAME, 'welcome')
```

#### `ID`
```
<html>
 <body>
  <h1 id="welcome">Welcome</h1>
</body>
</html>
```

```python
...
from selenium.webdriver.common.by import By

...
element = driver.find_element(By.ID, 'welcome')
```

#### `TAG_NAME`
```
<html>
 <body>
  <h1>Welcome</h1>
</body>
</html>
```

```python
...
from selenium.webdriver.common.by import By

...
element = driver.find_element(By.TAG_NAME, 'h1')
```

#### `CSS_SELECTOR`
```
<html>
 <body>
  <button id="login" class="btn btn-primary disabled">Welcome</button>
</body>
</html>
```

```python
...
from selenium.webdriver.common.by import By

...
element = driver.find_element(By.CSS_SELECTOR, '#login')
# or
element = driver.find_element(By.CSS_SELECTOR, "button[class='btn btn-primary disabled']") # used for elements with multiple class names
```

#### `XPATH`
```
<html>
 <body>
  <form id="loginForm">
   <input name="username" type="text" />
   <input name="password" type="password" />
   <input name="continue" type="submit" value="Login" />
   <input name="continue" type="button" value="Clear" />
  </form>
</body>
</html>
```
The form elements can be located like this:
```python
login_form = driver.find_element(By.XPATH, "/html/body/form[1]")
login_form = driver.find_element(By.XPATH, "//form[1]")
login_form = driver.find_element(By.XPATH, "//form[@id='loginForm']")
```

The username element can be located like this:
```python
username = driver.find_element(By.XPATH, "//form[input/@name='username']")
username = driver.find_element(By.XPATH, "//form[@id='loginForm']/input[1]")
username = driver.find_element(By.XPATH, "//input[@name='username']")
```

#### `LINK_TEXT` & `PARTIAL_LINK_TEXT`
```
<html>
 <body>
  <p>Are you sure you want to do this?</p>
  <a href="continue.html">Continue</a>
  <a href="cancel.html">Cancel</a>
</body>
</html>
```

```python
# using LINK_TEXT
continue_link = driver.find_element(By.LINK_TEXT, 'Continue')

# using PARTIAL_LINK_TEXT
continue_link = driver.find_element(By.PARTIAL_LINK_TEXT, 'Conti')
```

### Explicit waits
```python
...
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

...
wait = WebDriverWait(driver, 10)

wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, "img[class='fb_logo _8ilh img']")))

facebook_logo = driver.find_element(By.CSS_SELECTOR, "img[class='fb_logo _8ilh img']")

assert 'Facebook' in facebook_logo.get_attribute("alt") # returns AssertionError if the condition aren't met

print(facebook_logo.get_attribute("alt"))
```

#### or

```python
...
wait = WebDriverWait(driver, 10)

facebook_logo = wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, "img[class='fb_logo _8ilh img']")))

assert 'Facebook' in facebook_logo.get_attribute("alt") # returns AssertionError if the condition aren't met

print(facebook_logo.get_attribute("alt"))
```

#### or

```python
...
facebook_logo = WebDriverWait(driver, 10).until(
    (EC.presence_of_element_located((By.CSS_SELECTOR, "img[class='fb_logo _8ilh img']")))
)

assert 'Facebook' in facebook_logo.get_attribute("alt") # returns AssertionError if the condition aren't met

print(facebook_logo.get_attribute("alt"))
```

### Implicit waits
```python
driver.implicitly_wait(10) # implicitly waits for 10 seconds
```

### Working with select tag
```python
...
select = driver.find_element(By.NAME, 'dbvendor')

options = select.find_elements(By.TAG_NAME, 'option')

for option in options:
    if 'db2' in option.get_attribute('value'):
        option.click()
# or
...
from selenium.webdriver.support.ui import Select

...
select = Select(driver.find_element(By.NAME, 'dbvendor'))

# selecting by index
select.select_by_index(0)

# selecting by visible value
select.select_by_visible_value("db2")

# selecting by value
select.select_by_value("db2")

# deselecting all
select.deselect_all()

for option in options:
    if 'db2' in option.get_attribute('value'):
        option.click()
```

### Moving forward and backwards in your browser's history
* Moving forward
```python
driver.forward()
```

* Moving backward
```python
driver.back()
```

### Sending click events using `click()`
```python
...
submit = driver.find_element(By.ID, 'submit')

submit.click()
```

### Sending keys using `send_keys()`
```python
...
element = driver.find_element(By.ID, 'email')
element.send_keys('john.doe@email.com')
```

### Sending keys using `Keys`
```python
...
email = driver.find_element(By.ID, 'email')
password = driver.find_element(By.ID, 'password')

email.send_keys('john.doe@email.com')
password.send_keys('johndoe123')

password.send_keys(Keys.ENTER)
```