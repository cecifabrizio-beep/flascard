<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Set di Studio Giapponese SRS v2.1 (Fix Grafico)</title>
    <style>
        /* --- Stile Generale --- */
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            display: flex; justify-content: center; align-items: center;
            flex-direction: column; background-color: #f0f2f5;
            color: #333; margin: 0; padding: 20px;
            box-sizing: border-box; min-height: 100vh;
        }
        h1, h2, h3 { color: #2c3e50; text-align: center; }
        .container { width: 100%; max-width: 600px; margin-bottom: 30px; } /* Allargato leggermente per le tabelle */
        
        .card-ui {
            background-color: #ffffff; padding: 25px;
            border-radius: 16px;
            box-shadow: 0 6px 18px rgba(0,0,0,0.1);
            transition: transform 0.2s;
        }
        .card-ui-small {
            background-color: #fff; padding: 15px; border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
        }

        /* --- Navigazione Moduli --- */
        #main-nav { display: flex; gap: 8px; width: 100%; max-width: 600px; margin-bottom: 20px; flex-wrap: wrap; justify-content: center; }
        .nav-btn {
            padding: 10px 15px; font-size: 0.9rem; font-weight: 600;
            border: none; border-radius: 8px; cursor: pointer; transition: all 0.2s;
            background-color: #e5e5ea; color: #007aff; white-space: nowrap; flex: 1; min-width: 80px;
        }
        .nav-btn.active { background-color: #007aff; color: white; box-shadow: 0 4px 10px rgba(0,122,255,0.3); }
        .modulo-content { display: none; width: 100%; }

        /* --- Sezione Punteggio --- */
        #punteggio-container { text-align: center; font-weight: 600; color: #555; font-size: 0.90rem; line-height: 1.5; }
        .punteggio-info { color: #007aff; font-weight: bold; }

        /* --- Sezione Quiz --- */
        #quiz-container { margin-top: 20px; }
        
        /* FILTRO CATEGORIA */
        .filtro-container { margin-bottom: 15px; text-align: center; }
        #filtro-categoria { 
            width: 100%; padding: 12px; border-radius: 8px; border: 1px solid #007aff; 
            font-size: 1rem; background-color: #f0f8ff; font-weight: 600; color: #333; cursor: pointer;
        }

        #prompt-container { text-align: center; margin-bottom: 20px; min-height: 100px; display: flex; flex-direction: column; justify-content: center; }
        #prompt-label { font-size: 0.9rem; color: #777; margin-bottom: 10px; font-weight: bold; }
        #prompt-principale { font-size: 2.5rem; color: #333; margin: 0; }
        #prompt-secondario { font-size: 1.5rem; color: #555; margin-top: 5px; font-style: italic; }
        #prompt-principale .hiragana, #prompt-principale .katakana { font-size: 3rem; color: #34c759; }
        
        #input-risposta { width: 100%; padding: 12px; border: 1px solid #ccc; border-radius: 8px; box-sizing: border-box; font-size: 1.2rem; text-align: center; margin-bottom: 15px; }
        #risultato-controllo { min-height: 50px; font-size: 1.1rem; font-weight: bold; text-align: center; padding: 5px; }
        .corretto { color: #2ca049; } .sbagliato { color: #d92c23; }

        /* --- TASTIERA VIRTUALE --- */
        #virtual-keyboard { 
            display: none; 
            grid-template-columns: repeat(5, 1fr); 
            gap: 8px; 
            margin-bottom: 20px; 
            background: #f8f8f8; 
            padding: 10px; 
            border-radius: 12px; 
        }
        .key-btn { 
            background: white; border: 1px solid #ddd; border-radius: 6px; 
            padding: 10px 0; font-size: 1.4rem; cursor: pointer; font-weight: bold; 
            color: #333; transition: background 0.1s; text-align: center; min-height: 50px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
        }
        .key-btn:active { background: #e5e5ea; transform: scale(0.95); }
        .key-empty { pointer-events: none; border: none; background: transparent; }

        /* --- CONFIGURAZIONE KANA --- */
        .kana-config-panel { background: #f9f9f9; border: 1px solid #eee; border-radius: 12px; padding: 15px; margin-bottom: 20px; }
        .config-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 15px; text-align: left; }
        .checkbox-label { display: flex; align-items: center; gap: 8px; font-size: 0.95rem; cursor: pointer; background: white; padding: 8px; border-radius: 6px; border: 1px solid #ddd; }
        .config-actions { display: flex; gap: 10px; }
        .btn-small { flex: 1; padding: 8px; font-size: 0.8rem; border: 1px solid #ccc; background: white; border-radius: 6px; cursor: pointer; }

        /* --- Pulsanti --- */
        .controlli { display: flex; gap: 15px; }
        .controlli button, .btn { flex-grow: 1; padding: 12px 20px; font-size: 1rem; font-weight: bold; border: none; border-radius: 8px; cursor: pointer; transition: all 0.2s; color: white; width: 100%; box-sizing: border-box; }
        #pulsante-controlla { background-color: #007aff; }
        #pulsante-prossima { background-color: #34c759; }
        #pulsante-elimina { background-color: #ff3b30; margin-top: 15px; padding: 10px; font-size: 0.9rem; }
        
        #salva-sessione-container { text-align: center; margin-top: 20px; display: grid; gap: 10px; }
        #ripassa-errori-btn { background-color: #ff9500; }
        #ripassa-errori-btn:disabled { background-color: #ccc; cursor: not-allowed; }
        #salva-sessione { background-color: #5856d6; }

        /* --- Altri Stili --- */
        #esempio-display { font-size: 0.9rem; color: #555; margin-top: 10px; padding: 8px; background-color: #f8f8f8; border-radius: 6px; text-align: left; border-left: 3px solid #ccc; }
        .sezione-gestione { border-top: 1px solid #eee; margin-top: 20px; padding-top: 20px; }
        #form-aggiungi div { margin-bottom: 15px; }
        #form-aggiungi label { display: block; margin-bottom: 5px; font-weight: 600; color: #555; }
        #form-aggiungi input, .input-text { width: 100%; padding: 10px; border: 1px solid #ddd; border-radius: 6px; box-sizing: border-box; font-size: 1rem; }
        #form-aggiungi button { background-color: #5856d6; }
        .messaggio-feedback { text-align: center; font-weight: bold; height: 20px; margin-top: 10px; }
        .label-sezione { display: block; margin-bottom: 10px; font-weight: 600; color: #555; text-align: center; }
        #import-csv-file { display: block; width: 100%; margin-top: 10px; }
        .btn-audio { background: none; border: none; font-size: 1.5rem; cursor: pointer; padding: 0 5px; vertical-align: middle; }
        #prompt-principale .btn-audio { font-size: 1.8rem; position: relative; top: -2px; }
        
        /* --- TABELLE KANA (FIXED) --- */
        .table-container { overflow-x: auto; margin-bottom: 25px; border-radius: 8px; border: 1px solid #eee; }
        .kana-table { width: 100%; border-collapse: collapse; min-width: 300px; }
        .kana-table th, .kana-table td { border: 1px solid #ddd; padding: 10px; text-align: center; vertical-align: middle; }
        .kana-table th { background-color: #f8f9fa; color: #555; font-weight: bold; }
        
        /* Stili per il contenuto delle celle */
        .k-char { font-size: 1.4rem; font-weight: bold; color: #333; display: block; margin-bottom: 2px; }
        .k-romaji { font-size: 0.8rem; color: #888; display: block; }
        
        .btn-start-kana { background-color: #34c759; margin-bottom: 20px; font-size: 1.1rem; box-shadow: 0 4px 10px rgba(52, 199, 89, 0.3); }
        h4 { margin-top: 0; margin-bottom: 10px; border-bottom: 2px solid #eee; padding-bottom: 5px; color: #007aff; }

        /* --- STILI LISTA VOCABOLI --- */
        #lista-vocaboli-container { margin-top: 20px; max-height: 450px; overflow-y: auto; border: 1px solid #eee; border-radius: 8px; }
        .vocab-entry { padding: 15px; border-bottom: 1px solid #f0f0f0; line-height: 1.5; display: flex; justify-content: space-between; align-items: flex-start; }
        .vocab-entry-principale { display: flex; justify-content: space-between; align-items: center; font-weight: 600; font-size: 1.2rem; color: #333; flex-grow: 1; padding-right: 10px;}
        .vocab-entry-principale .vocab-jpn { font-size: 1.4rem; color: #007aff; }
        
        .vocab-group-right { display: flex; flex-direction: column; align-items: flex-end; text-align: right; }
        .vocab-romaji { font-size: 0.85rem; color: #888; font-style: italic; font-weight: normal; margin-top: 2px; }
        .vocab-tag { font-size: 0.75rem; color: #fff; background-color: #aaa; padding: 2px 6px; border-radius: 4px; margin-top: 4px; font-weight: bold; }

        .hiragana { color: #34c759; } .katakana { color: #ffcc00; } .kanji { color: #007aff; }
        .delete-vocab-btn { background: #ff3b30; color: white; border: none; border-radius: 6px; padding: 5px 12px; font-size: 0.9rem; cursor: pointer; }
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
            <div class="container card-ui-small" id="punteggio-container"></div>

            <div class="container card-ui" id="quiz-container">
                
                <div class="filtro-container">
                    <select id="filtro-categoria" onchange="cambiaCategoriaQuiz()">
                        <option value="TUTTI">📚 TUTTI I VOCABOLI</option>
                    </select>
                </div>

                <div id="prompt-container">
                    <div id="prompt-label"></div> 
                    <h2 id="prompt-principale"></h2>
                    <h3 id="prompt-secondario"></h3>
                </div>
                <input type="text" id="input-risposta" placeholder="...">
                
                <div id="virtual-keyboard"></div>

                <div id="risultato-controllo"></div>
                <div id="esempio-display"></div> 
                <div class="controlli">
                    <button id="pulsante-controlla" class="btn">Controlla</button>
                    <button id="pulsante-prossima" class="btn">Prossima</button>
                </div>
                <button id="pulsante-elimina" class="btn">Elimina Carta</button>
                
                <div id="nessuna-carta" style="display: none;"><p>Non ci sono carte in questa categoria!</p></div>
                
                <div id="salva-sessione-container">
                    <button id="ripassa-errori-btn" class="btn" disabled>Ripassa Errori Sessione (0)</button>
                    <button id="salva-sessione" class="btn">Salva Errori e Chiudi</button>
                    <p id="salva-messaggio"></p>
                </div>
            </div>

            <div class="container card-ui" id="form-container">
                <h2>Gestione Dati</h2>
                <div>
                    <button id="reset-dati-btn" class="btn" style="background-color: #34c759; margin-bottom:10px;">🔄 Ripristina Dati Vocaboli</button>
                    <p class="messaggio-feedback" style="font-weight:normal; font-size:0.9rem;">(Clicca qui se non vedi le categorie nel menu)</p>
                </div>
                
                <div class="sezione-gestione">
                    <label for="csv-url-input" class="label-sezione">Link al file .csv su GitHub:</label>
                    <input type="text" id="csv-url-input" class="input-text" placeholder="https://raw.githubusercontent.com/...">
                    <button id="update-url-btn" class="btn" style="margin-top:10px; background:#007aff;">Aggiorna da Link</button>
                    <p id="messaggio-update" class="messaggio-feedback"></p>
                </div>

                <div id="import-container" class="sezione-gestione">
                    <label id="import-label" for="import-csv-file" class="label-sezione">Importa da file locale</label>
                    <input type="file" id="import-csv-file" accept=".csv">
                    <p id="messaggio-import" class="messaggio-feedback"></p>
                </div>
                
                <div class="sezione-gestione">
                    <h2>Aggiungi Parola</h2>
                    <form id="form-aggiungi">
                        <div><label>Italiano:</label><input type="text" id="input-ita" required></div>
                        <div><label>Inglese:</label><input type="text" id="input-eng" required></div>
                        <div><label>Giapponese:</label><input type="text" id="input-jpn" required></div>
                        <div><label>Romaji:</label><input type="text" id="input-romaji" required></div>
                        <div><label>Categoria (Tag):</label><input type="text" id="input-tag" placeholder="Es. Saluti, Lezione 1..."></div>
                        <div><label>Esempio:</label><input type="text" id="input-esempi"></div>
                        <button type="submit" class="btn">Salva</button>
                    </form>
                    <p id="messaggio-salvataggio" class="messaggio-feedback"></p>
                </div>

                <button id="svuota-tutto-btn" class="btn" style="background-color: #d92c23; margin-top: 20px;">CANCELLA TUTTO</button>
            </div>
        </div> 
        
        <div id="modulo-hiragana" class="modulo-content">
            <div class="card-ui">
                <h2>Alfabeto Hiragana (ひらがな)</h2>
                
                <div class="kana-config-panel">
                    <h3>Seleziona righe da studiare:</h3>
                    <div class="config-grid" id="h-config-grid"></div>
                    <div class="config-actions">
                        <button class="btn-small" onclick="toggleAllKana('hiragana', true)">Tutto</button>
                        <button class="btn-small" onclick="toggleAllKana('hiragana', false)">Niente</button>
                    </div>
                </div>

                <button class="btn btn-start-kana" onclick="avviaQuizKana('hiragana')">🏋️ Avvia Quiz Hiragana</button>

                <h4>Gojūon (Suoni base)</h4>
                <div class="table-container">
                    <table class="kana-table">
                        <tr><th></th><th>A</th><th>I</th><th>U</th><th>E</th><th>O</th></tr>
                        <tr><td><b>-</b></td><td><span class="k-char">あ</span><span class="k-romaji">a</span></td><td><span class="k-char">い</span><span class="k-romaji">i</span></td><td><span class="k-char">う</span><span class="k-romaji">u</span></td><td><span class="k-char">え</span><span class="k-romaji">e</span></td><td><span class="k-char">お</span><span class="k-romaji">o</span></td></tr>
                        <tr><td><b>K</b></td><td><span class="k-char">か</span><span class="k-romaji">ka</span></td><td><span class="k-char">き</span><span class="k-romaji">ki</span></td><td><span class="k-char">く</span><span class="k-romaji">ku</span></td><td><span class="k-char">け</span><span class="k-romaji">ke</span></td><td><span class="k-char">こ</span><span class="k-romaji">ko</span></td></tr>
                        <tr><td><b>S</b></td><td><span class="k-char">さ</span><span class="k-romaji">sa</span></td><td><span class="k-char">し</span><span class="k-romaji">shi</span></td><td><span class="k-char">す</span><span class="k-romaji">su</span></td><td><span class="k-char">せ</span><span class="k-romaji">se</span></td><td><span class="k-char">そ</span><span class="k-romaji">so</span></td></tr>
                        <tr><td><b>T</b></td><td><span class="k-char">た</span><span class="k-romaji">ta</span></td><td><span class="k-char">ち</span><span class="k-romaji">chi</span></td><td><span class="k-char">つ</span><span class="k-romaji">tsu</span></td><td><span class="k-char">て</span><span class="k-romaji">te</span></td><td><span class="k-char">と</span><span class="k-romaji">to</span></td></tr>
                        <tr><td><b>N</b></td><td><span class="k-char">な</span><span class="k-romaji">na</span></td><td><span class="k-char">に</span><span class="k-romaji">ni</span></td><td><span class="k-char">ぬ</span><span class="k-romaji">nu</span></td><td><span class="k-char">ね</span><span class="k-romaji">ne</span></td><td><span class="k-char">の</span><span class="k-romaji">no</span></td></tr>
                        <tr><td><b>H</b></td><td><span class="k-char">は</span><span class="k-romaji">ha</span></td><td><span class="k-char">ひ</span><span class="k-romaji">hi</span></td><td><span class="k-char">ふ</span><span class="k-romaji">fu</span></td><td><span class="k-char">へ</span><span class="k-romaji">he</span></td><td><span class="k-char">ほ</span><span class="k-romaji">ho</span></td></tr>
                        <tr><td><b>M</b></td><td><span class="k-char">ま</span><span class="k-romaji">ma</span></td><td><span class="k-char">み</span><span class="k-romaji">mi</span></td><td><span class="k-char">む</span><span class="k-romaji">mu</span></td><td><span class="k-char">め</span><span class="k-romaji">me</span></td><td><span class="k-char">も</span><span class="k-romaji">mo</span></td></tr>
                        <tr><td><b>Y</b></td><td><span class="k-char">や</span><span class="k-romaji">ya</span></td><td></td><td><span class="k-char">ゆ</span><span class="k-romaji">yu</span></td><td></td><td><span class="k-char">よ</span><span class="k-romaji">yo</span></td></tr>
                        <tr><td><b>R</b></td><td><span class="k-char">ら</span><span class="k-romaji">ra</span></td><td><span class="k-char">り</span><span class="k-romaji">ri</span></td><td><span class="k-char">る</span><span class="k-romaji">ru</span></td><td><span class="k-char">れ</span><span class="k-romaji">re</span></td><td><span class="k-char">ろ</span><span class="k-romaji">ro</span></td></tr>
                        <tr><td><b>W</b></td><td><span class="k-char">わ</span><span class="k-romaji">wa</span></td><td></td><td></td><td></td><td><span class="k-char">を</span><span class="k-romaji">wo</span></td></tr>
                        <tr><td><b>N</b></td><td><span class="k-char">ん</span><span class="k-romaji">n</span></td><td></td><td></td><td></td><td></td></tr>
                    </table>
                </div>

                <h4>Dakuten & Handakuten</h4>
                <div class="table-container">
                    <table class="kana-table">
                        <tr><th></th><th>A</th><th>I</th><th>U</th><th>E</th><th>O</th></tr>
                        <tr><td><b>G</b></td><td><span class="k-char">が</span><span class="k-romaji">ga</span></td><td><span class="k-char">ぎ</span><span class="k-romaji">gi</span></td><td><span class="k-char">ぐ</span><span class="k-romaji">gu</span></td><td><span class="k-char">げ</span><span class="k-romaji">ge</span></td><td><span class="k-char">ご</span><span class="k-romaji">go</span></td></tr>
                        <tr><td><b>Z</b></td><td><span class="k-char">ざ</span><span class="k-romaji">za</span></td><td><span class="k-char">じ</span><span class="k-romaji">ji</span></td><td><span class="k-char">ず</span><span class="k-romaji">zu</span></td><td><span class="k-char">ぜ</span><span class="k-romaji">ze</span></td><td><span class="k-char">ぞ</span><span class="k-romaji">zo</span></td></tr>
                        <tr><td><b>D</b></td><td><span class="k-char">だ</span><span class="k-romaji">da</span></td><td><span class="k-char">ぢ</span><span class="k-romaji">ji</span></td><td><span class="k-char">づ</span><span class="k-romaji">zu</span></td><td><span class="k-char">で</span><span class="k-romaji">de</span></td><td><span class="k-char">ど</span><span class="k-romaji">do</span></td></tr>
                        <tr><td><b>B</b></td><td><span class="k-char">ば</span><span class="k-romaji">ba</span></td><td><span class="k-char">び</span><span class="k-romaji">bi</span></td><td><span class="k-char">ぶ</span><span class="k-romaji">bu</span></td><td><span class="k-char">べ</span><span class="k-romaji">be</span></td><td><span class="k-char">ぼ</span><span class="k-romaji">bo</span></td></tr>
                        <tr><td><b>P</b></td><td><span class="k-char">ぱ</span><span class="k-romaji">pa</span></td><td><span class="k-char">ぴ</span><span class="k-romaji">pi</span></td><td><span class="k-char">ぷ</span><span class="k-romaji">pu</span></td><td><span class="k-char">ぺ</span><span class="k-romaji">pe</span></td><td><span class="k-char">ぽ</span><span class="k-romaji">po</span></td></tr>
                    </table>
                </div>

                <h4>Yoon (Suoni contratti)</h4>
                <div class="table-container">
                    <table class="kana-table">
                        <tr><th></th><th>YA</th><th>YU</th><th>YO</th></tr>
                        <tr><td><b>K</b></td><td><span class="k-char">きゃ</span><span class="k-romaji">kya</span></td><td><span class="k-char">きゅ</span><span class="k-romaji">kyu</span></td><td><span class="k-char">きょ</span><span class="k-romaji">kyo</span></td></tr>
                        <tr><td><b>S</b></td><td><span class="k-char">しゃ</span><span class="k-romaji">sha</span></td><td><span class="k-char">しゅ</span><span class="k-romaji">shu</span></td><td><span class="k-char">しょ</span><span class="k-romaji">sho</span></td></tr>
                        <tr><td><b>C</b></td><td><span class="k-char">ちゃ</span><span class="k-romaji">cha</span></td><td><span class="k-char">ちゅ</span><span class="k-romaji">chu</span></td><td><span class="k-char">ちょ</span><span class="k-romaji">cho</span></td></tr>
                        <tr><td><b>N</b></td><td><span class="k-char">にゃ</span><span class="k-romaji">nya</span></td><td><span class="k-char">にゅ</span><span class="k-romaji">nyu</span></td><td><span class="k-char">にょ</span><span class="k-romaji">nyo</span></td></tr>
                        <tr><td><b>H</b></td><td><span class="k-char">ひゃ</span><span class="k-romaji">hya</span></td><td><span class="k-char">ひゅ</span><span class="k-romaji">hyu</span></td><td><span class="k-char">ひょ</span><span class="k-romaji">hyo</span></td></tr>
                        <tr><td><b>M</b></td><td><span class="k-char">みゃ</span><span class="k-romaji">mya</span></td><td><span class="k-char">みゅ</span><span class="k-romaji">myu</span></td><td><span class="k-char">みょ</span><span class="k-romaji">myo</span></td></tr>
                        <tr><td><b>R</b></td><td><span class="k-char">りゃ</span><span class="k-romaji">rya</span></td><td><span class="k-char">りゅ</span><span class="k-romaji">ryu</span></td><td><span class="k-char">りょ</span><span class="k-romaji">ryo</span></td></tr>
                        <tr><td><b>G</b></td><td><span class="k-char">ぎゃ</span><span class="k-romaji">gya</span></td><td><span class="k-char">ぎゅ</span><span class="k-romaji">gyu</span></td><td><span class="k-char">ぎょ</span><span class="k-romaji">gyo</span></td></tr>
                        <tr><td><b>J</b></td><td><span class="k-char">じゃ</span><span class="k-romaji">ja</span></td><td><span class="k-char">じゅ</span><span class="k-romaji">ju</span></td><td><span class="k-char">じょ</span><span class="k-romaji">jo</span></td></tr>
                        <tr><td><b>B</b></td><td><span class="k-char">びゃ</span><span class="k-romaji">bya</span></td><td><span class="k-char">びゅ</span><span class="k-romaji">byu</span></td><td><span class="k-char">びょ</span><span class="k-romaji">byo</span></td></tr>
                        <tr><td><b>P</b></td><td><span class="k-char">ぴゃ</span><span class="k-romaji">pya</span></td><td><span class="k-char">ぴゅ</span><span class="k-romaji">pyu</span></td><td><span class="k-char">ぴょ</span><span class="k-romaji">pyo</span></td></tr>
                    </table>
                </div>
            </div>
        </div> 

        <div id="modulo-katakana" class="modulo-content">
            <div class="card-ui">
                <h2>Alfabeto Katakana (カタカナ)</h2>

                <div class="kana-config-panel">
                    <h3>Seleziona righe da studiare:</h3>
                    <div class="config-grid" id="k-config-grid"></div>
                    <div class="config-actions">
                        <button class="btn-small" onclick="toggleAllKana('katakana', true)">Tutto</button>
                        <button class="btn-small" onclick="toggleAllKana('katakana', false)">Niente</button>
                    </div>
                </div>

                <button class="btn btn-start-kana" onclick="avviaQuizKana('katakana')">🏋️ Avvia Quiz Katakana</button>

                <h4>Gojūon (Suoni base)</h4>
                <div class="table-container">
                    <table class="kana-table">
                         <tr><th></th><th>A</th><th>I</th><th>U</th><th>E</th><th>O</th></tr>
                        <tr><td><b>-</b></td><td><span class="k-char">ア</span><span class="k-romaji">a</span></td><td><span class="k-char">イ</span><span class="k-romaji">i</span></td><td><span class="k-char">ウ</span><span class="k-romaji">u</span></td><td><span class="k-char">エ</span><span class="k-romaji">e</span></td><td><span class="k-char">オ</span><span class="k-romaji">o</span></td></tr>
                        <tr><td><b>K</b></td><td><span class="k-char">カ</span><span class="k-romaji">ka</span></td><td><span class="k-char">キ</span><span class="k-romaji">ki</span></td><td><span class="k-char">ク</span><span class="k-romaji">ku</span></td><td><span class="k-char">ケ</span><span class="k-romaji">ke</span></td><td><span class="k-char">コ</span><span class="k-romaji">ko</span></td></tr>
                        <tr><td><b>S</b></td><td><span class="k-char">サ</span><span class="k-romaji">sa</span></td><td><span class="k-char">シ</span><span class="k-romaji">shi</span></td><td><span class="k-char">ス</span><span class="k-romaji">su</span></td><td><span class="k-char">セ</span><span class="k-romaji">se</span></td><td><span class="k-char">ソ</span><span class="k-romaji">so</span></td></tr>
                        <tr><td><b>T</b></td><td><span class="k-char">タ</span><span class="k-romaji">ta</span></td><td><span class="k-char">チ</span><span class="k-romaji">chi</span></td><td><span class="k-char">ツ</span><span class="k-romaji">tsu</span></td><td><span class="k-char">テ</span><span class="k-romaji">te</span></td><td><span class="k-char">ト</span><span class="k-romaji">to</span></td></tr>
                        <tr><td><b>N</b></td><td><span class="k-char">ナ</span><span class="k-romaji">na</span></td><td><span class="k-char">ニ</span><span class="k-romaji">ni</span></td><td><span class="k-char">ヌ</span><span class="k-romaji">nu</span></td><td><span class="k-char">ネ</span><span class="k-romaji">ne</span></td><td><span class="k-char">ノ</span><span class="k-romaji">no</span></td></tr>
                        <tr><td><b>H</b></td><td><span class="k-char">ハ</span><span class="k-romaji">ha</span></td><td><span class="k-char">ヒ</span><span class="k-romaji">hi</span></td><td><span class="k-char">フ</span><span class="k-romaji">fu</span></td><td><span class="k-char">ヘ</span><span class="k-romaji">he</span></td><td><span class="k-char">ホ</span><span class="k-romaji">ho</span></td></tr>
                        <tr><td><b>M</b></td><td><span class="k-char">マ</span><span class="k-romaji">ma</span></td><td><span class="k-char">ミ</span><span class="k-romaji">mi</span></td><td><span class="k-char">ム</span><span class="k-romaji">mu</span></td><td><span class="k-char">メ</span><span class="k-romaji">me</span></td><td><span class="k-char">モ</span><span class="k-romaji">mo</span></td></tr>
                        <tr><td><b>Y</b></td><td><span class="k-char">ヤ</span><span class="k-romaji">ya</span></td><td></td><td><span class="k-char">ユ</span><span class="k-romaji">yu</span></td><td></td><td><span class="k-char">ヨ</span><span class="k-romaji">yo</span></td></tr>
                        <tr><td><b>R</b></td><td><span class="k-char">ラ</span><span class="k-romaji">ra</span></td><td><span class="k-char">リ</span><span class="k-romaji">ri</span></td><td><span class="k-char">ル</span><span class="k-romaji">ru</span></td><td><span class="k-char">レ</span><span class="k-romaji">re</span></td><td><span class="k-char">ロ</span><span class="k-romaji">ro</span></td></tr>
                        <tr><td><b>W</b></td><td><span class="k-char">ワ</span><span class="k-romaji">wa</span></td><td></td><td></td><td></td><td><span class="k-char">ヲ</span><span class="k-romaji">wo</span></td></tr>
                        <tr><td><b>N</b></td><td><span class="k-char">ン</span><span class="k-romaji">n</span></td><td></td><td></td><td></td><td></td></tr>
                    </table>
                </div>

                <h4>Dakuten & Handakuten</h4>
                <div class="table-container">
                    <table class="kana-table">
                        <tr><th></th><th>A</th><th>I</th><th>U</th><th>E</th><th>O</th></tr>
                        <tr><td><b>G</b></td><td><span class="k-char">ガ</span><span class="k-romaji">ga</span></td><td><span class="k-char">ギ</span><span class="k-romaji">gi</span></td><td><span class="k-char">グ</span><span class="k-romaji">gu</span></td><td><span class="k-char">ゲ</span><span class="k-romaji">ge</span></td><td><span class="k-char">ゴ</span><span class="k-romaji">go</span></td></tr>
                        <tr><td><b>Z</b></td><td><span class="k-char">ザ</span><span class="k-romaji">za</span></td><td><span class="k-char">ジ</span><span class="k-romaji">ji</span></td><td><span class="k-char">ズ</span><span class="k-romaji">zu</span></td><td><span class="k-char">ゼ</span><span class="k-romaji">ze</span></td><td><span class="k-char">ゾ</span><span class="k-romaji">zo</span></td></tr>
                        <tr><td><b>D</b></td><td><span class="k-char">ダ</span><span class="k-romaji">da</span></td><td><span class="k-char">ヂ</span><span class="k-romaji">ji</span></td><td><span class="k-char">ヅ</span><span class="k-romaji">zu</span></td><td><span class="k-char">デ</span><span class="k-romaji">de</span></td><td><span class="k-char">ド</span><span class="k-romaji">do</span></td></tr>
                        <tr><td><b>B</b></td><td><span class="k-char">バ</span><span class="k-romaji">ba</span></td><td><span class="k-char">ビ</span><span class="k-romaji">bi</span></td><td><span class="k-char">ブ</span><span class="k-romaji">bu</span></td><td><span class="k-char">ベ</span><span class="k-romaji">be</span></td><td><span class="k-char">ボ</span><span class="k-romaji">bo</span></td></tr>
                        <tr><td><b>P</b></td><td><span class="k-char">パ</span><span class="k-romaji">pa</span></td><td><span class="k-char">ピ</span><span class="k-romaji">pi</span></td><td><span class="k-char">プ</span><span class="k-romaji">pu</span></td><td><span class="k-char">ペ</span><span class="k-romaji">pe</span></td><td><span class="k-char">ポ</span><span class="k-romaji">po</span></td></tr>
                    </table>
                </div>

                <h4>Yoon (Suoni contratti)</h4>
                <div class="table-container">
                    <table class="kana-table">
                        <tr><th></th><th>YA</th><th>YU</th><th>YO</th></tr>
                        <tr><td><b>K</b></td><td><span class="k-char">キャ</span><span class="k-romaji">kya</span></td><td><span class="k-char">キュ</span><span class="k-romaji">kyu</span></td><td><span class="k-char">キョ</span><span class="k-romaji">kyo</span></td></tr>
                        <tr><td><b>S</b></td><td><span class="k-char">シャ</span><span class="k-romaji">sha</span></td><td><span class="k-char">シュ</span><span class="k-romaji">shu</span></td><td><span class="k-char">ショ</span><span class="k-romaji">sho</span></td></tr>
                        <tr><td><b>C</b></td><td><span class="k-char">チャ</span><span class="k-romaji">cha</span></td><td><span class="k-char">チュ</span><span class="k-romaji">chu</span></td><td><span class="k-char">チョ</span><span class="k-romaji">cho</span></td></tr>
                        <tr><td><b>N</b></td><td><span class="k-char">ニャ</span><span class="k-romaji">nya</span></td><td><span class="k-char">ニュ</span><span class="k-romaji">nyu</span></td><td><span class="k-char">ニョ</span><span class="k-romaji">nyo</span></td></tr>
                        <tr><td><b>H</b></td><td><span class="k-char">ヒャ</span><span class="k-romaji">hya</span></td><td><span class="k-char">ヒュ</span><span class="k-romaji">hyu</span></td><td><span class="k-char">ヒョ</span><span class="k-romaji">hyo</span></td></tr>
                        <tr><td><b>M</b></td><td><span class="k-char">ミャ</span><span class="k-romaji">mya</span></td><td><span class="k-char">ミュ</span><span class="k-romaji">myu</span></td><td><span class="k-char">ミョ</span><span class="k-romaji">myo</span></td></tr>
                        <tr><td><b>R</b></td><td><span class="k-char">リャ</span><span class="k-romaji">rya</span></td><td><span class="k-char">リュ</span><span class="k-romaji">ryu</span></td><td><span class="k-char">リョ</span><span class="k-romaji">ryo</span></td></tr>
                        <tr><td><b>G</b></td><td><span class="k-char">ギャ</span><span class="k-romaji">gya</span></td><td><span class="k-char">ギュ</span><span class="k-romaji">gyu</span></td><td><span class="k-char">ギョ</span><span class="k-romaji">gyo</span></td></tr>
                        <tr><td><b>J</b></td><td><span class="k-char">ジャ</span><span class="k-romaji">ja</span></td><td><span class="k-char">ジュ</span><span class="k-romaji">ju</span></td><td><span class="k-char">ジョ</span><span class="k-romaji">jo</span></td></tr>
                        <tr><td><b>B</b></td><td><span class="k-char">ビャ</span><span class="k-romaji">bya</span></td><td><span class="k-char">ビュ</span><span class="k-romaji">byu</span></td><td><span class="k-char">ビョ</span><span class="k-romaji">byo</span></td></tr>
                        <tr><td><b>P</b></td><td><span class="k-char">ピャ</span><span class="k-romaji">pya</span></td><td><span class="k-char">ピュ</span><span class="k-romaji">pyu</span></td><td><span class="k-char">ピョ</span><span class="k-romaji">pyo</span></td></tr>
                    </table>
                </div>
            </div>
        </div> 
        
        <div id="modulo-vocaboli" class="modulo-content">
            <div class="card-ui">
                <h2>Lista Vocaboli</h2>
                <p id="vocaboli-count"></p> 
                <button id="copia-vocaboli-btn" class="btn" style="background-color: #ff9500; margin-top: 10px;">Copia Vocaboli (CSV)</button>
                <div id="lista-vocaboli-container"></div>
            </div>
        </div> 
    </main>

    <script>
        // --- DATI INIZIALI (CSV EMBEDDED) ---
        const DATI_INIZIALI_CSV = `
Buongiorno,Good morning,おはよう,Ohayoo,Intro - Saluti
Buongiorno (cortese),Good morning (polite),おはようございます,Ohayoo gozaimasu,Intro - Saluti
Buon pomeriggio,Good afternoon,こんにちは,Konnichiwa,Intro - Saluti
Buonasera,Good evening,こんばんは,Konbanwa,Intro - Saluti
Arrivederci,Good-bye,さようなら,Sayoonara,Intro - Saluti
Buonanotte,Good night,おやすみ（なさい）,Oyasumi (nasai),Intro - Saluti
Grazie,Thank you,ありがとう,Arigatoo,Intro - Saluti
Grazie (cortese),Thank you (polite),ありがとうございます,Arigatoo gozaimasu,Intro - Saluti
Mi scusi / Mi dispiace,Excuse me / I'm sorry,すみません,Sumimasen,Intro - Saluti
No / Di nulla,No / Not at all,いいえ,Iie,Intro - Saluti
Vado e torno,I'll go and come back,いってきます,Itte kimasu,Intro - Saluti
Vai e torna,Please go and come back,いってらっしゃい,Itterasshai,Intro - Saluti
Sono a casa,I'm home,ただいま,Tadaima,Intro - Saluti
Bentornato,Welcome home,おかえり（なさい）,Okaeri (nasai),Intro - Saluti
Grazie per il cibo (prima),Thank you for the meal (before),いただきます,Itadakimasu,Intro - Saluti
Grazie per il cibo (dopo),Thank you for the meal (after),ごちそうさま（でした）,Gochisoosama (deshita),Intro - Saluti
Piacere di conoscerti,How do you do?,はじめまして,Hajimemashite,Intro - Saluti
Sono...,I am...,～です,... desu,Intro - Saluti
Piacere,Nice to meet you,よろしく おねがいします,Yoroshiku onegai shimasu,Intro - Saluti
Zero,Zero,ゼロ / れい,zero / ree,Intro - Numeri
Uno,One,いち,ichi,Intro - Numeri
Due,Two,に,ni,Intro - Numeri
Tre,Three,さん,san,Intro - Numeri
Quattro,Four,よん / し / (よ),yon / shi / (yo),Intro - Numeri
Cinque,Five,ご,go,Intro - Numeri
Sei,Six,ろく,roku,Intro - Numeri
Sette,Seven,なな / しち,nana / shichi,Intro - Numeri
Otto,Eight,はち,hachi,Intro - Numeri
Nove,Nine,きゅう / く,kyuu / ku,Intro - Numeri
Dieci,Ten,じゅう,juu,Intro - Numeri
Undici,Eleven,じゅういち,juuichi,Intro - Numeri
Dodici,Twelve,じゅうに,juuni,Intro - Numeri
Tredici,Thirteen,じゅうさん,juusan,Intro - Numeri
Quattordici,Fourteen,じゅうよん / じゅうし,juuyon / juushi,Intro - Numeri
Quindici,Fifteen,じゅうご,juugo,Intro - Numeri
Sedici,Sixteen,じゅうろく,juuroku,Intro - Numeri
Diciassette,Seventeen,じゅうなな / じゅうしち,juunana / juushichi,Intro - Numeri
Diciotto,Eighteen,じゅうはち,juuhachi,Intro - Numeri
Diciannove,Nineteen,じゅうきゅう / じゅうく,juukyuu / juuku,Intro - Numeri
Venti,Twenty,にじゅう,nijuu,Intro - Numeri
Trenta,Thirty,さんじゅう,sanjuu,Intro - Numeri
Quaranta,Forty,よんじゅう,yonjuu,Intro - Numeri
Cinquanta,Fifty,ごじゅう,gojuu,Intro - Numeri
Sessanta,Sixty,ろくじゅう,rokujuu,Intro - Numeri
Settanta,Seventy,ななじゅう,nanajuu,Intro - Numeri
Ottanta,Eighty,はちじゅう,hachijuu,Intro - Numeri
Novanta,Ninety,きゅうじゅう,kyuujuu,Intro - Numeri
Cento,One hundred,ひゃく,hyaku,Intro - Numeri
Duecento,Two hundred,にひゃく,nihyaku,Intro - Numeri
Trecento,Three hundred,さんびゃく,sanbyaku,Intro - Numeri
Quattrocento,Four hundred,よんひゃく,yonhyaku,Intro - Numeri
Cinquecento,Five hundred,ごひゃく,gohyaku,Intro - Numeri
Seicento,Six hundred,ろっぴゃく,roppyaku,Intro - Numeri
Settecento,Seven hundred,ななひゃく,nanahyaku,Intro - Numeri
Ottocento,Eight hundred,はっぴゃく,happyaku,Intro - Numeri
Novecento,Nine hundred,きゅうひゃく,kyuuhyaku,Intro - Numeri
Mille,One thousand,せん,sen,Intro - Numeri
Duemila,Two thousand,にせん,nisen,Intro - Numeri
Tremila,Three thousand,さんぜん,sanzen,Intro - Numeri
Quattromila,Four thousand,よんせん,yonsen,Intro - Numeri
Cinquemila,Five thousand,ごせん,gosen,Intro - Numeri
Seimila,Six thousand,ろくせん,rokusen,Intro - Numeri
Settemila,Seven thousand,ななせん,nanasen,Intro - Numeri
Ottomila,Eight thousand,はっせん,hassen,Intro - Numeri
Novemila,Nine thousand,きゅうせん,kyuusen,Intro - Numeri
Diecimila,Ten thousand,いちまん,ichiman,Intro - Numeri
Ventimila,Twenty thousand,にまん,niman,Intro - Numeri
L'una,One o'clock,いちじ,ichiji,Intro - Orario
Le due,Two o'clock,にじ,niji,Intro - Orario
Le tre,Three o'clock,さんじ,sanji,Intro - Orario
Le quattro,Four o'clock,よじ,yoji,Intro - Orario
Le cinque,Five o'clock,ごじ,goji,Intro - Orario
Le sei,Six o'clock,ろくじ,rokuji,Intro - Orario
Le sette,Seven o'clock,しちじ,shichiji,Intro - Orario
Le otto,Eight o'clock,はちじ,hachiji,Intro - Orario
Le nove,Nine o'clock,くじ,kuji,Intro - Orario
Le dieci,Ten o'clock,じゅうじ,juuji,Intro - Orario
Le undici,Eleven o'clock,じゅういちじ,juuichiji,Intro - Orario
Le dodici,Twelve o'clock,じゅうにじ,juuniji,Intro - Orario
Un minuto,One minute,いっぷん,ippun,Intro - Orario
Due minuti,Two minutes,にふん,nifun,Intro - Orario
Tre minuti,Three minutes,さんぷん,sanpun,Intro - Orario
Quattro minuti,Four minutes,よんぷん,yonpun,Intro - Orario
Cinque minuti,Five minutes,ごふん,gofun,Intro - Orario
Sei minuti,Six minutes,ろっぷん,roppun,Intro - Orario
Sette minuti,Seven minutes,ななふん,nanafun,Intro - Orario
Otto minuti,Eight minutes,はっぷん / はちふん,happun / hachifun,Intro - Orario
Nove minuti,Nine minutes,きゅうふん,kyuufun,Intro - Orario
Dieci minuti,Ten minutes,じゅっぷん,juppun,Intro - Orario
Undici minuti,Eleven minutes,じゅういっぷん,juuippun,Intro - Orario
Dodici minuti,Twelve minutes,じゅうにふん,juunifun,Intro - Orario
Tredici minuti,Thirteen minutes,じゅうさんぷん,juusanpun,Intro - Orario
Quattordici minuti,Fourteen minutes,じゅうよんぷん,juuyonpun,Intro - Orario
Quindici minuti,Fifteen minutes,じゅうごふん,juugofun,Intro - Orario
Sedici minuti,Sixteen minutes,じゅうろっぷん,juuroppun,Intro - Orario
Diciassette minuti,Seventeen minutes,じゅうななふん,juunanafun,Intro - Orario
Diciotto minuti,Eighteen minutes,じゅうはっぷん / じゅうはちふん,juuhappun / juuhachifun,Intro - Orario
Diciannove minuti,Nineteen minutes,じゅうきゅうふん,juukyuufun,Intro - Orario
Venti minuti,Twenty minutes,にじゅっぷん,nijuppun,Intro - Orario
Trenta minuti,Thirty minutes,さんじゅっぷん,sanjuppun,Intro - Orario
Università,college; university,だいがく,daigaku,L1 - Scuola e Persone
Scuola superiore,high school,こうこう,kookoo,L1 - Scuola e Persone
Studente,student,がくせい,gakusee,L1 - Scuola e Persone
Studente universitario,college student,だいがくせい,daigakusee,L1 - Scuola e Persone
Studente internazionale,international student,りゅうがくせい,ryuugakusee,L1 - Scuola e Persone
Insegnante / Professore,teacher; Professor...,せんせい,sensee,L1 - Scuola e Persone
Studente del ... anno,...year student,～ねんせい,... nensee,L1 - Scuola e Persone
Studente del primo anno,first-year student,いちねんせい,ichinensee,L1 - Scuola e Persone
Specializzazione,major,せんこう,senkoo,L1 - Scuola e Persone
Io,I,わたし,watashi,L1 - Scuola e Persone
Amico,friend,ともだち,tomodachi,L1 - Scuola e Persone
Sig./Sig.ra,Mr./Ms.,～さん,... san,L1 - Scuola e Persone
Persona ... (nazionalità),... people,～じん,... jin,L1 - Scuola e Persone
Giapponese (persona),Japanese people,にほんじん,nihonjin,L1 - Scuola e Persone
Adesso,now,いま,ima,L1 - Scuola e Persone
Mattina (A.M.),A.M.,ごぜん,gozen,L1 - Scuola e Persone
Pomeriggio (P.M.),P.M.,ごご,gogo,L1 - Scuola e Persone
Ore ...,...o'clock,～じ,... ji,L1 - Scuola e Persone
L'una (orario),one o'clock,いちじ,ichiji,L1 - Scuola e Persone
Mezza,half,はん,han,L1 - Scuola e Persone
Due e mezza,half past two,にじはん,niji han,L1 - Scuola e Persone
Giappone,Japan,にほん,Nihon,L1 - Scuola e Persone
USA,U.S.A.,アメリカ,Amerika,L1 - Scuola e Persone
Lingua ...,... language,～ご,... go,L1 - Scuola e Persone
Lingua giapponese,Japanese language,にほんご,nihongo,L1 - Scuola e Persone
... anni,... years old,～さい,... sai,L1 - Scuola e Persone
Telefono,telephone,でんわ,denwa,L1 - Scuola e Persone
Numero ...,number...,～ばん,... ban,L1 - Scuola e Persone
Numero,number,ばんごう,bangoo,L1 - Altro e Paesi
Nome,name,なまえ,namae,L1 - Altro e Paesi
Cosa / Che,what,なん／なに,nan/nani,L1 - Altro e Paesi
Ehm...,um...,あのう,anoo,L1 - Altro e Paesi
Sì,yes,はい,hai,L1 - Altro e Paesi
È così / Giusto,That's right,そうです,soo desu,L1 - Altro e Paesi
Capisco / È così?,I see.; Is that so?,そうですか,soo desu ka,L1 - Altro e Paesi
Gran Bretagna,Britain,イギリス,Igirisu,L1 - Altro e Paesi
Australia,Australia,オーストラリア,Oosutoraria,L1 - Altro e Paesi
Corea,Korea,かんこく,Kankoku,L1 - Altro e Paesi
Canada,Canada,カナダ,Kanada,L1 - Altro e Paesi
Cina,China,ちゅうごく,Chuugoku,L1 - Altro e Paesi
India,India,インド,Indo,L1 - Altro e Paesi
Egitto,Egypt,エジプト,Ejiputo,L1 - Altro e Paesi
Filippine,Philippines,フィリピン,Firipin,L1 - Altro e Paesi
Studi asiatici,Asian studies,アジアけんきゅう,ajia kenkyuu,L1 - Altro e Paesi
Economia,economics,けいざい,keezai,L1 - Altro e Paesi
Ingegneria,engineering,こうがく,koogaku,L1 - Altro e Paesi
Relazioni internazionali,international relations,こくさいかんけい,kokusaikankee,L1 - Altro e Paesi
Computer,computer,コンピューター,konpyuutaa,L1 - Altro e Paesi
Politica,politics,せいじ,seeji,L1 - Altro e Paesi
Biologia,biology,せいぶつがく,seebutsugaku,L1 - Altro e Paesi
Affari / Business,business,ビジネス,bijinesu,L1 - Altro e Paesi
Letteratura,literature,ぶんがく,bungaku,L1 - Altro e Paesi
Storia,history,れきし,rekishi,L1 - Altro e Paesi
Dottore,doctor,いしゃ,isha,L1 - Altro e Paesi
Impiegato,office worker,かいしゃいん,kaishain,L1 - Altro e Paesi
Questo (vicino a me),this one,これ,kore,L2 - Cose Luoghi e Cibo
Quello (vicino a te),that one,それ,sore,L2 - Cose Luoghi e Cibo
Quello (laggiù),that one (over there),あれ,are,L2 - Cose Luoghi e Cibo
Quale,which one,どれ,dore,L2 - Cose Luoghi e Cibo
Questo...,this...,この,kono,L2 - Cose Luoghi e Cibo
Quello...,that...,その,sono,L2 - Cose Luoghi e Cibo
Quello... (laggiù),that... (over there),あの,ano,L2 - Cose Luoghi e Cibo
Quale...,which...,どの,dono,L2 - Cose Luoghi e Cibo
Qui,here,ここ,koko,L2 - Cose Luoghi e Cibo
Lì,there,そこ,soko,L2 - Cose Luoghi e Cibo
Laggiù,over there,あそこ,asoko,L2 - Cose Luoghi e Cibo
Dove,where,どこ,doko,L2 - Cose Luoghi e Cibo
Chi,who,だれ,dare,L2 - Cose Luoghi e Cibo
Delizioso,delicious,おいしい,oishii,L2 - Cose Luoghi e Cibo
Pesce,fish,さかな,sakana,L2 - Cose Luoghi e Cibo
Cotoletta di maiale,pork cutlet,とんかつ,tonkatsu,L2 - Cose Luoghi e Cibo
Carne,meat,にく,niku,L2 - Cose Luoghi e Cibo
Menu,menu,メニュー,menyuu,L2 - Cose Luoghi e Cibo
Verdura,vegetable,やさい,yasai,L2 - Cose Luoghi e Cibo
Ombrello,umbrella,かさ,kasa,L2 - Cose Luoghi e Cibo
Borsa,bag,かばん,kaban,L2 - Cose Luoghi e Cibo
Scarpe,shoes,くつ,kutsu,L2 - Cose Luoghi e Cibo
Portafoglio,wallet,さいふ,saifu,L2 - Cose Luoghi e Cibo
Jeans,jeans,ジーンズ,jiinzu,L2 - Cose Luoghi e Cibo
Bicicletta,bicycle,じてんしゃ,jitensha,L2 - Cose Luoghi e Cibo
Giornale,newspaper,しんぶん,shinbun,L2 - Cose Luoghi e Cibo
Smartphone / Cellulare,smartphone; mobile,スマホ,sumaho,L2 - Cose Luoghi e Cibo
Maglietta,T-shirt,Tシャツ,tiishatsu,L2 - Cose Luoghi e Cibo
Orologio,watch; clock,とけい,tokee,L2 - Cose Luoghi e Cibo
Quaderno,notebook,ノート,nooto,L2 - Cose Luoghi e Cibo
Banca,bank,ぎんこう,ginkoo,L2 - Luoghi e Soldi
Convenience store,convenience store,コンビニ,konbini,L2 - Luoghi e Soldi
Bagno,toilet; restroom,トイレ,toire,L2 - Luoghi e Soldi
Biblioteca,library,としょかん,toshokan,L2 - Luoghi e Soldi
Ufficio postale,post office,ゆうびんきょく,yuubinkyoku,L2 - Luoghi e Soldi
Lingua inglese,English (language),えいご,eego,L1 - Altro e Paesi
Madre,mother,おかあさん,okaasan,L2 - Luoghi e Soldi
Padre,father,おとうさん,otoosan,L2 - Luoghi e Soldi
Quanto costa,how much,いくら,ikura,L2 - Luoghi e Soldi
...yen,...yen,～えん,...en,L2 - Luoghi e Soldi
Costoso / Alto,expensive; high,たかい,takai,L2 - Luoghi e Soldi`;

        // --- DATASETS KANA ESTESI (Base + Dakuten + Handakuten + Yoon) ---
        const HIRAGANA_DATA = [
            // --- BASE (46) ---
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
            
            // --- DAKUTEN (20) (Ga, Za, Da, Ba) ---
            {k:'が',r:'ga'}, {k:'ぎ',r:'gi'}, {k:'ぐ',r:'gu'}, {k:'げ',r:'ge'}, {k:'ご',r:'go'},
            {k:'ざ',r:'za'}, {k:'じ',r:'ji'}, {k:'ず',r:'zu'}, {k:'ぜ',r:'ze'}, {k:'ぞ',r:'zo'},
            {k:'だ',r:'da'}, {k:'ぢ',r:'ji'}, {k:'づ',r:'zu'}, {k:'で',r:'de'}, {k:'ど',r:'do'}, 
            {k:'ば',r:'ba'}, {k:'び',r:'bi'}, {k:'ぶ',r:'bu'}, {k:'べ',r:'be'}, {k:'ぼ',r:'bo'},

            // --- HANDAKUTEN (5) (Pa) ---
            {k:'ぱ',r:'pa'}, {k:'ぴ',r:'pi'}, {k:'ぷ',r:'pu'}, {k:'ぺ',r:'pe'}, {k:'ぽ',r:'po'},

            // --- YOON BASE (21) (Kya, Sha, Cha, Nya, Hya, Mya, Rya) ---
            {k:'きゃ',r:'kya'}, {k:'きゅ',r:'kyu'}, {k:'きょ',r:'kyo'},
            {k:'しゃ',r:'sha'}, {k:'しゅ',r:'shu'}, {k:'しょ',r:'sho'},
            {k:'ちゃ',r:'cha'}, {k:'ちゅ',r:'chu'}, {k:'ちょ',r:'cho'},
            {k:'にゃ',r:'nya'}, {k:'にゅ',r:'nyu'}, {k:'にょ',r:'nyo'},
            {k:'ひゃ',r:'hya'}, {k:'ひゅ',r:'hyu'}, {k:'ひょ',r:'hyo'},
            {k:'みゃ',r:'mya'}, {k:'みゅ',r:'myu'}, {k:'みょ',r:'myo'},
            {k:'りゃ',r:'rya'}, {k:'りゅ',r:'ryu'}, {k:'りょ',r:'ryo'},

            // --- YOON DAKUTEN/HANDAKUTEN (12) (Gya, Ja, Bya, Pya) ---
            {k:'ぎゃ',r:'gya'}, {k:'ぎゅ',r:'gyu'}, {k:'ぎょ',r:'gyo'},
            {k:'じゃ',r:'ja'},  {k:'じゅ',r:'ju'},  {k:'じょ',r:'jo'},
            {k:'びゃ',r:'bya'}, {k:'びゅ',r:'byu'}, {k:'びょ',r:'byo'},
            {k:'ぴゃ',r:'pya'}, {k:'ぴゅ',r:'pyu'}, {k:'ぴょ',r:'pyo'}
        ];

        const KATAKANA_DATA = [
            // --- BASE (46) ---
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

            // --- DAKUTEN (20) ---
            {k:'ガ',r:'ga'}, {k:'ギ',r:'gi'}, {k:'グ',r:'gu'}, {k:'ゲ',r:'ge'}, {k:'ゴ',r:'go'},
            {k:'ザ',r:'za'}, {k:'ジ',r:'ji'}, {k:'ズ',r:'zu'}, {k:'ゼ',r:'ze'}, {k:'ゾ',r:'zo'},
            {k:'ダ',r:'da'}, {k:'ヂ',r:'ji'}, {k:'ヅ',r:'zu'}, {k:'デ',r:'de'}, {k:'ド',r:'do'},
            {k:'バ',r:'ba'}, {k:'ビ',r:'bi'}, {k:'ブ',r:'bu'}, {k:'ベ',r:'be'}, {k:'ボ',r:'bo'},

            // --- HANDAKUTEN (5) ---
            {k:'パ',r:'pa'}, {k:'ピ',r:'pi'}, {k:'プ',r:'pu'}, {k:'ペ',r:'pe'}, {k:'ポ',r:'po'},

            // --- YOON BASE (21) ---
            {k:'キャ',r:'kya'}, {k:'キュ',r:'kyu'}, {k:'キョ',r:'kyo'},
            {k:'シャ',r:'sha'}, {k:'シュ',r:'shu'}, {k:'ショ',r:'sho'},
            {k:'チャ',r:'cha'}, {k:'チュ',r:'chu'}, {k:'チョ',r:'cho'},
            {k:'ニャ',r:'nya'}, {k:'ニュ',r:'nyu'}, {k:'ニョ',r:'nyo'},
            {k:'ヒャ',r:'hya'}, {k:'ヒュ',r:'hyu'}, {k:'ヒョ',r:'hyo'},
            {k:'ミャ',r:'mya'}, {k:'ミュ',r:'myu'}, {k:'ミョ',r:'myo'},
            {k:'リャ',r:'rya'}, {k:'リュ',r:'ryu'}, {k:'リョ',r:'ryo'},

            // --- YOON DAKUTEN/HANDAKUTEN (12) ---
            {k:'ギャ',r:'gya'}, {k:'ギュ',r:'gyu'}, {k:'ギョ',r:'gyo'},
            {k:'ジャ',r:'ja'},  {k:'ジュ',r:'ju'},  {k:'ジョ',r:'jo'},
            {k:'ビャ',r:'bya'}, {k:'ビュ',r:'byu'}, {k:'ビョ',r:'byo'},
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

        // --- DOM ELEMENTS ---
        const promptContainer = document.getElementById('prompt-container'); 
        const promptLabel = document.getElementById('prompt-label');
        const promptPrincipale = document.getElementById('prompt-principale');
        const promptSecondario = document.getElementById('prompt-secondario');
        const inputRisposta = document.getElementById('input-risposta');
        const risultatoControllo = document.getElementById('risultato-controllo');
        const punteggioDisplay = document.getElementById('punteggio-container');
        const btnControlla = document.getElementById('pulsante-controlla');
        const btnProssima = document.getElementById('pulsante-prossima');
        const btnElimina = document.getElementById('pulsante-elimina');
        const quizContainer = document.getElementById('quiz-container');
        const formContainer = document.getElementById('form-container'); 
        const esempioDisplay = document.getElementById('esempio-display'); 
        const virtualKeyboard = document.getElementById('virtual-keyboard');
        const filtroCategoria = document.getElementById('filtro-categoria');
        
        // --- STATE ---
        let mazzoPrincipale = [];
        let mazzoErroriPrioritari = [];
        let mazzoBacklog = []; // Coda di parole da fare (tutte quelle della categoria)
        let mazzoSessioneCorrente = []; // Le 10 attuali
        let subsetKanaAttivo = []; // I Kana selezionati per il quiz attuale
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
            btnControlla.addEventListener('click', controllaRisposta);
            document.getElementById('pulsante-prossima').addEventListener('click', prossimaParola);
            inputRisposta.addEventListener('keydown', (e) => {
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
                        formContainer.style.display = 'block'; 
                        btnElimina.style.display = 'block';
                        virtualKeyboard.style.display = 'none';
                        filtroCategoria.parentElement.style.display = 'block'; 
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
            document.getElementById('copia-vocaboli-btn').addEventListener('click', copiaVocaboli);
        }

        function ripristinaDatiVocaboli() {
            if(confirm("Questo caricherà i dati predefiniti (sovrascrivendo eventuali duplicati). Vuoi procedere?")) {
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
            virtualKeyboard.innerHTML = '';
            
            // Usiamo il subset attivo definito all'avvio del quiz (filtrato dall'utente)
            // Se è vuoto, usa tutto (fallback)
            let caratteriDaMostrare = subsetKanaAttivo.length > 0 ? subsetKanaAttivo : (modalitaQuiz.startsWith('hiragana') ? HIRAGANA_DATA : KATAKANA_DATA);
            
            // Estraiamo solo i caratteri .k
            let listaTasti = caratteriDaMostrare.map(x => x.k);
            
            // Mescoliamo COMPLETAMENTE l'array (nessuna colonna fissa)
            listaTasti = shuffleArray(listaTasti);
            
            listaTasti.forEach(char => {
               const btn = document.createElement('button');
               btn.className = 'key-btn';
               btn.textContent = char;
               btn.onclick = () => { inputRisposta.value += char; inputRisposta.focus(); };
               virtualKeyboard.appendChild(btn);
            });
            
            virtualKeyboard.style.gridTemplateColumns = 'repeat(5, 1fr)';
        }

        function avviaQuizKana(type) {
            const subset = getKanaSubset(type);
            if (subset.length === 0) { alert("Seleziona almeno una riga!"); return; }

            // Salviamo il subset attivo per usarlo nella generazione della tastiera
            subsetKanaAttivo = subset;

            const mazzoKana = subset.map(char => ({
                ita: char.r, 
                eng: char.r,
                jpn: char.k, 
                romaji: char.r,
                level: 0,
                type: type 
            }));

            mazzoSessioneCorrente = shuffleArray(mazzoKana);
            indiceSessione = 0;
            sessioneCorretti = 0;
            sessioneSbagliati = 0;
            erroriSessioneCorrente = new Set();
            modalitaQuiz = type + '_mode'; 
            
            mostraModulo('quiz');
            formContainer.style.display = 'none'; 
            btnElimina.style.display = 'none';
            filtroCategoria.parentElement.style.display = 'none'; // Nascondi filtro vocaboli
            
            // Genera la tastiera subito
            generaTastieraVirtuale();
            
            prossimaParola();
        }

        // --- CORE QUIZ LOGIC ---
        function prossimaParola() {
            // GESTIONE FINE SET VOCABOLI (BATCHING)
            if (modalitaQuiz === 'set_finito') {
                if (mazzoBacklog.length > 0) {
                    caricaProssimoBatch();
                    return;
                } else {
                    // Davvero finito tutto
                    modalitaQuiz = 'normale'; 
                    caricaMazzi(); // Ricomincia tutto
                    return;
                }
            }

            promptContainer.style.display = 'flex';
            inputRisposta.style.display = 'block';
            risultatoControllo.style.display = 'block';
            esempioDisplay.innerHTML = ''; 
            inputRisposta.value = "";
            risultatoControllo.innerHTML = "";
            inputRisposta.disabled = false;
            btnControlla.style.display = "block";
            btnProssima.style.display = "none";
            virtualKeyboard.style.display = 'none'; 

            let etichetta = "";
            const isKanaMode = modalitaQuiz.includes('_mode');

            if (isKanaMode || (modalitaQuiz === 'ripasso_errori' && parolaCorrente && (parolaCorrente.type === 'hiragana' || parolaCorrente.type === 'katakana'))) {
                // ... (Logica Kana Invariata) ...
                if (isKanaMode) {
                    if (indiceSessione >= mazzoSessioneCorrente.length) {
                        promptPrincipale.innerHTML = "🎉 Fine Pratica!";
                        promptSecondario.textContent = `Punteggio: ${sessioneCorretti}/${mazzoSessioneCorrente.length}`;
                        inputRisposta.style.display = 'none';
                        btnControlla.style.display = 'none';
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
                    promptLabel.innerHTML = `${etichetta} - Scrivi il Romaji`;
                    const cssClass = parolaCorrente.type === 'hiragana' ? 'hiragana' : 'katakana';
                    promptPrincipale.innerHTML = `<span class="${cssClass}">${parolaCorrente.jpn}</span>`;
                    promptSecondario.textContent = "";
                } else {
                    quizDirection = 'ITA_TO_JPN'; 
                    promptLabel.innerHTML = `${etichetta} - Che carattere è?`;
                    promptPrincipale.textContent = parolaCorrente.romaji;
                    promptSecondario.textContent = "(Usa la tastiera qui sotto)";
                    
                    // Rigenera tastiera mescolata per ogni domanda (opzionale, ma aumenta difficoltà)
                    // Se vuoi tastiera fissa per sessione, muovi questa chiamata fuori da prossimaParola
                    generaTastieraVirtuale(); 
                    virtualKeyboard.style.display = 'grid'; 
                }

            } else {
                // 2. GESTIONE VOCABOLI
                if (modalitaQuiz === 'ripasso_errori') {
                    if (mazzoRipassoAttivo.length > 0) {
                        parolaCorrente = mazzoRipassoAttivo.shift();
                        etichetta = `RIPASSO (${mazzoRipassoAttivo.length + 1})`;
                        
                        if(parolaCorrente.type !== 'vocab') {
                            mazzoRipassoAttivo.unshift(parolaCorrente);
                            setTimeout(prossimaParola, 0);
                            return; 
                        }
                    } else {
                        alert("Ripasso finito!"); modalitaQuiz = 'normale'; caricaMazzi(); return;
                    }
                } else {
                    // QUIZ NORMALE (CON BATCH DA 10)
                    if (indiceSessione >= mazzoSessioneCorrente.length) {
                        if (mazzoSessioneCorrente.length === 0) {
                            document.getElementById('nessuna-carta').style.display = 'block';
                            document.getElementById('quiz-container').querySelector('.controlli').style.display = 'none';
                            return;
                        }
                        
                        modalitaQuiz = 'set_finito';
                        promptPrincipale.innerHTML = "Set Completato!";
                        
                        // Controllo se ci sono altre carte nel backlog
                        if(mazzoBacklog.length > 0) {
                            promptSecondario.innerHTML = `Ne rimangono altre ${mazzoBacklog.length} in questa categoria.`;
                            btnProssima.textContent = "Carica prossime 10";
                        } else {
                            promptSecondario.innerHTML = "Hai finito tutte le parole di questa categoria!";
                            btnProssima.textContent = "Ricomincia da capo";
                        }
                        
                        inputRisposta.style.display = 'none';
                        btnControlla.style.display = 'none';
                        btnProssima.style.display = 'block'; // Mostra il tasto per avanzare
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
                    promptLabel.innerHTML = `TRADUCI ${etichetta} <small>${stelle}</small>`;
                    promptPrincipale.textContent = parolaCorrente.ita.split('/')[0];
                    promptSecondario.innerHTML = parolaCorrente.eng.split('/')[0] + tagInfo;
                } else {
                    quizDirection = 'JPN_TO_ITA';
                    promptLabel.innerHTML = `TRADUCI ${etichetta} <small>${stelle}</small>`;
                    const btnAudio = `<button class="btn-audio" onclick="parla('${jpnClean}')">🔊</button>`;
                    promptPrincipale.innerHTML = `${colorizeJapanese(jpnClean)} ${btnAudio}`;
                    promptSecondario.innerHTML = parolaCorrente.romaji.split('/')[0] + tagInfo;
                }
            }
            aggiornaPunteggio();
        }

        function controllaRisposta() {
            if (!parolaCorrente) return;
            const risp = inputRisposta.value.trim().toLowerCase();
            let ok = false;
            let err = "";
            
            inputRisposta.disabled = true;
            btnControlla.style.display = 'none';
            btnProssima.style.display = 'block';
            virtualKeyboard.style.display = 'none'; 
            
            const jpnClean = parolaCorrente.jpn.split('/')[0];
            const audioBtn = `<button class="btn-audio" onclick="parla('${jpnClean}')">🔊</button>`;

            if (parolaCorrente.type === 'hiragana' || parolaCorrente.type === 'katakana') {
                if (quizDirection === 'ITA_TO_JPN') {
                    ok = (risp === parolaCorrente.jpn);
                    err = `Risposta: <b>${parolaCorrente.jpn}</b>`;
                } else {
                    ok = (risp === parolaCorrente.romaji);
                    err = `Risposta: <b>${parolaCorrente.romaji}</b>`;
                }
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
                risultatoControllo.innerHTML = `<span class="corretto">Corretto!</span>`;
                sessioneCorretti++;
                if (erroriSessioneCorrente.has(parolaCorrente)) {
                    erroriSessioneCorrente.delete(parolaCorrente);
                }
                if(parolaCorrente.type === 'vocab') {
                    parolaCorrente.level = (parolaCorrente.level || 0) + 1;
                    if(parolaCorrente.level > 5) parolaCorrente.level = 5;
                    if(quizDirection === 'ITA_TO_JPN') parla(jpnClean);
                }
            } else {
                risultatoControllo.innerHTML = `<span class="sbagliato">Sbagliato.</span><br>${err}`;
                sessioneSbagliati++;
                erroriSessioneCorrente.add(parolaCorrente); 
                if(parolaCorrente.type === 'vocab') {
                    parolaCorrente.level = 0;
                }
            }
            
            if(parolaCorrente.esempi) {
                esempioDisplay.innerHTML = `📝 <i>${parolaCorrente.esempi}</i>`;
            }
            
            aggiornaPunteggio();
        }

        // --- UTILS ---
        function parseCSVLine(text) {
            const re = /(?:\"([^\"]*(?:\"\"[^\"]*)*)\")|([^\",]+)/g;
            const cols = [];
            let match;
            while (match = re.exec(text)) {
                let val = match[1] || match[2] || "";
                val = val.replace(/""/g, '"').trim();
                cols.push(val);
            }
            return cols;
        }

        function aggiornaPunteggio() {
            let html = `<span class="punteggio-info">Corretti: ${sessioneCorretti} | Errori: ${sessioneSbagliati}</span>`;
            if (!modalitaQuiz.includes('_mode')) {
                html += `<br><span style="font-size:0.8em">In attesa (Backlog): ${mazzoBacklog.length} | Da Rivedere: ${mazzoErroriPrioritari.length}</span>`;
            }
            punteggioDisplay.innerHTML = html;
            
            const btnRipassa = document.getElementById('ripassa-errori-btn');
            const numErrori = erroriSessioneCorrente.size;
            btnRipassa.textContent = `Ripassa Errori Sessione (${numErrori})`;
            btnRipassa.disabled = (numErrori === 0);
        }

        function parla(txt) {
            if (!synth) return;
            const u = new SpeechSynthesisUtterance(txt);
            if(japaneseVoice) u.voice = japaneseVoice;
            else u.lang = 'ja-JP';
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

        // --- STORAGE & IMPORT/EXPORT ---
        function caricaMazzi() {
            const dP = localStorage.getItem(KEY_MAZZO_PRINCIPALE);
            mazzoPrincipale = dP ? JSON.parse(dP) : [];
            const dE = localStorage.getItem(KEY_MAZZO_ERRORI);
            mazzoErroriPrioritari = dE ? JSON.parse(dE) : [];
            
            mazzoPrincipale.forEach(c => {
                if(typeof c.level==='undefined') c.level=0;
                c.type = 'vocab';
            });

            // Se il mazzo è vuoto, carica i dati di default!
            if (mazzoPrincipale.length === 0) {
                importaDatiDaStringa(DATI_INIZIALI_CSV);
                return; // l'import ricaricherà la pagina
            }
            
            aggiornaFiltroCategorie(); 
            creaNuovoSet();
            
            if(modalitaQuiz === 'normale') {
                if(mazzoSessioneCorrente.length === 0 && (mazzoPrincipale.length > 0 || mazzoErroriPrioritari.length > 0)) {
                    document.getElementById('nessuna-carta').style.display = 'block';
                    document.getElementById('quiz-container').querySelector('.controlli').style.display = 'none';
                    document.getElementById('input-risposta').style.display = 'none';
                    document.getElementById('prompt-container').style.display = 'none';
                } else {
                    document.getElementById('nessuna-carta').style.display = 'none';
                    document.getElementById('quiz-container').querySelector('.controlli').style.display = 'flex';
                    document.getElementById('input-risposta').style.display = 'block';
                    document.getElementById('prompt-container').style.display = 'flex';
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
                const opt = document.createElement('option');
                opt.value = tag;
                opt.textContent = `📂 ${tag}`;
                filtroCategoria.appendChild(opt);
            });
            
            if(currentSelection && Array.from(filtroCategoria.options).some(o => o.value === currentSelection)) {
                filtroCategoria.value = currentSelection;
            }
        }

        function cambiaCategoriaQuiz() {
            caricaMazzi();
        }
        
        function creaNuovoSet() {
            // 1. Resetta tutto
            mazzoBacklog = [];
            mazzoSessioneCorrente = [];
            indiceSessione = 0;
            modalitaQuiz = 'normale'; 

            if (mazzoPrincipale.length === 0 && mazzoErroriPrioritari.length === 0) return;

            const categoriaScelta = filtroCategoria.value;
            let filteredMain = mazzoPrincipale;
            
            // 2. Filtra il mazzo in base alla selezione
            if (categoriaScelta !== 'TUTTI') {
                filteredMain = mazzoPrincipale.filter(c => c.tag === categoriaScelta);
            }

            // 3. Ordina per livello (così studiamo prima quelle che conosciamo meno)
            // MA poi mescoleremo nel backlog per non avere blocchi noiosi
            const sortedMain = [...filteredMain].sort((a,b) => (a.level||0) - (b.level||0));
            
            let pool = [...mazzoErroriPrioritari];
            if(categoriaScelta !== 'TUTTI') {
                 pool = pool.filter(c => c.tag === categoriaScelta);
            }
            
            // 4. Crea il "Backlog" (la coda totale di cose da fare)
            // Mescoliamo qui per avere varietà, ma potremmo anche tenerle ordinate
            mazzoBacklog = [...pool, ...sortedMain];
            // shuffleArray(mazzoBacklog); // Decommenta se vuoi ordine totalmente casuale nel backlog

            // 5. Carica il primo batch
            caricaProssimoBatch();
        }

        function caricaProssimoBatch() {
            modalitaQuiz = 'normale';
            indiceSessione = 0;
            // Prendi i primi 10 dalla coda
            const batchSize = 10;
            mazzoSessioneCorrente = mazzoBacklog.splice(0, batchSize);
            
            // Mescoliamo QUESTI 10 così non sono in ordine alfabetico/livello durante il quiz
            mazzoSessioneCorrente = shuffleArray(mazzoSessioneCorrente);
            
            prossimaParola();
        }

        function salvaErroriSessione() {
            const erroriVocaboli = Array.from(erroriSessioneCorrente).filter(c => c.type === 'vocab');
            const nuovi = new Set([...mazzoErroriPrioritari, ...erroriVocaboli]);
            
            localStorage.setItem(KEY_MAZZO_ERRORI, JSON.stringify(Array.from(nuovi)));
            salvaMazzoPrincipale();
            location.reload();
        }

        function salvaMazzoPrincipale() {
             const map = new Map();
             [...mazzoPrincipale, ...mazzoErroriPrioritari, ...mazzoSessioneCorrente, ...mazzoBacklog].forEach(item => {
                 if(item.type === 'vocab') map.set(item.ita, item);
             });
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
                ita:document.getElementById('input-ita').value,
                eng:document.getElementById('input-eng').value,
                jpn:document.getElementById('input-jpn').value,
                romaji:document.getElementById('input-romaji').value,
                tag:document.getElementById('input-tag').value || "Generale",
                esempi:document.getElementById('input-esempi').value,
                level: 0,
                type: 'vocab'
            };
            mazzoPrincipale.push(n);
            salvaMazzoPrincipale();
            location.reload();
        }

        function gestisciImportaCSV(e) {
            const f=e.target.files[0]; if(!f)return;
            const r=new FileReader();
            r.onload=function(ev){
                importaDatiDaStringa(ev.target.result);
            };
            r.readAsText(f);
        }

        function gestisciAggiornaDaUrl(){
             const url = document.getElementById('csv-url-input').value;
             if(!url) return alert("Inserisci URL");
             fetch(url).then(r=>r.text()).then(t => {
                 importaDatiDaStringa(t);
             });
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
            if(document.getElementById('modulo-vocaboli').style.display === 'block') {
                mostraListaVocaboli();
            } else {
                location.reload(); 
            }
        }
        
        function mostraListaVocaboli() {
            const container = document.getElementById('lista-vocaboli-container'); 
            container.innerHTML = '';
            
            const all = [...mazzoPrincipale, ...mazzoErroriPrioritari].sort((a,b)=>a.ita.localeCompare(b.ita));
            document.getElementById('vocaboli-count').innerText = `Totale: ${all.length}`;
            
            all.forEach(p => {
                const el = document.createElement('div');
                el.className = 'vocab-entry';
                
                const htmlContent = `
                    <div class="vocab-entry-principale">
                        <span>${p.ita}</span> 
                        <div class="vocab-group-right">
                            <span class="vocab-jpn">${colorizeJapanese(p.jpn)}</span>
                            <span class="vocab-romaji">${p.romaji}</span>
                            ${p.tag ? `<span class="vocab-tag">${p.tag}</span>` : ''}
                        </div>
                    </div>
                `;
                el.innerHTML = htmlContent;

                const btn = document.createElement('button');
                btn.className = 'delete-vocab-btn';
                btn.textContent = 'X';
                btn.onclick = function() { eliminaParola(p.ita); };
                
                el.appendChild(btn);
                container.appendChild(el);
            });
        }
    </script>
</body>
</html>
