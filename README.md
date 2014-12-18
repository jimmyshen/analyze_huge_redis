What is it?
-----------

Analyzes huge Redis instances given a Redis URL.

Basic Usage
-----------

```
usage: analyze_huge_redis [-h] [--mode {scan,random}] [--rules RULES]
                          [--limit LIMIT] [--account] [--use_debug_object]
                          [--heartbeat HEARTBEAT]
                          redis_url

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
By default, this will call "DUMP" on each key that was scanned to determine length of the data (in bytes). You can
also opt to use DEBUG OBJECT (--use_debug_object) to accomplish this (in some environments this command may
be disabled).

Performance
-----------

While the script attempts to reduce RTT to the redis server as much as possible by pipelining commands, it is best to run the script near or on the same host as the Redis server.
