# Amazon Price Tracker  Web Scraper

A Python-based web scraper that monitors Amazon product prices over time, logs them to a CSV file, and **automatically sends an email alert when the price drops below a set threshold**. Built in Jupyter Notebook using BeautifulSoup, Requests, Pandas, and smtplib.


##  Project Overview

This project scrapes an Amazon product page to extract the **product title** and **current price**, saves the data with a **timestamp** into a CSV file, and checks if the price has fallen below a defined limit. If it has, it fires off an **email notification** so you never miss a deal.


## Features

- Scrapes product **title** and **price** from an Amazon listing
- Appends new records daily to a persistent **CSV file**
- Uses **Pandas** to read and display collected data
- Runs automatically every 24 hours via a `while True` loop with `time.sleep()`
- 📧 **Email alert** via `smtplib` when price drops below a threshold (e.g., ₹300)



## 🧰 Tech Stack

Tool                                                                        Purpose |

 Python 3                                                                  Core language 
 BeautifulSoup4                                                            HTML parsing 
 Requests                                                                  HTTP requests 
 Pandas                                                                    Data reading & display 
 CSV                                                                       Data storage 
 datetime                                                                  Timestamping records 
 smtplib                                                                   Email price alerts 
 Jupyter Notebook                                                          Development environment



## ⚙️ How It Works

1. **Sends an HTTP GET request** to the Amazon product URL with spoofed browser headers to avoid bot detection.
2. **Parses the HTML** using BeautifulSoup to find the product title (`id='productTitle'`) and price (`class='a-price-whole'`).
3. **Cleans the extracted text** using `.strip()`.
4. **Appends a new row** `[title, price, date]` to the CSV file daily.
5. **Checks the price** — if it's below the threshold (e.g., ₹300), calls `send_mail()`.
6. **`send_mail()`** connects to Gmail via SMTP SSL, logs in, and sends a custom alert email.
7. **Loops every 86,400 seconds** (24 hours) via `check_price()` inside a `while True` block.

---

## 📧 Email Alert Setup

The `send_mail()` function uses Gmail's SMTP server:

```python
def send_mail():
    server = smtplib.SMTP_SSL('smtp.gmail.com', 465)
    server.ehlo()
    server.login('your_email@gmail.com', 'your_app_password')

    subject = "The Shirt you want is below ₹300! Now is your chance to buy!"
    body = "This is the moment you've been waiting for. Don't miss it!"
    msg = f"Subject: {subject}\n\n{body}"

    server.sendmail('your_email@gmail.com', 'recipient@gmail.com', msg)
```

>  Use a [Gmail App Password](https://support.google.com/accounts/answer/185833) instead of your real password. Never commit credentials to GitHub  use environment variables or a `.env` file.


##  Disclaimer

This project is intended for **educational purposes only**. Web scraping may violate Amazon's [Terms of Service](https://www.amazon.com/gp/help/customer/display.html?nodeId=508088). Use responsibly and avoid excessive requests.
