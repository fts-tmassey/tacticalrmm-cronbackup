# tacticalrmm-cronbackup
Script to back up Tactical RMM via cron with rotating backup

Nothing fancy here:  a Bash script that uses the TRMM-supplied backup script via cron.  It runs the script as the specified
system user (use the same user as TRMM) and maintains a maximum number of backups automatically.
