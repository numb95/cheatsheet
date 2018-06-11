# Nginx

### install

`sudo apt install nginx`

### Config as a load balancer

#### The default method of nginx in round-robin. 

* edit  `/etc/nginx/sites-available/VHOST` and add this to the file. you can also remove other lines

```bash
upstream servers_address  {
  least_conn; #for least connected method
  ip_hash; #for hp_hash method
  server server1_ip_or_url weight=1 max_fails=4 fail_timeout=30;
  server server2_ip_or_url weight=2;
  server server3_ip_or_url weight=4;
  server server4_ip_or_url weight=8;
}
```

* then add:

```text
server {
  listen 80;
  location / {
    proxy_pass  http://servers_address;
    proxy_set_header HOST $host;
    
  }
}
```

* restart nginx 

{% hint style="info" %}
default weight is 1. wight 2 will be sent twice traffic as much weight 1. Also wight 8 sent 8 times traffic to a specific server as much weight 1. This sample will also work on weight 2 or 3
{% endhint %}

{% hint style="info" %}
**ip\_hash** is a method that respond to the clients based on their IP addresses. It's beside **round-robin** and **least connected.**
{% endhint %}

{% hint style="info" %}
**max\_fails** is a maximum amount of attempts that nginx send data to server if down. **fail\_timeout** is it's timeout :\)\)
{% endhint %}

{% hint style="info" %}
**least connected** is the method that nginx won't route traffic to a busy server.
{% endhint %}

