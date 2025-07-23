# Caddy

## Dockerfile
```
root@VM-F5rSUrAn0oze:/opt/build_docker# cat Dockerfile 
FROM caddy:builder AS builder

RUN xcaddy build \
    --with github.com/caddy-dns/dnspod \
    --with github.com/caddy-dns/alidns

FROM caddy:latest

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
```

# Config
```
{
  # globally tencentcloud 
  acme_dns tencentcloud {
    secret_id {env.TENCENTCLOUD_SECRET_ID}
    secret_key {env.TENCENTCLOUD_SECRET_KEY}
  }
  
  # globally alidns 
  acme_dns alidns {
    access_key_id {env.ALIYUN_ACCESS_KEY_ID}
    access_key_secret {env.ALIYUN_ACCESS_KEY_SECRET}
  }
}
dnspod.example.com {
  tls {
    dns tencentcloud {
      secret_id {env.TENCENTCLOUD_SECRET_ID}
      secret_key {env.TENCENTCLOUD_SECRET_KEY}
    }
  }
}

alidns.example.com {
  tls {
      dns alidns {
          # https://ram.console.aliyun.com/manage/ak
          access_key_id       id
          access_key_secret   secret
      }
  }
}
```
