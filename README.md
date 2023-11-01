
# gandi-ddns

A dependency-less script for declarative dynamic DNS using the gandi.net API.
This is a fork designed to be used in FreeBSD 12+ jails.

**Requires**: Python 3.9+


## Example configuration

The following locations will be searched for a configuration file (or you can 
specify the `--conf` argument):

- environment variable `GANDI_DDNS_CONFIG`
- ~/.config/gandi-ddns.ini
- /usr/local/etc/gandi-ddns/config.ini

```ini

[default]

# Required

wan_device = enp2s0
gandi_api_key = $GANDI_API_KEY

# Optional; called when record changes happen or fail. Takes a single arg, a string
# message. I use pushover.net for this.
notify_script = /usr/local/bin/pushover 

# Each domain has its own section. Within each section, the format is
#
#  [record type], [record name] = [record value], ...
#

[foobar.org]

# blank value will be filled in with running host's IP 
# or result of ipify.org API query for A and AAAA record types.
A, @
A, mail
CNAME, bmon = some-host.lan.

# values are CSVs for separate records with the same name.
MX, @ = 10 one-val.com., 20 other-val.com.

[hmmmm.com]

A, @
```

## Environment variables

- `GANDI_APIKEY`: API key for gandi.net
- `GANDI_DDNS_CONFIG`: path to configuration file

## Install

```sh
% curl $this_url/main.py > ~/.local/bin/gandi-ddns  # or wherever
% chmod +x ~/.local/bin/gandi-ddns

% $EDITOR ~/.config/gandi-ddns.conf

% gandi-ddns
```


Run it on a crontab if you want:
```sh
% cat /etc/cron.d/gandi-ddns

*/15 * * * * your-user /home/your-user/.local/bin/gandi-ddns
```
