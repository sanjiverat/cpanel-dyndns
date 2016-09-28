# Dynamic DNS using cpanel's json api

This might be useful if you already have a (shared) hosting account with cpanel, and you need to access your home computer from outside. The idea is that instead of using a dynamic dns service, you will run a small script on your home computer, maybe as a cron job, that will check your home computer's current IP, the IP pointed to by an A record on your hosting account's cpanel, and will update the cpanel's A record if it has changed.

It is assumed that you have already used the cpanel's web interface to create a A record for hostname.myzone.com pointing to some IP address. The script retrieves this record, and if the IP address is different from current IP of the computer running the script, modifies the record.
