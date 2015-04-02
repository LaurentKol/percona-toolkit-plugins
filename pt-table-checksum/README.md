# pt-table-checksum-multisource-replication

This plugin allows you to use Percona's tool pt-table-checksum to check consistency for MariaDB multi-source replicated databases.
This is done by using multi-source replication syntax to check SLAVE STATUS

Usage: PTP_CONNECTION_NAME='xxx' pt-table-checksum --plugin pt-table-checksum-multisource-repl.pl ...

This plugin should work from pt-table-checksum v2.2.8 and above
PTP_CONNECTION_NAME environment variable must be set according to which databases you are checking, so that correct replication connection latency is checked.

To use this plugin properly you should be aware of how pt-table-checksum works, in particular that for a given db/table, checksum statements and DML statements must go through same replication stream otherwise table's data won't be in same point in time if replication is lagging, therefore checksum result will not be accurate.
That being said, you should be fine if you use row-based replication and do not use --replicate-database option, that's because "pt-table-checksum executes USE to select the database that contains the table it’s currently working on".

TODO: Instead a requiring Replication Connection Name to be passed as argument,
      parse do/ignore db/table replication rules to determine which replication connection latency has to be checked.

