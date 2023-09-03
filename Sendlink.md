import base64
from googleapiclient.discovery import build
from google.oauth2 import service_account

# Set up your Gmail API credentials
SCOPES = ['https://www.googleapis.com/auth/gmail.compose']
SERVICE_ACCOUNT_FILE = 'your-service-account-credentials.json'  # Replace with your service account credentials file
EMAIL_ADDRESS = 'navvuladon123@gmail.com'  # Replace with your Gmail address
YOUTUBE_LINK = '[https://www.youtube.com/your_video](https://www.youtube.com/watch?v=xARaVwqynKY&ab_channel=StarEntertainments)'  # Replace with your YouTube link

# Initialize the Gmail API
credentials = service_account.Credentials.from_service_account_file(SERVICE_ACCOUNT_FILE, scopes=SCOPES)
service = build('gmail', 'v1', credentials=credentials)

# Get a list of your email contacts (assuming you have them in a file)
# Replace this with your method of retrieving email contacts
with open('vinthagopaludu', 'r') as file:
    contacts = file.read().splitlines()

# Compose and send emails to all contacts
for contact in contacts:
    message = f"Subject: Check out this YouTube video!\n\nHey {contact},\n\nI wanted to share this YouTube video with you: {YOUTUBE_LINK}"
    encoded_message = base64.urlsafe_b64encode(message.encode("utf-8")).decode("utf-8")

    try:
        service.users().messages().send(userId=EMAIL_ADDRESS, body={'raw': encoded_message}).execute()
        print(f"Email sent to {contact}")
    except Exception as e:
        print(f"Error sending email to {contact}: {str(e)}")
