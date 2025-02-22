# BASHtail

`bashtail-bin` script allows you to send logs in JSON format to Loki instance, archive old data (sent logs) and delete too old data (retention).

Example usage:

  ```bash
  ./bashtail-bin --lokiurl=http://promgraf.lmao.com --filename=/var/log/nginx/access.log --instance=$(hostname -s)
  ```
  where nginx log config looks like this:
  ```bash
  log_format geoip escape=none '{"time_iso8601": "$time_iso8601","ip": "$remote_addr",'
                               '"domain": "$http_host","request": "$request",'
                               '"country_code": "$geoip2_data_country_code",'
                               '"country_name": "$geoip2_data_country_name",'
                               '"city": "$geoip2_data_city_name"}';
  ```

## Installation

### script only

`bashtail-bin` is absolutely standalone, so you can place it anywhere you can. To make it work, you'll need some packages. 
* Debian/Ubuntu/etc..
  ```bash
  apt -y update 
  apt -y install coreutils findutils curl tar gzip
  ```

### systemd

`systemd` service with timer that runs approximately every 30s is avaible in [usr/lib/systemd/system](https://github.com/lukasbalonek/bashtail/blob/main/usr/lib/systemd/system)

* Install packages required.
  * Debian/Ubuntu/etc..
    ```bash
    apt -y update 
    apt -y install coreutils findutils curl tar gzip
    ```
* Copy
  * `etc/default/bashtail` to `/etc/default`
  * `usr/local/sbin/{bashtail,bashtail-bin}` to `/usr/local/sbin`
  * `usr/lib/systemd/system/bashtail.{service,timer}` to `/usr/lib/systemd/system/`
* Enable and start timer:
  ```bash
  systemctl enable --now bashtail.timer
  ```
  > 💡Check status with `service bashtail status`, `systemctl status bashtail` or `journalctl -f -u bashtail`