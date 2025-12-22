<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Set di Studio Giapponese SRS v3.2 (Clean)</title>
    <style>
        /* --- Stile Generale --- */
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            display: flex; justify-content: center; align-items: center;
            flex-direction: column; background-color: #f0f2f5;
            color: #333; margin: 0; padding: 10px;
            box-sizing: border-box; min-height: 100vh;
        }
        h1, h2, h3 { color: #2c3e50; text-align: center; }
        h4 { color: #007aff; border-bottom: 2px solid #e0e0e0; padding-bottom: 5px; margin-top: 30px; }
        
        .container { width: 100%; max-width: 600px; margin-bottom: 30px; }
        
        .card-ui {
            background-color: #ffffff; padding: 20px;
            border-radius: 16px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.08);
            margin-bottom: 20px;
        }

        /* --- Navigazione --- */
        #main-nav { display: flex; gap: 5px; width: 100%; max-width: 600px; margin-bottom: 15px; flex-wrap: wrap; justify-content: center; }
        .nav-btn {
            padding: 10px 12px; font-size: 0.9rem; font-weight: 600;
            border: none; border-radius: 8px; cursor: pointer; transition: all 0.2s;
            background-color: #e5e5ea; color: #007aff; flex: 1; min-width: 90px;
        }
        .nav-btn.active { background-color: #007aff; color: white; box-shadow: 0 4px 10px rgba(0,122,255,0.3); }
        .modulo-content { display: none; width: 100%; }

        /* --- Quiz & UI --- */
        #punteggio-container { text-align: center; font-weight: 600; color: #555; margin-bottom: 15px; }
        .punteggio-info { color: #007aff; font-weight: bold; }
        
        .filtro-container { margin-bottom: 15px; }
        #filtro-categoria { width: 100%; padding: 12px; border-radius: 8px; border: 1px solid #007aff; font-size: 1rem; background: #f0f8ff; font-weight: 600; }

        #prompt-container { text-align: center; margin: 20px 0; min-height: 80px; }
        #prompt-principale { font-size: 2.5rem; margin: 0; }
        #prompt-secondario { font-size: 1.2rem; color: #666; margin-top: 5px; font-style: italic; }
        .hiragana, .katakana { color: #34c759; }

        #input-risposta { width: 100%; padding: 12px; border: 2px solid #ddd; border-radius: 8px; font-size: 1.2rem; text-align: center; box-sizing: border-box; }
        #risultato-controllo { min-height: 40px; font-size: 1.1rem; font-weight: bold; text-align: center; margin: 10px 0; }
        .corretto { color: #2ca049; } .sbagliato { color: #d92c23; }

        /* --- TABELLE KANA --- */
        .table-wrapper { overflow-x: auto; border-radius: 8px; border: 1px solid #ddd; margin-bottom: 20px; }
        .kana-table { width: 100%; border-collapse: collapse; table-layout: fixed; min-width: 300px; }
        .kana-table td, .kana-table th { border: 1px solid #eee; padding: 8px 2px; text-align: center; vertical-align: middle; display: table-cell !important; }
        .kana-table th { background-color: #f2f2f7; color: #555; font-size: 0.85rem; font-weight: bold; }
        .k-char { font-size: 1.4rem; font-weight: bold; color: #333; display: block; line-height: 1.2; }
        .k-romaji { font-size: 0.75rem; color: #999; display: block; }

        /* --- TASTIERA VIRTUALE --- */
        #virtual-keyboard { display: none; grid-template-columns: repeat(5, 1fr); gap: 6px; margin: 15px 0; background: #f2f2f7; padding: 10px; border-radius: 10px; }
        .key-btn { background: white; border: 1px solid #ccc; border-radius: 6px; padding: 10px 0; font-size: 1.2rem; font-weight: bold; cursor: pointer; box-shadow: 0 2px 0 rgba(0,0,0,0.05); }
        .key-btn:active { background: #ddd; transform: translateY(2px); box-shadow: none; }

        /* --- Pulsanti e Form --- */
        .controlli { display: flex; gap: 10px; margin-top: 10px; }
        .btn { flex: 1; padding: 12px; font-size: 1rem; font-weight: bold; border: none; border-radius: 8px; cursor: pointer; color: white; transition: opacity 0.2s; width: 100%; }
        .btn:hover { opacity: 0.9; }
        #pulsante-controlla { background: #007aff; } #pulsante-prossima { background: #34c759; } #pulsante-elimina { background: #ff3b30; margin-top: 10px; }
        
        .sezione-gestione { border-top: 2px solid #f0f0f0; margin-top: 20px; padding-top: 20px; }
        .sezione-gestione h3 { margin-top: 0; color: #555; font-size: 1.1rem; }
        label { font-weight: bold; display: block; margin-bottom: 5px; color: #555; }
        input[type="text"] { width: 100%; padding: 10px; margin-bottom: 10px; border: 1px solid #ccc; border-radius: 6px; box-sizing: border-box; }
        
        /* Lista Vocaboli */
        .vocab-entry { display: flex; justify-content: space-between; align-items: center; padding: 12px; border-bottom: 1px solid #eee; }
        .vocab-jpn { font-size: 1.2rem; color: #007aff; font-weight: bold; }
        .vocab-romaji { font-size: 0.85rem; color: #888; font-style: italic; display: block; }
        .vocab-tag { font-size: 0.7rem; background: #eee; padding: 2px 6px; border-radius: 4px; color: #666; margin-left: 5px; }
        .delete-vocab-btn { background: #ff3b30; color: white; border: none; border-radius: 4px; padding: 5px 10px; cursor: pointer; margin-left: 10px; }

        /* Configurazione Kana */
        .kana-config-panel { background: #f9f9f9; padding: 15px; border-radius: 12px; margin-bottom: 20px; }
        .config-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 10px; }
        .checkbox-label { display: flex; align-items: center; gap: 8px; background: white; padding: 8px; border: 1px solid #eee; border-radius: 6px; cursor: pointer; font-size: 0.9rem; }
    </style>
</head>
<body>

    <h1>Set di Studio Giapponese SRS</h1>

    <nav id="main-nav">
        <button class="nav-btn" data-modulo="quiz">Quiz Vocaboli</button>
        <button class="nav-btn" data-modulo="hiragana">Hiragana</button>
        <button class="nav-btn" data-modulo="katakana">Katakana</button>
        <button class="nav-btn" data-modulo="vocaboli">Lista Vocaboli</button>
    </nav>

    <main id="app-container" class="container">

        <div id="modulo-quiz" class="modulo-content">
            <div class="card-ui">
                <div id="punteggio-container"></div>
                
                <div class="filtro-container">
                    <select id="filtro-categoria" onchange="cambiaCategoriaQuiz()">
                        <option value="TUTTI">📚 TUTTI I VOCABOLI</option>
                    </select>
                </div>

                <div id="quiz-container">
                    <div id="prompt-container">
                        <div id="prompt-label"></div> 
                        <h2 id="prompt-principale"></h2>
                        <h3 id="prompt-secondario"></h3>
                    </div>
                    
                    <input type="text" id="input-risposta" placeholder="Scrivi qui..." autocomplete="off">
                    
                    <div id="virtual-keyboard"></div>
                    <div id="risultato-controllo"></div>
                    <div id="esempio-display"></div> 

                    <div class="controlli">
                        <button id="pulsante-controlla" class="btn">Controlla</button>
                        <button id="pulsante-prossima" class="btn">Prossima</button>
                    </div>
                    
                    <button id="pulsante-elimina" class="btn">Elimina Carta</button>
                    <div id="nessuna-carta" style="display: none; text-align: center; margin-top:20px;"><p>Nessuna carta trovata.</p></div>
                </div>

                <div id="salva-sessione-container" style="margin-top:20px;">
                    <button id="ripassa-errori-btn" class="btn" style="background:#ff9500; margin-bottom:10px;" disabled>Ripassa Errori (0)</button>
                    <button id="salva-sessione" class="btn" style="background:#5856d6;">Salva e Esci</button>
                </div>
            </div>

            <div class="card-ui" id="form-container">
                <h2>Gestione Dati</h2>
                
                <div class="sezione-gestione">
                    <h3>Aggiornamento Online</h3>
                    <label>Link al file .csv su GitHub:</label>
                    <input type="text" id="csv-url-input" placeholder="https://raw.githubusercontent.com/..." value="">
                    <button id="update-url-btn" class="btn" style="background:#007aff;">Aggiorna da Link</button>
                    <p id="messaggio-update" class="messaggio-feedback"></p>
                </div>

                <div class="sezione-gestione">
                    <h3>Importa/Aggiungi</h3>
                    <label id="import-label" for="import-csv-file" style="cursor:pointer; background:#eee; padding:10px; text-align:center; border-radius:6px; margin-bottom:15px;">📂 Importa CSV Locale</label>
                    <input type="file" id="import-csv-file" accept=".csv" style="display:none;">
                    
                    <h3 style="margin-top:20px;">Aggiungi Parola Singola</h3>
                    <form id="form-aggiungi">
                        <label>Italiano: <input type="text" id="input-ita" required></label>
                        <label>Inglese: <input type="text" id="input-eng" required></label>
                        <label>Giapponese: <input type="text" id="input-jpn" required></label>
                        <label>Romaji: <input type="text" id="input-romaji" required></label>
                        <label>Categoria (Tag): <input type="text" id="input-tag"></label>
                        <button type="submit" class="btn">Salva Parola</button>
                    </form>
                </div>

                <div class="sezione-gestione">
                    <button id="svuota-tutto-btn" class="btn" style="background-color: #d92c23;">CANCELLA TUTTO</button>
                </div>
            </div>
        </div> 
        
        <div id="modulo-hiragana" class="modulo-content">
            <div class="card-ui">
                <h2>Alfabeto Hiragana</h2>
                <div class="kana-config-panel">
                    <h3>Seleziona gruppi:</h3>
                    <div class="config-grid" id="h-config-grid"></div>
                    <div class="config-actions">
                        <button class="btn-small" onclick="toggleAllKana('hiragana', true)">Seleziona Tutto</button>
                        <button class="btn-small" onclick="toggleAllKana('hiragana', false)">Deseleziona</button>
                    </div>
                </div>
                <button class="btn btn-start-kana" onclick="avviaQuizKana('hiragana')">🏋️ Avvia Quiz Hiragana</button>

                <h4>Suoni Base (Gojūon)</h4>
                <div class="table-wrapper">
                    <table class="kana-table">
                        <tr><th></th><th>A</th><th>I</th><th>U</th><th>E</th><th>O</th></tr>
                        <tr><th>-</th><td><span class="k-char">あ</span><span class="k-romaji">a</span></td><td><span class="k-char">い</span><span class="k-romaji">i</span></td><td><span class="k-char">う</span><span class="k-romaji">u</span></td><td><span class="k-char">え</span><span class="k-romaji">e</span></td><td><span class="k-char">お</span><span class="k-romaji">o</span></td></tr>
                        <tr><th>K</th><td><span class="k-char">か</span><span class="k-romaji">ka</span></td><td><span class="k-char">き</span><span class="k-romaji">ki</span></td><td><span class="k-char">く</span><span class="k-romaji">ku</span></td><td><span class="k-char">け</span><span class="k-romaji">ke</span></td><td><span class="k-char">こ</span><span class="k-romaji">ko</span></td></tr>
                        <tr><th>S</th><td><span class="k-char">さ</span><span class="k-romaji">sa</span></td><td><span class="k-char">し</span><span class="k-romaji">shi</span></td><td><span class="k-char">す</span><span class="k-romaji">su</span></td><td><span class="k-char">せ</span><span class="k-romaji">se</span></td><td><span class="k-char">そ</span><span class="k-romaji">so</span></td></tr>
                        <tr><th>T</th><td><span class="k-char">た</span><span class="k-romaji">ta</span></td><td><span class="k-char">ち</span><span class="k-romaji">chi</span></td><td><span class="k-char">つ</span><span class="k-romaji">tsu</span></td><td><span class="k-char">て</span><span class="k-romaji">te</span></td><td><span class="k-char">と</span><span class="k-romaji">to</span></td></tr>
                        <tr><th>N</th><td><span class="k-char">な</span><span class="k-romaji">na</span></td><td><span class="k-char">に</span><span class="k-romaji">ni</span></td><td><span class="k-char">ぬ</span><span class="k-romaji">nu</span></td><td><span class="k-char">ね</span><span class="k-romaji">ne</span></td><td><span class="k-char">の</span><span class="k-romaji">no</span></td></tr>
                        <tr><th>H</th><td><span class="k-char">は</span><span class="k-romaji">ha</span></td><td><span class="k-char">ひ</span><span class="k-romaji">hi</span></td><td><span class="k-char">ふ</span><span class="k-romaji">fu</span></td><td><span class="k-char">へ</span><span class="k-romaji">he</span></td><td><span class="k-char">ほ</span><span class="k-romaji">ho</span></td></tr>
                        <tr><th>M</th><td><span class="k-char">ま</span><span class="k-romaji">ma</span></td><td><span class="k-char">み</span><span class="k-romaji">mi</span></td><td><span class="k-char">む</span><span class="k-romaji">mu</span></td><td><span class="k-char">め</span><span class="k-romaji">me</span></td><td><span class="k-char">も</span><span class="k-romaji">mo</span></td></tr>
                        <tr><th>Y</th><td><span class="k-char">や</span><span class="k-romaji">ya</span></td><td></td><td><span class="k-char">ゆ</span><span class="k-romaji">yu</span></td><td></td><td><span class="k-char">よ</span><span class="k-romaji">yo</span></td></tr>
                        <tr><th>R</th><td><span class="k-char">ら</span><span class="k-romaji">ra</span></td><td><span class="k-char">り</span><span class="k-romaji">ri</span></td><td><span class="k-char">る</span><span class="k-romaji">ru</span></td><td><span class="k-char">れ</span><span class="k-romaji">re</span></td><td><span class="k-char">ろ</span><span class="k-romaji">ro</span></td></tr>
                        <tr><th>W</th><td><span class="k-char">わ</span><span class="k-romaji">wa</span></td><td></td><td></td><td></td><td><span class="k-char">を</span><span class="k-romaji">wo</span></td></tr>
                        <tr><th>N</th><td><span class="k-char">ん</span><span class="k-romaji">n</span></td><td></td><td></td><td></td><td></td></tr>
                    </table>
                </div>

                <h4>Suoni Impuri (Dakuten / Handakuten)</h4>
                <div class="table-wrapper">
                    <table class="kana-table">
                        <tr><th></th><th>A</th><th>I</th><th>U</th><th>E</th><th>O</th></tr>
                        <tr><th>G</th><td><span class="k-char">が</span><span class="k-romaji">ga</span></td><td><span class="k-char">ぎ</span><span class="k-romaji">gi</span></td><td><span class="k-char">ぐ</span><span class="k-romaji">gu</span></td><td><span class="k-char">げ</span><span class="k-romaji">ge</span></td><td><span class="k-char">ご</span><span class="k-romaji">go</span></td></tr>
                        <tr><th>Z</th><td><span class="k-char">ざ</span><span class="k-romaji">za</span></td><td><span class="k-char">じ</span><span class="k-romaji">ji</span></td><td><span class="k-char">ず</span><span class="k-romaji">zu</span></td><td><span class="k-char">ぜ</span><span class="k-romaji">ze</span></td><td><span class="k-char">ぞ</span><span class="k-romaji">zo</span></td></tr>
                        <tr><th>D</th><td><span class="k-char">だ</span><span class="k-romaji">da</span></td><td><span class="k-char">ぢ</span><span class="k-romaji">ji</span></td><td><span class="k-char">づ</span><span class="k-romaji">zu</span></td><td><span class="k-char">で</span><span class="k-romaji">de</span></td><td><span class="k-char">ど</span><span class="k-romaji">do</span></td></tr>
                        <tr><th>B</th><td><span class="k-char">ば</span><span class="k-romaji">ba</span></td><td><span class="k-char">び</span><span class="k-romaji">bi</span></td><td><span class="k-char">ぶ</span><span class="k-romaji">bu</span></td><td><span class="k-char">べ</span><span class="k-romaji">be</span></td><td><span class="k-char">ぼ</span><span class="k-romaji">bo</span></td></tr>
                        <tr><th>P</th><td><span class="k-char">ぱ</span><span class="k-romaji">pa</span></td><td><span class="k-char">ぴ</span><span class="k-romaji">pi</span></td><td><span class="k-char">ぷ</span><span class="k-romaji">pu</span></td><td><span class="k-char">ぺ</span><span class="k-romaji">pe</span></td><td><span class="k-char">ぽ</span><span class="k-romaji">po</span></td></tr>
                    </table>
                </div>

                <h4>Suoni Contratti (Yoon)</h4>
                <div class="table-wrapper">
                    <table class="kana-table">
                        <tr><th></th><th>YA</th><th>YU</th><th>YO</th></tr>
                        <tr><th>K</th><td><span class="k-char">きゃ</span><span class="k-romaji">kya</span></td><td><span class="k-char">きゅ</span><span class="k-romaji">kyu</span></td><td><span class="k-char">きょ</span><span class="k-romaji">kyo</span></td></tr>
                        <tr><th>S</th><td><span class="k-char">しゃ</span><span class="k-romaji">sha</span></td><td><span class="k-char">しゅ</span><span class="k-romaji">shu</span></td><td><span class="k-char">しょ</span><span class="k-romaji">sho</span></td></tr>
                        <tr><th>C</th><td><span class="k-char">ちゃ</span><span class="k-romaji">cha</span></td><td><span class="k-char">ちゅ</span><span class="k-romaji">chu</span></td><td><span class="k-char">ちょ</span><span class="k-romaji">cho</span></td></tr>
                        <tr><th>N</th><td><span class="k-char">にゃ</span><span class="k-romaji">nya</span></td><td><span class="k-char">にゅ</span><span class="k-romaji">nyu</span></td><td><span class="k-char">にょ</span><span class="k-romaji">nyo</span></td></tr>
                        <tr><th>H</th><td><span class="k-char">ひゃ</span><span class="k-romaji">hya</span></td><td><span class="k-char">ひゅ</span><span class="k-romaji">hyu</span></td><td><span class="k-char">ひょ</span><span class="k-romaji">hyo</span></td></tr>
                        <tr><th>M</th><td><span class="k-char">みゃ</span><span class="k-romaji">mya</span></td><td><span class="k-char">みゅ</span><span class="k-romaji">myu</span></td><td><span class="k-char">みょ</span><span class="k-romaji">myo</span></td></tr>
                        <tr><th>R</th><td><span class="k-char">りゃ</span><span class="k-romaji">rya</span></td><td><span class="k-char">りゅ</span><span class="k-romaji">ryu</span></td><td><span class="k-char">りょ</span><span class="k-romaji">ryo</span></td></tr>
                        <tr><th>G</th><td><span class="k-char">ぎゃ</span><span class="k-romaji">gya</span></td><td><span class="k-char">ぎゅ</span><span class="k-romaji">gyu</span></td><td><span class="k-char">ぎょ</span><span class="k-romaji">gyo</span></td></tr>
                        <tr><th>J</th><td><span class="k-char">じゃ</span><span class="k-romaji">ja</span></td><td><span class="k-char">じゅ</span><span class="k-romaji">ju</span></td><td><span class="k-char">じょ</span><span class="k-romaji">jo</span></td></tr>
                        <tr><th>B</th><td><span class="k-char">びゃ</span><span class="k-romaji">bya</span></td><td><span class="k-char">びゅ</span><span class="k-romaji">byu</span></td><td><span class="k-char">びょ</span><span class="k-romaji">byo</span></td></tr>
                        <tr><th>P</th><td><span class="k-char">ぴゃ</span><span class="k-romaji">pya</span></td><td><span class="k-char">ぴゅ</span><span class="k-romaji">pyu</span></td><td><span class="k-char">ぴょ</span><span class="k-romaji">pyo</span></td></tr>
                    </table>
                </div>
            </div>
        </div> 

        <div id="modulo-katakana" class="modulo-content">
            <div class="card-ui">
                <h2>Alfabeto Katakana</h2>
                <div class="kana-config-panel">
                    <h3>Seleziona gruppi:</h3>
                    <div class="config-grid" id="k-config-grid"></div>
                    <div class="config-actions">
                        <button class="btn-small" onclick="toggleAllKana('katakana', true)">Seleziona Tutto</button>
                        <button class="btn-small" onclick="toggleAllKana('katakana', false)">Deseleziona</button>
                    </div>
                </div>
                <button class="btn btn-start-kana" onclick="avviaQuizKana('katakana')">🏋️ Avvia Quiz Katakana</button>

                <h4>Suoni Base (Gojūon)</h4>
                <div class="table-wrapper">
                    <table class="kana-table">
                         <tr><th></th><th>A</th><th>I</th><th>U</th><th>E</th><th>O</th></tr>
                        <tr><th>-</th><td><span class="k-char">ア</span><span class="k-romaji">a</span></td><td><span class="k-char">イ</span><span class="k-romaji">i</span></td><td><span class="k-char">ウ</span><span class="k-romaji">u</span></td><td><span class="k-char">エ</span><span class="k-romaji">e</span></td><td><span class="k-char">オ</span><span class="k-romaji">o</span></td></tr>
                        <tr><th>K</th><td><span class="k-char">カ</span><span class="k-romaji">ka</span></td><td><span class="k-char">キ</span><span class="k-romaji">ki</span></td><td><span class="k-char">ク</span><span class="k-romaji">ku</span></td><td><span class="k-char">ケ</span><span class="k-romaji">ke</span></td><td><span class="k-char">コ</span><span class="k-romaji">ko</span></td></tr>
                        <tr><th>S</th><td><span class="k-char">サ</span><span class="k-romaji">sa</span></td><td><span class="k-char">シ</span><span class="k-romaji">shi</span></td><td><span class="k-char">ス</span><span class="k-romaji">su</span></td><td><span class="k-char">セ</span><span class="k-romaji">se</span></td><td><span class="k-char">ソ</span><span class="k-romaji">so</span></td></tr>
                        <tr><th>T</th><td><span class="k-char">タ</span><span class="k-romaji">ta</span></td><td><span class="k-char">チ</span><span class="k-romaji">chi</span></td><td><span class="k-char">ツ</span><span class="k-romaji">tsu</span></td><td><span class="k-char">テ</span><span class="k-romaji">te</span></td><td><span class="k-char">ト</span><span class="k-romaji">to</span></td></tr>
                        <tr><th>N</th><td><span class="k-char">ナ</span><span class="k-romaji">na</span></td><td><span class="k-char">ニ</span><span class="k-romaji">ni</span></td><td><span class="k-char">ヌ</span><span class="k-romaji">nu</span></td><td><span class="k-char">ネ</span><span class="k-romaji">ne</span></td><td><span class="k-char">ノ</span><span class="k-romaji">no</span></td></tr>
                        <tr><th>H</th><td><span class="k-char">ハ</span><span class="k-romaji">ha</span></td><td><span class="k-char">ヒ</span><span class="k-romaji">hi</span></td><td><span class="k-char">フ</span><span class="k-romaji">fu</span></td><td><span class="k-char">ヘ</span><span class="k-romaji">he</span></td><td><span class="k-char">ホ</span><span class="k-romaji">ho</span></td></tr>
                        <tr><th>M</th><td><span class="k-char">マ</span><span class="k-romaji">ma</span></td><td><span class="k-char">ミ</span><span class="k-romaji">mi</span></td><td><span class="k-char">ム</span><span class="k-romaji">mu</span></td><td><span class="k-char">メ</span><span class="k-romaji">me</span></td><td><span class="k-char">モ</span><span class="k-romaji">mo</span></td></tr>
                        <tr><th>Y</th><td><span class="k-char">ヤ</span><span class="k-romaji">ya</span></td><td></td><td><span class="k-char">ユ</span><span class="k-romaji">yu</span></td><td></td><td><span class="k-char">ヨ</span><span class="k-romaji">yo</span></td></tr>
                        <tr><th>R</th><td><span class="k-char">ラ</span><span class="k-romaji">ra</span></td><td><span class="k-char">リ</span><span class="k-romaji">ri</span></td><td><span class="k-char">ル</span><span class="k-romaji">ru</span></td><td><span class="k-char">レ</span><span class="k-romaji">re</span></td><td><span class="k-char">ロ</span><span class="k-romaji">ro</span></td></tr>
                        <tr><th>W</th><td><span class="k-char">ワ</span><span class="k-romaji">wa</span></td><td></td><td></td><td></td><td><span class="k-char">ヲ</span><span class="k-romaji">wo</span></td></tr>
                        <tr><th>N</th><td><span class="k-char">ン</span><span class="k-romaji">n</span></td><td></td><td></td><td></td><td></td></tr>
                    </table>
                </div>

                <h4>Suoni Impuri (Dakuten / Handakuten)</h4>
                <div class="table-wrapper">
                    <table class="kana-table">
                        <tr><th></th><th>A</th><th>I</th><th>U</th><th>E</th><th>O</th></tr>
                        <tr><th>G</th><td><span class="k-char">ガ</span><span class="k-romaji">ga</span></td><td><span class="k-char">ギ</span><span class="k-romaji">gi</span></td><td><span class="k-char">グ</span><span class="k-romaji">gu</span></td><td><span class="k-char">ゲ</span><span class="k-romaji">ge</span></td><td><span class="k-char">ゴ</span><span class="k-romaji">go</span></td></tr>
                        <tr><th>Z</th><td><span class="k-char">ザ</span><span class="k-romaji">za</span></td><td><span class="k-char">ジ</span><span class="k-romaji">ji</span></td><td><span class="k-char">ズ</span><span class="k-romaji">zu</span></td><td><span class="k-char">ゼ</span><span class="k-romaji">ze</span></td><td><span class="k-char">ゾ</span><span class="k-romaji">zo</span></td></tr>
                        <tr><th>D</th><td><span class="k-char">ダ</span><span class="k-romaji">da</span></td><td><span class="k-char">ヂ</span><span class="k-romaji">ji</span></td><td><span class="k-char">ヅ</span><span class="k-romaji">zu</span></td><td><span class="k-char">デ</span><span class="k-romaji">de</span></td><td><span class="k-char">ド</span><span class="k-romaji">do</span></td></tr>
                        <tr><th>B</th><td><span class="k-char">バ</span><span class="k-romaji">ba</span></td><td><span class="k-char">ビ</span><span class="k-romaji">bi</span></td><td><span class="k-char">ブ</span><span class="k-romaji">bu</span></td><td><span class="k-char">ベ</span><span class="k-romaji">be</span></td><td><span class="k-char">ボ</span><span class="k-romaji">bo</span></td></tr>
                        <tr><th>P</th><td><span class="k-char">パ</span><span class="k-romaji">pa</span></td><td><span class="k-char">ピ</span><span class="k-romaji">pi</span></td><td><span class="k-char">プ</span><span class="k-romaji">pu</span></td><td><span class="k-char">ペ</span><span class="k-romaji">pe</span></td><td><span class="k-char">ポ</span><span class="k-romaji">po</span></td></tr>
                    </table>
                </div>

                <h4>Suoni Contratti (Yoon)</h4>
                <div class="table-wrapper">
                    <table class="kana-table">
                        <tr><th></th><th>YA</th><th>YU</th><th>YO</th></tr>
                        <tr><th>K</th><td><span class="k-char">キャ</span><span class="k-romaji">kya</span></td><td><span class="k-char">キュ</span><span class="k-romaji">kyu</span></td><td><span class="k-char">キョ</span><span class="k-romaji">kyo</span></td></tr>
                        <tr><th>S</th><td><span class="k-char">シャ</span><span class="k-romaji">sha</span></td><td><span class="k-char">シュ</span><span class="k-romaji">shu</span></td><td><span class="k-char">ショ</span><span class="k-romaji">sho</span></td></tr>
                        <tr><th>C</th><td><span class="k-char">チャ</span><span class="k-romaji">cha</span></td><td><span class="k-char">チュ</span><span class="k-romaji">chu</span></td><td><span class="k-char">チョ</span><span class="k-romaji">cho</span></td></tr>
                        <tr><th>N</th><td><span class="k-char">ニャ</span><span class="k-romaji">nya</span></td><td><span class="k-char">ニュ</span><span class="k-romaji">nyu</span></td><td><span class="k-char">ニョ</span><span class="k-romaji">nyo</span></td></tr>
                        <tr><th>H</th><td><span class="k-char">ヒャ</span><span class="k-romaji">hya</span></td><td><span class="k-char">ヒュ</span><span class="k-romaji">hyu</span></td><td><span class="k-char">ヒョ</span><span class="k-romaji">hyo</span></td></tr>
                        <tr><th>M</th><td><span class="k-char">ミャ</span><span class="k-romaji">mya</span></td><td><span class="k-char">ミュ</span><span class="k-romaji">myu</span></td><td><span class="k-char">ミョ</span><span class="k-romaji">myo</span></td></tr>
                        <tr><th>R</th><td><span class="k-char">リャ</span><span class="k-romaji">rya</span></td><td><span class="k-char">リュ</span><span class="k-romaji">ryu</span></td><td><span class="k-char">リョ</span><span class="k-romaji">ryo</span></td></tr>
                        <tr><th>G</th><td><span class="k-char">ギャ</span><span class="k-romaji">gya</span></td><td><span class="k-char">ギュ</span><span class="k-romaji">gyu</span></td><td><span class="k-char">ギョ</span><span class="k-romaji">gyo</span></td></tr>
                        <tr><th>J</th><td><span class="k-char">ジャ</span><span class="k-romaji">ja</span></td><td><span class="k-char">ジュ</span><span class="k-romaji">ju</span></td><td><span class="k-char">ジョ</span><span class="k-romaji">jo</span></td></tr>
                        <tr><th>B</th><td><span class="k-char">ビャ</span><span class="k-romaji">bya</span></td><td><span class="k-char">ビュ</span><span class="k-romaji">byu</span></td><td><span class="k-char">ビョ</span><span class="k-romaji">byo</span></td></tr>
                        <tr><th>P</th><td><span class="k-char">ピャ</span><span class="k-romaji">pya</span></td><td><span class="k-char">ピュ</span><span class="k-romaji">pyu</span></td><td><span class="k-char">ピョ</span><span class="k-romaji">pyo</span></td></tr>
                    </table>
                </div>
            </div>
        </div> 
        
        <div id="modulo-vocaboli" class="modulo-content">
            <div class="card-ui">
                <h2>Lista Vocaboli</h2>
                <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:10px;">
                    <p id="vocaboli-count" style="font-weight:bold;"></p>
                    <button id="copia-vocaboli-btn" style="background:transparent; border:1px solid #ddd; padding:5px; cursor:pointer; border-radius:4px; font-size:0.8rem;">📋 Copia CSV</button>
                </div>
                <div style="text-align:right; margin-bottom:10px;">
                    <button id="svuota-lista-btn" style="background-color: #ff3b30; color:white; border:none; padding:8px 12px; border-radius:6px; cursor:pointer; font-weight:bold;">🗑 CANCELLA TUTTO</button>
                </div>
                <div id="lista-vocaboli-container"></div>
            </div>
        </div> 
    </main>

    <script>
        // --- DATASETS KANA ESTESI ---
        const HIRAGANA_DATA = [
            {k:'あ',r:'a'}, {k:'い',r:'i'}, {k:'う',r:'u'}, {k:'え',r:'e'}, {k:'お',r:'o'},
            {k:'か',r:'ka'}, {k:'き',r:'ki'}, {k:'く',r:'ku'}, {k:'け',r:'ke'}, {k:'こ',r:'ko'},
            {k:'さ',r:'sa'}, {k:'し',r:'shi'}, {k:'す',r:'su'}, {k:'せ',r:'se'}, {k:'そ',r:'so'},
            {k:'た',r:'ta'}, {k:'ち',r:'chi'}, {k:'つ',r:'tsu'}, {k:'て',r:'te'}, {k:'と',r:'to'},
            {k:'な',r:'na'}, {k:'に',r:'ni'}, {k:'ぬ',r:'nu'}, {k:'ね',r:'ne'}, {k:'の',r:'no'},
            {k:'は',r:'ha'}, {k:'ひ',r:'hi'}, {k:'ふ',r:'fu'}, {k:'へ',r:'he'}, {k:'ほ',r:'ho'},
            {k:'ま',r:'ma'}, {k:'み',r:'mi'}, {k:'む',r:'mu'}, {k:'め',r:'me'}, {k:'も',r:'mo'},
            {k:'や',r:'ya'}, {k:'ゆ',r:'yu'}, {k:'よ',r:'yo'},
            {k:'ら',r:'ra'}, {k:'り',r:'ri'}, {k:'る',r:'ru'}, {k:'れ',r:'re'}, {k:'ろ',r:'ro'},
            {k:'わ',r:'wa'}, {k:'を',r:'wo'}, {k:'ん',r:'n'},
            {k:'が',r:'ga'}, {k:'ぎ',r:'gi'}, {k:'ぐ',r:'gu'}, {k:'げ',r:'ge'}, {k:'ご',r:'go'},
            {k:'ざ',r:'za'}, {k:'じ',r:'ji'}, {k:'ず',r:'zu'}, {k:'ぜ',r:'ze'}, {k:'ぞ',r:'zo'},
            {k:'だ',r:'da'}, {k:'ぢ',r:'ji'}, {k:'づ',r:'zu'}, {k:'で',r:'de'}, {k:'ど',r:'do'}, 
            {k:'ば',r:'ba'}, {k:'び',r:'bi'}, {k:'ぶ',r:'bu'}, {k:'べ',r:'be'}, {k:'ぼ',r:'bo'},
            {k:'ぱ',r:'pa'}, {k:'ぴ',r:'pi'}, {k:'ぷ',r:'pu'}, {k:'ぺ',r:'pe'}, {k:'ぽ',r:'po'},
            {k:'きゃ',r:'kya'}, {k:'きゅ',r:'kyu'}, {k:'きょ',r:'kyo'}, {k:'しゃ',r:'sha'}, {k:'しゅ',r:'shu'}, {k:'しょ',r:'sho'},
            {k:'ちゃ',r:'cha'}, {k:'ちゅ',r:'chu'}, {k:'ちょ',r:'cho'}, {k:'にゃ',r:'nya'}, {k:'にゅ',r:'nyu'}, {k:'にょ',r:'nyo'},
            {k:'ひゃ',r:'hya'}, {k:'ひゅ',r:'hyu'}, {k:'ひょ',r:'hyo'}, {k:'みゃ',r:'mya'}, {k:'みゅ',r:'myu'}, {k:'みょ',r:'myo'},
            {k:'りゃ',r:'rya'}, {k:'りゅ',r:'ryu'}, {k:'りょ',r:'ryo'}, {k:'ぎゃ',r:'gya'}, {k:'ぎゅ',r:'gyu'}, {k:'ぎょ',r:'gyo'},
            {k:'じゃ',r:'ja'}, {k:'じゅ',r:'ju'}, {k:'じょ',r:'jo'}, {k:'びゃ',r:'bya'}, {k:'びゅ',r:'byu'}, {k:'びょ',r:'byo'},
            {k:'ぴゃ',r:'pya'}, {k:'ぴゅ',r:'pyu'}, {k:'ぴょ',r:'pyo'}
        ];

        const KATAKANA_DATA = [
            {k:'ア',r:'a'}, {k:'イ',r:'i'}, {k:'ウ',r:'u'}, {k:'エ',r:'e'}, {k:'オ',r:'o'},
            {k:'カ',r:'ka'}, {k:'キ',r:'ki'}, {k:'ク',r:'ku'}, {k:'ケ',r:'ke'}, {k:'コ',r:'ko'},
            {k:'サ',r:'sa'}, {k:'シ',r:'shi'}, {k:'ス',r:'su'}, {k:'セ',r:'se'}, {k:'ソ',r:'so'},
            {k:'タ',r:'ta'}, {k:'チ',r:'chi'}, {k:'ツ',r:'tsu'}, {k:'テ',r:'te'}, {k:'ト',r:'to'},
            {k:'ナ',r:'na'}, {k:'ニ',r:'ni'}, {k:'ヌ',r:'nu'}, {k:'ネ',r:'ne'}, {k:'ノ',r:'no'},
            {k:'ハ',r:'ha'}, {k:'ヒ',r:'hi'}, {k:'フ',r:'fu'}, {k:'ヘ',r:'he'}, {k:'ホ',r:'ho'},
            {k:'マ',r:'ma'}, {k:'ミ',r:'mi'}, {k:'ム',r:'mu'}, {k:'メ',r:'me'}, {k:'モ',r:'mo'},
            {k:'ヤ',r:'ya'}, {k:'ユ',r:'yu'}, {k:'ヨ',r:'yo'},
            {k:'ラ',r:'ra'}, {k:'リ',r:'ri'}, {k:'ル',r:'ru'}, {k:'レ',r:'re'}, {k:'ロ',r:'ro'},
            {k:'ワ',r:'wa'}, {k:'ヲ',r:'wo'}, {k:'ン',r:'n'},
            {k:'ガ',r:'ga'}, {k:'ギ',r:'gi'}, {k:'グ',r:'gu'}, {k:'ゲ',r:'ge'}, {k:'ゴ',r:'go'},
            {k:'ザ',r:'za'}, {k:'ジ',r:'ji'}, {k:'ズ',r:'zu'}, {k:'ゼ',r:'ze'}, {k:'ゾ',r:'zo'},
            {k:'ダ',r:'da'}, {k:'ヂ',r:'ji'}, {k:'ヅ',r:'zu'}, {k:'デ',r:'de'}, {k:'ド',r:'do'},
            {k:'バ',r:'ba'}, {k:'ビ',r:'bi'}, {k:'ブ',r:'bu'}, {k:'ベ',r:'be'}, {k:'ボ',r:'bo'},
            {k:'パ',r:'pa'}, {k:'ピ',r:'pi'}, {k:'プ',r:'pu'}, {k:'ペ',r:'pe'}, {k:'ポ',r:'po'},
            {k:'キャ',r:'kya'}, {k:'キュ',r:'kyu'}, {k:'キョ',r:'kyo'}, {k:'シャ',r:'sha'}, {k:'シュ',r:'shu'}, {k:'ショ',r:'sho'},
            {k:'チャ',r:'cha'}, {k:'チュ',r:'chu'}, {k:'チョ',r:'cho'}, {k:'ニャ',r:'nya'}, {k:'ニュ',r:'nyu'}, {k:'ニョ',r:'nyo'},
            {k:'ヒャ',r:'hya'}, {k:'ヒュ',r:'hyu'}, {k:'ヒョ',r:'hyo'}, {k:'ミャ',r:'mya'}, {k:'ミュ',r:'myu'}, {k:'ミョ',r:'myo'},
            {k:'リャ',r:'rya'}, {k:'リュ',r:'ryu'}, {k:'リョ',r:'ryo'}, {k:'ギャ',r:'gya'}, {k:'ギュ',r:'gyu'}, {k:'ギョ',r:'gyo'},
            {k:'ジャ',r:'ja'}, {k:'ジュ',r:'ju'}, {k:'ジョ',r:'jo'}, {k:'ビャ',r:'bya'}, {k:'ビュ',r:'byu'}, {k:'ビョ',r:'byo'},
            {k:'ピャ',r:'pya'}, {k:'ピュ',r:'pyu'}, {k:'ピョ',r:'pyo'}
        ];

        const KANA_ROWS = {
            'Vocali (A-O)': [0, 5],
            'Riga K': [5, 10],
            'Riga S': [10, 15],
            'Riga T': [15, 20],
            'Riga N': [20, 25],
            'Riga H': [25, 30],
            'Riga M': [30, 35],
            'Riga Y': [35, 38],
            'Riga R': [38, 43],
            'Riga W/N': [43, 46],
            'Dakuten (Ga, Za, Da, Ba)': [46, 66],
            'Handakuten (Pa)': [66, 71],
            'Yoon Base (Kya, Sha...)': [71, 92],
            'Yoon Daku/Handaku (Gya...)': [92, 104]
        };

        // --- VARIABILI DI STATO ---
        let mazzoPrincipale = [];
        let mazzoErroriPrioritari = [];
        let mazzoBacklog = []; 
        let mazzoSessioneCorrente = []; 
        let subsetKanaAttivo = []; 
        let erroriSessioneCorrente = new Set();       
        let mazzoRipassoAttivo = [];
        let indiceSessione = 0;           
        let parolaCorrente = null;
        let quizDirection = 'ITA_TO_JPN';
        let modalitaQuiz = 'normale'; 
        let sessioneCorretti = 0;
        let sessioneSbagliati = 0;

        const KEY_MAZZO_PRINCIPALE = 'mioMazzoPrincipale';
        const KEY_MAZZO_ERRORI = 'mioMazzoErrori';
        const KEY_HIRAGANA_CONFIG = 'mioConfigurazioneHiragana';
        const KEY_KATAKANA_CONFIG = 'mioConfigurazioneKatakana';
        
        // --- AUDIO ---
        const synth = window.speechSynthesis;
        let japaneseVoice = null;

        document.addEventListener('DOMContentLoaded', () => {
            initAudio();
            generaPannelloConfigurazione('hiragana');
            generaPannelloConfigurazione('katakana');
            caricaMazzi();
            setupEventListeners();
            mostraModulo('quiz');
        });

        function initAudio() {
            const loadVoices = () => {
                const voices = synth.getVoices();
                japaneseVoice = voices.find(v => v.lang.startsWith('ja')) || null;
            };
            if (synth.onvoiceschanged !== undefined) {
                synth.onvoiceschanged = loadVoices;
            }
            loadVoices();
        }

        function setupEventListeners() {
            const btnControlla = document.getElementById('pulsante-controlla');
            const btnProssima = document.getElementById('pulsante-prossima');
            const btnElimina = document.getElementById('pulsante-elimina');

            btnControlla.addEventListener('click', controllaRisposta);
            btnProssima.addEventListener('click', prossimaParola);
            document.getElementById('input-risposta').addEventListener('keydown', (e) => {
                if (e.key === 'Enter') {
                    if (btnControlla.style.display !== 'none') controllaRisposta();
                    else if (btnProssima.style.display !== 'none') prossimaParola();
                }
            });
            
            document.getElementById('main-nav').addEventListener('click', (e) => {
                if (e.target.closest('.nav-btn')) {
                    const btn = e.target.closest('.nav-btn');
                    const targetModulo = btn.dataset.modulo;
                    
                    if (targetModulo === 'quiz' && (modalitaQuiz === 'hiragana_mode' || modalitaQuiz === 'katakana_mode')) {
                        modalitaQuiz = 'normale';
                        document.getElementById('form-container').style.display = 'block'; 
                        btnElimina.style.display = 'block';
                        document.getElementById('virtual-keyboard').style.display = 'none';
                        document.getElementById('filtro-categoria').parentElement.style.display = 'block'; 
                        caricaMazzi(); 
                    }
                    mostraModulo(targetModulo);
                }
            });
            
            document.getElementById('salva-sessione').addEventListener('click', () => {
                if(modalitaQuiz === 'hiragana_mode' || modalitaQuiz === 'katakana_mode') {
                    alert("Sessione Kana terminata!");
                    location.reload();
                } else {
                    salvaErroriSessione();
                }
            });

            document.getElementById('ripassa-errori-btn').addEventListener('click', avviaRipassoErrori);
            
            btnElimina.addEventListener('click', () => {
                if(modalitaQuiz.includes('mode') && modalitaQuiz !== 'normale' && modalitaQuiz !== 'ripasso_errori') { 
                    alert("Non puoi eliminare lettere dell'alfabeto!"); return; 
                }
                eliminaParola(parolaCorrente.ita);
            });
            
            document.getElementById('reset-dati-btn').addEventListener('click', ripristinaDatiVocaboli);
            document.getElementById('form-aggiungi').addEventListener('submit', gestisciSalvataggioForm);
            document.getElementById('import-csv-file').addEventListener('change', gestisciImportaCSV);
            document.getElementById('update-url-btn').addEventListener('click', gestisciAggiornaDaUrl);
            document.getElementById('svuota-tutto-btn').addEventListener('click', svuotaMazziTotali);
            document.getElementById('svuota-lista-btn').addEventListener('click', svuotaMazziTotali);
            document.getElementById('copia-vocaboli-btn').addEventListener('click', copiaVocaboli);
        }

        function ripristinaDatiVocaboli() {
            // Nota: DATI_INIZIALI_CSV è vuoto in questa versione clean.
            // Questa funzione serve se l'utente incolla i dati nel codice.
            if (!DATI_INIZIALI_CSV || DATI_INIZIALI_CSV.trim() === "") {
                alert("Nessun dato di default trovato nel codice. Usa 'Importa da file locale' per caricare i tuoi vocaboli.");
                return;
            }
            if(confirm("Questo caricherà i dati predefiniti. Vuoi procedere?")) {
                importaDatiDaStringa(DATI_INIZIALI_CSV);
            }
        }

        function importaDatiDaStringa(csvText) {
             const lines = csvText.split('\n');
             lines.forEach(line => {
                if(!line.trim()) return;
                const c = parseCSVLine(line);
                if(c.length < 4) return;
                mazzoPrincipale.push({ita:c[0], eng:c[1], jpn:c[2], romaji:c[3], tag:c[4]||"Generale", esempi:c[5]||"", level:0, type:'vocab'});
             });
             salvaMazzoPrincipale(); 
             location.reload();
        }

        // --- GESTIONE KANA ---
        function generaPannelloConfigurazione(type) {
            const containerId = type === 'hiragana' ? 'h-config-grid' : 'k-config-grid';
            const storageKey = type === 'hiragana' ? KEY_HIRAGANA_CONFIG : KEY_KATAKANA_CONFIG;
            const container = document.getElementById(containerId);
            container.innerHTML = '';
            
            const savedConfig = JSON.parse(localStorage.getItem(storageKey) || '{}');
            
            for (const [label, range] of Object.entries(KANA_ROWS)) {
                const isChecked = savedConfig[label] !== false; 
                const div = document.createElement('label');
                div.className = 'checkbox-label';
                div.innerHTML = `<input type="checkbox" value="${label}" ${isChecked ? 'checked' : ''} onchange="salvaConfigurazioneKana('${type}')"> ${label}`;
                container.appendChild(div);
            }
        }

        function salvaConfigurazioneKana(type) {
            const containerId = type === 'hiragana' ? 'h-config-grid' : 'k-config-grid';
            const storageKey = type === 'hiragana' ? KEY_HIRAGANA_CONFIG : KEY_KATAKANA_CONFIG;
            const checkboxes = document.querySelectorAll(`#${containerId} input[type="checkbox"]`);
            const config = {};
            checkboxes.forEach(cb => { config[cb.value] = cb.checked; });
            localStorage.setItem(storageKey, JSON.stringify(config));
        }

        function toggleAllKana(type, stato) {
            const containerId = type === 'hiragana' ? 'h-config-grid' : 'k-config-grid';
            const checkboxes = document.querySelectorAll(`#${containerId} input[type="checkbox"]`);
            checkboxes.forEach(cb => cb.checked = stato);
            salvaConfigurazioneKana(type);
        }

        function getKanaSubset(type) {
            const containerId = type === 'hiragana' ? 'h-config-grid' : 'k-config-grid';
            const dataset = type === 'hiragana' ? HIRAGANA_DATA : KATAKANA_DATA;
            const checkboxes = document.querySelectorAll(`#${containerId} input[type="checkbox"]`);
            let subset = [];
            checkboxes.forEach(cb => {
                if (cb.checked) {
                    const range = KANA_ROWS[cb.value];
                    subset = subset.concat(dataset.slice(range[0], range[1]));
                }
            });
            return subset;
        }

        function generaTastieraVirtuale() {
            const vk = document.getElementById('virtual-keyboard');
            vk.innerHTML = '';
            
            let caratteriDaMostrare = subsetKanaAttivo.length > 0 ? subsetKanaAttivo : (modalitaQuiz.startsWith('hiragana') ? HIRAGANA_DATA : KATAKANA_DATA);
            let listaTasti = caratteriDaMostrare.map(x => x.k);
            listaTasti = shuffleArray(listaTasti);
            
            listaTasti.forEach(char => {
               const btn = document.createElement('button');
               btn.className = 'key-btn';
               btn.textContent = char;
               btn.onclick = () => { 
                   const inp = document.getElementById('input-risposta');
                   inp.value += char; 
                   inp.focus(); 
                };
               vk.appendChild(btn);
            });
            vk.style.gridTemplateColumns = 'repeat(5, 1fr)';
        }

        function avviaQuizKana(type) {
            const subset = getKanaSubset(type);
            if (subset.length === 0) { alert("Seleziona almeno una riga!"); return; }
            subsetKanaAttivo = subset;
            const mazzoKana = subset.map(char => ({
                ita: char.r, eng: char.r, jpn: char.k, romaji: char.r, level: 0, type: type 
            }));
            mazzoSessioneCorrente = shuffleArray(mazzoKana);
            indiceSessione = 0; sessioneCorretti = 0; sessioneSbagliati = 0;
            erroriSessioneCorrente = new Set(); modalitaQuiz = type + '_mode'; 
            mostraModulo('quiz'); 
            document.getElementById('form-container').style.display = 'none'; 
            document.getElementById('pulsante-elimina').style.display = 'none';
            document.getElementById('filtro-categoria').parentElement.style.display = 'none'; 
            generaTastieraVirtuale();
            prossimaParola();
        }

        function prossimaParola() {
            const btnControlla = document.getElementById('pulsante-controlla');
            const btnProssima = document.getElementById('pulsante-prossima');
            const inputRisposta = document.getElementById('input-risposta');

            if (modalitaQuiz === 'set_finito') {
                if (mazzoBacklog.length > 0) { caricaProssimoBatch(); return; } 
                else { modalitaQuiz = 'normale'; caricaMazzi(); return; }
            }

            document.getElementById('prompt-container').style.display = 'block';
            inputRisposta.style.display = 'block';
            document.getElementById('risultato-controllo').style.display = 'block';
            document.getElementById('esempio-display').innerHTML = ''; 
            inputRisposta.value = ""; 
            document.getElementById('risultato-controllo').innerHTML = "";
            inputRisposta.disabled = false; btnControlla.style.display = "block"; btnProssima.style.display = "none";
            document.getElementById('virtual-keyboard').style.display = 'none'; 

            let etichetta = "";
            const isKanaMode = modalitaQuiz.includes('_mode');

            if (isKanaMode || (modalitaQuiz === 'ripasso_errori' && parolaCorrente && (parolaCorrente.type === 'hiragana' || parolaCorrente.type === 'katakana'))) {
                if (isKanaMode) {
                    if (indiceSessione >= mazzoSessioneCorrente.length) {
                        document.getElementById('prompt-principale').innerHTML = "🎉 Fine Pratica!";
                        document.getElementById('prompt-secondario').textContent = `Punteggio: ${sessioneCorretti}/${mazzoSessioneCorrente.length}`;
                        inputRisposta.style.display = 'none'; btnControlla.style.display = 'none';
                        return;
                    }
                    parolaCorrente = mazzoSessioneCorrente[indiceSessione];
                    indiceSessione++;
                    etichetta = `${parolaCorrente.type.toUpperCase()} (${indiceSessione}/${mazzoSessioneCorrente.length})`;
                } else {
                    if (mazzoRipassoAttivo.length > 0) {
                        parolaCorrente = mazzoRipassoAttivo.shift();
                        etichetta = `RIPASSO (${mazzoRipassoAttivo.length + 1})`;
                    } else {
                        alert("Ripasso finito!"); modalitaQuiz = 'normale'; caricaMazzi(); return;
                    }
                }
                
                if (Math.random() < 0.5) {
                    quizDirection = 'JPN_TO_ITA';
                    document.getElementById('prompt-label').innerHTML = `${etichetta} - Scrivi il Romaji`;
                    const cssClass = parolaCorrente.type === 'hiragana' ? 'hiragana' : 'katakana';
                    document.getElementById('prompt-principale').innerHTML = `<span class="${cssClass}">${parolaCorrente.jpn}</span>`;
                    document.getElementById('prompt-secondario').textContent = "";
                } else {
                    quizDirection = 'ITA_TO_JPN'; 
                    document.getElementById('prompt-label').innerHTML = `${etichetta} - Che carattere è?`;
                    document.getElementById('prompt-principale').textContent = parolaCorrente.romaji;
                    document.getElementById('prompt-secondario').textContent = "(Usa la tastiera qui sotto)";
                    generaTastieraVirtuale(); 
                    document.getElementById('virtual-keyboard').style.display = 'grid'; 
                }

            } else {
                if (modalitaQuiz === 'ripasso_errori') {
                    if (mazzoRipassoAttivo.length > 0) {
                        parolaCorrente = mazzoRipassoAttivo.shift();
                        etichetta = `RIPASSO (${mazzoRipassoAttivo.length + 1})`;
                        if(parolaCorrente.type !== 'vocab') { mazzoRipassoAttivo.unshift(parolaCorrente); setTimeout(prossimaParola, 0); return; }
                    } else { alert("Ripasso finito!"); modalitaQuiz = 'normale'; caricaMazzi(); return; }
                } else {
                    if (indiceSessione >= mazzoSessioneCorrente.length) {
                        if (mazzoSessioneCorrente.length === 0) {
                            document.getElementById('nessuna-carta').style.display = 'block';
                            document.querySelector('.controlli').style.display = 'none'; return;
                        }
                        modalitaQuiz = 'set_finito';
                        document.getElementById('prompt-principale').innerHTML = "Set Completato!";
                        if(mazzoBacklog.length > 0) {
                            document.getElementById('prompt-secondario').innerHTML = `Ne rimangono altre ${mazzoBacklog.length}.`;
                            btnProssima.textContent = "Carica prossime 10";
                        } else {
                            document.getElementById('prompt-secondario').innerHTML = "Hai finito tutte le parole!";
                            btnProssima.textContent = "Ricomincia";
                        }
                        inputRisposta.style.display = 'none'; btnControlla.style.display = 'none'; btnProssima.style.display = 'block';
                        return;
                    }
                    parolaCorrente = mazzoSessioneCorrente[indiceSessione];
                    indiceSessione++;
                    etichetta = `Set ${indiceSessione}/10`;
                }

                const stelle = "⭐".repeat(parolaCorrente.level || 0);
                const jpnClean = parolaCorrente.jpn.split('/')[0];
                const tagInfo = parolaCorrente.tag ? `<br><small style="color:#aaa; font-size:0.6rem;">${parolaCorrente.tag}</small>` : '';
                
                if (Math.random() < 0.5) {
                    quizDirection = 'ITA_TO_JPN';
                    document.getElementById('prompt-label').innerHTML = `TRADUCI ${etichetta} <small>${stelle}</small>`;
                    document.getElementById('prompt-principale').textContent = parolaCorrente.ita.split('/')[0];
                    document.getElementById('prompt-secondario').innerHTML = parolaCorrente.eng.split('/')[0] + tagInfo;
                } else {
                    quizDirection = 'JPN_TO_ITA';
                    document.getElementById('prompt-label').innerHTML = `TRADUCI ${etichetta} <small>${stelle}</small>`;
                    const btnAudio = `<button class="btn-audio" onclick="parla('${jpnClean}')">🔊</button>`;
                    document.getElementById('prompt-principale').innerHTML = `${colorizeJapanese(jpnClean)} ${btnAudio}`;
                    document.getElementById('prompt-secondario').innerHTML = parolaCorrente.romaji.split('/')[0] + tagInfo;
                }
            }
            aggiornaPunteggio();
        }

        function controllaRisposta() {
            if (!parolaCorrente) return;
            const inputRisposta = document.getElementById('input-risposta');
            const risp = inputRisposta.value.trim().toLowerCase();
            let ok = false; let err = "";
            
            inputRisposta.disabled = true; 
            document.getElementById('pulsante-controlla').style.display = 'none'; 
            document.getElementById('pulsante-prossima').style.display = 'block'; 
            document.getElementById('virtual-keyboard').style.display = 'none'; 
            
            const jpnClean = parolaCorrente.jpn.split('/')[0];
            const audioBtn = `<button class="btn-audio" onclick="parla('${jpnClean}')">🔊</button>`;

            if (parolaCorrente.type === 'hiragana' || parolaCorrente.type === 'katakana') {
                if (quizDirection === 'ITA_TO_JPN') { ok = (risp === parolaCorrente.jpn); err = `Risposta: <b>${parolaCorrente.jpn}</b>`; } 
                else { ok = (risp === parolaCorrente.romaji); err = `Risposta: <b>${parolaCorrente.romaji}</b>`; }
            } else {
                if (quizDirection === 'ITA_TO_JPN') {
                    const validi = [...parolaCorrente.romaji.split('/'), ...parolaCorrente.jpn.split('/')].map(s=>s.trim().toLowerCase());
                    ok = validi.includes(risp);
                    err = `Risposta: <b>${parolaCorrente.romaji}</b> (${colorizeJapanese(jpnClean)} ${audioBtn})`;
                } else {
                    const validi = [...parolaCorrente.ita.split('/'), ...parolaCorrente.eng.split('/')].map(s=>s.trim().toLowerCase());
                    ok = validi.includes(risp);
                    err = `Risposta: <b>${parolaCorrente.ita}</b>`;
                }
            }

            if (ok) {
                document.getElementById('risultato-controllo').innerHTML = `<span class="corretto">Corretto!</span>`; sessioneCorretti++;
                if (erroriSessioneCorrente.has(parolaCorrente)) { erroriSessioneCorrente.delete(parolaCorrente); }
                if(parolaCorrente.type === 'vocab') {
                    parolaCorrente.level = (parolaCorrente.level || 0) + 1;
                    if(parolaCorrente.level > 5) parolaCorrente.level = 5;
                    if(quizDirection === 'ITA_TO_JPN') parla(jpnClean);
                }
            } else {
                document.getElementById('risultato-controllo').innerHTML = `<span class="sbagliato">Sbagliato.</span><br>${err}`; sessioneSbagliati++;
                erroriSessioneCorrente.add(parolaCorrente); 
                if(parolaCorrente.type === 'vocab') { parolaCorrente.level = 0; }
            }
            
            if(parolaCorrente.esempi) { document.getElementById('esempio-display').innerHTML = `📝 <i>${parolaCorrente.esempi}</i>`; }
            aggiornaPunteggio();
        }

        function parseCSVLine(text) {
            const re = /(?:\"([^\"]*(?:\"\"[^\"]*)*)\")|([^\",]+)/g;
            const cols = []; let match;
            while (match = re.exec(text)) {
                let val = match[1] || match[2] || ""; val = val.replace(/""/g, '"').trim(); cols.push(val);
            }
            return cols;
        }

        function aggiornaPunteggio() {
            let html = `<span class="punteggio-info">Corretti: ${sessioneCorretti} | Errori: ${sessioneSbagliati}</span>`;
            if (!modalitaQuiz.includes('_mode')) { html += `<br><span style="font-size:0.8em">In attesa: ${mazzoBacklog.length} | Da Rivedere: ${mazzoErroriPrioritari.length}</span>`; }
            document.getElementById('punteggio-container').innerHTML = html;
            const btnRipassa = document.getElementById('ripassa-errori-btn');
            const numErrori = erroriSessioneCorrente.size;
            btnRipassa.textContent = `Ripassa Errori (${numErrori})`;
            btnRipassa.disabled = (numErrori === 0);
        }

        function parla(txt) {
            if (!synth) return;
            const u = new SpeechSynthesisUtterance(txt);
            if(japaneseVoice) u.voice = japaneseVoice; else u.lang = 'ja-JP';
            synth.speak(u);
        }

        function colorizeJapanese(text) {
            let out = '';
            for (let c of text) {
                const code = c.codePointAt(0);
                if (code >= 0x3040 && code <= 0x309F) out += `<span class="hiragana">${c}</span>`;
                else if (code >= 0x30A0 && code <= 0x30FF) out += `<span class="katakana">${c}</span>`;
                else if (code >= 0x4E00 && code <= 0x9FFF) out += `<span class="kanji">${c}</span>`;
                else out += c;
            }
            return out;
        }
        
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        function caricaMazzi() {
            const dP = localStorage.getItem(KEY_MAZZO_PRINCIPALE);
            mazzoPrincipale = dP ? JSON.parse(dP) : [];
            const dE = localStorage.getItem(KEY_MAZZO_ERRORI);
            mazzoErroriPrioritari = dE ? JSON.parse(dE) : [];
            mazzoPrincipale.forEach(c => { if(typeof c.level==='undefined') c.level=0; c.type = 'vocab'; });

            // DATI_INIZIALI_CSV è vuoto qui (clean version), quindi non fa nulla se il mazzo è vuoto.
            aggiornaFiltroCategorie(); creaNuovoSet();
            if(modalitaQuiz === 'normale') {
                if(mazzoSessioneCorrente.length === 0 && (mazzoPrincipale.length > 0 || mazzoErroriPrioritari.length > 0)) {
                    document.getElementById('nessuna-carta').style.display = 'block';
                    document.querySelector('.controlli').style.display = 'none';
                    document.getElementById('input-risposta').style.display = 'none';
                    document.getElementById('prompt-container').style.display = 'none';
                } else {
                    document.getElementById('nessuna-carta').style.display = 'none';
                    document.querySelector('.controlli').style.display = 'flex';
                    document.getElementById('input-risposta').style.display = 'block';
                    document.getElementById('prompt-container').style.display = 'block';
                    prossimaParola();
                }
            }
        }
        
        function aggiornaFiltroCategorie() {
            const currentSelection = filtroCategoria.value;
            const allTags = new Set();
            mazzoPrincipale.forEach(c => { if(c.tag) allTags.add(c.tag); });
            filtroCategoria.innerHTML = '<option value="TUTTI">📚 TUTTI I VOCABOLI</option>';
            Array.from(allTags).sort().forEach(tag => {
                const opt = document.createElement('option'); opt.value = tag; opt.textContent = `📂 ${tag}`; filtroCategoria.appendChild(opt);
            });
            if(currentSelection && Array.from(filtroCategoria.options).some(o => o.value === currentSelection)) { filtroCategoria.value = currentSelection; }
        }

        function cambiaCategoriaQuiz() { caricaMazzi(); }
        
        function creaNuovoSet() {
            mazzoBacklog = []; mazzoSessioneCorrente = []; indiceSessione = 0; modalitaQuiz = 'normale'; 
            if (mazzoPrincipale.length === 0 && mazzoErroriPrioritari.length === 0) return;
            const categoriaScelta = filtroCategoria.value;
            let filteredMain = mazzoPrincipale;
            if (categoriaScelta !== 'TUTTI') { filteredMain = mazzoPrincipale.filter(c => c.tag === categoriaScelta); }
            const sortedMain = [...filteredMain].sort((a,b) => (a.level||0) - (b.level||0));
            let pool = [...mazzoErroriPrioritari];
            if(categoriaScelta !== 'TUTTI') { pool = pool.filter(c => c.tag === categoriaScelta); }
            mazzoBacklog = [...pool, ...sortedMain];
            caricaProssimoBatch();
        }

        function caricaProssimoBatch() {
            modalitaQuiz = 'normale'; indiceSessione = 0;
            const batchSize = 10;
            mazzoSessioneCorrente = mazzoBacklog.splice(0, batchSize);
            mazzoSessioneCorrente = shuffleArray(mazzoSessioneCorrente);
            prossimaParola();
        }

        function salvaErroriSessione() {
            const erroriVocaboli = Array.from(erroriSessioneCorrente).filter(c => c.type === 'vocab');
            const nuovi = new Set([...mazzoErroriPrioritari, ...erroriVocaboli]);
            localStorage.setItem(KEY_MAZZO_ERRORI, JSON.stringify(Array.from(nuovi)));
            salvaMazzoPrincipale(); location.reload();
        }

        function salvaMazzoPrincipale() {
             const map = new Map();
             [...mazzoPrincipale, ...mazzoErroriPrioritari, ...mazzoSessioneCorrente, ...mazzoBacklog].forEach(item => { if(item.type === 'vocab') map.set(item.ita, item); });
             const errors = new Set(mazzoErroriPrioritari.map(i=>i.ita));
             const cleanMain = Array.from(map.values()).filter(i => !errors.has(i.ita));
             localStorage.setItem(KEY_MAZZO_PRINCIPALE, JSON.stringify(cleanMain));
        }

        function getUniqueTotalCountFromLocalStorage() {
            const p = JSON.parse(localStorage.getItem(KEY_MAZZO_PRINCIPALE)||'[]');
            const e = JSON.parse(localStorage.getItem(KEY_MAZZO_ERRORI)||'[]');
            return new Set([...p.map(x=>x.ita), ...e.map(x=>x.ita)]).size;
        }
        
        function mostraModulo(id) {
            document.querySelectorAll('.modulo-content').forEach(m => m.style.display = 'none');
            document.getElementById('modulo-'+id).style.display = 'block';
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.toggle('active', b.dataset.modulo === id));
            if (id === 'vocaboli') mostraListaVocaboli();
        }

        function gestisciSalvataggioForm(e) {
            e.preventDefault();
            const n = {
                ita:document.getElementById('input-ita').value, eng:document.getElementById('input-eng').value,
                jpn:document.getElementById('input-jpn').value, romaji:document.getElementById('input-romaji').value,
                tag:document.getElementById('input-tag').value || "Generale", esempi:document.getElementById('input-esempi').value,
                level: 0, type: 'vocab'
            };
            mazzoPrincipale.push(n); salvaMazzoPrincipale(); location.reload();
        }

        function gestisciImportaCSV(e) {
            const f=e.target.files[0]; if(!f)return;
            const r=new FileReader();
            r.onload=function(ev){ importaDatiDaStringa(ev.target.result); }; r.readAsText(f);
        }

        function gestisciAggiornaDaUrl(){
             const url = document.getElementById('csv-url-input').value;
             if(!url) return alert("Inserisci URL");
             fetch(url).then(r=>r.text()).then(t => { importaDatiDaStringa(t); });
        }

        function svuotaMazziTotali(){ if(confirm("Sicuro di voler cancellare tutto?")) { localStorage.clear(); location.reload(); } }
        
        function copiaVocaboli(){ 
            const all = [...mazzoPrincipale, ...mazzoErroriPrioritari];
            let csv = "Italiano,Inglese,Giapponese,Romaji,Tag,Esempi\n";
            all.forEach(p => { csv += `"${p.ita}","${p.eng}","${p.jpn}","${p.romaji}","${p.tag||''}","${p.esempi || ''}"\n`; });
            navigator.clipboard.writeText(csv).then(() => alert("Copiato negli appunti!"));
        }
        
        function avviaRipassoErrori(){ modalitaQuiz='ripasso_errori'; mazzoRipassoAttivo=[...erroriSessioneCorrente]; prossimaParola(); }
        
        function eliminaParola(key){ 
            mazzoPrincipale = mazzoPrincipale.filter(x => x.ita !== key); 
            mazzoErroriPrioritari = mazzoErroriPrioritari.filter(x => x.ita !== key); 
            salvaMazzoPrincipale(); 
            if(document.getElementById('modulo-vocaboli').style.display === 'block') { mostraListaVocaboli(); } else { location.reload(); }
        }
        
        function mostraListaVocaboli() {
            const container = document.getElementById('lista-vocaboli-container'); container.innerHTML = '';
            const all = [...mazzoPrincipale, ...mazzoErroriPrioritari].sort((a,b)=>a.ita.localeCompare(b.ita));
            document.getElementById('vocaboli-count').innerText = `Totale: ${all.length}`;
            all.forEach(p => {
                const el = document.createElement('div'); el.className = 'vocab-entry';
                const htmlContent = `
                    <div class="vocab-entry-principale">
                        <span>${p.ita}</span> 
                        <div class="vocab-group-right">
                            <span class="vocab-jpn">${colorizeJapanese(p.jpn)}</span>
                            <span class="vocab-romaji">${p.romaji}</span>
                            ${p.tag ? `<span class="vocab-tag">${p.tag}</span>` : ''}
                        </div>
                    </div>`;
                el.innerHTML = htmlContent;
                const btn = document.createElement('button'); btn.className = 'delete-vocab-btn'; btn.textContent = 'X';
                btn.onclick = function() { eliminaParola(p.ita); };
                el.appendChild(btn); container.appendChild(el);
            });
        }
    </script>
</body>
</html>
