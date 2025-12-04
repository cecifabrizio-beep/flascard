<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Set di Studio Giapponese SRS (Random Puro)</title>
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
            transition: transform 0.2s;
        }
        .card-ui-small {
            background-color: #fff; padding: 15px; border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
        }

        /* --- Navigazione Moduli --- */
        #main-nav { display: flex; gap: 10px; width: 100%; max-width: 450px; margin-bottom: 20px; }
        .nav-btn {
            flex: 1; padding: 10px 5px; font-size: 0.85rem; font-weight: 600;
            border: none; border-radius: 8px; cursor: pointer; transition: all 0.2s;
            background-color: #e5e5ea; color: #007aff;
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
            grid-template-columns: repeat(5, 1fr); /* Sempre 5 colonne per ordine visivo */
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
        .table-container { overflow-x: auto; }
        .kana-table { width: 100%; border-collapse: collapse; margin-top: 15px; font-size: 1.1rem; text-align: center; }
        .kana-table td, .kana-table th { border: 1px solid #ddd; padding: 10px 8px; }
        .kana-table th { background-color: #f4f4f4; font-weight: 600; }
        .kana-table .kana { font-size: 1.4rem; font-weight: bold; color: #333; }
        .kana-table .romaji { font-size: 0.9rem; color: #555; }
        .btn-start-kana { background-color: #34c759; margin-bottom: 20px; font-size: 1.1rem; box-shadow: 0 4px 10px rgba(52, 199, 89, 0.3); }
        
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

                <h3>Gojūon (Suoni base)</h3>
                <div class="table-container">
                    <table class="kana-table">
                        <tr><th></th><th>A</th><th>I</th><th>U</th><th>E</th><th>O</th></tr>
                        <tr><td><b>-</b></td><td class="kana">あ</td><td class="kana">い</td><td class="kana">う</td><td class="kana">え</td><td class="kana">お</td></tr>
                        <tr><td><b>K</b></td><td class="kana">か</td><td class="kana">き</td><td class="kana">く</td><td class="kana">け</td><td class="kana">こ</td></tr>
                        <tr><td><b>S</b></td><td class="kana">さ</td><td class="kana">し</td><td class="kana">す</td><td class="kana">せ</td><td class="kana">そ</td></tr>
                        <tr><td><b>T</b></td><td class="kana">た</td><td class="kana">ち</td><td class="kana">つ</td><td class="kana">て</td><td class="kana">と</td></tr>
                        <tr><td><b>N</b></td><td class="kana">な</td><td class="kana">に</td><td class="kana">ぬ</td><td class="kana">ね</td><td class="kana">の</td></tr>
                        <tr><td><b>H</b></td><td class="kana">は</td><td class="kana">ひ</td><td class="kana">ふ</td><td class="kana">へ</td><td class="kana">ほ</td></tr>
                        <tr><td><b>M</b></td><td class="kana">ま</td><td class="kana">み</td><td class="kana">む</td><td class="kana">め</td><td class="kana">も</td></tr>
                        <tr><td><b>Y</b></td><td class="kana">や</td><td></td><td class="kana">ゆ</td><td></td><td class="kana">よ</td></tr>
                        <tr><td><b>R</b></td><td class="kana">ら</td><td class="kana">り</td><td class="kana">る</td><td class="kana">れ</td><td class="kana">ろ</td></tr>
                        <tr><td><b>W</b></td><td class="kana">わ</td><td></td><td></td><td></td><td class="kana">を</td></tr>
                        <tr><td><b>N</b></td><td class="kana">ん</td><td></td><td></td><td></td><td></td></tr>
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

                <div class="table-container">
                    <table class="kana-table">
                         <tr><th></th><th>A</th><th>I</th><th>U</th><th>E</th><th>O</th></tr>
                        <tr><td><b>-</b></td><td class="kana">ア</td><td class="kana">イ</td><td class="kana">ウ</td><td class="kana">エ</td><td class="kana">オ</td></tr>
                        <tr><td><b>K</b></td><td class="kana">カ</td><td class="kana">キ</td><td class="kana">ク</td><td class="kana">ケ</td><td class="kana">コ</td></tr>
                        <tr><td><b>S</b></td><td class="kana">サ</td><td class="kana">シ</td><td class="kana">ス</td><td class="kana">セ</td><td class="kana">ソ</td></tr>
                        <tr><td><b>T</b></td><td class="kana">タ</td><td class="kana">チ</td><td class="kana">ツ</td><td class="kana">テ</td><td class="kana">ト</td></tr>
                        <tr><td><b>N</b></td><td class="kana">ナ</td><td class="kana">ニ</td><td class="kana">ヌ</td><td class="kana">ネ</td><td class="kana">ノ</td></tr>
                        <tr><td><b>H</b></td><td class="kana">ハ</td><td class="kana">ヒ</td><td class="kana">フ</td><td class="kana">ヘ</td><td class="kana">ホ</td></tr>
                        <tr><td><b>M</b></td><td class="kana">マ</td><td class="kana">ミ</td><td class="kana">ム</td><td class="kana">メ</td><td class="kana">モ</td></tr>
                        <tr><td><b>Y</b></td><td class="kana">ヤ</td><td></td><td class="kana">ユ</td><td></td><td class="kana">ヨ</td></tr>
                        <tr><td><b>R</b></td><td class="kana">ラ</td><td class="kana">リ</td><td class="kana">ル</td><td class="kana">レ</td><td class="kana">ロ</td></tr>
                        <tr><td><b>W</b></td><td class="kana">ワ</td><td></td><td></td><td></td><td class="kana">ヲ</td></tr>
                        <tr><td><b>N</b></td><td class="kana">ン</td><td></td><td></td><td></td><td></td></tr>
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
Buongiorno,Good morning,おはよう,Ohayoo,Intro - Saluti e Espressioni
Buongiorno (cortese),Good morning (polite),おはようございます,Ohayoo gozaimasu,Intro - Saluti e Espressioni
Buon pomeriggio,Good afternoon,こんにちは,Konnichiwa,Intro - Saluti e Espressioni
Buonasera,Good evening,こんばんは,Konbanwa,Intro - Saluti e Espressioni
Arrivederci,Good-bye,さようなら,Sayoonara,Intro - Saluti e Espressioni
Buonanotte,Good night,おやすみ（なさい）,Oyasumi (nasai),Intro - Saluti e Espressioni
Grazie,Thank you,ありがとう,Arigatoo,Intro - Saluti e Espressioni
Grazie (cortese),Thank you (polite),ありがとうございます,Arigatoo gozaimasu,Intro - Saluti e Espressioni
Mi scusi / Mi dispiace,Excuse me / I'm sorry,すみません,Sumimasen,Intro - Saluti e Espressioni
No / Di nulla,No / Not at all,いいえ,Iie,Intro - Saluti e Espressioni
Vado e torno,I'll go and come back,いってきます,Itte kimasu,Intro - Saluti e Espressioni
Vai e torna,Please go and come back,いってらっしゃい,Itterasshai,Intro - Saluti e Espressioni
Sono a casa,I'm home,ただいま,Tadaima,Intro - Saluti e Espressioni
Bentornato,Welcome home,おかえり（なさい）,Okaeri (nasai),Intro - Saluti e Espressioni
Grazie per il cibo (prima di mangiare),Thank you for the meal (before eating),いただきます,Itadakimasu,Intro - Saluti e Espressioni
Grazie per il cibo (dopo aver mangiato),Thank you for the meal (after eating),ごちそうさま（でした）,Gochisoosama (deshita),Intro - Saluti e Espressioni
Piacere di conoscerti,How do you do?,はじめまして,Hajimemashite,Intro - Saluti e Espressioni
Sono...,I am...,～です,... desu,Intro - Saluti e Espressioni
Piacere,Nice to meet you,よろしく おねがいします,Yoroshiku onegai shimasu,Intro - Saluti e Espressioni
Zero,Zero,ゼロ / れい,zero / ree,Intro - Numeri (0-100)
Uno,One,いち,ichi,Intro - Numeri (0-100)
Due,Two,に,ni,Intro - Numeri (0-100)
Tre,Three,さん,san,Intro - Numeri (0-100)
Quattro,Four,よん / し / (よ),yon / shi / (yo),Intro - Numeri (0-100)
Cinque,Five,ご,go,Intro - Numeri (0-100)
Sei,Six,ろく,roku,Intro - Numeri (0-100)
Sette,Seven,なな / しち,nana / shichi,Intro - Numeri (0-100)
Otto,Eight,はち,hachi,Intro - Numeri (0-100)
Nove,Nine,きゅう / く,kyuu / ku,Intro - Numeri (0-100)
Dieci,Ten,じゅう,juu,Intro - Numeri (0-100)
Undici,Eleven,じゅういち,juuichi,Intro - Numeri (0-100)
Dodici,Twelve,じゅうに,juuni,Intro - Numeri (0-100)
Tredici,Thirteen,じゅうさん,juusan,Intro - Numeri (0-100)
Quattordici,Fourteen,じゅうよん / しゅうよん,juuyon / shuuyon,Intro - Numeri (0-100)
Quindici,Fifteen,じゅうご,juugo,Intro - Numeri (0-100)
Sedici,Sixteen,じゅうろく,juuroku,Intro - Numeri (0-100)
Diciassette,Seventeen,じゅうなな / じゅうしち,juunana / juushichi,Intro - Numeri (0-100)
Diciotto,Eighteen,じゅうはち,juuhachi,Intro - Numeri (0-100)
Diciannove,Nineteen,じゅうきゅう / じゅうく,juukyuu / juuku,Intro - Numeri (0-100)
Venti,Twenty,にじゅう,nijuu,Intro - Numeri (0-100)
Trenta,Thirty,さんじゅう,sanjuu,Intro - Numeri (0-100)
Quaranta,Forty,よんじゅう,yonjuu,Intro - Numeri (0-100)
Cinquanta,Fifty,ごじゅう,gojuu,Intro - Numeri (0-100)
Sessanta,Sixty,ろくじゅう,rokujuu,Intro - Numeri (0-100)
Settanta,Seventy,ななじゅう,nanajuu,Intro - Numeri (0-100)
Ottanta,Eighty,はちじゅう,hachijuu,Intro - Numeri (0-100)
Novanta,Ninety,きゅうじゅう,kyuujuu,Intro - Numeri (0-100)
Cento,One hundred,ひゃく,hyaku,Intro - Numeri (0-100)
Università,college; university,だいがく,daigaku,L1 - Scuola Persone e Lingua
Scuola superiore,high school,こうこう,kookoo,L1 - Scuola Persone e Lingua
Studente,student,がくせい,gakusee,L1 - Scuola Persone e Lingua
Studente universitario,college student,だいがくせい,daigakusee,L1 - Scuola Persone e Lingua
Studente internazionale,international student,りゅうがくせい,ryuugakusee,L1 - Scuola Persone e Lingua
Insegnante / Professore,teacher; Professor...,せんせい,sensee,L1 - Scuola Persone e Lingua
Studente del ... anno,...year student,～ねんせい,... nensee,L1 - Scuola Persone e Lingua
Studente del primo anno,first-year student,いちねんせい,ichinensee,L1 - Scuola Persone e Lingua
Specializzazione,major,せんこう,senkoo,L1 - Scuola Persone e Lingua
Io,I,わたし,watashi,L1 - Scuola Persone e Lingua
Amico,friend,ともだち,tomodachi,L1 - Scuola Persone e Lingua
Sig./Sig.ra,Mr./Ms.,～さん,... san,L1 - Scuola Persone e Lingua
Persona ... (nazionalità),... people,～じん,... jin,L1 - Scuola Persone e Lingua
Giapponese (persona),Japanese people,にほんじん,nihonjin,L1 - Scuola Persone e Lingua
Adesso,now,いま,ima,L1 - Scuola Persone e Lingua
Mattina (A.M.),A.M.,ごぜん,gozen,L1 - Scuola Persone e Lingua
Pomeriggio (P.M.),P.M.,ごご,gogo,L1 - Scuola Persone e Lingua
Ore ...,...o'clock,～じ,... ji,L1 - Scuola Persone e Lingua
L'una (orario),one o'clock,いちじ,ichiji,L1 - Scuola Persone e Lingua
Mezza,half,はん,han,L1 - Scuola Persone e Lingua
Due e mezza,half past two,にじはん,niji han,L1 - Scuola Persone e Lingua
Giappone,Japan,にほん,Nihon,L1 - Scuola Persone e Lingua
USA,U.S.A.,アメリカ,Amerika,L1 - Scuola Persone e Lingua
Lingua ...,... language,～ご,... go,L1 - Scuola Persone e Lingua
Lingua giapponese,Japanese language,にほんご,nihongo,L1 - Scuola Persone e Lingua
... anni,... years old,～さい,... sai,L1 - Scuola Persone e Lingua
Telefono,telephone,でんわ,denwa,L1 - Scuola Persone e Lingua
Numero ...,number...,～ばん,... ban,L1 - Scuola Persone e Lingua
L'una,One o'clock,いちじ,ichiji,Intro - Orario (Ore e Minuti)
Le due,Two o'clock,にじ,niji,Intro - Orario (Ore e Minuti)
Le tre,Three o'clock,さんじ,sanji,Intro - Orario (Ore e Minuti)
Le quattro,Four o'clock,よじ,yoji,Intro - Orario (Ore e Minuti)
Le cinque,Five o'clock,ごじ,goji,Intro - Orario (Ore e Minuti)
Le sei,Six o'clock,ろくじ,rokuji,Intro - Orario (Ore e Minuti)
Le sette,Seven o'clock,しちじ,shichiji,Intro - Orario (Ore e Minuti)
Le otto,Eight o'clock,はちじ,hachiji,Intro - Orario (Ore e Minuti)
Le nove,Nine o'clock,くじ,kuji,Intro - Orario (Ore e Minuti)
Le dieci,Ten o'clock,じゅうじ,juuji,Intro - Orario (Ore e Minuti)
Le undici,Eleven o'clock,じゅういちじ,juuichiji,Intro - Orario (Ore e Minuti)
Le dodici,Twelve o'clock,じゅうにじ,juuniji,Intro - Orario (Ore e Minuti)
Un minuto,One minute,いっぷん,ippun,Intro - Orario (Ore e Minuti)
Due minuti,Two minutes,にふん,nifun,Intro - Orario (Ore e Minuti)
Tre minuti,Three minutes,さんぷん,sanpun,Intro - Orario (Ore e Minuti)
Quattro minuti,Four minutes,よんぷん,yonpun,Intro - Orario (Ore e Minuti)
Cinque minuti,Five minutes,ごふん,gofun,Intro - Orario (Ore e Minuti)
Sei minuti,Six minutes,ろっぷん,roppun,Intro - Orario (Ore e Minuti)
Sette minuti,Seven minutes,ななふん,nanafun,Intro - Orario (Ore e Minuti)
Otto minuti,Eight minutes,はっぷん / はちふん,happun / hachifun,Intro - Orario (Ore e Minuti)
Nove minuti,Nine minutes,きゅうふん,kyuufun,Intro - Orario (Ore e Minuti)
Dieci minuti,Ten minutes,じゅっぷん,juppun,Intro - Orario (Ore e Minuti)
Undici minuti,Eleven minutes,じゅういっぷん,juuippun,Intro - Orario (Ore e Minuti)
Dodici minuti,Twelve minutes,じゅうにふん,juunifun,Intro - Orario (Ore e Minuti)
Tredici minuti,Thirteen minutes,じゅうさんぷん,juusanpun,Intro - Orario (Ore e Minuti)
Quattordici minuti,Fourteen minutes,じゅうよんぷん,juuyonpun,Intro - Orario (Ore e Minuti)
Quindici minuti,Fifteen minutes,じゅうごふん,juugofun,Intro - Orario (Ore e Minuti)
Sedici minuti,Sixteen minutes,じゅうろっぷん,juuroppun,Intro - Orario (Ore e Minuti)
Diciassette minuti,Seventeen minutes,じゅうななふん,juunanafun,Intro - Orario (Ore e Minuti)
Diciotto minuti,Eighteen minutes,じゅうはっぷん / じゅうはちふん,juuhappun / juuhachifun,Intro - Orario (Ore e Minuti)
Diciannove minuti,Nineteen minutes,じゅうきゅうふん,juukyuufun,Intro - Orario (Ore e Minuti)
Venti minuti,Twenty minutes,にじゅっぷん,nijuppun,Intro - Orario (Ore e Minuti)
Trenta minuti,Thirty minutes,さんじゅっぷん,sanjuppun,Intro - Orario (Ore e Minuti)
Numero,number,ばんごう,bangoo,L1 - Altro e Espressioni
Nome,name,なまえ,namae,L1 - Altro e Espressioni
Cosa / Che,what,なん／なに,nan/nani,L1 - Altro e Espressioni
Ehm...,um...,あのう,anoo,L1 - Altro e Espressioni
Sì,yes,はい,hai,L1 - Altro e Espressioni
È così / Giusto,That's right,そうです,soo desu,L1 - Altro e Espressioni
Capisco / È così?,I see.; Is that so?,そうですか,soo desu ka,L1 - Altro e Espressioni
Gran Bretagna,Britain,イギリス,Igirisu,L1 - Altro e Espressioni
Australia,Australia,オーストラリア,Oosutoraria,L1 - Altro e Espressioni
Corea,Korea,かんこく,Kankoku,L1 - Altro e Espressioni
Canada,Canada,カナダ,Kanada,L1 - Altro e Espressioni
Cina,China,ちゅうごく,Chuugoku,L1 - Altro e Espressioni
India,India,インド,Indo,L1 - Altro e Espressioni
Egitto,Egypt,エジプト,Ejiputo,L1 - Altro e Espressioni
Filippine,Philippines,フィリピン,Firipin,L1 - Altro e Espressioni
Studi asiatici,Asian studies,アジアけんきゅう,ajia kenkyuu,L1 - Altro e Espressioni
Economia,economics,けいざい,keezai,L1 - Altro e Espressioni
Ingegneria,engineering,こうがく,koogaku,L1 - Altro e Espressioni
Relazioni internazionali,international relations,こくさいかんけい,kokusaikankee,L1 - Altro e Espressioni
Computer,computer,コンピューター,konpyuutaa,L1 - Altro e Espressioni
Politica,politics,せいじ,seeji,L1 - Altro e Espressioni
Biologia,biology,せいぶつがく,seebutsugaku,L1 - Altro e Espressioni
Affari / Business,business,ビジネス,bijinesu,L1 - Altro e Espressioni
Letteratura,literature,ぶんがく,bungaku,L1 - Altro e Espressioni
Storia,history,れきし,rekishi,L1 - Altro e Espressioni
Dottore,doctor,いしゃ,isha,L1 - Altro e Espressioni
Impiegato,office worker,かいしゃいん,kaishain,L1 - Altro e Espressioni
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
Quaderno,notebook,ノート,nooto,L2 - Cose Luoghi e Cibo`;

        // --- DATASETS KANA ---
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
            {k:'わ',r:'wa'}, {k:'を',r:'wo'}, {k:'ん',r:'n'}
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
            {k:'ワ',r:'wa'}, {k:'ヲ',r:'wo'}, {k:'ン',r:'n'}
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
            'Riga W/N': [43, 46]
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
