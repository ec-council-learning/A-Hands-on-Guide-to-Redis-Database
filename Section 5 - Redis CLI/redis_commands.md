# Redis CLI Cheat Sheet

## Modes of Redis CLI

### Interactive Mode
- Default mode when running `redis-cli` without arguments.
- Allows typing and executing commands directly.
- Enter interactive mode:
  ```sh
  redis-cli
  ```

### Non-Interactive Mode
- Executes commands from a file.
- Example file (`commands.txt`):
  ```
  SET key1 value1
  SET key2 value2
  GET key1
  GET key2
  DEL key1
  DEL key2
  ```
- Run commands in non-interactive mode:
  ```sh
  redis-cli < commands.txt
  ```

### Raw Output Mode
- Displays output in machine-readable format.
- Use `--raw` flag:
  ```sh
  redis-cli --raw
  ```
- Example:
  ```sh
  redis-cli SET mykey 5
  redis-cli INCRBY mykey 5
  ```

### Monitor Mode
- Shows real-time commands executed on the Redis server.
- Run in one terminal:
  ```sh
  redis-cli monitor
  ```
- Execute commands in another terminal to see the output.

---

## Connecting to Redis

### Specifying Host and Port
- Default: `127.0.0.1:6379`
- Custom host/port:
  ```sh
  redis-cli -h 192.168.1.100 -p 6380
  ```

### Authentication (Password)
- If authentication is required:
  ```sh
  redis-cli -h 192.168.1.100 -p 6380 -a mypassword
  ```

### Selecting a Database
- Default database is `DB 0`.
- Switch to database `2` interactively:
  ```sh
  SELECT 2
  ```
- Connect to database `2` directly:
  ```sh
  redis-cli -n 2
  ```
- Check databases with keys:
  ```sh
  redis-cli INFO keyspace
  ```

---

## Why Redis Works This Way
- **Simplicity & Performance:** Focuses on speed and efficiency.
- **No Schema or Setup:** Uses lightweight logical partitions.
- **Fixed Number of Databases:** Pre-defined in `redis.conf`.

---

## Secure Connections (SSL/TLS)
- If Redis is configured with SSL:
  ```sh
  redis-cli -h redis.example.com -p 6379 --tls
  ```

---

## Redis CLI with Cluster Mode
- Connect to a Redis cluster:
  ```sh
  redis-cli -h redis-cluster-host -p 7000 -c
  ```
- The CLI automatically redirects based on key's hash slot.

---
