# `drop-plans`
This is a Python script will drop plans on SingleStore nodes using a variety of criteria.

## Usage

```
usage: drop-plans [-h] [-u USER] [-H HOST] [-P PORT] [-v] [-d] [-w]
                  [-q QUERY_FILTER]
                  [types ...]

Drop plans on SingleStoreDB nodes.

positional arguments:
  types

optional arguments:
  -h, --help            show this help message and exit
  -u USER, --user USER
  -H HOST, --host HOST
  -P PORT, --port PORT
  -v, --verbose
  -d, --dry-run
  -w, --clear-wasm-cache
  -q QUERY_FILTER, --query-filter QUERY_FILTER
```

The `types` argument specifies which kind of node(s) to affect.  One or more of the following types may be specified.
- `master`: drop plans on master aggregator
- `child`: drop plans on child aggregators
- `leaf`: drop plans on leaf nodes
- `all`: drop plans on all nodes

The `--dry-run` option will perform a simulated execution in which no changes will actually be made to the cluster.

The `--clear-wasm-cache` option will also clear the Wasm module cache on each node.

The `--query-filter` option, if specified, will constrain the dropped plans to some specific query text.  The filter is passed to the `LIKE` keyword with wildcards affixed to either side, like this:  `...query_text LIKE '%filter%'...`.  This option is useful for surgical removal of plans; non-matching plans will be left untouched.

The `--user`, `--host`, and `--port` options specify the target database's username, hostname, and port, respectively.  The script will interactively prompt for the password.

## Examples

Drop all plans on all nodes:
```
drop-plans --user $DBUSER --host $DBHOST --port $DBPORT all
```

Drop all plans on leaf nodes:
```
drop-plans --user $DBUSER --host $DBHOST --port $DBPORT leaf
```

Drop all plans on leaf nodes and clear the Wasm cache:
```
drop-plans --user $DBUSER --host $DBHOST --port $DBPORT --clear-wasm-cache leaf
```

Drop plans whose query text contains the string `my_func` on leaf and child aggregator nodes, but not the master:
```
drop-plans --user $DBUSER --host $DBHOST --port $DBPORT --query-filter my_func leaf child
```

Same as above, but as a dry run only:
```
drop-plans --user $DBUSER --host $DBHOST --port $DBPORT --dry-run --query-filter my_func leaf child
```

