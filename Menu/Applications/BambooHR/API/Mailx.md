# Send Automated Linux email with gmail
mailx with a gmail Account profile.

      echo "body" | mail -s "subject" -r "MCP SambaTV<noreply@samba.tv>"  -S replyto="Rodney<rodney@samba.tv>" helpdesk@samba.tv 
      
      echo "I've got a little challenge for you, Sark - a new recruit. He's a tough case, but I want him treated in the usual manner. Train him for the games...End of line" | mail -s "Master Control Program" -r "MCP SambaTV<noreply@samba.tv>"  -S replyto="Rodney<rodney@samba.tv>" rodney@samba.tv   
    
    chmod +x test.php
    ./test.php


## Commands
-a file Allows you to attach the given file to the e-mail

-b address Sends blind carbon copies to the comma separated e-mail address list

-c address Sends copies to a list of users

-q file Sets the message contents from the given file

-r from address Sets the from address of the e-mail to be sent

-s subject Sets the e-mail subject

https://www.binarytides.com/linux-mailx-command/

Any email sent from the system will get sent through the gmail servers. 
 * host email domain with Gsuite 
 * make a **noreply** mailbox
 * set up 2 factor authentication on it
 * password for less secure apps
 
 ## Install mailx
 
      yum install heirloom-mailx
      sudo apt-get install heirloom-mailx
      
 ## Create the gmail account for mailx
 
 Add the following information into ~/.mailrc with **nano ~/.mailrc**
 
      account gmail {
          set smtp-use-starttls
          set ssl-verify=ignore
          set smtp-auth=login
          set smtp=smtp://smtp.gmail.com:587
          set from="noreply@samba.tv(tix.sambatv)"
          set smtp-auth-user=noreply@samba.tv
          set smtp-auth-password=Less_secure_apps_password
          set ssl-verify=ignore
      }
 Save and exit 
      
      ctrl C or D
      Y
 Customize settings above
 1. Save and send a text message to yourself
 
        echo "Test, Get Schwifty" | mail someone@samba.tv
 2. subject 
 
        mail -s "Mail Subject" someone@samba.tv
 3. Take message from file
 
        mail -s "Subject" someone@samba.tv < /path/to/file
 or  

        echo "This is message body" | mail -s "Subject" someone@samba.tv
 4. Multiple recipients( comma separated)
  
        echo "Body" | mail -s "Subject" someone@samba.tv,someone2@samba.tv
        
 5. CC and BCC
       
        echo "Body" | mail -s "subject" -c ccuser@samba.tv someone@samba.tv
       
 6. From name and address
 
        echo "Body" | mail -s "Subject" -r "Tix SambaTV<noreply@samba.tv>" someone@samba.tv
        
 7. Reply-to address
 
        echo "Body" | mail -s "Subject" -S replyto="Rodney<rodney@samba.tv>" someone@samba.tv
        
 8. Attachments
 
        q
 9. external SMTP server **Doesn't work**
 
        echo "This is the message body and contains the message" | mailx -v -r "noreply@samba.tv" -s "This is the subject" -S smtp="mail.example.com:587" -S smtp-use-starttls -S smtp-auth=login -S smtp-auth-user="tix@samba.tv" -S smtp-auth-password="" -S ssl-verify=ignore rodney@samba.tv
       
       
        echo -e "Mail body text" | mailx -A gmail -s "Mail subject" your@email
      
 You should receive your email soon from your gmail account
 
 _For automated system-wide emails to be sent correctly you need to login as root. You can **sudo su** to get access as root
 then repeat this process_
 
https://stackoverflow.com/questions/16799407/mailx-and-gmail-nss-config-dir
 https://serverfault.com/questions/498588/smtp-gmail-com-from-bash-gives-error-in-certificate-peers-certificate-issuer
