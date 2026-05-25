---
type: CarCharge
_organized: true
---
# Domain and Private IP

Domain: [ingeacarcharge.biz](http://ingeacarcharge.biz)\
\
Purchased in dynadot

```text
fly ips list -a car-charge-web-staging

 VERSION ? IP                     ? TYPE                              ? REGION ? CREATED AT        
 v4      ? 168.220.81.92          ? public ingress (dedicated, $2/mo) ? global ? Apr 22 2026 22:08 
 v6      ? 2a09:8280:1::fb:ee09:0 ? public ingress (dedicated)        ? global ? Apr 8 2026 22:34  
 v4      ? 66.241.124.76          ? public ingress (shared)           ?        ? Jan 1 0001 00:00  

Learn more about Fly.io public, private, shared and dedicated IP addresses in our docs: https://fly.io/docs/networking/services/
```

Domain: [carchargeapp.com](https://carchargeapp.com)

Purchased in dynadot

```text
fly ips list -a uni-charge-landing

 VERSION ? IP                      ? TYPE                              ? REGION ? CREATED AT        
 v4      ? 213.188.215.34          ? public ingress (dedicated, $2/mo) ? global ? 1m4s ago          
 v6      ? 2a09:8280:1::10e:65ab:0 ? public ingress (dedicated)        ? global ? Apr 30 2026 16:08 
 v4      ? 66.241.124.133          ? public ingress (shared)           ?        ? Jan 1 0001 00:00  

Learn more about Fly.io public, private, shared and dedicated IP addresses in our docs: https://fly.io/docs/networking/services/
```

```text
ly certs add carchargeapp.com -a uni-charge-landing



✓ Certificate created for carchargeapp.com

Recommended DNS setup:
  A    carchargeapp.com → 213.188.215.34
  AAAA carchargeapp.com → 2a09:8280:1::10e:65ab:0

Tip: Run fly certs add www.carchargeapp.com to cover the www subdomain.

Run fly certs check carchargeapp.com to check validation progress.
Run fly certs setup carchargeapp.com for alternative DNS setups.
```

```text
fly certs add www.carchargeapp.com -a uni-charge-landing

✓ Certificate created for www.carchargeapp.com

Recommended DNS setup:
  A    www.carchargeapp.com → 213.188.215.34
  AAAA www.carchargeapp.com → 2a09:8280:1::10e:65ab:0

Run fly certs check www.carchargeapp.com to check validation progress.
Run fly certs setup www.carchargeapp.com for alternative DNS setups.
```
