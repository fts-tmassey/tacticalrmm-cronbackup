# tacticalrmm-cronbackup
Script to back up Tactical RMM via cron with rotating backup

Nothing fancy here:  a Bash script that uses the TRMM-supplied backup script via cron.  It runs the script as the specified system user (use the same user as you used to install TRMM) and maintains a maximum number of backups automatically.

# General installation information
These are suggested, general instructions, based on Ubuntu 20.04.  You will likely need to adapt them for your environment, especially the user and path used to install TRMM.  This also assumes that you have sudo access.  If you're running as root, remove the "sudo" portion of any commands where it's present.

## Put script in place
Download script copy/move it to a cron-schedued directory.  For Ubuntu 20.04:
```
wget https://github.com/fts-tmassey/tacticalrmm-cronbackup/raw/main/trmmcronbackup
sudo mv trmmcronbackup /etc/cron.daily
sudo chown root:root /etc/cron.daily/trmm-backup-with-rotation
sudo chmod 755 /etc/cron.daily/trmm-backup-with-rotation
```
## Modify for your environment
Because we're launching it directly from a cron folder, we can't exactly use parameters.  You will need to update the internal variables to refiect your environment:
```
sudo nano /etc/cron.daily/trmmcronbackup
```
Update the configuration variables to meet your needs and installation details.  You will likely need to change at least the SCRIPT_USER and
PATH_TO_BACKUPS to reflect the user and path you used when you installed TRMM;  the rest are either established by TRMM
or reflect personal preference.
* BACKUP_COUNT : Number of backups for the script to keep
* SCRIPT_USER : The user you used to install/run TRMM.  We will run the backup with the same user.
* PATH_TO_SCRIPT : This is the complete path to the script that will be run to back up TRMM -- most likley the backup.sh script supplied by TRMM!
* PATH_TO_BACKUPS : The path where your backups will be stored.  If you use the default TRMM script, this will not need to be changed (unless they change the path in the future).
* VERBOSE : Allows you to see lots of details if you run it manually.

## Test the script
It's easy to test:
```
sudo /etc/cron.daily/trmm-backup-with-rotation
```
If "VERBOSE=1", you will not see any output unless there is a serious error.  If you want to see details, temporarily update the scipt to "VERBOSE=0".

# TODO Items
TRMM recommends that you periodically prune database items to reduce backup size.  I'd like to add this to the script (or maybe have a different script for cron.monthly).  But until my backups get too large to deal with, I've not dug into it yet.  Patches gratefully considered!  :)
