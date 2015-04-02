# multisource-replication plugin
This plugin allows you to use Percona's tool pt-table-checksum to check consistency for MariaDB multi-source replicated databases.

Usage: `PTP_CONNECTION_NAME='xxx' pt-table-checksum --plugin multisource-replication ...`

#### Requirements
   * pt-table-checksum v2.2.8 or above.
   * `PTP_CONNECTION_NAME` environment variable must be set according to which databases you are checking, so that correct replication connection latency is checked.

#### How does it works
`pt-table-checksum` use `SHOW SLAVE STATUS` to check slave's latency, this plugin will adapt SHOW SLAVE STATUS syntax to support MariaDB multi-source replication if required.

When checking consistency on a replica that use multi-source replication you have to be careful that checksum statements and DML statements are going through same replication stream otherwise table's data won't be in same point in time if replication is lagging, therefore checksum result will not be accurate.
That being said, you should be fine if you use row-based replication and do not use --replicate-database option, that's because "pt-table-checksum executes USE to select the database that contains the table it’s currently working on".

#### TODO
   * Evaluate do/ignore db/table replication rules to determine which replication connection latency has to be checked so that Replication Connection Name isn't required as argument and that when checking databases from multiple connection correct replication stream latency is monitored.

