# University of Bristol specific notes on migrating

## Export old DB from previous production host:
```
sudo -u netreg pg_dump -oO -E UTF8 sauron > sauron_073_`date +%Y-%m-%d`.sql
iconv -f latin1 -t utf-8//TRANSLIT sauron_073_`date +%Y-%m-%d`.sql > sauron_073_`date +%Y-%m-%d`-utf8.sql
```

Then copy the dump file over to the new production host

## Import DB snapshot to new procduction host:
```
# drop the existing postgres DB from service validation phase
sudo -u postgres dropdb sauron

# create an empty DB
sudo -u postgres createdb -O sauron sauron

# Import the dump file
sudo -u sauron psql sauron < sauron_073_`date +%Y-%m-%d`-utf8.sql

# Upgrade the sauron DB schema
sudo -u sauron psql sauron < src/dbconvert_1.4to1.5
```

Puppet will take care of the rest...
