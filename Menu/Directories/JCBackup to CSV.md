        pwsh

        Install-Module JumpCloud -Scope CurrentUser

        $BackupLocation = "/Users/rodneynobles/Backup/JCBackup"

        $JumpCloudAPIKey = "" 

        Set-Location $BackupLocation

        Connect-JCOnline -JumpCloudAPIKey $JumpCloudAPIKey -force 

        Get-JCBackup -All

        $BackupLocation = '/Users/rodneynobles/Backup/JCBackup'

        $JumpCloudAPIKey = 'API key'

        env EDITOR=nano crontab -e 

        * * * * *  /usr/local/bin/pwsh /Users/rodneynobles/Backup/JumpCloudCSVBackup.ps1 &>/tmp/JCBackup.log

        ctrl + x
        Y
        ENTER

        crontab -l

## Backing up JumpCloud Directory Information to CSV


Get-JCBackup command which contains examples for backing up specific items.


Prerequisites: JumpCloud PowerShell module version 1.5.0 or later.

//
    What's the issue? Did you have a look at Get-ChildiItem cmdlet help? 
To point you in the right direction.. you can use this oneliner:
Get-ChildItem | Where-Object {$_.LastWriteTime -ge $(get-date).AddDays(-30)}
And add -Recurse to Get-ChildItem if needed. Then you can pipe Copy-Item at the end of the oneliner to copy to the target folder. 
I strongly suggest to you to get familiar with the Get-Help cmdlet.
//

# Install JC Pwsh Module
     -> pwsh
     -> Install-Module JumpCloud -Scope CurrentUser

# Step 1: Download or create the JumpCloudCSVBackup.ps1 file on your local machine.
  
  ## Note version 1.5.0 or later of the JumpCloud Pwsh module must be installed to leverage this script.

  ## After running the script five CSV files will be created within the backup location directory specified.  

  ## Populate the below variables before running the script

      $BackupLocation = "" 
    # Enter full path to desired backup folder for backup CSV files. Example path on Mac "/Users/Buster/Backup/JCBackup"
      
     -> $BackupLocation = "/Users/rodneynobles/Backup/JCBackup"

    -> $JumpCloudAPIKey = "" 
    # The JumpCloud API key can be found in the admin console by selecting the drop down icon next to admin email in top right and selecting "API Settings"

   ## --------------- Do not modify below this line ----------

    -> Set-Location $BackupLocation

    -> Connect-JCOnline -JumpCloudAPIKey $JumpCloudAPIKey -force 
  #Force parameter used to auth to JumpCloud API without update check

    -> Get-JCBackup -All

# Step 2: Create a folder to save the backup CSV files in and then fill in the $BackupLocation and $JumpCloudAPIKey variables within the JumpCloudCSVBackup.ps1 file.

Example: 

    -> $BackupLocation = '/Users/rodneynobles/Backup/JCBackup'
    -> $JumpCloudAPIKey = 'API key'
Step 3: Determine when you want to run the crontab and convert this time to a cron schedule expression.
  daily
  hourly

Step 4: Create the crontab
  @daily

Ener the command:

    -> env EDITOR=nano crontab -e 
This will open crontab in the nano editor.

Enter the crontab schedule expression you created in Step 3 followed by ‘/usr/local/bin/pwsh’ 
   
    */5 * * * * /usr/local/bin/pwsh

and then the full path to the filled out JumpCloudCSVBackup.ps1 file followed by ‘&>/tmp/JCBackup.log’
    
     /Users/rodneynobles/Backup/JumpCloudCSVBackup.ps1 &>/tmp/JCBackup.log
    
    -> * * * * *  /usr/local/bin/pwsh /Users/rodneynobles/Backup/JumpCloudCSVBackup.ps1 &>/tmp/JCBackup.log
By appending the command with ‘&>/tmp/JCBackup.log’ any errors created when the crontab runs will be stored in the file JCBackup.log located the /tmp directory.

     -> ctrl + x
     -> Y
     -> ENTER
Press ‘ctrl + x’ to exit the nano editor followed by ‘Y’ and then ‘enter’ at the prompt to save the crontab

     -> crontab -l
To see if your crontab has been saved the command ‘crontab -l’ will list all crontabs

*Protip*
    
    tail /tmp/JCBackup.log
    
    man tail -f
Example:

30 9 * * *  /usr/local/bin/pwsh /Users/Buster/Backup/JumpCloudCSVBackup.ps1 &>/tmp/JCBackup.log
Example crontab to run at 9:30 am where the JumpCloudCSVBackup.ps1 script file is stored in the ‘/Users/Buster/Backup' folder. The backup CSV files will be created daily and stored in the folder provided in the variable $BackupLocation as detailed above.

# 3
https://github.com/TheJumpCloud/support/wiki/Get-JCBackup

# 4
https://jumpcloud.desk.com/customer/en/portal/articles/2948198-backing-up-jumpcloud-directory-information-to-csv
