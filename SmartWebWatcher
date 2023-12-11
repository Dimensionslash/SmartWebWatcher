import requests
from bs4 import BeautifulSoup
from datetime import datetime, timedelta
from urllib.parse import urlparse
import time

def download_page(url, download_location, prev_content):
    # Make a request to the website
    response = requests.get(url)

    if response.status_code == 200:
        # Parse the HTML content
        soup = BeautifulSoup(response.text, 'html.parser')

        # Extract the domain name from the URL
        domain_name = urlparse(url).hostname if urlparse(url).hostname else "unknown"

        # Generate a unique filename with the timestamp and domain name
        timestamp = datetime.now().strftime("%Y%m%d%H%M%S")
        filename = f"{domain_name}_downloaded_page_{timestamp}.html"
        full_path = f"{download_location}\\{filename}"

        # Get the current content of the page
        current_content = str(soup)

        # Compare with the previous content
        if current_content != prev_content:
            # Save the page content to the specified file
            with open(full_path, "w", encoding="utf-8") as file:
                file.write(current_content)
            print(f"Page downloaded successfully to: {full_path}")
            return current_content
        else:
            print("No changes detected. Waiting for the next check...")
            return prev_content
    else:
        print(f"Error: {response.status_code}")
        return prev_content

# Ask the user for the website and download location
url = input("Enter the URL of the website you want to download: ")
download_location = input("Enter the download location: ")

# Initial download
prev_content = download_page(url, download_location, "")

# Loop to check for changes and download every 10 seconds
while True:
    time.sleep(10)
    print("Checking for changes...")
    prev_content = download_page(url, download_location, prev_content)
