###  Cache-Aside (or Lazy Loading) pattern
In a standard 3-tier web architecture, the routing intelligence that decides whether to fetch data from Redis or the Database lies entirely within the Application Tier (Backend).

#### The Request Flow
```
[ Client / Frontend ]
         │
         ▼
[ Application Tier ]  ◄── (Routing Intelligence Lives Here)
     │         │
     │(Cache   │(Cache
     │ Miss)   │ Hit)
     ▼         ▼
  [  DB  ]  [ Redis ]
```
#### Example in Python 
```
def get_user_profile(user_id):
    cache_key = f"user:{user_id}"
    
    # 1. Cache Lookup
    cached_data = redis_client.get(cache_key)
    
    if cached_data:
        # --- Cache Hit ---
        return deserialize(cached_data) 
        
    # --- Cache Miss ---
    # 2. DB Lookup / DB Call
    db_data = database.query("SELECT * FROM users WHERE id = ?", user_id)
    
    if db_data:
        # 3. Populate Cache (so the next request is a Cache Hit)
        redis_client.setex(cache_key, ttl_seconds=3600, value=serialize(db_data))
        
    return db_data

```
Why this approach is preferred
By placing this intelligence in the application logic, you gain complete control over:
- `TTL` (Time-to-Live): Deciding exactly how long data should stay in Redis before expiring.
- `Serialization`: Converting complex database rows into fast, lightweight strings (like JSON) for Redis.
- `Fallback Safety`: If Redis goes down completely, your application logic can catch the error and gracefully route all traffic to the DB without crashing the website.

- #### Keep in kind , Redis is key value store , App logic does the JSON conversion to store into Redis

```
[ Relational DB ]   ──(1. SQL Rows)──►  [ Application Logic ]  ──(2. Serialization)──►  [ Redis Cache ]
Table: Users                            - Runs SQL Query                                Key: user:101
Table: Orders                           - Combines/Maps Data                            Value: JSON or Hash
                                        - Converts to JSON/String
```
