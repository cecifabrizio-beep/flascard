<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Set di Studio Giapponese</title>
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
        .container { width: 100%; max-width: 450px; margin-bottom: 30px; }
        
        .card-ui {
            background-color: #ffffff; padding: 25px;
            border-radius: 16px;
            box-shadow: 0 6px 18px rgba(0,0,0,0.1);
        }
        .card-ui-small {
            background-color: #fff; padding: 15px; border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
        }

        /* --- Navigazione Moduli --- */
        #main-nav {
            display: flex;
            gap: 10px;
            width: 100%;
            max-width: 450px;
            margin-bottom: 20px;
        }
        .nav-btn {
            flex: 1; padding: 10px 15px; font-size: 0.9rem; font-weight: 600;
            border: none; border-radius: 8px; cursor: pointer; transition: all 0.2s;
            background-color: #e5e5ea; color: #007aff;
        }
        .nav-btn.active {
            background-color: #007aff; color: white;
            box-shadow: 0 4px 10px rgba(0,122,255,0.3);
        }
        .modulo-content {
            display: none; /* Nascosti di default */
            width: 100%;
        }

        /* --- Sezione Punteggio --- */
        #punteggio-container {
            text-align: center; font-weight: 600;
            color: #555; font-size: 0.90rem;
            line-height: 1.5;
        }
        #punteggio-container span { margin: 0 5px; }
        .punteggio-corr { color: #2ca049; }
        .punteggio-sbag { color: #d92c23; }
        .punteggio-prio { color: #007aff; font-weight: bold; }
        .punteggio-mazzo { color: #5856d6; }
        .punteggio-sessione { color: #ff9500; font-weight: bold; }

        /* --- Sezione Quiz --- */
        #quiz-container { margin-top: 20px; }
        #prompt-container { text-align: center; margin-bottom: 20px; min-height: 100px; display: flex; flex-direction: column; justify-content: center; }
        #prompt-label { font-size: 0.9rem; color: #777; margin-bottom: 10px; font-weight: bold; }
        #prompt-principale { font-size: 2.2rem; color: #333; margin: 0; }
        #prompt-secondario { font-size: 1.5rem; color: #555; margin-top: 5px; font-style: italic; }
        
        /* Assicura che i caratteri colorati siano grandi nel prompt */
        #prompt-principale .hiragana, 
        #prompt-principale .katakana, 
        #prompt-principale .kanji {
            font-size: 2.2rem;
        }

        #input-risposta {
            width: 100%; padding: 12px; border: 1px solid #ccc; border-radius: 8px;
            box-sizing: border-box; font-size: 1.2rem; text-align: center; margin-bottom: 15px;
        }
        
        #risultato-controllo { min-height: 50px; font-size: 1.1rem; font-weight: bold; text-align: center; padding: 5px; }
        .corretto { color: #2ca049; }
        .sbagliato { color: #d92c23; }

        /* --- Pulsanti --- */
        .controlli { display: flex; gap: 15px; }
        .controlli button, .btn {
            flex-grow: 1; padding: 12px 20px; font-size: 1rem; font-weight: bold;
            border: none; border-radius: 8px; cursor: pointer; transition: all 0.2s;
            color: white; width: 100%; box-sizing: border-box;
        }
        #pulsante-controlla { background-color: #007aff; }
        #pulsante-prossima { background-color: #34c759; }
        #pulsante-elimina { background-color: #ff3b30; margin-top: 15px; padding: 10px; font-size: 0.9rem; }
        
        #salva-sessione-container { 
            text-align: center; margin-top: 20px; 
            display: grid; gap: 10px;
        }
        #ripassa-errori-btn { background-color: #ff9500; }
        #ripassa-errori-btn:disabled { background-color: #ccc; cursor: not-allowed; }
        #salva-sessione { background-color: #5856d6; }
        #salva-messaggio { font-size: 0.9rem; color: #333; height: 20px; font-weight: bold; }
        
        /* --- Frase Esempio (Nuovo Stile) --- */
        #esempio-display, .vocab-esempi {
            font-size: 0.9rem;
            color: #555;
            margin-top: 10px;
            padding: 8px;
            background-color: #f8f8f8;
            border-radius: 6px;
            text-align: left;
            border-left: 3px solid #ccc;
        }

        /* --- Form Aggiungi e Importa --- */
        .sezione-gestione { border-top: 1px solid #eee; margin-top: 20px; padding-top: 20px; }
        #form-aggiungi div { margin-bottom: 15px; }
        #form-aggiungi label { display: block; margin-bottom: 5px; font-weight: 600; color: #555; }
        #form-aggiungi input, .input-text {
            width: 100%; padding: 10px; border: 1px solid #ddd; border-radius: 6px;
            box-sizing: border-box; font-size: 1rem;
        }
        #form-aggiungi button { background-color: #5856d6; }
        .messaggio-feedback { text-align: center; font-weight: bold; height: 20px; margin-top: 10px; }
        
        /* --- Sezioni Importa --- */
        .label-sezione { display: block; margin-bottom: 10px; font-weight: 600; color: #555; text-align: center; }
        #import-csv-file { display: block; width: 100%; margin-top: 10px; }
        #import-dettagli { display: none; margin-top: 15px; }
        #import-dettagli h3 { font-size: 1rem; margin-top: 0; margin-bottom: 10px; color: #d92c23; text-align: center; }
        #import-errori-lista {
            font-size: 0.85rem; max-height: 150px;
            overflow-y: auto; background: #fdf6f6;
            border: 1px solid #f5c6cb; padding: 10px; margin: 0;
            border-radius: 6px; list-style-type: none;
        }
        #import-errori-lista li { margin-bottom: 5px; }
        #nessuna-carta { text-align: center; color: #777; padding: 20px; }
        
        /* --- Stile Audio --- */
        .btn-audio { 
            background: none; border: none; font-size: 1.5rem;
            cursor: pointer; padding: 0 5px; vertical-align: middle; 
        }
        #prompt-principale .btn-audio {
            font-size: 1.8rem; position: relative; top: -2px;
        }
        .btn-audio:hover { opacity: 0.7; }
        
        /* --- Stile Update URL --- */
        #update-url-btn { background-color: #34c759; margin-top: 10px; }
        
        /* --- Stile Tabelle Kana e Vocaboli --- */
        .table-container { /* Ora usato solo per KANA */
            overflow-x: auto;
        }
        .kana-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
            font-size: 1.1rem;
            text-align: center;
        }
        .kana-table td, .kana-table th {
            border: 1px solid #ddd;
            padding: 10px 8px;
        }
        .kana-table th {
            background-color: #f4f4f4;
            font-weight: 600;
        }
        .kana-table .kana {
            font-size: 1.4rem;
            font-weight: bold;
            color: #333;
        }
        .kana-table .romaji {
            font-size: 0.9rem;
            color: #555;
        }
        
        /* --- NUOVO LAYOUT LISTA VOCABOLI --- */
        #lista-vocaboli-container {
            margin-top: 20px;
            max-height: 450px; /* Scroll verticale se la lista è lunga */
            overflow-y: auto;
            border: 1px solid #eee;
            border-radius: 8px;
        }

        .vocab-entry {
            padding: 15px;
            border-bottom: 1px solid #f0f0f0;
            line-height: 1.5;
            display: flex; /* Abilita flexbox per il pulsante */
            justify-content: space-between;
            align-items: flex-start;
        }
        .vocab-entry-content {
            flex-grow: 1;
            padding-right: 15px;
        }
        .vocab-entry:last-child {
            border-bottom: none;
        }

        .vocab-entry-principale {
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-weight: 600;
            font-size: 1.2rem;
            color: #333;
        }
        .vocab-entry-principale .vocab-jpn {
            font-size: 1.4rem;
        }

        .vocab-entry-secondario {
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 0.95rem;
            color: #555;
            margin-top: 5px;
        }
        .vocab-entry-secondario .vocab-romaji {
            font-style: italic;
            font-weight: 500;
        }
        
        /* CLASSI PER LA COLORAZIONE RICHIESTA */
        .hiragana { color: #34c759; } /* Verde */
        .katakana { color: #ffcc00; } /* Giallo */
        .kanji { color: #007aff; } /* Blu */
        
        /* Stile pulsante Copia */
        #copia-vocaboli-btn { background-color: #ff9500; margin-top: 10px; }
        
        /* Stile Pulsante Cancella Singola Parola nella Lista */
        .delete-vocab-btn {
            background: #ff3b30;
            color: white;
            border: none;
            border-radius: 6px;
            padding: 5px 8px;
            font-size: 0.8rem;
            cursor: pointer;
            line-height: 1;
            margin-left: 10px;
            transition: background 0.2s;
            flex-shrink: 0;
        }
        .delete-vocab-btn:hover {
            background: #cc2e26;
        }
        /* Stile Pulsante Cancella Tutto */
        #svuota-tutto-btn {
            background-color: #d92c23; 
            margin-top: 20px;
        }

    </style>
</head>
<body>

    <h1>Set di Studio Giapponese</h1>

    <nav id="main-nav">
        <button class="nav-btn" data-modulo="quiz">Quiz</button>
        <button class="nav-btn" data-modulo="hiragana">Hiragana</button>
        <button class="nav-btn" data-modulo="katakana">Katakana</button>
        <button class="nav-btn" data-modulo="vocaboli">Vocaboli</button>
    </nav>

    <main id="app-container" class="container">

        <div id="modulo-quiz" class="modulo-content">
            <div class="container card-ui-small" id="punteggio-container"></div>

            <div class="container card-ui" id="quiz-container">
                <div id="prompt-container">
                    <div id="prompt-label"></div> 
                    <h2 id="prompt-principale"></h2>
                    <h3 id="prompt-secondario"></h3>
                </div>
                <input type="text" id="input-risposta" placeholder="...">
                <div id="risultato-controllo"></div>
                <div id="esempio-display"></div> 
                <div class="controlli">
                    <button id="pulsante-controlla" class="btn">Controlla</button>
                    <button id="pulsante-prossima" class="btn">Prossima</button>
                </div>
                <button id="pulsante-elimina" class="btn">Elimina Carta dal Mazzo Principale</button>
                
                <div id="nessuna-carta" style="display: none;"><p>Non ci sono carte! Aggiungine qualcuna.</p></div>
                
                <div id="salva-sessione-container">
                    <button id="ripassa-errori-btn" class="btn" disabled>Ripassa Errori Sessione (0)</button>
                    <button id="salva-sessione" class="btn">Salva Errori e Inizia Nuova Sessione</button>
                    <p id="salva-messaggio"></p>
                </div>
            </div>

            <div class="container card-ui" id="form-container">
                <h2>Aggiornamento Online</h2>
                <div>
                    <label for="csv-url-input" class="label-sezione">Link al tuo file .csv su GitHub:</label>
                    <input type="text" id="csv-url-input" class="input-text" placeholder="https://raw.githubusercontent.com/tuo-nome/...">
                    <button id="update-url-btn" class="btn">Aggiorna da Link</button>
                    <p id="messaggio-update" class="messaggio-feedback"></p>
                </div>

                <div id="import-container" class="sezione-gestione">
                    <label id="import-label" for="import-csv-file" class="label-sezione">Oppure... Importa da file locale</label>
                    <input type="file" id="import-csv-file" accept=".csv">
                    <p id="messaggio-import" class="messaggio-feedback"></p>
                    <div id="import-dettagli">
                        <h3>Parole Saltate:</h3>
                        <ul id="import-errori-lista"></ul>
                    </div>
                </div>
                
                <div class="sezione-gestione">
                    <h2>Aggiungi una Parola Manualmente</h2>
                    <form id="form-aggiungi">
                        <div><label for="input-ita">Italiano:</label><input type="text" id="input-ita" required></div>
                        <div><label for="input-eng">Inglese:</label><input type="text" id="input-eng" required></div>
                        <div><label for="input-jpn">Giapponese (Kanji/Hiragana):</label><input type="text" id="input-jpn" required></div>
                        <div><label for="input-romaji">Romaji (Pronuncia):</label><input type="text" id="input-romaji" required></div>
                        <div><label for="input-esempi">Frase Esempio (Opzionale):</label><input type="text" id="input-esempi"></div>
                        <button type="submit" class="btn">Salva Parola Manualmente</button>
                    </form>
                    <p id="messaggio-salvataggio" class="messaggio-feedback"></p>
                </div>

                <button id="svuota-tutto-btn" class="btn" style="background-color: #d92c23; margin-top: 20px;">CANCELLA TUTTO IL VOCABOLARIO</button>
            </div>
        </div> 
        
        <div id="modulo-hiragana" class="modulo-content">
            <div class="card-ui">
                <h2>Alfabeto Hiragana (ひらがな)</h2>
                <h3>Gojūon (Suoni base)</h3>
                <div class="table-container">
                    <table class="kana-table">
                        <tr><th></th><th>A</th><th>I</th><th>U</th><th>E</th><th>O</th></tr>
                        <tr><td><b>-</b></td><td class="kana">あ</td><td class="kana">い</td><td class="kana">う</td><td class="kana">え</td><td class="kana">お</td></tr>
                        <tr><td></td><td class="romaji">a</td><td class="romaji">i</td><td class="romaji">u</td><td class="romaji">e</td><td class="romaji">o</td></tr>
                        <tr><td><b>K</b></td><td class="kana">か</td><td class="kana">き</td><td class="kana">く</td><td class="kana">け</td><td class="kana">こ</td></tr>
                        <tr><td></td><td class="romaji">ka</td><td class="romaji">ki</td><td class="romaji">ku</td><td class="romaji">ke</td><td class="romaji">ko</td></tr>
                        <tr><td><b>S</b></td><td class="kana">さ</td><td class="kana">し</td><td class="kana">す</td><td class="kana">せ</td><td class="kana">そ</td></tr>
                        <tr><td></td><td class="romaji">sa</td><td class="romaji">shi</td><td class="romaji">su</td><td class="romaji">se</td><td class="romaji">so</td></tr>
                        <tr><td><b>T</b></td><td class="kana">た</td><td class="kana">ち</td><td class="kana">つ</td><td class="kana">て</td><td class="kana">と</td></tr>
                        <tr><td></td><td class="romaji">ta</td><td class="romaji">chi</td><td class="romaji">tsu</td><td class="romaji">te</td><td class="romaji">to</td></tr>
                        <tr><td><b>N</b></td><td class="kana">な</td><td class="kana">に</td><td class="kana">ぬ</td><td class="kana">ね</td><td class="kana">の</td></tr>
                        <tr><td></td><td class="romaji">na</td><td class="romaji">ni</td><td class="romaji">nu</td><td class="romaji">ne</td><td class="romaji">no</td></tr>
                        <tr><td><b>H</b></td><td class="kana">は</td><td class="kana">ひ</td><td class="kana">ふ</td><td class="kana">へ</td><td class="kana">ほ</td></tr>
                        <tr><td></td><td class="romaji">ha</td><td class="romaji">hi</td><td class="romaji">fu</td><td class="romaji">he</td><td class="romaji">ho</td></tr>
                        <tr><td><b>M</b></td><td class="kana">ま</td><td class="kana">み</td><td class="kana">む</td><td class="kana">め</td><td class="kana">も</td></tr>
                        <tr><td></td><td class="romaji">ma</td><td class="romaji">mi</td><td class="romaji">mu</td><td class="romaji">me</td><td class="romaji">mo</td></tr>
                        <tr><td><b>Y</b></td><td class="kana">や</td><td></td><td class="kana">ゆ</td><td></td><td class="kana">よ</td></tr>
                        <tr><td></td><td class="romaji">ya</td><td></td><td class="romaji">yu</td><td></td><td class="romaji">yo</td></tr>
                        <tr><td><b>R</b></td><td class="kana">ら</td><td class="kana">り</td><td class="kana">る</td><td class="kana">れ</td><td class="kana">ろ</td></tr>
                        <tr><td></td><td class="romaji">ra</td><td class="romaji">ri</td><td class="romaji">ru</td><td class="romaji">re</td><td class="romaji">ro</td></tr>
                        <tr><td><b>W</b></td><td class="kana">わ</td><td></td><td></td><td></td><td class="kana">を</td></tr>
                        <tr><td></td><td class="romaji">wa</td><td></td><td></td><td></td><td class="romaji">o (wo)</td></tr>
                        <tr><td><b>N</b></td><td class="kana">ん</td><td></td><td></td><td></td><td></td></tr>
                        <tr><td></td><td class="romaji">n</td><td></td><td></td><td></td><td></td></tr>
                    </table>
                </div>
            </div>
        </div> 

        <div id="modulo-katakana" class="modulo-content">
            <div class="card-ui">
                <h2>Alfabeto Katakana (カタカナ)</h2>
                <h3>Gojūon (Suoni base)</h3>
                <div class="table-container">
                    <table class="kana-table">
                        <tr><th></th><th>A</th><th>I</th><th>U</th><th>E</th><th>O</th></tr>
                        <tr><td><b>-</b></td><td class="kana">ア</td><td class="kana">イ</td><td class="kana">ウ</td><td class="kana">エ</td><td class="kana">オ</td></tr>
                        <tr><td></td><td class="romaji">a</td><td class="romaji">i</td><td class="romaji">u</td><td class="romaji">e</td><td class="romaji">o</td></tr>
                        <tr><td><b>K</b></td><td class="kana">カ</td><td class="kana">キ</td><td class="kana">ク</td><td class="kana">ケ</td><td class="kana">コ</td></tr>
                        <tr><td></td><td class="romaji">ka</td><td class="romaji">ki</td><td class="romaji">ku</td><td class="romaji">ke</td><td class="romaji">ko</td></tr>
                        <tr><td><b>S</b></td><td class="kana">サ</td><td class="kana">シ</td><td class="kana">ス</td><td class="kana">セ</td><td class="kana">ソ</td></tr>
                        <tr><td></td><td class="romaji">sa</td><td class="romaji">shi</td><td class="romaji">su</td><td class="romaji">se</td><td class="romaji">so</td></tr>
                        <tr><td><b>T</b></td><td class="kana">タ</td><td class="kana">チ</td><td class="kana">ツ</td><td class="kana">テ</td><td class="kana">ト</td></tr>
                        <tr><td></td><td class="romaji">ta</td><td class="romaji">chi</td><td class="romaji">tsu</td><td class="romaji">te</td><td class="romaji">to</td></tr>
                        <tr><td><b>N</b></td><td class="kana">ナ</td><td class="kana">ニ</td><td class="kana">ヌ</td><td class="kana">ネ</td><td class="kana">ノ</td></tr>
                        <tr><td></td><td class="romaji">na</td><td class="romaji">ni</td><td class="romaji">nu</td><td class="romaji">ne</td><td class="romaji">no</td></tr>
                        <tr><td><b>H</b></td><td class="kana">ハ</td><td class="kana">ヒ</td><td class="kana">フ</td><td class="kana">ヘ</td><td class="kana">ホ</td></tr>
                        <tr><td></td><td class="romaji">ha</td><td class="romaji">hi</td><td class="romaji">fu</td><td class="romaji">he</td><td class="romaji">ho</td></tr>
                        <tr><td><b>M</b></td><td class="kana">マ</td><td class="kana">ミ</td><td class="kana">ム</td><td class="kana">メ</td><td class="kana">モ</td></tr>
                        <tr><td></td><td class="romaji">ma</td><td class="romaji">mi</td><td class="romaji">mu</td><td class="romaji">me</td><td class="romaji">mo</td></tr>
                        <tr><td><b>Y</b></td><td class="kana">ヤ</td><td></td><td class="kana">ユ</td><td></td><td class="kana">ヨ</td></tr>
                        <tr><td></td><td class="romaji">ya</td><td></td><td class="romaji">yu</td><td></td><td class="romaji">yo</td></tr>
                        <tr><td><b>R</b></td><td class="kana">ラ</td><td class="kana">リ</td><td class="kana">ル</td><td class="kana">レ</td><td class="kana">ロ</td></tr>
                        <tr><td></td><td class="romaji">ra</td><td class="romaji">ri</td><td class="romaji">ru</td><td class="romaji">re</td><td class="romaji">ro</td></tr>
                        <tr><td><b>W</b></td><td class="kana">ワ</td><td></td><td></td><td></td><td class="kana">ヲ</td></tr>
                        <tr><td></td><td class="romaji">wa</td><td></td><td></td><td></td><td class="romaji">o (wo)</td></tr>
                        <tr><td><b>N</b></td><td class="kana">ン</td><td></td><td></td><td></td><td></td></tr>
                        <tr><td></td><td class="romaji">n</td><td></td><td></td><td></td><td></td></tr>
                    </table>
                </div>
            </div>
        </div> 
        
        <div id="modulo-vocaboli" class="modulo-content">
            <div class="card-ui">
                <h2>Lista Vocaboli</h2>
                <p>Tutti i vocaboli presenti nel tuo mazzo, ordinati per Italiano. I caratteri sono colorati: <span class="hiragana">Hiragana (verde)</span>, <span class="katakana">Katakana (giallo)</span> e <span class="kanji">Kanji (blu)</span>.</p>
                
                <p id="vocaboli-count"></p> 
                
                <button id="copia-vocaboli-btn" class="btn" style="background-color: #ff9500; margin-top: 10px;">Copia Vocaboli (CSV)</button>

                <div id="lista-vocaboli-container">
                </div>
            </div>
        </div> 
    </main>


    <script>
        // --- 1. Elementi DOM ---
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
        const salvaMessaggio = document.getElementById('salva-messaggio');
        const nessunaCartaMsg = document.getElementById('nessuna-carta');
        const quizContainer = document.getElementById('quiz-container');
        const btnSalvaSessione = document.getElementById('salva-sessione');
        const btnRipassaErrori = document.getElementById('ripassa-errori-btn');
        const form = document.getElementById('form-aggiungi');
        const inputIta = document.getElementById('input-ita');
        const inputEng = document.getElementById('input-eng');
        const inputJpn = document.getElementById('input-jpn');
        const inputRomaji = document.getElementById('input-romaji');
        const inputEsempi = document.getElementById('input-esempi'); 
        const messaggioSalvataggio = document.getElementById('messaggio-salvataggio');
        const fileInput = document.getElementById('import-csv-file');
        const importMessaggio = document.getElementById('messaggio-import');
        const importDettagli = document.getElementById('import-dettagli');
        const importErroriLista = document.getElementById('import-errori-lista');
        const urlInput = document.getElementById('csv-url-input');
        const updateUrlBtn = document.getElementById('update-url-btn');
        const updateMessaggio = document.getElementById('messaggio-update');
        const esempioDisplay = document.getElementById('esempio-display'); 
        const svuotaTuttoBtn = document.getElementById('svuota-tutto-btn'); 
        
        const navButtons = document.querySelectorAll('.nav-btn');
        const mainNav = document.getElementById('main-nav');
        const moduliContenuto = document.querySelectorAll('.modulo-content');
        const vocaboliContainer = document.getElementById('lista-vocaboli-container');
        const vocaboliCountElement = document.getElementById('vocaboli-count'); 
        const copiaVocaboliBtn = document.getElementById('copia-vocaboli-btn'); 

        // --- 2. Dati e Stato ---
        let totalUniqueCards = 0; 
        let audioEngineWoken = false; 
        
        let mazzoPrincipale = [];
        let mazzoErroriPrioritari = [];
        let erroriSessioneCorrente;      
        let mazzoRipassoAttivo = [];
        let mazzoSessioneCorrente = []; 
        let indiceSessione = 0;          
        let parolaCorrente = null;
        let quizDirection = 'ITA_TO_JPN';
        let modalitaQuiz = 'normale'; 
        let sessioneCorretti = 0;
        let sessioneSbagliati = 0;

        const KEY_MAZZO_PRINCIPALE = 'mioMazzoPrincipale';
        const KEY_MAZZO_ERRORI = 'mioMazzoErrori';
        const KEY_CSV_URL = 'mioLinkCsvGitHub';
        
        let japaneseVoice = null;
        const synth = window.speechSynthesis;

        // --- 3. Funzioni Audio e Utility ---
        function caricaVoceGiapponese() {
            if (!synth) return;
            const voices = synth.getVoices();
            japaneseVoice = voices.find(voice => voice.lang.startsWith('ja'));
            if (!japaneseVoice && voices.length > 0) { console.warn("Nessuna voce 'ja-JP' trovata."); }
        }
        
        function parla(testo) {
            if (!synth || !testo) return;
            if (synth.speaking) { synth.cancel(); }
            const utterance = new SpeechSynthesisUtterance(testo);
            if (!japaneseVoice) {
                const voices = synth.getVoices();
                japaneseVoice = voices.find(voice => voice.lang.startsWith('ja'));
            }
            if (japaneseVoice) { utterance.voice = japaneseVoice; } 
            else { utterance.lang = 'ja-JP'; } 
            utterance.onerror = (event) => { console.error('Speech Synthesis Error:', event.error); };
            synth.speak(utterance);
        }
        
        function wakeUpAudioEngine() {
            if (synth && !audioEngineWoken) {
                const utterance = new SpeechSynthesisUtterance(' '); 
                synth.speak(utterance); 
                audioEngineWoken = true;
            }
        }
        
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }
        
        function getUniqueTotalCountFromLocalStorage() {
            const datiPrincipali = JSON.parse(localStorage.getItem(KEY_MAZZO_PRINCIPALE) || '[]');
            const datiErrori = JSON.parse(localStorage.getItem(KEY_MAZZO_ERRORI) || '[]');
            const mazzoCompleto = [...datiPrincipali, ...datiErrori];
            const mazzoUnico = Array.from(new Map(mazzoCompleto.map(item => [item.ita.toLowerCase(), item])).values());
            return mazzoUnico.length;
        }


        // --- 4. Funzione Creazione Set ---
        function creaNuovoSet(numParole = 10) {
            mazzoSessioneCorrente = [];
            indiceSessione = 0;
            
            let paroleTotaliDisponibili = mazzoErroriPrioritari.length + mazzoPrincipale.length;
            let paroleDaPrendere = Math.min(numParole, paroleTotaliDisponibili);
            
            if (paroleDaPrendere === 0) { return; }

            let paroleDaErrori = mazzoErroriPrioritari.splice(0, paroleDaPrendere);
            mazzoSessioneCorrente.push(...paroleDaErrori);
            
            let numRimanenti = paroleDaPrendere - paroleDaErrori.length;
            if (numRimanenti > 0) {
                let paroleDaMazzo = mazzoPrincipale.splice(0, numRimanenti);
                mazzoSessioneCorrente.push(...paroleDaMazzo);
            }
            mazzoSessioneCorrente = shuffleArray(mazzoSessioneCorrente);
        }
        
        // --- 5. Funzioni Quiz ---
        function prossimaParola() {
            if (modalitaQuiz === 'set_finito') {
                modalitaQuiz = 'normale'; 
                creaNuovoSet();          
            }

            promptContainer.style.display = 'flex';
            inputRisposta.style.display = 'block';
            risultatoControllo.style.display = 'block';
            esempioDisplay.innerHTML = ''; 
            
            if (modalitaQuiz === 'ripasso_errori') {
                if (mazzoRipassoAttivo.length > 0) {
                    parolaCorrente = mazzoRipassoAttivo.shift();
                    promptLabel.textContent = `✨ RIPASSO (${mazzoRipassoAttivo.length + 1} Rimanenti) ✨`;
                } else {
                    alert("Complimenti! Hai ripassato tutti gli errori di questa sessione.");
                    modalitaQuiz = 'normale';
                }
            }
            
            if (modalitaQuiz === 'normale') {
                if (indiceSessione < mazzoSessioneCorrente.length) {
                    parolaCorrente = mazzoSessioneCorrente[indiceSessione];
                    indiceSessione++;
                    
                    let etichettaBase = `(Set ${indiceSessione}/${mazzoSessioneCorrente.length}):`;
                    const jpnClean = parolaCorrente.jpn.split('/')[0];

                    if (Math.random() < 0.5) {
                        quizDirection = 'ITA_TO_JPN';
                        promptLabel.textContent = `TRADUCI ${etichettaBase} (Romaji o Giapponese)`;
                        promptPrincipale.textContent = parolaCorrente.ita.split('/')[0];
                        promptSecondario.textContent = parolaCorrente.eng.split('/')[0];
                        inputRisposta.placeholder = "Scrivi qui la tua risposta...";
                    } else {
                        quizDirection = 'JPN_TO_ITA';
                        promptLabel.textContent = `TRADUCI ${etichettaBase} (Italiano o Inglese)`;
                        
                        const jpnColorato = colorizeJapanese(jpnClean); 
                        const audioBtnHtml = `<button class="btn-audio" onclick="parla('${jpnClean}')">🔊</button>`;
                        
                        promptPrincipale.innerHTML = `${jpnColorato} ${audioBtnHtml}`;
                        promptSecondario.textContent = parolaCorrente.romaji.split('/')[0];
                        inputRisposta.placeholder = "Scrivi la traduzione italiana...";
                    }
                } else {
                    modalitaQuiz = 'set_finito';
                    promptLabel.textContent = "BRAVO!";
                    promptPrincipale.innerHTML = "🎉 Set Completato! 🎉";
                    promptSecondario.textContent = `Errori salvati: ${mazzoErroriPrioritari.length}, Mazzo: ${mazzoPrincipale.length}`;
                    inputRisposta.style.display = 'none';
                    risultatoControllo.style.display = 'none';
                    btnControlla.style.display = 'none';
                    btnProssima.textContent = 'Inizia Prossimo Set';
                    btnProssima.style.display = 'block';
                    
                    if (mazzoErroriPrioritari.length === 0 && mazzoPrincipale.length === 0) {
                        promptPrincipale.innerHTML = "🎉 MAZZO COMPLETATO! 🎉";
                        promptSecondario.textContent = "Hai finito tutte le parole. Importane di nuove!";
                        btnProssima.style.display = 'none';
                    }
                    return; 
                }
            }

            inputRisposta.value = "";
            risultatoControllo.innerHTML = "";
            inputRisposta.disabled = false;
            btnControlla.style.display = "block";
            btnProssima.style.display = "none";
            aggiornaPunteggio();
        }

        function controllaRisposta() {
            if (!parolaCorrente) return; 
            const rispostaUtente = inputRisposta.value.trim().toLowerCase();
            let isCorrect = false;
            let messaggioErrore = "";
            inputRisposta.disabled = true; 
            btnControlla.style.display = "none";
            btnProssima.style.display = "block"; 
            
            const jpnClean = parolaCorrente.jpn.split('/')[0];
            const jpnColorato = colorizeJapanese(jpnClean); 
            let audioBtnHtml = `<button class="btn-audio" onclick="parla('${jpnClean}')">🔊</button>`;
            
            if (quizDirection === 'JPN_TO_ITA') { audioBtnHtml = ""; }

            if (quizDirection === 'ITA_TO_JPN') {
                const risposteCorretteRomaji = parolaCorrente.romaji.trim().toLowerCase().split('/');
                const risposteCorretteJpn = parolaCorrente.jpn.trim().toLowerCase().split('/'); 
                isCorrect = (risposteCorretteRomaji.includes(rispostaUtente) || risposteCorretteJpn.includes(rispostaUtente));
                messaggioErrore = `Risposte: <b>${risposteCorretteRomaji[0]}</b> (<b>${jpnColorato}</b>${audioBtnHtml})`;
            } else { 
                const risposteCorretteIta = parolaCorrente.ita.trim().toLowerCase().split('/');
                const risposteCorretteEng = parolaCorrente.eng.trim().toLowerCase().split('/');
                isCorrect = (risposteCorretteIta.includes(rispostaUtente) || risposteCorretteEng.includes(rispostaUtente));
                messaggioErrore = `Risposte: <b>${risposteCorretteIta[0]}</b> (o <b>${risposteCorretteEng[0]}</b>)`;
            }
            
            if (parolaCorrente.esempi && parolaCorrente.esempi.trim() !== "") {
                esempioDisplay.innerHTML = `<div id="esempio-display">Esempio: ${parolaCorrente.esempi}</div>`;
            } else {
                esempioDisplay.innerHTML = '';
            }

            if (isCorrect) {
                risultatoControllo.innerHTML = `🎉 Corretto! ${audioBtnHtml}`;
                risultatoControllo.className = "corretto";
                sessioneCorretti++;
                if (quizDirection === 'ITA_TO_JPN') parla(jpnClean);
            } else {
                risultatoControllo.innerHTML = `<span class="sbagliato">Sbagliato.</span><br> ${messaggioErrore}`;
                risultatoControllo.className = "sbagliato";
                sessioneSbagliati++;
                erroriSessioneCorrente.add(parolaCorrente);
                if (modalitaQuiz === 'ripasso_errori') {
                    mazzoRipassoAttivo.push(parolaCorrente);
                }
            }
            aggiornaPunteggio();
        }
        
        function avviaRipassoErrori() {
            if (erroriSessioneCorrente.size === 0) {
                alert("Nessun errore in questa sessione da ripassare!");
                return;
            }
            modalitaQuiz = 'ripasso_errori';
            mazzoRipassoAttivo = Array.from(erroriSessioneCorrente); 
            erroriSessioneCorrente.clear(); 
            alert(`Modalità Ripasso Attivata! Ripasserai ${mazzoRipassoAttivo.length} parole.`);
            prossimaParola();
        }

        // --- 6. Funzioni Salvataggio e Gestione Mazzo ---
        function aggiornaPunteggio() {
            punteggioDisplay.innerHTML = `
                <span class="punteggio-corr">Corretti: ${sessioneCorretti}</span> | 
                <span class="punteggio-sbag">Sbagliati: ${sessioneSbagliati}</span> <br> 
                <span class="punteggio-prio">Da Rivedere: ${mazzoErroriPrioritari.length}</span> |
                <span class="punteggio-mazzo">Mazzo: ${mazzoPrincipale.length}</span> |
                <span class="punteggio-sessione">Errori: ${erroriSessioneCorrente.size + mazzoRipassoAttivo.length}</span>
                <br>
                <b>Totale Vocaboli Unici: ${totalUniqueCards}</b>
            `;
            const numErroriSessione = erroriSessioneCorrente.size + mazzoRipassoAttivo.length;
            if (numErroriSessione > 0) {
                btnRipassaErrori.disabled = false;
                btnRipassaErrori.textContent = `Ripassa Errori Sessione (${numErroriSessione})`;
            } else {
                btnRipassaErrori.disabled = true;
                btnRipassaErrori.textContent = `Ripassa Errori Sessione (0)`;
            }
        }

        function eliminaParola(itaKey) {
            if (confirm(`Sei sicuro di voler ELIMINARE PERMANENTEMENTE la carta "${itaKey}" dal tuo mazzo? Questa azione è irreversibile!`)) {
                let mazzoP = JSON.parse(localStorage.getItem(KEY_MAZZO_PRINCIPALE) || '[]');
                let mazzoE = JSON.parse(localStorage.getItem(KEY_MAZZO_ERRORI) || '[]');
                mazzoP = mazzoP.filter(c => c.ita !== itaKey);
                mazzoE = mazzoE.filter(c => c.ita !== itaKey);
                localStorage.setItem(KEY_MAZZO_PRINCIPALE, JSON.stringify(mazzoP));
                localStorage.setItem(KEY_MAZZO_ERRORI, JSON.stringify(mazzoE));
                alert('Carta eliminata. Ricarico l\'app...');
                location.reload(); 
            }
        }
        
        function svuotaMazziTotali() {
            if (confirm("ATTENZIONE! Sei sicuro di voler ELIMINARE TUTTO il vocabolario salvato (tutte le parole e gli errori)? Questa azione è irreversibile!")) {
                localStorage.removeItem(KEY_MAZZO_PRINCIPALE);
                localStorage.removeItem(KEY_MAZZO_ERRORI);
                alert("Tutti i vocaboli sono stati eliminati. Ricarico l'app...");
                location.reload();
            }
        }
        
        function salvaMazzoPrincipale() {
            const mazzoCompleto = [...mazzoPrincipale, ...mazzoErroriPrioritari, ...mazzoSessioneCorrente];
            const mazzoUnico = Array.from(new Map(mazzoCompleto.map(item => [item.ita, item])).values());
            localStorage.setItem(KEY_MAZZO_PRINCIPALE, JSON.stringify(mazzoUnico));
        }

        function salvaErroriSessione() {
            const tuttiErroriDaSalvare = new Set([...erroriSessioneCorrente, ...mazzoRipassoAttivo]);
            localStorage.setItem(KEY_MAZZO_ERRORI, JSON.stringify(Array.from(tuttiErroriDaSalvare)));
            salvaMessaggio.textContent = "Errori salvati! Ricarico la sessione...";
            setTimeout(() => { location.reload(); }, 1500);
        }

        function caricaMazzi() {
            const datiPrincipali = localStorage.getItem(KEY_MAZZO_PRINCIPALE);
            mazzoPrincipale = datiPrincipali ? JSON.parse(datiPrincipali) : [];
            const datiErrori = localStorage.getItem(KEY_MAZZO_ERRORI);
            mazzoErroriPrioritari = datiErrori ? JSON.parse(datiErrori) : [];
            
            totalUniqueCards = getUniqueTotalCountFromLocalStorage();
            
            const paroleInErrori = new Set(mazzoErroriPrioritari.map(p => p.ita));
            mazzoPrincipale = mazzoPrincipale.filter(p => !paroleInErrori.has(p.ita));
            
            mazzoPrincipale = shuffleArray(mazzoPrincipale);
            mazzoErroriPrioritari = shuffleArray(mazzoErroriPrioritari);

            const urlSalvat = localStorage.getItem(KEY_CSV_URL);
            if (urlSalvat) { urlInput.value = urlSalvat; }
            
            erroriSessioneCorrente = new Set();
            sessioneCorretti = 0;
            sessioneSbagliati = 0;
            
            creaNuovoSet();
        }

        function aggiungiParolaAlMazzo(nuovaParola) {
             const parolaTrimmed = {
                ita: nuovaParola.ita ? nuovaParola.ita.trim() : "",
                eng: nuovaParola.eng ? nuovaParola.eng.trim() : "",
                jpn: nuovaParola.jpn ? nuovaParola.jpn.trim() : "",
                romaji: nuovaParola.romaji ? nuovaParola.romaji.trim() : "",
                esempi: nuovaParola.esempi ? nuovaParola.esempi.trim() : "" 
            };
            if (!parolaTrimmed.ita || !parolaTrimmed.eng || !parolaTrimmed.jpn || !parolaTrimmed.romaji) {
                return { success: false, motivo: "Campi mancanti" };
            }
            
            // Controllo duplicati su ITA + JPN
            const uniqueKey = `${parolaTrimmed.ita.toLowerCase()}||${parolaTrimmed.jpn.toLowerCase()}`;
            const esisteGia = [...mazzoPrincipale, ...mazzoErroriPrioritari, ...mazzoSessioneCorrente, ...mazzoRipassoAttivo]
                .some(carta => `${carta.ita.toLowerCase()}||${carta.jpn.toLowerCase()}` === uniqueKey);
            
            if (esisteGia) {
                return { success: false, motivo: "Duplicato (Stesso significato italiano E stessa parola giapponese)" };
            }
            
            mazzoPrincipale.push(parolaTrimmed);
            return { success: true };
        }

        function gestisciSalvataggioForm(event) {
            event.preventDefault();
            const nuovaParola = {
                ita: inputIta.value,
                eng: inputEng.value,
                jpn: inputJpn.value,
                romaji: inputRomaji.value,
                esempi: inputEsempi.value 
            };
            const risultato = aggiungiParolaAlMazzo(nuovaParola);
            if (risultato.success) {
                salvaMazzoPrincipale(); 
                form.reset();
                messaggioSalvataggio.textContent = "Parola salvata! Ricarico l\'app...";
                setTimeout(() => { location.reload(); }, 1500); 
            } else {
                alert(`Errore: ${risultato.motivo}. La parola non è stata salvata.`);
            }
        }
        
        function processaTestoCSV(testo) {
            const righe = testo.split('\n');
            let paroleAggiunte = 0;
            let paroleSaltate = 0; 
            importDettagli.style.display = "none";
            importErroriLista.innerHTML = "";
            const primaRiga = righe[0].toLowerCase();
            const inizio = (primaRiga.includes("italiano") || primaRiga.includes("inglese")) ? 1 : 0;
            for (let i = inizio; i < righe.length; i++) {
                const riga = righe[i].trim();
                if (riga === "") continue;
                const colonne = riga.split(','); 
                if (colonne.length < 4) {
                    paroleSaltate++;
                    paroleSaltate.push({ parola: riga.split(',')[0] || "Riga illeggibile", motivo: "Malformata (Mancano campi obbligatori)" });
                    continue;
                }
                const parola = { 
                    ita: colonne[0], 
                    eng: colonne[1], 
                    jpn: colonne[2], 
                    romaji: colonne[3], 
                    esempi: colonne[4] || "" 
                };
                const risultato = aggiungiParolaAlMazzo(parola);
                if (risultato.success) {
                    paroleAggiunte++;
                } else {
                    paroleSaltate++;
                    // Non logghiamo l'errore se è un semplice duplicato durante un ricaricamento massivo, per pulizia
                }
            }
            
            salvaMazzoPrincipale();
            return { aggiunte: paroleAggiunte, saltate: paroleSaltate };
        }
        
        function gestisciImportaCSV(event) {
            const file = event.target.files[0];
            if (!file) return;
            importMessaggio.textContent = "Caricamento in corso...";
            const reader = new FileReader();
            reader.onload = function(e) {
                const testo = e.target.result;
                const { aggiunte, saltate } = processaTestoCSV(testo);
                importMessaggio.textContent = `Importazione completata! Aggiunte: ${aggiunte}, Saltate/Duplicati: ${saltate}.`;
                importMessaggio.className = "corretto";
                fileInput.value = ""; 
                if (aggiunte > 0) {
                    setTimeout(() => {
                        alert(`Aggiunte ${aggiunte} nuove parole. Ricarica la pagina per iniziare!`);
                        location.reload();
                    }, 1000);
                }
            };
            reader.onerror = function() {
                importMessaggio.textContent = "Errore nella lettura del file.";
                importMessaggio.className = "sbagliato";
            };
            reader.readAsText(file, "UTF-8");
        }
        
        function gestisciAggiornaDaUrl() {
            const url = urlInput.value.trim();
            if (!url) {
                alert("Per favor, inserisci un link URL al file .csv");
                return;
            }
            localStorage.setItem(KEY_CSV_URL, url);
            updateMessaggio.textContent = "Aggiornamento in corso...";
            updateMessaggio.className = "";
            try {
                fetch(url).then(response => {
                    if (!response.ok) { throw new Error(`Errore di rete: ${response.statusText}`); }
                    return response.text();
                }).then(testo => {
                    const { aggiunte, saltate } = processaTestoCSV(testo);
                    updateMessaggio.textContent = `Aggiornato! Aggiunte: ${aggiunte}, Saltate/Duplicati: ${saltate}.`;
                    updateMessaggio.className = "corretto";
                    if (aggiunte > 0) {
                        setTimeout(() => {
                            alert(`Aggiunte ${aggiunte} nuove parole. Ricarica la pagina per iniziare!`);
                            location.reload();
                        }, 1000);
                    } else {
                        alert("Nessuna nuova parola trovata nel file remoto.");
                    }
                }).catch(error => {
                    console.error("Errore aggiornamento da URL:", error);
                    updateMessaggio.textContent = "Errore! Controlla il link e la tua connessione.";
                    updateMessaggio.className = "sbagliato";
                });
            } catch (error) {
                console.error("Errore aggiornamento da URL (sincrono):", error);
            }
        }

        function controllaMazzoVuoto() {
            if (mazzoPrincipale.length === 0 && mazzoErroriPrioritari.length === 0 && mazzoSessioneCorrente.length === 0) {
                nessunaCartaMsg.style.display = "block";
                quizContainer.style.display = "none";
            } else {
                nessunaCartaMsg.style.display = "none";
                quizContainer.style.display = "block";
                if (parolaCorrente === null) { 
                    prossimaParola();
                }
            }
        }
        
        // --- 7. Funzioni di Navigazione ---
        function colorizeJapanese(jpnText) {
            let output = '';
            for (const char of jpnText) {
                const code = char.codePointAt(0);
                if (code >= 0x3040 && code <= 0x309F) {
                    output += `<span class="hiragana">${char}</span>`;
                } else if (code >= 0x30A0 && code <= 0x30FF) {
                    output += `<span class="katakana">${char}</span>`;
                } else if (code >= 0x4E00 && code <= 0x9FFF) {
                    output += `<span class="kanji">${char}</span>`;
                } else {
                    output += char;
                }
            }
            return output;
        }

        function copiaVocaboli() {
            const datiPrincipali = JSON.parse(localStorage.getItem(KEY_MAZZO_PRINCIPALE) || '[]');
            const datiErrori = JSON.parse(localStorage.getItem(KEY_MAZZO_ERRORI) || '[]');
            const mazzoCompleto = [...datiPrincipali, ...datiErrori];
            const mazzoUnico = Array.from(new Map(mazzoCompleto.map(item => [item.ita.toLowerCase(), item])).values());
            
            let csvContent = "Italiano,Inglese,Giapponese,Romaji,Esempi\n"; 
            
            for (const parola of mazzoUnico) {
                const ita = parola.ita.split('/')[0];
                const eng = parola.eng.split('/')[0];
                const jpn = parola.jpn.split('/')[0];
                const romaji = parola.romaji.split('/')[0];
                const esempi = parola.esempi || ""; 
                csvContent += `"${ita}","${eng}","${jpn}","${romaji}","${esempi}"\n`;
            }
            
            navigator.clipboard.writeText(csvContent).then(() => {
                alert(`Copiato ${mazzoUnico.length} vocaboli in formato CSV negli appunti!`);
            }, () => {
                alert("Errore durante la copia negli appunti.");
            });
        }

        function mostraListaVocaboli() {
            const datiPrincipali = JSON.parse(localStorage.getItem(KEY_MAZZO_PRINCIPALE) || '[]');
            const datiErrori = JSON.parse(localStorage.getItem(KEY_MAZZO_ERRORI) || '[]');
            
            const mazzoCompleto = [...datiPrincipali, ...datiErrori];
            const mazzoUnico = Array.from(new Map(mazzoCompleto.map(item => [item.ita.toLowerCase(), item])).values());

            mazzoUnico.sort((a, b) => a.ita.localeCompare(b.ita, 'it'));

            if (vocaboliCountElement) {
                vocaboliCountElement.innerHTML = `<b>Totale Parole nel Mazzo Quiz: ${totalUniqueCards}</b>`;
            }

            vocaboliContainer.innerHTML = ""; 
            
            if (mazzoUnico.length === 0) {
                vocaboliContainer.innerHTML = "<p>Nessun vocabolo trovato. Aggiungine qualcuno nel modulo Quiz!</p>";
                return;
            }

            let listaHtml = "";
            for (const parola of mazzoUnico) {
                const ita = parola.ita.split('/')[0];
                const eng = parola.eng.split('/')[0];
                const jpn = parola.jpn.split('/')[0];
                const romaji = parola.romaji.split('/')[0];
                const esempi = parola.esempi || ""; 

                const jpnColorato = colorizeJapanese(jpn); 
                
                const esempiHtml = esempi 
                    ? `<div class="vocab-esempi">Esempio: ${esempi}</div>`
                    : '';

                listaHtml += `
                    <div class="vocab-entry">
                        <div class="vocab-entry-content">
                            <div class="vocab-entry-principale">
                                <span class="vocab-ita">${ita}</span>
                                <span class="vocab-jpn">${jpnColorato}</span> 
                            </div>
                            <div class="vocab-entry-secondario">
                                <span class="vocab-eng">${eng}</span>
                                <span class="vocab-romaji">${romaji}</span>
                            </div>
                            ${esempiHtml}
                        </div>
                        <button class="delete-vocab-btn" data-delete-key="${parola.ita}">[X]</button>
                    </div>
                `;
            }

            vocaboliContainer.innerHTML = listaHtml;
            
            document.querySelectorAll('.delete-vocab-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    const itaKey = e.target.dataset.deleteKey;
                    eliminaParola(itaKey);
                });
            });
        }

        function mostraModulo(idModulo) {
            wakeUpAudioEngine();
            
            moduliContenuto.forEach(modulo => {
                modulo.style.display = 'none';
            });
            navButtons.forEach(btn => {
                btn.classList.toggle('active', btn.dataset.modulo === idModulo);
            });
            const moduloDaMostrare = document.getElementById(`modulo-${idModulo}`);
            if (moduloDaMostrare) {
                moduloDaMostrare.style.display = 'block'; 
            }
            if (idModulo === 'quiz') {
                controllaMazzoVuoto();
            } else if (idModulo === 'vocaboli') {
                mostraListaVocaboli();
            }
        }

        // --- 8. Inizializzazione ---
        document.addEventListener('DOMContentLoaded', () => {
            if (synth) {
                synth.onvoiceschanged = caricaVoceGiapponese;
                caricaVoceGiapponese(); 
            }
            caricaMazzi();
            
            btnControlla.addEventListener('click', controllaRisposta);
            btnProssima.addEventListener('click', prossimaParola);
            btnElimina.addEventListener('click', () => {
                if (parolaCorrente) {
                    eliminaParola(parolaCorrente.ita);
                }
            }); 
            btnSalvaSessione.addEventListener('click', salvaErroriSessione);
            btnRipassaErrori.addEventListener('click', avviaRipassoErrori);
            form.addEventListener('submit', gestisciSalvataggioForm); 
            fileInput.addEventListener('change', gestisciImportaCSV);
            updateUrlBtn.addEventListener('click', gestisciAggiornaDaUrl);
            inputRisposta.addEventListener('keydown', (e) => {
                if (e.key === 'Enter') {
                    if (btnControlla.style.display === 'block') {
                        controllaRisposta();
                    } else if (btnProssima.style.display === 'block') {
                        prossimaParola();
                    }
                }
            });

            svuotaTuttoBtn.addEventListener('click', svuotaMazziTotali);

            mainNav.addEventListener('click', (e) => {
                if (e.target.closest('.nav-btn')) {
                    const btn = e.target.closest('.nav-btn');
                    const idModulo = btn.dataset.modulo;
                    mostraModulo(idModulo);
                }
            });

            if (copiaVocaboliBtn) {
                copiaVocaboliBtn.addEventListener('click', copiaVocaboli);
            }
            
            mostraModulo('quiz');
        });

    </script>
</body>
</html>
