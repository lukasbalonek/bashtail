# BASHtail

This script allows you to send logs in JSON format ta Loki instance.

Example usage:

  ```bash
  ./bashtail --lokiurl=http://promgraf.lmao.com --filename=/var/log/nginx/access.log --instance=$(hostname -s)
  ```
  where nginx log config looks like this:
  ```bash
  log_format geoip escape=none '{"time_iso8601": "$time_iso8601","ip": "$remote_addr",'
                               '"domain": "$http_host","request": "$request",'
                               '"country_code": "$geoip2_data_country_code",'
                               '"country_name": "$geoip2_data_country_name",'
                               '"city": "$geoip2_data_city_name"}';
  ```
