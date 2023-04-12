---
description: 用户可以单独将一些代理放入特定文件中,通过引用该文件,用户可以快速将这些相同的代理填充到不同的策略组中
---

# 代理集合

## 示例

```
proxy-providers:
  provider1:
    type: http
    url: ""
    path: ./proxy_providers/provider1.yaml
    interval: 3600
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300

  provider2:
    type: file
    path: ./proxy_providers/provider2.yaml
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300
```

### name

如`provider1`,为provider的name,name不能重复

### type

provider类型,可选`http/file`

### url

类型为`http`是则需要配置

### path

文件路径,不可重复

### interval

更新provider的时间,单位为秒

### health-check

健康检查(测试延迟)

#### enable

是否启用,可选 `true/false`

#### url

健康检查地址,推荐为204地址,以获得更准确的延迟

#### interval

健康检查间隔时间,单位为秒
