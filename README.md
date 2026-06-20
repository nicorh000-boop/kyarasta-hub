  <!DOCTYPE html>
  <html lang="ja">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-title" content="きゃらスタ">
    <title>きゃらスタ</title>
    <style>
      * { box-sizing: border-box; margin: 0; padding: 0; }
      body {
        font-family: 'Hiragino Sans', 'Meiryo', sans-serif;
        background: linear-gradient(135deg, #f5f3ff 0%, #ede9fe 100%);
        min-height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
      }
      .card {
        background: #fff;
        border-radius: 20px;
        padding: 40px 32px;
        text-align: center;
        box-shadow: 0 8px 32px rgba(109,40,217,.15);
        max-width: 340px;
        width: 90%;
      }
      .logo { font-size: 3.5rem; margin-bottom: 12px; }
      h1 { font-size: 1.6rem; color: #111; margin-bottom: 6px; }
      .sub { font-size: .9rem; color: #888; margin-bottom: 32px; line-height: 1.5; }
      .start-btn {
        display: block;
        width: 100%;
        padding: 18px;
        background: linear-gradient(135deg, #6d28d9, #7c3aed);
        color: #fff;
        border: none;
        border-radius: 14px;
        font-size: 1.2rem;
        font-weight: bold;
        cursor: pointer;
        box-shadow: 0 4px 16px rgba(109,40,217,.35);
        transition: transform .12s, box-shadow .12s;
        letter-spacing: .05em;
      }
      .start-btn:active { transform: scale(.97); box-shadow: 0 2px 8px rgba(109,40,217,.2); }
      .start-btn:disabled { background: #c4b5fd; box-shadow: none; cursor: default; }
      .status { margin-top: 16px; font-size: .85rem; color: #9ca3af; min-height: 1.2em; }
      .status.error { color: #ef4444; }
      .footer { margin-top: 28px; font-size: .75rem; color: #c4b5fd; }
    </style>
  </head>
  <body>
    <div class="card">
      <div class="logo">⭐</div>
      <h1>きゃらスタ</h1>
      <p class="sub">AI先生と一緒に<br>毎日コツコツ勉強しよう！</p>
      <button class="start-btn" id="start-btn" onclick="launch()">START</button>
      <div class="status" id="status"></div>
      <div class="footer">5教科 × AIキャラクター学習</div>
    </div>
    <script>
      const GIST_ID = '13f071b9608f9c0dbcdd5971dd5a6181'
      const GIST_API = `https://api.github.com/gists/${GIST_ID}`
      async function launch() {
        const btn = document.getElementById('start-btn')
        const status = document.getElementById('status')
        btn.disabled = true
        status.textContent = '接続中...'
        status.className = 'status'
        try {
          const res = await fetch(`${GIST_API}?t=${Date.now()}`, {
            headers: { Accept: 'application/vnd.github+json' }
          })
          if (!res.ok) throw new Error(`status ${res.status}`)
          const data = await res.json()
          const content = data.files['kyarasta-url.json']?.content
          if (!content) throw new Error('file not found')
          const { url } = JSON.parse(content)
          if (!url) throw new Error('サーバーが起動していません')
          status.textContent = '接続完了！ジャンプします...'
          setTimeout(() => { window.location.href = url }, 400)
        } catch (e) {
          status.textContent = e.message === 'サーバーが起動していません'
            ? 'サーバーが起動していません。先生に連絡してね。'
            : '接続に失敗しました。もう一度試してね。'
          status.className = 'status error'
          btn.disabled = false
        }
      }
    </script>
  </body>
  </html>
