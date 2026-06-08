# HuggingMes — Terminal Setup Guide

Skip the UI env-builder. Deploy directly to Hugging Face Spaces and configure everything via JupyterLab terminal.

## Quick Start

### 1. Create the Space
- Go to [huggingface.co/new](https://huggingface.co/spaces/create)
- **Repository name:** `HuggingMes` (or your choice)
- **License:** MIT (or your choice)
- **Space SDK:** Docker
- **Select this repo:** `tamim-coder1/HuggingMes`

The Space will boot with JupyterLab terminal accessible at `/terminal/`.

### 2. Access the Terminal
Once the Space starts, click **Terminal** → opens JupyterLab at `/terminal/`
- **Default password:** auto-generated (check Space logs or use `GATEWAY_TOKEN` if set)

### 3. Create `.env` File
```bash
cd /opt/data

# Create your .env file
cat > .env << 'EOF'
# Required
GATEWAY_TOKEN=your_secure_token_here
LLM_MODEL=google/gemini-2.0-flash
LLM_API_KEY=your_api_key_here

# Optional - Backup to HuggingFace Dataset
HF_TOKEN=hf_your_token_here
SYNC_INTERVAL=600

# Optional - Telegram Bot
# TELEGRAM_BOT_TOKEN=your_bot_token_here
# TELEGRAM_ALLOWED_USERS=123456789,987654321

EOF
```

### 4. Verify Configuration
```bash
# Check the file was created
cat .env

# Source it to verify syntax
source .env

# Test key is set
echo $LLM_API_KEY
```

### 5. Restart the Space
Settings take effect on next boot. Go to **Settings** → **Restart** or the Space auto-restarts.

## Environment Variables

### Required
```bash
GATEWAY_TOKEN=your_secure_token          # Protects the web UI
LLM_MODEL=google/gemini-2.0-flash        # Model (format: provider/model-name)
LLM_API_KEY=your_api_key_here            # API key for the provider
```

### Optional - Backup
```bash
HF_TOKEN=hf_your_token_here              # HuggingFace token (enables dataset backup)
SYNC_INTERVAL=600                        # Backup every 600 seconds
BACKUP_DATASET_NAME=huggingmes-backup    # Dataset name (default shown)
```

### Optional - Telegram
```bash
TELEGRAM_BOT_TOKEN=your_bot_token_here        # Get from @BotFather
TELEGRAM_ALLOWED_USERS=123456789,987654321   # Allowed user IDs (comma-separated)
TELEGRAM_MODE=webhook                        # or "polling"
```

### Optional - Terminal/Jupyter
```bash
DEV_MODE=true                                 # Enable terminal (default: true)
JUPYTER_TOKEN=your_jupyter_password_here     # Override terminal password
```

## Full Environment Reference

See `.env.example` in the repo root for all 100+ options including:
- Provider-specific API keys (OpenAI, Anthropic, DeepSeek, etc.)
- Custom model endpoints
- Cloudflare proxy setup
- Advanced Hermes configuration

## Terminal Commands

Once configured, use the terminal to:

```bash
# Check Hermes version
hermes --version

# View current configuration
cat /opt/data/config.yaml

# Install additional Python packages
pip install pandas requests

# Install system packages
sudo apt-get update && sudo apt-get install -y ffmpeg

# Restart the Hermes gateway
pkill -f "hermes gateway"

# View logs
tail -f /opt/data/logs/gateway.log
```

## Troubleshooting

### Terminal not showing?
- Check `DEV_MODE` is not set to `false`
- Verify `GATEWAY_TOKEN` is set
- Restart the Space

### API key not being recognized?
```bash
# Source the .env in terminal
source ~/.bashrc
echo $LLM_API_KEY
```

### Gateway crashes on startup?
```bash
# Check the logs
tail -50 /opt/data/logs/gateway.log

# Verify config syntax
python3 -c "import yaml; yaml.safe_load(open('/opt/data/config.yaml'))"
```

### HuggingFace backup not working?
```bash
# Test HF token
huggingface-cli login --token $HF_TOKEN
```

## Access Points

Once running:
- **Hermes UI:** `https://your-space.hf.space/app/`
- **Terminal:** `https://your-space.hf.space/terminal/`
- **Gateway API:** `http://127.0.0.1:8642/` (internal only)
- **Dashboard:** `http://127.0.0.1:9119/` (internal only)

## Need Help?

1. Check the **Space Logs** tab for startup errors
2. Access terminal and run: `tail -f /opt/data/logs/gateway.log`
3. Read the full config reference in `.env.example`
