# Cloudflare Pages 自動部署設定

這個倉庫是靜態網頁，`index.html` 就在根目錄，**不需要建置指令**。
用 Cloudflare 後台接 GitHub，之後每次 `git push` 就會自動部署。

## 一次性設定（約 3 分鐘）

1. 登入 <https://dash.cloudflare.com> → 左側 **Workers & Pages** → **Create** → **Pages** → **Connect to Git**
2. 選 **GitHub** → 授權 Cloudflare
   - 授權範圍選 **Only select repositories**，只勾 `tokyo-winter-trip`
3. 選擇這個倉庫 → **Begin setup**
4. 建置設定：

   | 欄位 | 值 |
   |---|---|
   | Project name | `tokyo-winter-trip`（會決定網址 `tokyo-winter-trip.pages.dev`） |
   | Production branch | `main` |
   | Framework preset | **None** |
   | Build command | 留空 |
   | Build output directory | `/`（根目錄） |

5. **Save and Deploy**，約 1 分鐘後拿到網址 `https://tokyo-winter-trip.pages.dev`

## 之後的流程

```bash
# 改完 index.html
git add index.html
git commit -m "更新行程"
git push
```

push 到 `main` 會觸發 Production 部署；push 到其他分支會產生 Preview 網址，方便先看過再合併。

## 限制存取（選用）

這份是已移除個人資訊的公開版本，通常不需要限制。若日後放了敏感內容，`.pages.dev` 網址預設任何人拿到連結都能開，可以這樣鎖：

- **Cloudflare Access**：專案 → **Settings** → **Access policy** → 開啟，設定只有指定 Email 收到一次性驗證碼才能開啟（Zero Trust 免費方案含 50 人）
- 或 Preview 部署單獨保護：**Settings → General → Access policy → Protect preview deployments**

## 疑難排解

- **看不到 private 倉庫**：回到 GitHub → Settings → Applications → Cloudflare Pages → 重新授權，確認有勾到這個倉庫
- **部署成功但是空白頁**：確認 Build output directory 是 `/`，不是 `dist` 或 `public`
- **改了沒更新**：Cloudflare 後台 **Deployments** 看最新一筆是不是對應到你的 commit；瀏覽器按 Ctrl+F5 強制重整
