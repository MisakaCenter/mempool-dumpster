This directory contains a bunch of useful scripts.

---

Working with Parquet files:

```bash
# You can query parquet files using clickhouse-local (https://clickhouse.com/docs/en/operations/utilities/clickhouse-local)
clickhouse local -q 'select hash,hex(rawTx) from "transactions.parquet" limit 1;'
```

More helpers:

```bash
# source stats - all tx
journalctl -u mempool-collector -o cat --since "10m ago" | grep "source_stats_all" | awk '{ $1=""; $2=""; $3=""; print $0}' | jq

# source stats - only specific sources
journalctl -u mempool-collector -o cat --since "10m ago" | grep "source_stats_all" | awk '{ $1=""; $2=""; $3=""; print $0}' | jq '.local + "   " + .apool'

# source stats - tx first
journalctl -u mempool-collector -o cat --since "1h ago" | grep "source_stats_first" | awk '{ $1=""; $2=""; $3=""; print $0}' | jq
```

CSV tricks

```bash
# get unique hashes from sourcelog or main csv file
cat file.csv | sed 's/,/ /g' | awk '{ print $2}' | sort | uniq > unique.txt

# who sent trash?
cat /mnt/data/mempool-dumpster/2023-09-13/trash/*.csv | sed 's/,/ /g' | awk '{ print $3}' | sort | uniq -c
```
