# migrate-elasticseach
Migrates data from one elasticsearch cluster to another

## usage

`migrate-elasticseach <source> <destination> [regex of indexes to copy]`

### options

    -v, --verbose    enable debug messages
    -O, --overwrite  erase all documents in an index before copying
    -R, --remove  remove indexes not on source
    -s, --painless script script to run (e.g. for renaming, moving fields)
    -y, --yes  confirm

## examples

Migrate all indexes removing all existing data first

`migrate-elasticseach -RO clusterA clusterB`

Migrate only the daily indexes before October

`migrate-elasticsearch -O clusterA clusterB daily\.2017-(01|02|03|04|05|06|07|08|09|10).*`

Migrate any indexes that do not already exist (and have a least 1 document)

`migrate-elasticsearch clusterA clusterB`

Migrate a context suggester with a script

`migrate-elasticsearch -ROy clusterA clusterB context_index  "ctx._source.payload = ctx._source.suggest.payload; ctx._source.output = ctx._source.suggest.remove(\"output\"); ctx._source.payload = ctx._source.suggest.remove(\"payload\"); ctx._source.suggest.contexts = ctx._source.suggest.remove(\"context\");"`
