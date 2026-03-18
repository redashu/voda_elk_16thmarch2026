# 🔝 Top 20 KQL Examples (Unparsed Logs)

## 1️⃣ Get all logs
```kql
*
```

## 2️⃣ Search keyword
```kql
event.original: "GET"
```

## 3️⃣ Search specific endpoint
```kql
event.original: "/login"
```

## 4️⃣ Wildcard search (API paths)
```kql
event.original: "/api/*"
```

## 5️⃣ Filter by HTTP method
```kql
event.original: "POST"
```

## 6️⃣ Multiple methods
```kql
event.original: ("GET" or "POST")
```

## 7️⃣ Find status code 200
```kql
event.original: " 200 "
```

> **Note:** Spaces matter to avoid matching 2000, 1200, etc.

## 8️⃣ Find 404 errors
```kql
event.original: " 404 "
```

## 9️⃣ Find server errors (500)
```kql
event.original: " 500 "
```

## 🔟 Find all errors (4xx + 5xx approx)
```kql
event.original: (" 4*" or " 5*")
```

## 1️⃣1️⃣ Requests from specific IP
```kql
event.original: "192.168.1.10"
```

## 1️⃣2️⃣ Exclude IP
```kql
NOT event.original: "192.168.1.10"
```

## 1️⃣3️⃣ Combine conditions (AND)
```kql
event.original: "POST" AND event.original: "/login"
```

## 1️⃣4️⃣ Login failures (common pattern)
```kql
event.original: "/login" AND event.original: " 401 "
```

## 1️⃣5️⃣ Search user agent (basic)
```kql
event.original: "Mozilla"
```

## 1️⃣6️⃣ Exclude bots
```kql
NOT event.original: "bot"
```

## 1️⃣7️⃣ Time filter (last 1 hour)
```kql
@timestamp >= now-1h
```

## 1️⃣8️⃣ Exact phrase match
```kql
event.original: "\"GET /login HTTP/1.1\""
```

## 1️⃣9️⃣ Complex real-world query
```kql
event.original: "POST" AND 
event.original: "/api/" AND 
event.original: " 500 " AND 
NOT event.original: "127.0.0.1"
```

## 2️⃣0️⃣ Detect suspicious scanning
```kql
event.original: ("/admin" or "/wp-admin" or "/phpmyadmin")
```

## ⚠️ Limitations of Using event.original

| Problem | Why |
|---------|-----|
| ❌ No structured filtering | Everything is text |
| ❌ Slow queries | Full-text search |
| ❌ Inaccurate matches | String-based |
| ❌ No aggregations | Can't group properly |


