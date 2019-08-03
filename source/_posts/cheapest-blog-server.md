---
title: "Cheapest Cloud Server for blog website(2015.10)"
date: "2015-10-22T15:31:00+08:00"
slug: cheapest-blog-server
---

I write tech blog and I just need a cheapest linux server. Here's my requirements:

* linux
* network band (not too slow to support ssh visit and small web static files fetching)
* (optional) ip address
* (optional) not in china (china web servers needs ICP cert), if not in china, not blocked by china network firewall

1 USD to 6.35 RMB

Service Provider(A-Z) | Min price (per 30d) | Spec(CPU core/Memory/Disk)  |  I like it  | I don't like it | link |
---|---|---|---|---|---|
aws(t2.micro)| $9.36 | 1/1G/8G | free 12 months (credit card needed)|float ip| [price](https://aws.amazon.com/ec2/pricing/) |
digitalocean| $5 | 1/0.5G/20G | ssd disk |1T trasnfer|[price](https://www.digitalocean.com/pricing/)
google cloud(f1-micro)| $4.09 | 1/0.6G/- | no transfer limit, free $300 for 60 days  ||[price](https://cloud.google.com/products/calculator/#id=25f893d9-8425-41b1-95eb-7fdc453056e9)
linode(1GB)| $10 | 1/1G/24G | ssd disk | 2T trasnfer |[price](https://www.linode.com/pricing)
rackspace(General1-1)| $27 | 1/1G/20G | ssd disk ||[price](http://www.rackspace.com/cloud/public-pricing)
microsoft(A0)| $14.4 | 1/0.75G/19G |||[price](https://azure.microsoft.com/en-us/pricing/details/cloud-services/)
阿里云（china青岛）| ￥49.5 | 1/0.5G/20G ||float ip|[price](https://ecs-buy.aliyun.com/#/prepay)
阿里云（us）| ￥75 | 1/1G/20G ||float ip|[price](https://ecs-buy.aliyun.com/#/prepay)
美团云(china)| ￥68.8 | 1/1G/25G ||float ip|[price](https://mos.meituan.com/buy/)
腾讯（china）| ￥65 | 1/1G/8G |static ip||[price](http://manage.qcloud.com/shoppingcart/shop.php?tab=cvm)
腾讯（us）| ￥75 | 1/1G/8G |static ip||[price](http://manage.qcloud.com/shoppingcart/shop.php?tab=cvm)

So i'll choose between google and digital occean.
