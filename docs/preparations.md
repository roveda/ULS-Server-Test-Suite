# Preparations


Throughout the documentation, the term "stage environment" will be used to identify the staging or test or whatever environment is used for the tests.

Set up the stage environment as a new ULS-server, you do not need any data from the production environment.

If you are updating an existing server you may want to check that the update script has kept all monitoring settings.
Keep that in mind.

All threshold definitions concerning limits, is-alives and combined limits may be deleted! 
Check, if they have been converted (if necessary) from the old ULS version to the new version 
after having run the ULS update script but before continuing here!

Remove all defined limits, aggregations, value retentions and mail reports. 
Login to the ULS-server of the stage environment as user "uls" and issue the commands:

:warning: DO NOT DO THAT IN PRODUCTION

```
$ u2w_interpreter /home/uls/testsuite/del_all_limits.s2w
$ u2w_interpreter /home/uls/testsuite/del_all_mailreports.s2w
```
(ask your ULS administrator if the scripts cannot be found at that location)

Alternatively, you can disable all notification destinations from all threshold definitions by issuing (that keeps the definitions, though):

```
$ . /etc/ulsserver.conf
$ /home/uls/testsuite/uls_deactivate_all.sh
```

Check that the regular ULS jobs are correctly executed by cron:
```
# vi /etc/cron.d/ulsserver
```

Should look like:

```
# Alte Werte loeschen
38 21 * * * uls /usr/local/bin/jobmonitor2uls -s ULS -t Delete-Old-Loggings /srv/uls/clean/mysql_delete_old_loggings >/dev/null 2>&1
#
# isAlives testen
2-57/5 * * * * uls /usr/local/bin/jobmonitor2uls -s ULS -t Test-isAlives /srv/uls/ulsmeld/do_ulsmeld_isalive test_isalive >/dev/null 2>&1
#
# Limits testen
3-58/5 * * * * uls /usr/local/bin/jobmonitor2uls -s ULS -t Test-Limits /srv/uls/ulsmeld/do_ulsmeld test_limits test_kombilimits >/dev/null 2>&1
#
# Kombi-Limits testen
4-59/5 * * * * uls /usr/bin/jobmonitor2uls -s ULS -t Test-Kombilimits /srv/uls/ulsmeld/do_ulsmeld test_kombilimits >/dev/null 2>&1
#
# Mailreports versenden
4 * * * * uls /usr/local/bin/jobmonitor2uls -s ULS -t Jobs:Mailreports /srv/uls/jobs/send_mailreports >/dev/null 2>&1
#
# Komprimierung
1 5 * * * uls /usr/local/bin/jobmonitor2uls -s ULS -t Jobs:Compression /srv/uls/jobs/komprimierung >/dev/null 2>&1
```

Verify, that the test_limits is executed at 3-58/5! If not, calculations cannot be compared to the values in thuis guide.

The ULS-server always marks the last checked value in its database to continue with the next execution of the test scripts. 
So you need to reset these marks before starting the test scripts (or wait until it is executed by cron).

```
$ /srv/uls/ulsmeld/do_ulsmeld -f test_limits
$ /srv/uls/ulsmeld/do_ulsmeld -f test_kombilimits
$ /srv/uls/ulsmeld/do_ulsmeld_isalive test_isalive
```

That will perhaps run for a long time depending on the number of meanwhile unprocessed values!

Clear the log file for the notification messages:

```
$ echo > /tmp/ulsserver.meldungen.out
$ chmod 664 /tmp/ulsserver.meldungen.out
```
Disable ULS test cron file:

```
# uvi /etc/cron.d/uls_testsuite
```
and disable all entries.

Check for test e-mail processing, ULSTESTMELDFILE should be activated:
```
# vi /etc/ulsserver.conf
…
ULSTESTMELDFILE=/tmp/ulsserver.meldungen.out
```
Remember that when checking the e-mail test for notification destinations.
You have to look into that file to find the e-mail messages.

