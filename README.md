# APK Fastest Mirror
*A Pure BusyBox APK mirror selector for Alpine Linux*

MIT License (See: https://github.com/padthaitofuhot/apkfastestmirror)

## Installation
```text
alpine:~# wget https://raw.githubusercontent.com/padthaitofuhot/apkfastestmirror/master/apkfastestmirror.sh
alpine:~# ash ./apkfastestmirror.sh --install
Installing to /usr/local/bin/apkfastestmirror
Generating /etc/apk/apkfastestmirror.conf
alpine:~#
```

## Usage
```text
Usage
-h, --help        Show usage and exit
-q, --quiet       Be quiet - only print results
    --shut-up     Be more quiet - print nothing
-v, --verbose     Be verbose - print progress (default)
    --http-only   Only use HTTP checks to determine mirror performance
    --icmp-only   Only use ICMP checks to determine mirror performance
-s, --samples <n> Test performance n times before selecting mirror
                      the default is two samples.
-r, --replace     Replace /etc/apk/repositories with fastest mirror
-u, --update      Update APK indexes (default, useful with --replace)
    --genconf     Create /etc/apk/fastestmirror.conf if it does not
                  already exist and exit
    --install     Install as /usr/local/bin/apkfastestmirror and exit
```

## Configuration
All variables that can be set in the script header can also be set in the config file: `/etc/apk/apkfastestmirror.conf`

##### Behavior Variables

| Variable  | Default | Description
| :--- | :---: | :--- |
| `replace_apk_repositories` | true | Replace /etc/apk/repositories with output |
| `update_indexes_after_replace` | false | (with `--replace`) Also update APK indexes |
| `verbose` | true | Be verbose (and get a progress bar) |
| `quiet` | false | Be quiet - only print results |
| `shut_up` | false | Be more quiet - print nothing |

##### Mirror Variables

| Variable  | Default | Description
| :--- | :---: | :--- |
| `mirrors_custom` | *-* | Your custom HTTP mirrors, perhaps local mirrors |
| `mirrors_published` | *see script* | Published mirrors that are usually available

##### Measurement Variables

| Variable  | Default | Description
| :--- | :---: | :--- |
| `http_only` | false | Got firewall problems? Maybe these can help. |
| `icmp_only` | false | |
| `sampling_rounds` | 2 | Sampling rounds before selecting mirror
| `icmp_count` | 6 | Number of ICMP pings to send |
| `http_timeout` | 1 | HTTP timeout in seconds (useful on laggy links)
| `http_use_proxy` | on |  "on" or "off"; defaults to "on", but does nothing unless the $http_proxy environment variable is set |

### Variable Precedence

*command line > config file > script header*

If the config file exists, its variables override the script. If arguments are given on the command line, they override the config file.

## Regarding Proxies

When using proxies, you will want to set `sampling_rounds` to something > 1 to prime the proxy's cache. If you do not, a suboptimal mirror may be selected if, for example, the proxy gets a cache hit on a slower mirror and a cache miss on the mirror apkfastestmirror would have otherwise selected.
