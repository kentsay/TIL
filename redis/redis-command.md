### Redis Command
======

This is a quick note of the Redis commands that I often used. Using the right command makes a big difference on performance. Full document can be reference from [here](https://redis.io/commands).

#### GET/SET Key
Get the value of key. If the key does not exist the special value nil is returned. An error is returned if the value stored at key is not a string, because GET only handles string values.

```bash
redis> GET nonexisting
(nil)
redis> SET mykey "Hello"
"OK"
redis> GET mykey
"Hello"
redis>
```

#### SADD key member [member ...]
Add the specified members to the set stored at key. Specified members that are already a member of this set are ignored. If key does not exist, a new set is created before adding the specified members.
An error is returned when the value stored at key is not a set.

```bash
redis> SADD myset "Hello"
(integer) 1
redis> SADD myset "World"
(integer) 1
redis> SADD myset "World"
(integer) 0
redis> SMEMBERS myset
1) "Hello"
2) "World"
redis>
```

#### SMEMBERS
Returns all the members of the set value stored at key.
This has the same effect as running SINTER with one argument key.


#### SREM key member [member ...]
Remove the specified members from the set stored at key. Specified members that are not a member of this set are ignored. If key does not exist, it is treated as an empty set and this command returns 0.
An error is returned when the value stored at key is not a set.

```bash
redis> SADD myset "one"
(integer) 1
redis> SADD myset "two"
(integer) 1
redis> SADD myset "three"
(integer) 1
redis> SREM myset "one"
(integer) 1
redis> SREM myset "four"
(integer) 0
redis> SMEMBERS myset
1) "two"
2) "three"
redis>
```
