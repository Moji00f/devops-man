```
pmm-admin list

pmm-admin status

pmm-admin summary

systemctl status pmm-agent

pmm-admin check-network
```
```
sudo pmm-admin add mysql --username=pmm --password=*** --host=mydb --port=3306 --query-source=perfschema
```
with token PMM --> PMM ADD INSTANCE
```
sudo pmm-admin config --server-insecure-tls --server-url=https://192.168.1.100:8443 --server-token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

```
version: '3'

volumes:
  pmm-data:
    driver: local

services:
  pmm-server:
    image: percona/pmm-server:latest
    container_name: pmm-server
    restart: always
    ports:
      - "8000:80"
      - "8443:443"
    volumes:
      - pmm-data:/srv
    environment:
      - DISABLE_UPDATE=true  # غیرفعال کردن watchtower
      - ENABLE_BACKUP_MANAGEMENT=true
    networks:
      - monitoring-network

  pmm-client:
    image: percona/pmm-client:latest
    container_name: pmm-client
    restart: always
    command:
      - "pmm-admin"
      - "config"
      - "--server-insecure-tls"
      - "--server-url=https://pmm-server:8443"
      - "--server-token=TOKEN_HERE"  # replace your tokekn
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
    depends_on:
      - pmm-server
    networks:
      - monitoring-network
      - mariadb-network  # pmm-client and mariadb have same network
networks:
  monitoring-network:
    driver: bridge
  mariadb-network:
    external: true  # 
```

```
# افزودن با پارامترهای پیشرفته
docker exec pmm-client pmm-admin add mysql \
  --username=pmm \
  --password=your_password \
  --host=mydb \
  --port=3306 \
  --service-name=mariadb-service \
  --query-source=perfschema \
  --tls \
  --tls-skip-verify
```

```
# بررسی اتصال شبکه بین کانتینرها
docker exec pmm-client ping mydb

# مشاهده لاگ‌های real-time
sudo docker logs -f pmm-server

# بررسی سرویس‌های اضافه شده
docker exec pmm-client pmm-admin list

# تست اتصال به PMM Server
curl -k https://localhost:8443/ping

# مشاهده متریک‌های جمع‌آوری شده
docker exec pmm-client pmm-admin show-passwords

# ریست سرویس PMM Agent
docker exec pmm-client pmm-agent restart
```
