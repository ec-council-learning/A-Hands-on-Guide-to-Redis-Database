# Redis Replication and High Availability Commands

## Start Primary Redis Server
```sh
redis-server primary.conf
```

## Start Replica Redis Servers
```sh
redis-server ./replica.conf
```

## Connect to Redis Instances
```sh
redis-cli  # Connect to primary
redis-cli -p 6380  # Connect to replica
```

## Monitor Replica Server
```sh
MONITOR
```

## Execute Write Command on Primary
```sh
127.0.0.1:6379> SET foo bar
```

## Start Redis Sentinel Instances
```sh
redis-server ./sentinel1.conf --sentinel
redis-server ./sentinel2.conf --sentinel
redis-server ./sentinel3.conf --sentinel
```

## Simulate Primary Server Failure
```sh
Ctrl + C  # Stop primary Redis instance
```

## Validate New Primary
```sh
redis-cli -p 6380 INFO replication
```
