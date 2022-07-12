NOTE: 2022/04/12 - I am not actively tracking TRMM development at this time, so future changes in backup.sh may break this script.

# tacticalrmm-cronbackup
Script to back up Tactical RMM via cron with rotating backup

Nothing fancy here:  a Bash script that uses the TRMM-supplied backup script via cron.  It runs the script as the same user as you used to install TRMM and maintains a maximum number of backups automatically.

# General installation information
These are suggested, general instructions, based on Ubuntu 20.04.  These instructions assume that you are logged in as the same user that you used to install TRMM and that this user has sudo access.  (Please note that sudo access is required by the underlying backup script supplied by TRMM as well.)

You will not likely need to modify this script for a typical TRMM installation, unless you want to change the number of backups to keep.

## Test the TRMM-supplied backup script
This script uses the TRMM-supplied backup.sh script to actually perform the backup, so this script must already be in place and ready to use.  In addition, running the script will perform a bit of initial configuration on our behalf, so we will need to run this script successfully at least once.

According to the TRMM developers (Ref:  https://github.com/amidaware/tacticalrmm/issues/1056), backup.sh will always be downloaded as part of the initial install in /rmm so we will use this directly:  you do not need to download it manually.  However, it is possible that you will need to mark it as executable (though it's always been marked for me).  Let's mark it just in case, then run the backup script and make sure that we get an output file:
```
chmod +x /rmm/backup.sh
/rmm/backup.sh
ls -lh /rmmbackups/
```
You should now see a backup file!  If you have any isuses with this, check out the TRMM doc here:  https://docs.tacticalrmm.com/backup/  Make sure you have a properly-running backup.sh before you continue.

## Put cron script in place
Download the script and move it to a cron-schedued directory (cron.daily in this case).  For Ubuntu 20.04:
```
wget https://github.com/fts-tmassey/tacticalrmm-cronbackup/raw/main/trmmcronbackup
sudo mv trmmcronbackup /etc/cron.daily
sudo chown root:root /etc/cron.daily/trmmcronbackup
sudo chmod 755 /etc/cron.daily/trmmcronbackup
```

## Modify for your environment
Because we're launching it directly from a cron folder, we can't exactly use parameters.  The script uses internal variables to reflect your environment.  These variables use the default TRMM configuration or detect what they need to know, so you should not need to make any changes.  The only exception might be the number of backups to keep.

If you have a non-standard TRMM configuration, or if you wish to change the number of backups kept, you will need to change the variables to match your needs.  Start by editing the script:
```
sudo nano /etc/cron.daily/trmmcronbackup
```
Update the configuration variables to meet your needs and installation details, then save and exit.

## Test the script
It's easy to test:
```
sudo /etc/cron.daily/trmmcronbackup
```
If `VERBOSE=FALSE`, the default, you will not see any output unless there is a serious error.  If you want to see details, temporarily update the scipt to `VERBOSE=TRUE`.

Now, check to see if the backup worked correctly:
```
ls -lh /rmmbackups/
```
You should now see a new backup created just now.  If you want to test the rotation, keep running the script until you exceed the number of backups to keep and confirm that the oldest backup is deleted.

# TODO Items
TRMM recommends that you do a prune of the Audit Log and Pending Actions database items before running a backup, but only provide a GUI method of doing this.  I'd like to add this to the script (or maybe have a different script for cron.monthly).  But until my backups get too large or slow to deal with, I've not dug into it yet.  Patches gratefully considered!  :)
