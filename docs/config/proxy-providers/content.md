proxy-providers:
  vless_reality_grpc:
    type: http
    interval: 3600
    url: "https://pastebin.com/raw/your-pastebin-link-here"
    path: ./proxies/vless_reality_grpc.yaml
    health-check:
      enable: true
      interval: 600
      url: http://www.gstatic.com/generate_204

proxies:
- name: "1d676878-vless_reality_grpc"
  type: vless
  server: 104.28.251.182
  port: 17034
  uuid: 1d676878-cd95-47d5-9df8-86abe9256867
  network: grpc
  tls: true
  servername: osxapps.itunes.apple.com
  udp: true
  client-fingerprint: chrome
  reality-opts:
    public-key: S4RVSg9WZbjVWILgPb_x2j_A1YV1svCuynlo7XhnxkY
    short-id: 6ba85179e30d4fc2
  grpc-opts:
    grpc-service-name: grpc
