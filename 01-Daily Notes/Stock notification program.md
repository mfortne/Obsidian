Creation Date: 2025-08-28
2025-08-28T12:51:05-04:00
## Tags: #stocks #trading #programing #python #options

# Notes:
Haha, back to the markets—music to my ears! As a battle-hardened options slinger, automating stock price pulls and notifications is a game-changer. It keeps you in the loop without gluing your eyes to the screen, perfect for spotting entry/exit points on those volatile tickers we're chasing with your $500 war chest.

I'll walk you through building a simple Python program step by step. This script will:
- Pull real-time stock prices using the `yfinance` library (free Yahoo Finance API wrapper).
- Check prices at regular intervals (e.g., every 5 minutes).
- Send desktop notifications (via `plyer`) or email alerts (via `smtplib`) when the price hits a user-defined threshold (e.g., alert if TSLA drops below $200).

### Prerequisites
- Install required libraries: Run `pip install yfinance plyer schedule` in your terminal (if using desktop notifications). For email, Python's built-in `smtplib` works, but you'll need an email account (Gmail works with app passwords).
- This runs indefinitely, so use it in a terminal or as a background process.
- Note: Markets are closed weekends/holidays, so yfinance will return the last close price then. For after-hours, it handles extended data.

### Step-by-Step Code Breakdown
1. **Imports:** Bring in the necessities.
2. **Configuration:** Set your stock symbol, check interval, and alert thresholds.
3. **Price Fetching:** Function to get the current price.
4. **Notification Setup:** Functions for desktop pop-ups or email.
5. **Main Loop:** Schedule periodic checks and trigger alerts.
6. **Running It:** Start the scheduler.

Here's the full script. Copy-paste into a file like `stock_alert.py` and run with `python stock_alert.py`.

```python
import yfinance as yf
import schedule
import time
from plyer import notification  # For desktop notifications
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart

# Step 1: Configuration
STOCK_SYMBOL = "TSLA"  # Change to your ticker, e.g., "GME" or "AAPL"
CHECK_INTERVAL_MINUTES = 5  # How often to check price
ALERT_THRESHOLD_LOW = 200.0  # Alert if price drops below this
ALERT_THRESHOLD_HIGH = 250.0  # Alert if price rises above this

# Email config (optional; comment out if using only desktop notifications)
EMAIL_SENDER = "your.email@gmail.com"  # Your email
EMAIL_PASSWORD = "your_app_password"   # Use Gmail app password (not regular password)
EMAIL_RECEIVER = "recipient.email@gmail.com"  # Where to send alerts
SMTP_SERVER = "smtp.gmail.com"
SMTP_PORT = 587

# Step 2: Function to fetch current stock price
def get_stock_price(symbol):
    try:
        stock = yf.Ticker(symbol)
        data = stock.history(period="1d")  # Get latest data
        if not data.empty:
            return data['Close'].iloc[-1]  # Latest closing price (or current if market open)
        else:
            return None
    except Exception as e:
        print(f"Error fetching price: {e}")
        return None

# Step 3: Function for desktop notification
def send_desktop_notification(title, message):
    notification.notify(
        title=title,
        message=message,
        app_name="Stock Alert",
        timeout=10  # Seconds to show
    )

# Step 4: Function for email notification (optional)
def send_email_notification(subject, body):
    try:
        msg = MIMEMultipart()
        msg['From'] = EMAIL_SENDER
        msg['To'] = EMAIL_RECEIVER
        msg['Subject'] = subject
        msg.attach(MIMEText(body, 'plain'))

        server = smtplib.SMTP(SMTP_SERVER, SMTP_PORT)
        server.starttls()
        server.login(EMAIL_SENDER, EMAIL_PASSWORD)
        server.sendmail(EMAIL_SENDER, EMAIL_RECEIVER, msg.as_string())
        server.quit()
        print("Email sent successfully!")
    except Exception as e:
        print(f"Error sending email: {e}")

# Step 5: Function to check price and notify
def check_price_and_notify():
    price = get_stock_price(STOCK_SYMBOL)
    if price is not None:
        print(f"Current {STOCK_SYMBOL} price: ${price:.2f}")
        
        if price < ALERT_THRESHOLD_LOW:
            message = f"{STOCK_SYMBOL} price dropped below ${ALERT_THRESHOLD_LOW}! Current: ${price:.2f}"
            send_desktop_notification("Price Alert - Low", message)
            # send_email_notification("Low Price Alert", message)  # Uncomment for email
            
        elif price > ALERT_THRESHOLD_HIGH:
            message = f"{STOCK_SYMBOL} price rose above ${ALERT_THRESHOLD_HIGH}! Current: ${price:.2f}"
            send_desktop_notification("Price Alert - High", message)
            # send_email_notification("High Price Alert", message)  # Uncomment for email
    else:
        print("Failed to fetch price.")

# Step 6: Schedule the checks
schedule.every(CHECK_INTERVAL_MINUTES).minutes.do(check_price_and_notify)

# Main loop to run the scheduler
print(f"Starting stock alert for {STOCK_SYMBOL}. Checking every {CHECK_INTERVAL_MINUTES} minutes.")
while True:
    schedule.run_pending()
    time.sleep(1)  # Sleep to avoid high CPU usage
```

### How It Works
- **Fetching Prices:** `yfinance` pulls data from Yahoo. It's reliable and free (no API key needed).
- **Scheduling:** `schedule` library runs the check every X minutes. Adjust `CHECK_INTERVAL_MINUTES` as needed (e.g., 1 for hyper-aggressive monitoring, but don't spam Yahoo).
- **Notifications:**
  - Desktop: Pops up a system notification (works on Windows/Mac/Linux).
  - Email: Uncomment the lines in `check_price_and_notify()` and fill in your email details. For Gmail, generate an "app password" in your Google account settings (under Security > App Passwords) to avoid 2FA issues.
- **Customization Ideas:**
  - Add multiple stocks: Turn `STOCK_SYMBOL` into a list and loop through it.
  - Advanced Alerts: Integrate with Twilio for SMS (pip install twilio) or Slack webhooks.
  - Logging: Write prices to a CSV with pandas for historical tracking.
  - Error Handling: Already basic, but add retries if Yahoo flakes out.
- **Testing:** Run it during market hours. For off-hours, it'll grab the last close. If you hit rate limits, increase the interval.

This setup's kept simple for your small-account grind—focus on 1-2 tickers to avoid overload. If it blows up (code errors), hit me with details, and I'll tweak. Remember, automate wisely; don't let bots make trades without human oversight, or you'll wake up to a zeroed account. Let's stack those gains! 🚀

#### Reference: