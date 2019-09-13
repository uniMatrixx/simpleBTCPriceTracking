# simpleBTCPriceTracking
#Simple BTC price tracking with coindesk API
#You can start with this example to develope complex apps for the other coins !
#Firstly you will import modules

import requests
import time
from email.mime.multipart import  MIMEMultipart
from email.mime.text import  MIMEText
import smtplib
import getpass

## This function provides to send mail 
def send_mail():  
    #messagetext = 'This is bitcoin price tracking app'
    msg = MIMEMultipart()
    msg['From'] = "ENTER YOUR MAIL"
    msg['To'] = "WHERE WILL YOU SEND"
    msg['Subject'] = " CHECK BITCOIN PRICE !!!"

    message = "\nBitcoin prices are now " + str(
        bitcoin_rate)

    msg.attach(MIMEText(message,'plain'))
   
    s = smtplib.SMTP("smtp.live.com",587) # I used live.com mail
    s.ehlo()
    s.starttls()
    s.ehlo()
    s.login('your mail','your mail password')
    s.sendmail("Your mail pass","Receiver Mail",msg.as_string())
    s.quit()

    print("Email send Succesfully!")


#Here it is while loop. With this method we will provide check price of bitcoin every 3 or 5 minutes. Check if else blocks
while True:
    url ="https://api.coindesk.com/v1/bpi/currentprice.json"   
    response = requests.get(
        url,
        headers = {"Accept": "application/json"},
    )
    data = response.json()
    bpi= data['bpi']
    USD = bpi['USD']
    bitcoin_rate = int(USD['rate_float'])
    if bitcoin_rate< int(10360):
        send_mail()
        print("check again in 3 minutes. CTRL+C to quit")

        time.sleep(180)
    else:
        time.sleep(300)
        print('Price is ' + str(bitcoin_rate) + '. Check again in 5 minutes. Ctrl+c to quit')
