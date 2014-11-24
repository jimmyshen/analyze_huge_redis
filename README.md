What is it?
-----------

Analyzes huge Redis instances given a 'redis URL'.

Usage
-----------

```
analyze_huge_redis [-h] [--mode {scan,random}] [--rules RULES]
                          [--limit LIMIT] [--account] [--heartbeat HEARTBEAT]
                          redis_url

positional arguments:
  redis_url             Redis URL

optional arguments:
  -h, --help            show this help message and exit
  --mode {scan,random}  Choose key scan method (default: scan)
  --rules RULES         Rules JSON file
  --limit LIMIT         Limit number of keys to scan
  --account             Account # of bytes used (DEBUG OBJECT must be
                        available)
  --heartbeat HEARTBEAT
                        Print to stdout every...
```
