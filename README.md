#Sgpt 

```bash
#!/data/data/com.termux/files/usr/bin/bash

echo "╔══════════════════════════════╗"
echo "║     SGPT Installer v1.0      ║"
echo "║   Powered by Sarvam AI       ║"
echo "╚══════════════════════════════╝"

echo "[*] Updating packages..."
pkg update -y && pkg upgrade -y

echo "[*] Installing Python..."
pkg install python -y

echo "[*] Installing imagemagick..."
pkg install imagemagick -y

echo "[*] Installing pip requests..."
pip install requests

echo "[*] Downloading sgpt..."
cat > $PREFIX/bin/sgpt << 'EOF'
#!/usr/bin/env python3
import subprocess, sys, os

try:
    import requests
except ImportError:
    subprocess.run([sys.executable, "-m", "pip", "install", "requests"], check=True)
    import requests

import json

API_KEY = "sk_dlfv5m66_La4cv4twEKJTTFumb54xRKPH"
API_URL = "https://api.sarvam.ai/v1/chat/completions"

BANNER = """
╔══════════════════════════════╗
║        SGPT - Terminal AI    ║
║     Powered by Sarvam AI     ║
╚══════════════════════════════╝
Type 'exit' or 'quit' to stop.
"""

def chat(user_message):
    headers = {
        "API-Subscription-Key": API_KEY,
        "Content-Type": "application/json"
    }
    payload = {
        "model": "sarvam-105b",
        "messages": [{"role": "user", "content": user_message}],
        "temperature": 0.8,
        "top_p": 1,
        "stream": True,
        "reasoning_effort": "low"
    }
    try:
        response = requests.post(API_URL, headers=headers, json=payload, stream=True)
        response.raise_for_status()
        print("\n[SGPT]: ", end="", flush=True)
        for line in response.iter_lines():
            if line:
                line = line.decode("utf-8")
                if line.startswith("data: "):
                    data = line[6:]
                    if data == "[DONE]":
                        break
                    chunk = json.loads(data)
                    choices = chunk.get("choices", [])
                    if choices:
                        delta = choices[0].get("delta", {})
                        if "content" in delta and delta["content"]:
                            print(delta["content"], end="", flush=True)
        print("\n")
    except requests.exceptions.ConnectionError:
        print("\n[!] No internet connection.")
    except requests.exceptions.HTTPError as e:
        print(f"\n[!] API Error: {e}")
    except Exception as e:
        print(f"\n[!] Error: {e}")

def main():
    print(BANNER)
    if len(sys.argv) > 1:
        chat(" ".join(sys.argv[1:]))
    else:
        while True:
            try:
                user_input = input("You: ").strip()
                if not user_input:
                    continue
                if user_input.lower() in ["exit", "quit"]:
                    print("[*] Goodbye!")
                    break
                chat(user_input)
            except KeyboardInterrupt:
                print("\n[*] Goodbye!")
                break

if __name__ == "__main__":
    main()
EOF

echo "[*] Setting permissions..."
chmod +x $PREFIX/bin/sgpt

echo ""
echo "╔══════════════════════════════╗"
echo "║   ✅ SGPT Installed!          ║"
echo "║   Type: sgpt                 ║"
echo "║   Or:   sgpt \"your question\" ║"
echo "╚══════════════════════════════╝"
```

---

**Termux mein install karne ke steps:**

```bash
# Step 1 - File save karo
nano install.sh

# Step 2 - Upar wala code paste karo (Ctrl+X, Y, Enter)

# Step 3 - Permission do
chmod +x install.sh

# Step 4 - Run karo
bash install.sh
```

**Bas! Ab koi bhi sirf type kare:**
```bash
sgpt
# ya
sgpt "Python kya hai?"
```
