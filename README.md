<!doctype html>
<html lang="ja">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>加盟店データ登録</title>
  <style>
    :root{
      --bg:#f6f8fb;
      --panel:#ffffff;
      --muted:#6b7280;
      --border:#d1d5db;
      --ink:#0f172a;
      --accent:#1677ff; /* 鮮やかな青（スクショ風） */
      --accent-ink:#fff;
      --danger:#d92d20;
      --shadow:0 10px 24px rgba(15,23,42,.06);
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0; background:var(--bg); color:var(--ink);
      font:16px/1.6 system-ui,-apple-system,"Segoe UI",Roboto,"Hiragino Kaku Gothic ProN","Hiragino Sans",Meiryo,Helvetica,Arial,sans-serif;
    }

    /* 外枠エリア（Figmaのフレーム風） */
    .frame{
      max-width:1200px; margin:28px auto; padding:24px; border:1px solid #cbd5e1; border-radius:8px; background:var(--panel); box-shadow:var(--shadow);
    }
    .page-header{
      margin:0 0 20px; padding:22px; text-align:center; border:1px solid #111; border-radius:6px; background:#f1f2f4;
    }
    .page-header h1{ margin:0; font-size:28px; letter-spacing:.04em; }
    .page-sub{ margin:6px 0 0; color:var(--muted); font-size:14px; }

    /* 入力パネル */
    .card{ background:#f9fafb; border:1px solid var(--border); border-radius:8px; padding:22px; }

    .field{ margin-bottom:22px; }
    .label{ display:block; font-weight:700; margin:0 0 8px; }
    .required::after{ content:"*"; color:var(--danger); margin-left:4px; font-weight:700; }

    .control{ position:relative; }
    .control input,
    .control select{
      appearance:none; width:100%; padding:12px 14px; border:1px solid var(--border); border-radius:6px; background:#fff; color:var(--ink);
      transition:border-color .15s ease, box-shadow .15s ease;
    }
    .control select{ background-image:url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="%236b7280" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="6 9 12 15 18 9"/></svg>'); background-repeat:no-repeat; background-position:right 10px center; padding-right:40px; }
    .control input:focus, .control select:focus{ outline:0; border-color:var(--accent); box-shadow:0 0 0 3px rgba(22,119,255,.18); }

    .helper{ font-size:12px; color:var(--muted); margin-top:6px; }
    .error{ font-size:12px; color:var(--danger); margin-top:6px; min-height:1.1em; }

    /* % サフィックス */
    .with-suffix{ position:relative; }
    .with-suffix input{ padding-right:40px; }
    .suffix{ position:absolute; right:12px; top:50%; transform:translateY(-50%); color:var(--muted); pointer-events:none; }

    /* ボタン */
    .actions{ display:grid; grid-template-columns:1fr 1fr; gap:24px; margin-top:26px; }
    button{ display:inline-block; width:100%; padding:12px 18px; border-radius:8px; border:1px solid transparent; cursor:pointer; font-weight:700; letter-spacing:.02em; }
    .btn-outline{ background:#e9f1ff; color:var(--accent); border-color:#c9defe; }
    .btn-outline:hover{ filter:brightness(1.03); }
    .btn-primary{ background:var(--accent); color:var(--accent-ink); }
    .btn-primary:hover{ filter:brightness(1.03); }
    .btn-primary:disabled{ opacity:.6; cursor:not-allowed; }

    /* レイアウト補助 */
    @media (min-width: 840px){
      .grid{ display:grid; grid-template-columns:1fr 1fr; gap:18px 24px; }
      .grid .field.full{ grid-column:1 / -1; }
    }
  </style>
</head>
<body>
  <div class="frame">
    <div class="page-header">
      <h1>加盟店データ登録</h1>
      <p class="page-sub">〜取引先名、新規定義日、原価率〜</p>
    </div>

    <div class="card">
      <form id="merchantForm" novalidate>
        <div class="grid">
          <!-- 取引先（DSBDチェーン名） -->
          <div class="field full">
            <label for="chain" class="label required">DSBDチェーン名</label>
            <div class="control">
              <select id="chain" name="chain" required>
                <option value="" selected>取引先名を選択してください</option>
                <option>Alphaフーズ</option>
                <option>Betaマート</option>
                <option>Gammaリテール</option>
                <option>Delta商事</option>
              </select>
            </div>
            <div class="helper">取引先名を選択してください</div>
            <div class="error" id="chainError">※必須項目です。選択してください</div>
          </div>

          <!-- 新規定義日（日数） -->
          <div class="field full">
            <label for="days" class="label required">新規定義日</label>
            <div class="control">
              <input id="days" name="days" type="number" inputmode="numeric" placeholder="29日〜730日を入力してください" min="29" max="730" step="1" required />
            </div>
            <div class="helper">29〜730日の整数で入力</div>
            <div class="error" id="daysError">※必須項目です。入力してください</div>
          </div>

          <!-- 原価率（%） -->
          <div class="field full">
            <label for="costRate" class="label required">原価率</label>
            <div class="control with-suffix">
              <input id="costRate" name="costRate" type="number" inputmode="decimal" placeholder="0.01〜99.99値を入力してください" min="0.01" max="99.99" step="0.01" required />
              <span class="suffix">%</span>
            </div>
            <div class="helper">小数点2桁まで可（例：34.50）</div>
            <div class="error" id="rateError">※必須項目です。入力してください</div>
          </div>
        </div>

        <div class="actions">
          <button type="reset" id="clearBtn" class="btn-outline">クリア</button>
          <button type="submit" id="submitBtn" class="btn-primary">登録</button>
        </div>
      </form>
    </div>
  </div>

  <script>
    const form = document.getElementById('merchantForm');
    const chain = document.getElementById('chain');
    const days = document.getElementById('days');
    const rate = document.getElementById('costRate');

    const chainError = document.getElementById('chainError');
    const daysError = document.getElementById('daysError');
    const rateError = document.getElementById('rateError');

    /** 数値の妥当性チェック（原価率: 0.01〜99.99, 小数2桁まで） */
    function isValidRate(v){
      if(v === '' || v === null) return false;
      const n = Number(v);
      if(!Number.isFinite(n)) return false;
      if(n < 0.01 || n >= 100) return false;
      // 小数2桁まで
      return /^\d{1,2}(\.\d{1,2})?$/.test(String(v));
    }

    /** 日数: 29〜730 の整数 */
    function isValidDays(v){
      if(v === '' || v === null) return false;
      const n = Number(v);
      return Number.isInteger(n) && n >= 29 && n <= 730;
    }

    function showError(el, msgEl, ok){
      el.setAttribute('aria-invalid', ok ? 'false' : 'true');
      msgEl.textContent = ok ? '' : msgEl.dataset.msg || msgEl.textContent;
    }

    // 初期メッセージを data 属性に退避（クリア→再表示用）
    [chainError, daysError, rateError].forEach(el=>{ el.dataset.msg = el.textContent; el.textContent = ''; });

    // 入力のリアルタイム検証
    chain.addEventListener('change', ()=>{
      showError(chain, chainError, chain.value !== '');
    });
    days.addEventListener('input', ()=>{
      showError(days, daysError, isValidDays(days.value));
    });
    rate.addEventListener('input', ()=>{
      showError(rate, rateError, isValidRate(rate.value));
    });

    // クリア時にエラーも消す
    document.getElementById('clearBtn').addEventListener('click', ()=>{
      setTimeout(()=>{ [chainError, daysError, rateError].forEach(el=> el.textContent=''); }, 0);
    });

    form.addEventListener('submit', (e)=>{
      e.preventDefault();
      const okChain = chain.value !== '';
      const okDays  = isValidDays(days.value);
      const okRate  = isValidRate(rate.value);

      showError(chain, chainError, okChain);
      showError(days, daysError, okDays);
      showError(rate, rateError, okRate);

      if(!(okChain && okDays && okRate)) return; // 不正なら送信しない

      // 実際の送信の代わりにサンプル処理
      const payload = {
        chain: chain.value,
        newCustomerDays: Number(days.value),
        costRate: Number(rate.value)
      };
      console.log('登録データ', payload);

      // フィードバック（必要に応じて置き換え）
      document.getElementById('submitBtn').disabled = true;
      document.getElementById('submitBtn').textContent = '送信中…';

      setTimeout(()=>{
        alert('登録が完了しました。\n\n' + JSON.stringify(payload, null, 2));
        document.getElementById('submitBtn').disabled = false;
        document.getElementById('submitBtn').textContent = '登録';
        // フォームをリセット（必要に応じて）
        form.reset();
      }, 600);
    });
  </script>
</body>
</html>
