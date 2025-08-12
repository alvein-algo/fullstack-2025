
# W1 週一筆記：環境建置 + Hello Web 專案

目標：完成 WSL2 + Ubuntu 開發環境、安裝必備工具、建立第一個網頁並上傳 GitHub

---

## 1️⃣ 安裝 WSL2 + Ubuntu 22.04
> 在 **Windows PowerShell（系統管理員）** 執行

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
wsl --set-default-version 2
wsl --install -d Ubuntu-22.04
```

💡 **用途**：建立 Ubuntu 小環境（在 Windows 裡跑 Linux）

---

## 2️⃣ Ubuntu 基本設定
> 在 **Ubuntu 終端機** 執行

```bash
sudo apt update && sudo apt -y upgrade
sudo apt -y install build-essential git curl unzip python3-venv python3-pip
```

💡 **用途**：更新系統並安裝常用開發工具

---

## 3️⃣ 安裝 Node.js（nvm）

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
nvm install --lts
node -v
npm -v
```

💡 **用途**：安裝 Node.js 與 npm，方便前端開發

---

## 4️⃣ 安裝 Docker

```bash
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER
newgrp docker
docker run --rm hello-world
```

💡 **用途**：容器化運行資料庫與服務

---

## 5️⃣ 安裝 .NET 8 SDK

```bash
wget https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt update
sudo apt install -y dotnet-sdk-8.0
dotnet --info | head -n 10
```

💡 **用途**：後續 C# 或 .NET 開發

---

## 6️⃣ 建立 Hello Web 專案

```bash
mkdir -p ~/fullstack-2025/hello-web && cd ~/fullstack-2025/hello-web

# index.html
cat > index.html <<'EOF'
<!doctype html>
<html lang="zh-Hant">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Hello Web</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <header class="site-header">
      <h1>👋 Hello, Web!</h1>
      <p>HTML＝骨架、CSS＝衣服、JS＝大腦</p>
    </header>
    <main class="container">
      <section class="card">我是一張卡片 A</section>
      <section class="card">我是一張卡片 B</section>
    </main>
    <footer class="site-footer">© 2025 My First Page</footer>
  </body>
</html>
EOF

# style.css
cat > style.css <<'EOF'
* { box-sizing: border-box; }
.container {
  max-width: 960px; margin: 24px auto; padding: 0 16px;
  display: grid; grid-template-columns: 1fr; gap: 16px;
}
.card { padding: 16px; border: 1px solid #ddd; border-radius: 8px; }
.site-header, .site-footer {
  text-align: center; padding: 16px; background: #f7f7f7; border-radius: 8px;
}
@media (min-width: 768px) {
  .container { grid-template-columns: 1fr 1fr; }
}
EOF
```

💡 **用途**：完成第一個 RWD 靜態網頁

---

## 7️⃣ 本機預覽網站

```bash
python3 -m http.server 8000
```

💡 **用途**：快速啟動本機測試伺服器

---

## 8️⃣ 設定 Git SSH 金鑰

```bash
ssh-keygen -t ed25519 -C "你的GitHub信箱"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub
```

💡 **用途**：設定與 GitHub 的安全連線

---

## 9️⃣ 測試 SSH 與 GitHub 連線

```bash
ssh -T git@github.com
```

第一次會輸入 `yes`，成功訊息：
```
Hi <你的GitHub帳號>! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## 🔟 推送到 GitHub

```bash
cd ~/fullstack-2025
git init
git add .
git commit -m "W1D1: 建立 hello-web 並完成環境設置"
git branch -M main
git remote add origin git@github.com:你的帳號/fullstack-2025.git
git push -u origin main
```

💡 **用途**：將專案推送到 GitHub
