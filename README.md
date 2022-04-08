# tacticalrmm-cronbackup
Script to back up Tactical RMM via cron with rotating backup

Nothing fancy here:  a Bash script that uses the TRMM-supplied backup script via cron.  It runs the script as the specified
system user (use the same user as TRMM) and maintains a maximum number of backups automatically.

Because it's designed to be a drop-in cron script, it is configured with internal variables:
```
  BACKUP_COUNT=8                           # Number of backups to keep
  SCRIPT_USER=tactical                     # User to run backup as
  PATH_TO_SCRIPT=/home/tactical/backup.sh  # Path to TRMM backup script
  PATH_TO_BACKUPS=/rmmbackups              # Path to generated backups
  VERBOSE=1                                # 1 is silent; use 0 for manual verbose runs
```
Make sure you update these variables to match your installation.  You will likely need to change the SCRIPT_USER and
PATH_TO_BACKUPS to reflect the user and path you used when you installed TRMM;  the rest are either established by TRMM
or reflect personal preference.
