# 🚀 Ruflo V3 - Windows PowerShell Setup Guide

**Platform:** Windows 10/11  
**Shell:** PowerShell (v5.0+)  
**Last Updated:** May 8, 2026

---

## 📋 Prerequisites

### 1. **Node.js** (v20+)
```powershell
# Check if Node.js is installed
node --version

# If not, download from:
# https://nodejs.org/en (LTS version recommended)
```

### 2. **pnpm** (Package Manager)
```powershell
# Install pnpm globally
npm install -g pnpm

# Verify installation
pnpm --version
```

### 3. **Git**
```powershell
# Check if Git is installed
git --version

# If not, download from:
# https://git-scm.com/download/win
```

### 4. **Visual C++ Build Tools** (Optional but recommended)
- Download: https://visualstudio.microsoft.com/visual-cpp-build-tools/
- Install with C++ workload

---

## 🔧 Step-by-Step Setup

### Step 1: Clone Repository
```powershell
# Navigate to desired directory
cd C:\Projects

# Clone the repository
git clone https://github.com/hk-covid/crowd.git
cd crowd\skesh\ruflo-main
```

### Step 2: Install Dependencies
```powershell
# Install all 748 packages
pnpm install

# Wait for completion (~2-3 minutes)
```

### Step 3: Build Project
```powershell
# Compile TypeScript
pnpm run build

# Check for successful output (no errors)
```

### Step 4: Initialize Ruflo
```powershell
# Full initialization with all components
npx ruflo init --full --yes

# This creates:
# - .claude/ directory (agents, skills, commands)
# - .claude-flow/ directory (runtime config)
# - .mcp.json (MCP server config)
# - .swarm/ directory (memory database)
```

---

## 🎯 Running Services (Windows PowerShell)

### Option A: Individual Services (Recommended for Testing)

**Terminal 1 - Start Daemon:**
```powershell
cd C:\Projects\crowd\skesh\ruflo-main
npx ruflo daemon start
```

**Terminal 2 - Start Dev Server:**
```powershell
cd C:\Projects\crowd\skesh\ruflo-main
pnpm run dev
```

**Terminal 3 - Start MCP Server:**
```powershell
cd C:\Projects\crowd\skesh\ruflo-main
npx ruflo mcp start
```

### Option B: Start Everything at Once
```powershell
cd C:\Projects\crowd\skesh\ruflo-main
npx ruflo init --start-all
```

### Option C: Using Start-Process (Background)
```powershell
# Start daemon in background
Start-Process -NoNewWindow -FilePath "npx" -ArgumentList "ruflo daemon start"

# Start dev server in background
Start-Process -NoNewWindow -FilePath "pnpm" -ArgumentList "run dev"

# Start MCP server in background
Start-Process -NoNewWindow -FilePath "npx" -ArgumentList "ruflo mcp start"

# Give services time to initialize
Start-Sleep -Seconds 5

# Check status
npx ruflo status
```

---

## 🔍 Verify Everything is Running

```powershell
# Check system status
npx ruflo status

# View daemon logs
Get-Content -Path .\.claude-flow\daemon.log -Tail 20

# Check running processes
Get-Process | Where-Object {$_.ProcessName -like "*node*"}

# Test MCP connection
npx ruflo mcp test
```

---

## 📊 Expected Output

After successful startup, you should see:

```
✅ RUFLO V3 [RUNNING]

Swarm
[INFO]   Swarm ready (PID: XXXXX)

Agents
+--------+-------+
| Status | Count |
+--------+-------+
| Active |  0-5  |
| Idle   |  0-5  |
| Total  |  23+  |
+--------+-------+

MCP Server
[OK]     Running (27 tools enabled)

Memory
[OK]     Initialized (HNSW index ready)
```

---

## 🛑 Stopping Services

```powershell
# Stop daemon
npx ruflo daemon stop

# Stop MCP server
Stop-Process -Name node -Force  # (or use Ctrl+C in terminal)

# Stop dev server
Stop-Process -Name node -Force  # (or use Ctrl+C in terminal)

# Kill all node processes associated with Ruflo
Get-Process node | Stop-Process -Force
```

---

## 📁 Key Directories (Windows Paths)

```
C:\Projects\crowd\skesh\ruflo-main
├── .claude\                    # Claude Code integration
│   ├── agents\                 # Agent definitions
│   ├── skills\                 # Skill modules
│   ├── commands\               # CLI commands
│   └── settings.json           # Configuration
│
├── .claude-flow\               # Runtime system
│   ├── config.yaml             # Main config
│   ├── data\                   # Persistent storage
│   ├── logs\                   # Log files
│   ├── daemon.log              # Daemon logs
│   └── sessions\               # Session data
│
├── .swarm\                     # Agent swarm data
│   └── memory.db               # Vector memory
│
├── dist\                       # Compiled output
├── node_modules\               # Dependencies
└── package.json                # Configuration
```

---

## 🐛 Troubleshooting on Windows

### Issue: Port Already in Use
```powershell
# Find process using port 3000
netstat -ano | findstr :3000

# Kill the process (replace PID)
taskkill /PID <PID> /F
```

