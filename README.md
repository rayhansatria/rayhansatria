import smtplib
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
import schedule
import time

def send_email(subject, body, to_email, smtp_server, smtp_port, smtp_username, smtp_password):
    msg = MIMEMultipart()
    msg['From'] = smtp_username
    msg['To'] = to_email
    msg['Subject'] = subject
    msg.attach(MIMEText(body, 'plain'))

    with smtplib.SMTP(smtp_server, smtp_port) as server:
        server.starttls()
        server.login(smtp_username, smtp_password)
        server.sendmail(smtp_username, to_email, msg.as_string())

def generate_report():
    # Logic to generate your daily report
    report_content = "This is your daily report content."

    # Replace with your email and SMTP server details
    to_email = "recipient@example.com"
    smtp_server = "smtp.example.com"
    smtp_port = 587
    smtp_username = "your_username"
    smtp_password = "your_password"

    subject = "Daily Report"
    send_email(subject, report_content, to_email, smtp_server, smtp_port, smtp_username, smtp_password)
    print("Daily report sent successfully.")

# Schedule the script to run daily at a specific time (adjust as needed)
schedule.every().day.at("08:00").do(generate_report)

while True:
    schedule.run_pending()
    time.sleep(1)
