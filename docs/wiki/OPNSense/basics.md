# Basics setups


## Unbound



## Local domain management

Many cases:
* Use unbound
  * To assign DNS zones to IPs (A/CNAME records)
  * To assign authoritatives DNS zones (SOA/NS records ?)
* Use dnsmasq
  * Todo

### Hosts overrides

Go to: `Services > Unbound > Overrides`, in the Hosts overrides:

| Host | Domain    | Type | Value         | Description |   |
|------|-----------|------|---------------|-------------|---|
| *    | infra.dev | A    | 192.168.42.10 |             |   |
| *    | apps.dev  | A    | 192.168.42.11 |             |   |
| *    | apps.pro  | A    | 192.168.42.11 |             |   |


### Domains overrides
Go to: `Services > Unbound > Overrides`, in the Hosts overrides:

| Domain     | Ip            | Description |   |
|------------|---------------|-------------|---|
| infra.apps | 192.168.42.10 |             |   |
| k3s.apps   | 192.168.42.10 |             |   |
| k8s.apps   | 192.168.42.10 |             |   |

### DNSmasq config

TODO
