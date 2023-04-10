---
description: clash的dns模块配置
---

# DNS

## 示例

以下的拆分说明皆是 dns: 下的配置项

```yaml
dns:
  enable: true
  prefer-h3: true
  listen: 0.0.0.0:1053
  ipv6: true
  default-nameserver:
    - 114.114.114.114
    - 8.8.8.8
    - tls://1.12.12.12:853
    - tls://223.5.5.5:853
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  fake-ip-filter:
    - '*.lan'
    - localhost.ptlogin2.qq.com
  nameserver-policy:
    'www.baidu.com': '114.114.114.114'
    '+.internal.crop.com': '10.0.0.1'
    'geosite:cn': https://doh.pub/dns-query
  nameserver:
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
  fallback:
    - tls://8.8.4.4
    - tls://1.1.1.1
  proxy-server-nameserver:
    - https://doh.pub/dns-query
  fallback-filter:
    geoip: true
    geoip-code: CN
    geosite:
      - gfw
    ipcidr:
      - 240.0.0.0/4
    domain:
      - '+.google.com'
      - '+.facebook.com'
      - '+.youtube.com'
```

### enable

可选值 `true/false`

是否启用,如为false，则使用系统dns解析

```yaml
dns:
  enable: true
```

### prefer-h3

可选值 `true/false`

是否开启DOH的http/3

```yaml
prefer-h3: true
```

### listen

dns服务监听

```yaml
listen: 0.0.0.0:1053
```

### IPV6

可选值 `true/false`

是否解析IPV6,如为false,则回应AAAA的空解析

```yaml
ipv6: true
```

### enhanced-mode

可选值 `fake-ip / redir-host`

clash的dns处理模式

```yaml
enhanced-mode: fake-ip
```

### fake-ip-range

格式为 `ip/掩码`

fakeip下的IP段设置, tun网卡的默认ip也使用此值

```yaml
fake-ip-range: 198.18.0.1/16
```

### fake-ip-filter

fakeip过滤,以下地址不会下发fakeip映射用于连接

```yaml
fake-ip-filter:
  - '*.lan'
  - localhost.ptlogin2.qq.com
```

### use-hosts

可选值 `true/false`

是否查询系统hosts

```yaml
use-hosts: true
```

### default-nameserver

默认dns,用于解析 DNS服务器 的域名

必须为ip,可为加密dns

```yaml
default-nameserver:
  - 114.114.114.114
  - 8.8.8.8
  - tls://223.5.5.5:853
  - https://223.5.5.5/dns-query
```

### nameserver-policy

指定域名查询的解析服务器,可使用 geosite,优先于`nameserver/fallback 查询`

可书写多个,并发查询,以下仅作为书写演示,建议根据自己需求写

<pre class="language-yaml"><code class="lang-yaml"><strong>nameserver-policy:
</strong>  'www.baidu.com': '114.114.114.114'
  '+.internal.crop.com': '10.0.0.1'
  'geosite:cn': https://doh.pub/dns-query
  'geosite:geolocation-!cn': [tls://8.8.4.4, https://1.0.0.1/dns-query]
  "www.baidu.com,+.google.cn": [https://doh.pub/dns-query, https://dns.alidns.com/dns-query]
  "geosite:private,apple": [https://doh.pub/dns-query, https://dns.alidns.com/dns-query]
</code></pre>

{% hint style="info" %}
`使用geosite时,应当使用引号包裹`
{% endhint %}

### nameserver

默认的域名解析服务器,如不配置 `fallback/proxy-server-nameserver` ,则所有域名都由nameserver解析

```yaml
nameserver:
  - tls://dot.pub
  - https://dns.alidns.com/dns-query
```

### proxy-server-nameserver

代理节点域名解析服务器,仅用于解析代理节点的域名

```yaml
proxy-server-nameserver:
 - https://doh.pub/dns-query
```

### fallback

后备域名解析服务器,一般情况下使用境外DNS,保证结果可信

配置`fallback`后默认启用`fallback-filter`,`geoip-code`为cn

```yaml
fallback:
  - tls://dns.google
  - https://1.0.0.1/dns-query
```

### fallback-filter

后备域名解析服务器筛选,满足条件的将使用`fallback`结果或只使用`fallback`解析

#### geoip

可选值为 `true/false`

是否启用fallback filter

#### geoip-code

可选值为 国家缩写,默认值为 `CN`

除了 `geoip-code` 配置的国家IP,其他的IP结果会被视为污染

`geoip-code` 配置的国家的结果会直接采用,否则将采用`fallback`结果

#### geosite

可选值为对于的geosite内包含的集合

geosite列表的内容被视为已污染,匹配到geosite的域名,将只使用`fallback`解析,不去使用`nameserver`

#### ipcidr

书写内容为 `IP/掩码`

这些网段的结果会被视为污染,`nameserver`解析出这些结果时将会采用`fallback`的解析结果

#### domain

这些域名被视为已污染,匹配到这些域名,会直接使用`fallback`解析,不去使用`nameserver`

```yaml
fallback-filter:
    geoip: true
    geoip-code: CN
    geosite:
      - gfw
    ipcidr:
      - 240.0.0.0/4
    domain:
      - '+.google.com'
      - '+.facebook.com'
      - '+.youtube.com'
```

## 部分特殊写法

此部分可用于所有的dns服务器

### dns经过代理查询

书写格式为dns服务器后 `#策略组或节点`

书写规范应带引号,以防出现特殊字符,如需过代理查询,应配置`proxy-server-nameserver`,以防出现鸡蛋问题

```yaml
nameserver:
  - 'tls://dns.google#proxy'
```

### 强制http/3

强制gttp/3,与`perfer-h3`无关,强制开启doh的http/3支持,若不支持将无法使用

```yaml
nameserver:
  - 'tls://dot.pub#h3=true'
```

### 指定DNS出口网卡

```yaml
nameserver:
  - 'tls://8.8.4.4#en0'
```

### 指定策略组和使用 http/3

```yaml
nameserver:
  - 'https://mozilla.cloudflare-dns.com/dns-query#proxy&h3=true'
```
