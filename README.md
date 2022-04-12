# gabitabi

A basic [gabi](https://github.com/app-sre/gabi) client that can output CSV or JSON.

## Installation

Download and copy `gabitabi` to somewhere in your `$PATH`.

## Usage

```
usage: gabitabi [-h] [-u GABIURL] [-t TOKEN] [-q QUERY] [-Q QUERYFILE] [-o FILE] [--sep SEP] [--no-csv] [--ping] [--debug]

gabitabi: Submit a SQL query to gabi and format the response as CSV.

optional arguments:
  -h, --help            show this help message and exit
  -u GABIURL, --url GABIURL
                        Gabi instance URL. Can also use env GABI_URL
  -t TOKEN, --token TOKEN
                        Openshift console token. Can also use env OCP_CONSOLE_TOKEN
  -q QUERY, --query QUERY
                        SQL query to execute. If '-', will read from STDIN
  -Q QUERYFILE, --query-file QUERYFILE
                        Process query from file. (Overrides -q)
  -o FILE, --output FILE
                        Output file name. If omitted, will use STDOUT
  --sep SEP             Output field separator. Default is TAB.
  --no-csv              Do not convert to CSV.
  --ping                Detect if gabi is available and exit.
  --debug               Set logging level to DEBUG.
```

## Examples

### Basic usage

```bash
$> gabitabi -t MYBIGBADTOKEN -u "https://gabi.coolserver.eek.nope" -o important_stuff.csv -q "select current_timestamp;"
```

### Change CSV delimiter to a comma

```bash
$> gabitabi -t MYBIGBADTOKEN -u "https://gabi.coolserver.eek.nope" -o important_stuff.csv --sep "," -q "select current_timestamp;"
```

### Environment Variables

```bash
export OCP_CONSOLE_TOKEN=MYBIGBADTOKEN
export GABI_URL="https://gabi.coolserver.eek.nope"

$> gabitabi -o important_stuff.csv -q "select current_timestamp;"
```

### Pipe output

```bash
# Set those environment variables!

$> gabitabi  -q "select x from generate_series(1, 100) x;" | less
```

### Read query from STDIN

```bash
# Set those environment variables!

$> cat <<EOF | gabitabi -q - -o all_my_tables.csv
select table_schema,
       table_name
  from information_schema.tables
 where table_schema not in ('pg_catalog', 'information_schema')
 order
    by table_schema,
       table_name
;
EOF
```

### Read query from file

```bash
# Set those environment variables!

$> echo "select 1;" >/tmp/eek.sql
$> gabitabi -Q /tmp/eek.sql -o blah.csv
```

### No CSV Formatting

```bash
# Set those environment variables!

$> cat <<EOF | gabitabi --no-csv -q - | jq
select table_schema,
       table_name
  from information_schema.tables
 where table_schema not in ('pg_catalog', 'information_schema')
 order
    by table_schema,
       table_name
;
EOF
```
