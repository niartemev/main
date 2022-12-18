from selenium import webdriver
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
from selenium.common.exceptions import TimeoutException
from selenium.webdriver.support.ui import Select
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.chrome.service import Service
import time
from pyotp import *

who = "init"
#initialize the browser
def init(customer,action):
    print("initing")
    
    options = Options()
    options.add_experimental_option('excludeSwitches', ['enable-logging'])
    driver = webdriver.Chrome(options=options)

    delay = 10
    totp = TOTP("FGQCIRF6Y2IYCBAK") #CHANGE THIS
    driver.get("https://manage.barracudamsp.com/Login.aspx?ReturnUrl=%2f")
    driver.maximize_window()
    login(driver,delay,totp,action,customer)

#get info
def get_info():
    global who
    customer = input("Customer:")
    action = int(input("Action (1 - whitelist,2 - block):"))
    who = input("Who are we blocking? (wildcards accepted):")
    print(who)
    validate(customer,action)
    
#validate input
def validate(customer,action):
    if (action != 1 and action != 2):
        print("Wrong action")
        get_info()
    else:
        init(customer,action)

#action function
def act_on(driver,action,customer):
    delay = 1
    myElem = WebDriverWait(driver,delay).until(EC.presence_of_element_located((By.NAME, "sp_email")))
    driver.find_element(By.NAME, "sp_email").send_keys(who)

    select = Select(driver.find_element(By.ID, "sp_policy"))
    print(select.options)
    if (action == 1):
        select.select_by_value("exempt")
    else:
        select.select_by_value("block")
    driver.find_element(By.XPATH, "//a[@title='Add']").click()
    time.sleep(10)

def to_senders(driver,action,customer):
    delay = 1.5
    retries
    for i in range(retries):
        try:
            driver.get("https://ess.barracudanetworks.com/settings/sender_policies")
            myElem = WebDriverWait(driver,delay).until(EC.presence_of_element_located((By.NAME, "sp_email")))
            act_on(driver,action,customer) 
        except TimeoutException:
            print("Timed out on senders")

#dhitelist address
def whitelist(driver,delay,customer):
    print("whitelisting")
#perform the action
def perform(driver,delay,action,customer):
    try:
        driver.get("https://manage.barracudamsp.com/SSOHandoff.aspx?type=bcc")
        myElem2 = WebDriverWait(driver, delay).until(EC.presence_of_element_located((By.ID, "hui-account-dropdown")))
        driver.find_element(By.ID, 'hui-account-dropdown').click()
        time.sleep(0.85)
        driver.find_element(By.XPATH, "//*[contains(text(),'Aging Ways')]").click()
        myElem3 = WebDriverWait(driver, delay).until(EC.presence_of_element_located((By.ID, "hui-account-dropdown")))
        time.sleep(0.5)
        driver.get("https://ess.barracudanetworks.com/settings/sender_policies")
        myElem4 = WebDriverWait(driver, delay).until(EC.presence_of_element_located((By.CSS_SELECTOR, '[data-title="Domains"]')))
        act_on(driver,action,customer)
        
    except TimeoutException:
        print("Perform failed 1")
        perform(driver,delay,action,customer)
#Try to login
def login(driver,delay,totp,action,customer):
    try:
        myElem = WebDriverWait(driver, delay).until(EC.presence_of_element_located((By.ID, 'ctl00_start_LoginForm1_txtUserName')))
        driver.find_element(By.ID, "ctl00_start_LoginForm1_txtUserName").send_keys("username") #change this
        driver.find_element(By.ID, "ctl00_start_LoginForm1_txtPassword").send_keys("password")  #and this
        driver.find_element(By.ID, "ctl00_start_LoginForm1_LoginButton").click()

        try:
            token = totp.now()
            myElem2 = WebDriverWait(driver, delay).until(EC.presence_of_element_located((By.ID, "ctl00_start_txtAuthenticationCode")))
            driver.find_element(By.ID, "ctl00_start_txtAuthenticationCode").send_keys(token)
            driver.find_element(By.ID, "ctl00_start_btnAuthenticate").click()
            myElem2 = WebDriverWait(driver, delay).until(EC.presence_of_element_located((By.ID, "ctl00_ContentMain_Repeater1_ctl00_HyperLink1")))
            perform(driver,delay,action,customer)
        except TimeoutException:
            print("failed to load(nested)")
    except TimeoutException:
        print("failed to load(inside)")

#main control
def main():
    get_info()


main()






