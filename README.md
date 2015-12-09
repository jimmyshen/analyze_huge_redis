What is it?
-----------

This script samples a Redis keyspace in order to converge on rough estimates for the number of keys or the space usage of certain keys. It assumes no knowledge of key naming by letting the user define key groupings using regular expressions.

Basic Usage
-----------

```
usage: analyze_huge_redis [-h] [--mode {scan,random}] [--rules RULES]
                          [--limit LIMIT] [--account] [--use_debug_object]
                          [--heartbeat HEARTBEAT] [--sample SAMPLE]
                          redis_url

Analyzes keyspace of huge Redis databases

positional arguments:
  redis_url             Redis URL

optional arguments:
  -h, --help            show this help message and exit
  --mode {scan,random}  Choose key scan method (default: scan)
  --rules RULES         Rules JSON file
  --limit LIMIT         Limit number of keys to scan
  --account             Account # of bytes used
  --use_debug_object    Account for bytes using DEBUG OBJECT
  --heartbeat HEARTBEAT
                        Print to stdout every...
  --sample SAMPLE       Show samples of N items for each group
```

Rules JSON Format
-----------------

You can specify a set of "rules" for the analyzer to determine how to aggregate keys. For example, if you have a
set of keys that logically define a "Users" model and another set of keys that define an "Articles" model, you could
set up your rules like so:

```json
[
  {
    "label": "Users",
    "pattern": "^users:\\d+"
  },
  {
    "label": "Articles",
    "pattern": "^articles:\\d+"
  },
  {
    "label": "Other",
    "pattern": ".*"
  }
]
```

The "pattern" is a regular expression that is used to match against a key. Patterns are tried in succession until
the first match.

Memory Use Accounting
---------------------

If you need to determine the amount of memory consumed by a certain type of key, you can use the --account option.
By default, this will call `DUMP` on each sampled key to roughly estimate the size of data in bytes. You can
also opt to use `DEBUG OBJECT` (`--use_debug_object`) to accomplish this, though you will find that in most production deployments of Redis that `DEBUG OBJECT` is explicitly disabled.

Performance
-----------

While the script attempts to reduce RTT to the redis server as much as possible by pipelining commands, it is best to run the script near or on the same host as the Redis server.

Alternatives
------------

- [redis-rdb-tools](https://github.com/sripathikrishnan/redis-rdb-tools) : Offline key analysis of RDB files
- [redis-audit](https://github.com/snmaynard/redis-audit) : A similar script for Ruby
