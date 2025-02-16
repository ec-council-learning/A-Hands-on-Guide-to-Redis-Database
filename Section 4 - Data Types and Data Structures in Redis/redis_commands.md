## Redis Commands Cheat Sheet

### Strings
#### SET
```sh
SET language Turkish
GET language
```
#### MSET and MGET
```sh
MSET language Turkish name Suleyman country Turkiye
MGET language name country
```
#### INCR/INCRBY
```sh
SET total_crashes 0
INCR total_crashes
INCRBY total_crashes 10
```

### Lists
#### LPUSH
```sh
LPUSH work:queue:ids 101
LPUSH work:queue:ids 237
```
#### LPOP
```sh
LPUSH work:queue:ids 101
LPUSH work:queue:ids 237
LPOP work:queue:ids
LPOP work:queue:ids
```
#### LLEN
```sh
LLEN work:queue:ids
```
#### LMOVE
```sh
LPUSH board:todo:ids 101
LPUSH board:todo:ids 273
LMOVE board:todo:ids board:in-progress:ids LEFT LEFT
LRANGE board:todo:ids 0 -1
LRANGE board:in-progress:ids 0 -1
```
#### LTRIM
```sh
LPUSH notifications:user:1 "You've got mail!"
LTRIM notifications:user:1 0 99
LPUSH notifications:user:1 "Your package will be delivered at 12:01 today."
LTRIM notifications:user:1 0 99
```

### Sets
#### SADD
```sh
SADD fruits apple orange banana
```
#### SMEMBERS
```sh
SMEMBERS fruits
SISMEMBER fruits apple
SISMEMBER fruits mango
```
#### SINTER
```sh
SADD fruits_set1 apple orange banana
SADD fruits_set2 apple mango kiwi
SADD fruits_set3 apple pineapple watermelon
SINTER fruits_set1 fruits_set2 fruits_set3
```
#### SCARD
```sh
SADD fruits apple orange banana mango
SCARD fruits 
```

### Hashes
#### HSET, HGET
```sh
HSET bike model Deimos brand Ergonom type 'Enduro bikes' price 4972
HGET bike model
HGET bike price
```
#### HMGET
```sh
HMGET bike model price no-such-field
```
#### HINCRBY
```sh
HSET myhash field 5
HINCRBY myhash field 1
HINCRBY myhash field -1
HINCRBY myhash field -10
```

### Sorted Sets
#### ZADD
```sh
ZADD scores 100 John 85 Alice 90 Bob 95 Eve
```
#### ZRANGE
```sh
ZRANGE scores 0 -1
```
#### WITHSCORES option
```sh
ZRANGE scores 0 -1 WITHSCORES
```
#### ZRANK
```sh
ZRANK scores Bob
```
#### ZREVRANK
```sh
ZREVRANK scores Bob
```

### Streams
#### XADD
```sh
XADD user_activity_stream * name "Alice" action "login" timestamp "2023-07-22T10:30:00"
```
#### XLEN
```sh
XLEN user_activity_stream
```
#### XRANGE and XREVRANGE
```sh
XADD user_activity_stream * name "Alice" action "login" timestamp "2023-07-22T10:30:00"
XADD user_activity_stream * name "Bob" action "logout" timestamp "2023-08-22T10:30:00"
XRANGE user_activity_stream 1609411200000-0 1609411200000-5
XRANGE user_activity_stream - +
XRANGE user_activity_stream - + COUNT 2
XRANGE user_activity_stream 1609411200000-0 + COUNT 2
XREVRANGE user_activity_stream + - COUNT 1
```
#### XREAD
```sh
XREAD COUNT 5 STREAMS user_activity_stream 0
XREAD COUNT 5 STREAMS user_activity_stream other_stream  0 0
XREAD BLOCK 0 STREAMS user_activity_stream $
XADD user_activity_stream * name Canan action update timestamp 2023-07-26
XREAD BLOCK 0 STREAMS user_activity_stream $
```

### Consumer Groups
#### XGROUP
```sh
XGROUP CREATE user_activity_stream user_group 0
XGROUP CREATE user_activity_stream user_group_3 $ MKSTREAM
```
#### XREADGROUP
```sh
XGROUP CREATE mystream mygroup $
XGROUP CREATE newstream mygroup $ MKSTREAM
XADD mystream * message apple
XADD mystream * message orange
XADD mystream * message strawberry
XADD mystream * message apricot
XADD mystream * message banana
XREADGROUP GROUP mygroup Alice COUNT 1 STREAMS mystream >
```

### Bitmaps
#### SETBIT
```sh
SETBIT online_users 100 1
SETBIT online_users 200 1
```
#### GETBIT
```sh
GETBIT online_users 100
```
#### BITOP
```sh
SETBIT bitmap1 0 1
SETBIT bitmap1 2 1
SETBIT bitmap1 4 1
SETBIT bitmap2 2 1
SETBIT bitmap2 3 1
SETBIT bitmap2 6 1
BITOP AND result_bitmap bitmap1 bitmap2
