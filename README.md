# automated_email_project_via_python

```python
import smtplib              #main library using which we can send emails
from email.mime.text import MIMEText        #help us build text in the email
from email.mime.multipart import MIMEMultipart      #Combine entire mail together

import os
from email.mime.base import MIMEBase        #Needed to attach files in email
from email import encoders                  #this function encodes the attachment for smooth flow



#Setup email config
smtp_server = "smtp.gmail.com"
smtp_port = 587
sender_email = "gaga8204@gmail.com"
email_password = "qwyykgjptksbvydm"      #unique 16 digit generated password via settings on Gmail
receiver_email = "waranganya@gmail.com"

#Email development
subject = "Test_Automation_py"
body = "This is a automated email"

#Compose mail
message = MIMEMultipart()

message["From"] = sender_email
message["To"] = receiver_email
message["Subject"] = subject
message.attach(MIMEText(body,"plain"))


#include attachments
file_path = ['employee.csv','ex.txt']

for file in file_path:
    if os.path.exists(file):
        with open(file, "rb") as attachment:
            part = MIMEBase("application", "octet-stream")
            part.set_payload(attachment.read())
            encoders.encode_base64(part)  
            part.add_header(
                "Content-Disposition",
                f"attachment; filename={os.path.basename(file)}",
            )
            message.attach(part)
    else:
        print(f"File path doesn't {file} exists")


try:
    #Setup to server
    server = smtplib.SMTP(smtp_server,smtp_port)

    #Start the server
    server.starttls()

    #Login to account
    server.login(sender_email,email_password)

    #Send email
    server.sendmail(sender_email,receiver_email,message.as_string())
    print("Mail sent successfully")
except Exception as e:
    print(f"Error found as : {e}")

```
