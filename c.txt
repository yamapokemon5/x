#!/bin/bash

# 1. Baixar e instalar o Chrome
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install -y ./google-chrome-stable_current_amd64.deb
rm google-chrome-stable_current_amd64.deb

# 2. Criar wrapper google-chrome-safe
sudo tee /usr/local/bin/google-chrome-safe > /dev/null << 'EOF'
#!/bin/bash
/usr/bin/google-chrome-stable --no-sandbox --disable-dev-shm-usage --disable-gpu "$@"
EOF
sudo chmod +x /usr/local/bin/google-chrome-safe

# 3. Ajustar o atalho oficial do Chrome
sudo sed -i 's|Exec=/usr/bin/google-chrome-stable %U|Exec=/usr/local/bin/google-chrome-safe %U|' /usr/share/applications/google-chrome.desktop

# 4. Testar execução
google-chrome-safe

# 5. (Opcional) Criar atalho no Desktop
mkdir -p ~/Desktop
tee ~/Desktop/chrome.desktop > /dev/null << 'EOF'
[Desktop Entry]
Version=1.0
Type=Application
Name=Google Chrome
Exec=/usr/local/bin/google-chrome-safe
Icon=google-chrome
Terminal=false
Categories=Network;WebBrowser;
EOF
chmod +x ~/Desktop/chrome.desktop
