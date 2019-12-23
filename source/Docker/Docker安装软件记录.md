## Docker安装软件记录

### 安装redis

```sh
docker run -p 6379:6379 -v /home/redis/data:/data --restart=always -d redis redis-server --appendonly yes 
```

