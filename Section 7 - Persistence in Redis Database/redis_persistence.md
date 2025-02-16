
# Testing RDB and AOF Persistence Modes in Redis (Local Testing)

---

## Step 1: Prepare the Redis Environment
1. **Start Redis**:  
   Start Redis locally on your system:
   ```bash
   redis-server --save "5 3"
   ```

   This configures Redis to create a snapshot:
   - Every 5 seconds if at least 3 keys are changed.

2. **Access Redis CLI**:  
   ```bash
   redis-cli
   ```

---

## Step 2: Test RDB Snapshot
1. **Set the Snapshot Configuration**:  
   If you didn’t pass the configuration when starting Redis, you can set it dynamically:
   ```bash
   CONFIG SET save "5 3"
   ```

2. **Add Data to Trigger a Snapshot**:  
   - Add 3 keys:
     ```bash
     MSET key1 "value1" key2 "value2" key3 "value3"
     ```
   - Wait for 5 seconds to allow Redis to meet the snapshot condition.

3. **Verify Snapshot Creation**:  
   Check the `dump.rdb` file in the Redis working directory (usually `/var/lib/redis/` or `/usr/local/var/db/redis/`):
   ```bash
   ls -l /path/to/redis/dump.rdb
   ```
   Look for the file's timestamp to confirm that it was updated.

4. **Trigger Another Snapshot**:  
   - Wait 5 seconds after the first snapshot.
   - Add or modify at least 3 keys:
     ```bash
     MSET key4 "value4" key5 "value5" key6 "value6"
     ```
   - Verify the new snapshot:
     ```bash
     ls -l /path/to/redis/dump.rdb
     ```

5. **Manual Save Commands**:  
   Test manual snapshot commands:
   - **Blocking Save**:
     ```bash
     SAVE
     ```
     This will block the Redis server while creating the snapshot.

   - **Non-Blocking Save**:
     ```bash
     BGSAVE
     ```
     This creates the snapshot asynchronously.

6. **Confirm Snapshot Data**:  
   Restart Redis:
   ```bash
   redis-server --save "5 3"
   ```
   Verify that the saved keys are loaded on startup.

---

## Step 3: Test AOF Persistence
1. **Enable AOF**:  
   ```bash
   CONFIG SET appendonly yes
   CONFIG REWRITE
   ```

2. **Add Data to Trigger AOF**:  
   - Add some data:
     ```bash
     MSET keyA "valueA" keyB "valueB" keyC "valueC"
     ```

3. **Verify AOF File**:  
   Check the Redis working directory for the `appendonly.aof` file:
   ```bash
   ls -l /path/to/redis/appendonly.aof
   ```

4. **Test AOF Rewrite**:  
   - Trigger a rewrite to optimize the AOF file:
     ```bash
     BGREWRITEAOF
     ```
   - Verify that the AOF file size is reduced after the rewrite.

5. **Test `appendfsync` Modes**:  
   Change the `appendfsync` behavior to test the performance and durability of writes:
   - No write to disk (only OS writes):
     ```bash
     CONFIG SET appendfsync no
     ```
   - Write to disk every second (default):
     ```bash
     CONFIG SET appendfsync everysec
     ```
   - Write to disk immediately after every command:
     ```bash
     CONFIG SET appendfsync always
     ```

6. **Verify Durability**:  
   Restart Redis and check if the data persists:
   ```bash
   redis-server --appendonly yes
   ```

---

## Step 4: Test Specific Scenarios
1. **Add Data**:  
   Test data persistence by adding or modifying keys:
   ```bash
   SET keyX "valueX"
   SET keyY "valueY"
   ```

2. **Restart Redis**:  
   ```bash
   redis-server --appendonly yes
   ```

3. **Check Persistence**:  
   Verify if the keys are still present:
   ```bash
   GET keyX
   GET keyY
   ```

---

## Step 5: Compare Performance of RDB and AOF
1. **Test Speed and Disk Usage**:  
   - Add a large amount of data to Redis:
     ```bash
     for i in {1..10000}; do redis-cli SET key$i value$i; done
     ```
   - Measure snapshot and write speeds under RDB and AOF modes.

2. **Run Stress Tests**:  
   Use Redis's benchmarking tool:
   ```bash
   redis-benchmark -q -n 10000 -c 50
   ```

3. **Monitor Performance**:  
   Check latency, I/O, and memory usage:
   ```bash
   INFO persistence
   ```

---

## Observations
- **RDB**:  
  - Lower disk usage.  
  - Faster load times but may lose data since the last snapshot.

- **AOF**:  
  - Greater durability but higher disk usage.  
  - Slower write performance depending on `appendfsync`.

