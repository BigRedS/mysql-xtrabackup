# mysql-xtrabackup

A wrapper round Percona's Xtrabackup script:

https://www.percona.com/software/mysql-database/percona-xtrabackup

this is meant to be cronned and used to produce a series of copies of the db 
to be kept as backups.

Stick something like this in your crontab:

    0 2 1 * * *  mysql-xtrabackup --defaults-file ~/backups.my.cnf --label monthly --max-days 95

and keep about three month's worth of monthly archives of your DB, stored 
named something like this:

    /home/mysql-xtrabackup/monthly/2018-01-01_02.00.01_fri-mysql-xtrabackup.tar.gz
    /home/mysql-xtrabackup/monthly/2018-02-01_02.00.01_fri-mysql-xtrabackup.tar.gz
    /home/mysql-xtrabackup/monthly/2018-03-01_02.00.01_fri-mysql-xtrabackup.tar.gz
    /home/mysql-xtrabackup/monthly/2018-04-01_02.00.01_fri-mysql-xtrabackup.tar.gz

And each of these is just a gzipped tarball of the `/var/lib/mysql` directory 
that innobackupex produced. See the --help output (or `./help.txt`) for an 
explanation of what the labels mean.


Logfiles are written so as to be self-rotating; the above backups will have 
produced these logfiles:

    /var/log/mysql-xtrabackup/jan-monthly
    /var/log/mysql-xtrabackup/feb-monthly
    /var/log/mysql-xtrabackup/mar-monthly
    /var/log/mysql-xtrabackup/apr-monthly

so you'll only ever have 12 of them, and can always inspect the last file to 
get the status of the last backup (see my `check_log` project for how I do that
in icinga).
