# JSON Library  

**Category:** Data Handling / Logs & API Data  

**What it is:**  
The `json` library is part of Python’s standard library for working with JSON data. It allows parsing (reading) and serializing (writing) JSON, making it ideal for handling structured data in logs, API responses, configuration files, and cybersecurity workflows.  

**Use cases:**  
- Parsing JSON from API responses or files  
- Processing large log streams in NDJSON / JSON Lines format  
- Counting occurrences or filtering specific events  
- Navigating nested JSON structures safely  
- Preparing structured data for analysis or automation  

---

## 🔹 Example Snippets  

### Core Concepts: Single Document vs Log Stream
```python
import json

# Single JSON document
single_doc = [
  {"user": "alice", "status": "ACTIVE"},
  {"user": "bob", "status": "LOCKED"}
]

# Log stream (NDJSON / JSON Lines)
log_stream = """
{"event_id": 1, "user": "alice", "action": "LOGIN_SUCCESS"}
{"event_id": 2, "user": "root", "action": "FILE_DELETE"}
"""
```
### Workflow A: Processing a "Single Document" (API/File)
```Python
import requests
import json

# From a file
with open("config.json", 'r') as f:
    config_data = json.load(f)
    print(config_data["system_id"])

# From an API
response = requests.get("https://pokeapi.co/api/v2/pokemon/ditto")
if response.status_code == 200:
    pokemon_data = response.json()
    print(pokemon_data["name"])
```
### Workflow B: Processing a "Log Stream" (Line by Line)
```Python
log_data = """
{"event_id": 1, "status": "SUCCESS"}
{"event_id": 2, "status": "FAILED"}
"""
log_lines = log_data.splitlines()

for line in log_lines:
    if not line:
        continue
    log_entry = json.loads(line)
    if log_entry["status"] == "FAILED":
        print(f"Failure on event ID: {log_entry['event_id']}")
```
#### Output (Workflow B)
```Bash
Failure on event ID: 2
```
### Workflow C: Cross-Reference (Nested Loop)
```Python
SENSITIVE_KEYWORDS = ["password", "secret", "private_key", "config"]
UPLOADED_FILES = [
    "/home/bob/downloads/my_passwords.txt",
    "/home/system/app/server.config",
    "/home/alice/pictures/vacation.jpg",
]

for file in UPLOADED_FILES:
    for keyword in SENSITIVE_KEYWORDS:
        if keyword in file:
            print(f"Potential sensitive file found: {file}")
            break
```
#### Output (Workflow C)
```Bash
Potential sensitive file found: /home/bob/downloads/my_passwords.txt
Potential sensitive file found: /home/system/app/server.config
```
### Advanced Patterns: Counting and Filtering
```Python
# Scorekeeper: count occurrences
log_stream = """
{"user": "alice", "action": "LOGIN"}
{"user": "bob", "action": "LOGOUT"}
{"user": "alice", "action": "UPLOAD"}
"""
log_lines = log_stream.splitlines()
user_counts = {}

for line in log_lines:
    if not line: continue
    log_entry = json.loads(line)
    user = log_entry["user"]
    user_counts[user] = user_counts.get(user, 0) + 1

print(user_counts)
```
#### Output (Advanced Patterns: Counting and Filtering)
```Bash
{'alice': 2, 'bob': 1}
```
```Python
# Filter first, then count
auth_log = """
{"source_ip": "104.18.32.142", "status": "FAILED"}
{"source_ip": "192.168.1.10", "status": "SUCCESS"}
{"source_ip": "104.18.32.142", "status": "FAILED"}
"""
log_lines = auth_log.splitlines()
failed_ip_counts = {}

for line in log_lines:
    if not line: continue
    log_entry = json.loads(line)
    if log_entry["status"] == "FAILED":
        ip = log_entry["source_ip"]
        failed_ip_counts[ip] = failed_ip_counts.get(ip, 0) + 1

print(failed_ip_counts)
```
#### Output (Advanced Patterns: Counting and Filtering)
```Bash
{'104.18.32.142': 2}
```
### Navigating Nested JSON
```Python
# Nested dictionary
user_profile = {
    "username": "alice",
    "contact": {"email": "alice@company.com"}
}
email = user_profile['contact']['email']

# Nested list of dictionaries
user_profile = {
    "username": "alice",
    "login_history": [
        {"timestamp": "...", "ip": "192.168.1.10"},
        {"timestamp": "...", "ip": "203.0.113.15"}
    ]
}
latest_ip = user_profile['login_history'][-1]['ip']
```
#### More advanced and professional techniques when you understand JSON patterns and nesting
```Python
# Safe key access
log_entry = {"user": "alice", "status": "SUCCESS"}
ip = log_entry.get("source_ip", "N/A")
print(ip)

# Robust error handling
log_lines = ["{\"status\": \"SUCCESS\"}", "invalid json"]
for i, line in enumerate(log_lines, 1):
    try:
        log_entry = json.loads(line)
        print(f"Processed line {i}: {log_entry}")
    except json.JSONDecodeError:
        print(f"[Warning] Could not parse JSON on line {i}: {line}")

# Pretty printing
import json
data = {"user": "alice", "details": {"group": "admin", "active": True}}
print(json.dumps(data, indent=4))
```
### Output (.get and error handling)
```bash
Source IP: N/A
Processed line 1: {'status': 'SUCCESS'}
[Warning] Could not parse JSON on line 2: invalid json
{
    "user": "alice",
    "details": {
        "group": "admin",
        "active": true
    }
}
```
### Notes/Tips:
- Use .get() to safely access keys
- Always validate JSON input before processing.
- Use line-by-line parsing for large log streams to conserve memory.
- Combine filtering and counting to generate summaries efficiently.
- Pretty-print for debugging complex structures.
