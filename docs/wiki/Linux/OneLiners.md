# One LIners

List of oneliners

## Linux
Collection of oneliners, stolen from: https://gist.github.com/mrjk/93b923ba97b71ca4226ac13048318685

### Docker

List all containers and their IPs
```
docker ps -q | xargs -n 1 docker inspect --format '{{ .Name }} {{range .NetworkSettings.Networks}} {{.IPAddress}}{{end}}' | sed 's#^/##';
```

### Networking

Webservers
```
python3 -m http.server 8000
python2 -m SimpleHTTPServer 8000
php -S 127.0.0.1:8000
busybox httpd -f -p 8000
ruby -run -ehttpd . -p8000

# TCP connection
nc ...
```

### Converters

yaml2json
```
python3 -c 'import sys, yaml; yaml.dump(yaml.safe_load(sys.stdin), sys.stdout)'
```

## Ideas/TODO:
See: https://www.digitalocean.com/community/tutorials/how-to-use-netcat-to-establish-and-test-tcp-and-udp-connections