### Issue: Permission Denied
```powershell
# Run PowerShell as Administrator
# Right-click PowerShell → Run as Administrator

# Or set execution policy
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### Issue: pnpm Not Found
```powershell
# Refresh environment
$env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("Path","User")

# Or restart PowerShell
```

### Issue: Build Fails
```powershell
# Clear cache and reinstall
rm -r node_modules
rm pnpm-lock.yaml
pnpm install
pnpm run build
```

### Issue: Memory Database Not Initializing
```powershell
# Delete old database
rm .\.swarm\memory.db

# Reinitialize
npx ruflo daemon stop
npx ruflo init --full --yes
```

---

## 🎓 Using with Claude Code

### Step 1: Configure VS Code
1. Open VS Code settings: `Ctrl + ,`
2. Search for "Custom Instructions"
3. Add path to `.claude/settings.json`

### Step 2: Connect MCP Server
1. Open `.mcp.json`
2. Verify `autoStart: false` is set
3. Restart Claude Code to load MCP tools

### Step 3: Start Using
```
# In Claude Code chat:
/agent spawn analysis --task "analyze my code"

# Or use directly:
memory_store({namespace: "x", key: "y", value: {...}})
swarm_init({agents: ["analysis", "review"], task: "..."})
```

---

## 📊 System Requirements (Windows)

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| **RAM** | 4GB | 8GB+ |
| **Disk** | 2GB free | 5GB+ free |
| **CPU** | Dual-core | Quad-core+ |
| **Node.js** | v18 | v20+ |
| **PowerShell** | v5.0 | v7+ |

---

## 🚀 Quick Start Script

Save as `start-ruflo.ps1`:

```powershell
# Ruflo V3 Startup Script for Windows

param(
    [string]$Action = "start",
    [string]$RufloPath = "C:\Projects\crowd\skesh\ruflo-main"
)

function Start-Ruflo {
    Push-Location $RufloPath
    
    Write-Host "🚀 Starting Ruflo V3..." -ForegroundColor Green
    
    # Check prerequisites
    if (-not (Get-Command node -ErrorAction SilentlyContinue)) {
        Write-Host "❌ Node.js not found. Install from https://nodejs.org/" -ForegroundColor Red
        exit 1
    }
    
    if (-not (Get-Command pnpm -ErrorAction SilentlyContinue)) {
        Write-Host "❌ pnpm not found. Install with: npm install -g pnpm" -ForegroundColor Red
        exit 1
    }
    
    Write-Host "✓ Prerequisites verified" -ForegroundColor Green
    
    # Start services
    Write-Host "Starting daemon..." -ForegroundColor Cyan
    Start-Process -NoNewWindow -FilePath "npx" -ArgumentList "ruflo daemon start"
    
    Start-Sleep -Seconds 2
    
    Write-Host "Starting dev server..." -ForegroundColor Cyan
    Start-Process -NoNewWindow -FilePath "pnpm" -ArgumentList "run dev"
    
    Start-Sleep -Seconds 2
    
    Write-Host "Starting MCP server..." -ForegroundColor Cyan
    Start-Process -NoNewWindow -FilePath "npx" -ArgumentList "ruflo mcp start"
    
    Start-Sleep -Seconds 3
    
    Write-Host "✅ Ruflo V3 Started Successfully!" -ForegroundColor Green
    Write-Host "   MCP Server: http://localhost:3000" -ForegroundColor Yellow
    Write-Host "   Daemon: Running" -ForegroundColor Yellow
    Write-Host "   Dev Mode: Watching for changes" -ForegroundColor Yellow
    
    Pop-Location
}

function Stop-Ruflo {
    Write-Host "⏹️  Stopping Ruflo V3..." -ForegroundColor Red
    
    Get-Process node -ErrorAction SilentlyContinue | Stop-Process -Force
    
    Write-Host "✅ Ruflo V3 Stopped" -ForegroundColor Green
}

function Show-Status {
    Push-Location $RufloPath
    npx ruflo status
    Pop-Location
}

# Execute action
switch ($Action.ToLower()) {
    "start" { Start-Ruflo }
    "stop" { Stop-Ruflo }
    "status" { Show-Status }
    default {
        Write-Host "Usage: .\start-ruflo.ps1 -Action [start|stop|status]" -ForegroundColor Yellow
    }
}
```

**Usage:**
```powershell
# Make script executable
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

# Run script
.\start-ruflo.ps1 -Action start
.\start-ruflo.ps1 -Action status
.\start-ruflo.ps1 -Action stop
```

---

## 📚 Additional Resources

- **Repository:** https://github.com/hk-covid/crowd
- **Ruflo Docs:** `./ README.md`
- **Architecture:** `./SETUP_REVIEW.md`
- **Quick Ref:** `./QUICK_START.md`

---

## ✨ You're All Set!

Your Ruflo V3 installation on Windows is ready. Start the services and begin using multi-agent orchestration with Claude Code! 🎉

**Next Steps:**
1. ✅ Install prerequisites
2. ✅ Clone repository
3. ✅ Run `pnpm install`
4. ✅ Run `pnpm run build`
5. ✅ Run `npx ruflo init --full --yes`
6. ✅ Start services (daemon, dev, MCP)
7. ✅ Use in Claude Code

---

*Guide for Windows PowerShell | Version 3.6.29 | May 2026*
