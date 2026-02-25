<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Latihan Nahwu RTL</title>
    
    <link rel="manifest" href="data:application/manifest+json,{%22name%22:%22Latihan%20Nahwu%22,%22short_name%22:%22Nahwu%22,%22start_url%22:%22.%22,%22display%22:%22standalone%22,%22background_color%22:%22#0f172a%22,%22theme_color%22:%22#38bdf8%22,%22icons%22:[{%22src%22:%22https://cdn-icons-png.flaticon.com/512/3389/3389041.png%22,%22sizes%22:%22512x512%22,%22type%22:%22image/png%22}]}">

    <style>
        :root {
            --bg-color: #0f172a;
            --container-bg: #1e293b;
            --text-main: #f8fafc;
            --accent-color: #38bdf8;
            --success: #22c55e;
            --error: #ef4444;
            --border-subtle: #334155;
        }

        body {
            font-family: 'Inter', 'Segoe UI', sans-serif;
            background-color: var(--bg-color);
            color: var(--text-main);
            display: flex;
            justify-content: center;
            padding: 20px;
            margin: 0;
            min-height: 100vh;
        }

        .container {
            background: var(--container-bg);
            padding: 25px;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
            max-width: 600px;
            width: 100%;
            border: 1px solid var(--border-subtle);
        }

        h2 { text-align: center; color: var(--accent-color); margin-bottom: 5px; }
        p { text-align: center; opacity: 0.7; font-size: 0.9rem; margin-bottom: 25px; }

        table {
            width: 100%;
            border-collapse: separate;
            border-spacing: 0 10px;
            direction: rtl;
        }

        th { color: #64748b; font-size: 0.75rem; text-transform: uppercase; padding: 10px; }

        td {
            padding: 12px;
            background: rgba(15, 23, 42, 0.4);
            border-top: 1px solid var(--border-subtle);
            border-bottom: 1px solid var(--border-subtle);
        }

        td:first-child { border-radius: 0 12px 12px 0; border-right: 1px solid var(--border-subtle); width: 40%; }
        td:last-child { border-radius: 12px 0 0 12px; border-left: 1px solid var(--border-subtle); }

        .arabic-text { font-size: 1.5rem; font-weight: bold; color: #fff; }

        select {
            width: 100%;
            padding: 8px;
            background: #0f172a;
            color: var(--accent-color);
            border: 1px solid var(--border-subtle);
            border-radius: 8px;
            font-weight: bold;
            outline: none;
        }

        .correct td { border-color: var(--success); background: rgba(34, 197, 94, 0.1); }
        .wrong td { border-color: var(--error); background: rgba(239, 68, 68, 0.1); }

        .actions {
            margin-top: 25px;
            display: flex;
            gap: 10px;
            justify-content: center;
        }

        button {
            padding: 12px 25px;
            border-radius: 10px;
            font-weight: bold;
            cursor: pointer;
            border: none;
            flex: 1;
        }

        .btn-check { background: var(--accent-color); color: #0f172a; }
        .btn-reset { background: var(--border-subtle); color: white; }

        #result-summary {
            margin-top: 20px;
            text-align: center;
            font-size: 1.5rem;
            font-weight: bold;
            color: var(--success);
        }
    </style>
</head>
<body>

<div class="container" dir="rtl">
    <h2>لَاتِهَانُ النَّحْوِ</h2>
    <p>Pilih Jenis Kata & Tandanya</p>

    <table>
        <thead>
            <tr>
                <th>Contoh</th>
                <th>Jenis</th>
                <th>Tanda</th>
            </tr>
        </thead>
        <tbody id="table-body"></tbody>
    </table>

    <div class="actions">
        <button class="btn-check" onclick="evaluateQuiz()">Cek Skor</button>
        <button class="btn-reset" onclick="resetQuiz()">Reset</button>
    </div>
    
    <div id="result-summary"></div>
</div>

<script>
    const data = [
        { word: "رَجُلٌ", type: "Isim", sign: "Tanwin" },
        { word: "سَوْفَ يَعْلَمُ", type: "Fi'il", sign: "Saufa (سَوْفَ)" },
        { word: "مِنْ", type: "Huruf", sign: "A'damiyah" },
        { word: "الْبَابُ", type: "Isim", sign: "Alif Lam (ال)" },
        { word: "قَدْ قَامَ", type: "Fi'il", sign: "Qad (قَدْ)" }
    ];

    const types = ["Isim", "Fi'il", "Huruf"];
    const signs = ["Tanwin", "Alif Lam (ال)", "Saufa (سَوْفَ)", "Qad (قَدْ)", "A'damiyah"];

    function initTable() {
        const body = document.getElementById('table-body');
        body.innerHTML = data.map((item, i) => `
            <tr id="row-${i}">
                <td class="arabic-text">${item.word}</td>
                <td>
                    <select id="type-${i}">
                        <option value="">-</option>
                        ${types.map(t => `<option value="${t}">${t}</option>`).join('')}
                    </select>
                </td>
                <td>
                    <select id="sign-${i}">
                        <option value="">-</option>
                        ${signs.map(s => `<option value="${s}">${s}</option>`).join('')}
                    </select>
                </td>
            </tr>
        `).join('');
    }

    function evaluateQuiz() {
        let score = 0;
        data.forEach((item, i) => {
            const uType = document.getElementById(`type-${i}`).value;
            const uSign = document.getElementById(`sign-${i}`).value;
            const row = document.getElementById(`row-${i}`);
            if (uType === item.type && uSign === item.sign) {
                row.className = "correct";
                score++;
            } else {
                row.className = "wrong";
            }
        });
        document.getElementById('result-summary').innerText = `Skor: ${score} / ${data.length}`;
    }

    function resetQuiz() {
        initTable();
        document.getElementById('result-summary').innerText = "";
    }

    // Register Service Worker sederhana untuk fitur offline
    if ('serviceWorker' in navigator) {
        navigator.serviceWorker.register('data:text/javascript,self.addEventListener("fetch", (e) => e.respondWith(fetch(e.request)));');
    }

    initTable();
</script>
</body>
</html>
