# k8s-intro

[k8slight.xyz](https://k8slight.xyz)

## Fix apt

```sh
sed "s/\(deb\|security\).debian/archive.debian/;/stretch-updates/d" -i /etc/apt/sources.list
apt update && apt install -y --no-install-recommends nano procps vim
```

## Modify and restart nginx

```sh
nano /etc/nginx/conf.d/default.conf
nginx -s reload
```

## Show pod name on website

```sh
sed -i "s|/usr/share/nginx/html;|/usr/share/nginx/html2;|" /etc/nginx/conf.d/default.conf 
mkdir -p /usr/share/nginx/html2 && echo "$HOSTNAME" > /usr/share/nginx/html2/index.html
nginx -s reload
```

## Kill main process of container

```sh
kill 1
```
