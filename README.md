# Linux AI Ops Agent

A small Python agent that connects to a local Ollama model and then uses SSH to inspect Linux services, read logs, identify likely root causes, and optionally repair safe issues.

## Features
- Connects to a local Ollama server
- SSHs into a remote Linux machine
- Checks service status via `systemctl`
- Reads recent logs via `journalctl`
- Reads likely config files for common services
- Identifies likely root cause via LLM or rule-based fallback
- Safely restarts or reloads common services when the task explicitly requests a fix
- Verifies service status after a fix

## Requirements
- Python 3.10+
- SSH access to the target machine
- A local Ollama server running on your machine
- An SSH private key (recommended over password auth)

## Setup

1. Install dependencies:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

2. Pull a model in Ollama:

```bash
ollama pull qwen2.5:7b-instruct
```

3. Copy the config example and edit it:

```bash
cp config.example.json config.json
```

Update `config.json` with your actual SSH target and Ollama endpoint.

## Usage

### One-shot task

```bash
python agent.py --task "check nginx and fix it"
```

### Interactive mode

```bash
python agent.py --interactive
```

## Example tasks

- `check nginx and tell me the likely root cause`
- `investigate docker and report if it is unhealthy`
- `check ssh and fix it`
- `look at redis and diagnose the issue`

## Safety rules

This project is intentionally conservative:
- Commands are limited by a hardcoded allowlist.
- Only a narrow set of service restart/reload commands are auto-executed.
- File writes are guarded and can create a `.bak` copy before modifying a file.
- The tool only performs smart, safe recovery for common services.

## Important note

This is a practical agent scaffold for local ops automation. For production usage, add:
- approval gates before mutations
- audit logs
- stronger service-specific logic
- secrets management
- role-based SSH users

## License

MIT
















































































































































































































































































































































































































































































































































