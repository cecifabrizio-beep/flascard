<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Set di Studio Giapponese SRS v4.0 (Ascolto)</title>
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
            width: 100%; box-sizing: border-box; 
        }

        /* --- Navigazione --- */
        #main-nav { display: flex; gap: 5px; width: 100%; max-width: 600px; margin-bottom: 15px; flex-wrap: wrap; justify-content: center; }
        .nav-btn {
            padding: 10px 5px; font-size: 0.9rem; font-weight: 600;
            border: none; border-radius: 8px; cursor: pointer; transition: all 0.2s;
            background-color: #e5e5ea; color: #007aff; flex: 1; min-width: 80px; text-align: center;
        }
        .nav-btn.active { background-color: #007aff; color: white; box-shadow: 0 4px 10px rgba(0,122,255,0.3); }
        .modulo-content { display: none; width: 100%; }

        /* --- Quiz & UI --- */
        #punteggio-container { text-align: center; font-weight: 600; color: #555; margin-bottom: 15px; }
        .punteggio-info { color: #007aff; font-weight: bold; }
        
        .filtro-container { margin-bottom: 15px; }
        select { width: 100%; padding: 12px; border-radius: 8px; border: 1px solid #007aff; font-size: 1rem; background: #f0f8ff; font-weight: 600; }

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

        /* --- SEZIONE ASCOLTO --- */
        .frase-entry { 
            display: flex; 
            align-items: flex-start; 
            padding: 15px; 
            background: #fdfdfd; 
            border: 1px solid #eee; 
            border-radius: 10px; 
            margin-bottom: 10px; 
            gap: 15px;
        }
        .play-btn {
            background-color: #34c759; 
            color: white; 
            border: none; 
            border-radius: 50%; 
            width: 50px; 
            height: 50px; 
            font-size: 1.5rem; 
            cursor: pointer; 
            flex-shrink: 0;
            display: flex; justify-content: center; align-items: center;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        .play-btn:active { transform: scale(0.95); }
        .frase-text { flex-grow: 1; }
        .frase-jpn { font-size: 1.3rem; color: #333; font-weight: bold; margin-bottom: 4px; display: block; }
        .frase-romaji { font-size: 0.9rem; color: #777; margin-bottom: 4px; display: block; }
        .frase-ita { font-size: 1rem; color: #007aff; font-weight: 500; display: block; }
        .frase-tag { font-size: 0.7rem; background: #ddd; color: #444; padding: 2px 6px; border-radius: 4px; float: right; }
        .spoiler-active .frase-ita { background: #e0e0e0; color: transparent; border-radius: 4px; cursor: pointer; }
        .spoiler-active .frase-ita:hover { color: #007aff; }

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
        <button class="nav-btn" data-modulo="ascolto">🎧 Ascolto</button>
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
                    <div id="nessuna-carta" style="display: none; text-align: center; margin-top:20px; color:#666;"><p>Nessuna carta trovata in questa categoria.</p></div>
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
                    <label id="import-label" for="import-csv-file" style="cursor:pointer; background:#eee; padding:10px; text-align:center; border-radius:6px; margin-bottom:15px; display:block;">📂 Importa CSV Locale</label>
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
                    <button id="svuota-tutto-btn" class="btn" style="background-color: #d92c23;">CANCELLA E RIPRISTINA TUTTO</button>
                </div>
            </div>
        </div> 

        <div id="modulo-ascolto" class="modulo-content">
            <div class="card-ui">
                <h2>Ascolto Frasi</h2>
                <div class="filtro-container">
                    <label style="margin-bottom:5px;">Filtra per Categoria:</label>
                    <select id="filtro-frasi" onchange="mostraListaFrasi()">
                        <option value="TUTTI">🎧 TUTTE LE FRASI</option>
                    </select>
                </div>
                <div style="margin-bottom: 15px; text-align: center;">
                    <label class="checkbox-label" style="justify-content: center;">
                        <input type="checkbox" id="spoiler-mode" onchange="mostraListaFrasi()"> Nascondi traduzione Italiana
                    </label>
                </div>
                <div id="lista-frasi-container"></div>
                
                <div class="sezione-gestione">
                    <h3>Aggiungi Nuova Frase</h3>
                    <form id="form-aggiungi-frase">
                        <label>Italiano: <input type="text" id="f-ita" required></label>
                        <label>Giapponese: <input type="text" id="f-jpn" required></label>
                        <label>Romaji: <input type="text" id="f-romaji" required></label>
                        <label>Categoria (Tag): <input type="text" id="f-tag" placeholder="Es. Presentazioni"></label>
                        <button type="submit" class="btn">Salva Frase</button>
                    </form>
                </div>
                 <div class="sezione-gestione">
                    <button id="reset-frasi-btn" class="btn" style="background-color: #34c759;">🔄 Ripristina Frasi Default</button>
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
                <div class="table-wrapper"><table class="kana-table" id="h-table-gojuon"></table></div>
                <h4>Suoni Impuri</h4>
                <div class="table-wrapper"><table class="kana-table" id="h-table-dakuten"></table></div>
                <h4>Suoni Contratti</h4>
                <div class="table-wrapper"><table class="kana-table" id="h-table-yoon"></table></div>
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
                <div class="table-wrapper"><table class="kana-table" id="k-table-gojuon"></table></div>
                <h4>Suoni Impuri</h4>
                <div class="table-wrapper"><table class="kana-table" id="k-table-dakuten"></table></div>
                <h4>Suoni Contratti</h4>
                <div class="table-wrapper"><table class="kana-table" id="k-table-yoon"></table></div>
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
        // --- DATI VOCABOLI ---
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
Costoso / Alto,expensive; high,たかい,takai,L2 - Luoghi e Soldi
Film,movie,えいが,eiga,L3 - Svago e Cibo
Musica,music,おんがく,ongaku,L3 - Svago e Cibo
Rivista,magazine,ざっし,zasshi,L3 - Svago e Cibo
Sport,sports,スポーツ,supootsu,L3 - Svago e Cibo
Appuntamento,date,デート,deeto,L3 - Svago e Cibo
Tennis,tennis,テニス,tenisu,L3 - Svago e Cibo
TV,TV,テレビ,terebi,L3 - Svago e Cibo
Gelato,ice cream,アイスクリーム,aisukuriimu,L3 - Svago e Cibo
Hamburger,hamburger,ハンバーガー,hanbaagaa,L3 - Svago e Cibo
Sake / Alcolici,sake; alcoholic drink,おさけ,osake,L3 - Svago e Cibo
Tè verde,green tea,おちゃ,ocha,L3 - Svago e Cibo
Caffè,coffee,コーヒー,koohii,L3 - Svago e Cibo
Acqua,water,みず,mizu,L3 - Svago e Cibo
Colazione,breakfast,あさごはん,asagohan,L3 - Svago e Cibo
Pranzo,lunch,ひるごはん,hirugohan,L3 - Svago e Cibo
Cena,dinner,ばんごはん,bangohan,L3 - Svago e Cibo
Casa,home; house,いえ,ie,L3 - Tempo e Luoghi
Casa mia / Casa,home; house; my place,うち,uchi,L3 - Tempo e Luoghi
Scuola,school,がっこう,gakkou,L3 - Tempo e Luoghi
Bar / Caffè,cafe,カフェ,kafe,L3 - Tempo e Luoghi
Domani,tomorrow,あした,ashita,L3 - Tempo e Luoghi
Oggi,today,きょう,kyou,L3 - Tempo e Luoghi
Mattina,morning,あさ,asa,L3 - Tempo e Luoghi
Stasera,tonight,こんばん,konban,L3 - Tempo e Luoghi
Ogni giorno,every day,まいにち,mainichi,L3 - Tempo e Luoghi
Ogni sera,every night,まいばん,maiban,L3 - Tempo e Luoghi
Fine settimana,weekend,しゅうまつ,shuumatsu,L3 - Tempo e Luoghi
Sabato,Saturday,どようび,doyoubi,L3 - Tempo e Luoghi
Domenica,Sunday,にちようび,nichiyoubi,L3 - Tempo e Luoghi
Quando,when,いつ,itsu,L3 - Tempo e Luoghi
Verso... (orario),at about...,～ごろ,...goro,L3 - Tempo e Luoghi
Andare,to go,いく,iku,L3 - Verbi
Tornare,to go back; to return,かえる,kaeru,L3 - Verbi
Ascoltare,to listen; to hear,きく,kiku,L3 - Verbi
Bere,to drink,のむ,nomu,L3 - Verbi
Parlare,to speak; to talk,はなす,hanasu,L3 - Verbi
Leggere,to read,よむ,yomu,L3 - Verbi
Alzarsi,to get up,おきる,okiru,L3 - Verbi
Mangiare,to eat,たべる,taberu,L3 - Verbi
Dormire,to sleep,ねる,neru,L3 - Verbi
Vedere / Guardare,to see; to look at; to watch,みる,miru,L3 - Verbi
Venire,to come,くる,kuru,L3 - Verbi
Fare,to do,する,suru,L3 - Verbi
Studiare,to study,べんきょうする,benkyousuru,L3 - Verbi
Buono / Bene,good,いい,ii,L3 - Aggettivi e Espressioni
Presto,early,はやい,hayai,L3 - Aggettivi e Espressioni
Non molto,not much,あまり,amari,L3 - Aggettivi e Espressioni
Per niente,not at all,ぜんぜん,zenzen,L3 - Aggettivi e Espressioni
Di solito,usually,たいてい,taitei,L3 - Aggettivi e Espressioni
Un po',a little,ちょっと,chotto,L3 - Aggettivi e Espressioni
A volte,sometimes,ときどき,tokidoki,L3 - Aggettivi e Espressioni
Spesso,often,よく,yoku,L3 - Aggettivi e Espressioni
È vero / Fammi vedere,That's right.; Let me see.,そうですね,sou desu ne,L3 - Aggettivi e Espressioni
Ma,but,でも,demo,L3 - Aggettivi e Espressioni
Che ne dici di...?,How about...?,どうですか,dou desu ka,L3 - Aggettivi e Espressioni
Sì (informale),yes (informal),ええ,ee,L3 - Aggettivi e Espressioni`;

        // --- DATI FRASI (CSV EMBEDDED) ---
        const DATI_FRASI_CSV = `
Il mio nome è Mario.,Watashi no namae wa Mario desu.,私の名前はマリオです。,Presentazioni
Piacere di conoscerti.,Yoroshiku onegai shimasu.,よろしくお願いします。,Presentazioni
Che ore sono?,Ima nanji desu ka?,今何時ですか？,Tempo
Quanto costa questo?,Kore wa ikura desu ka?,これはいくらですか？,Shopping
Dov'è il bagno?,Toire wa doko desu ka?,トイレはどこですか？,Luoghi
Mi piace il sushi.,Watashi wa sushi ga suki desu.,私は寿司が好きです。,Gusti
Non capisco.,Wakarimasen.,わかりません。,Espressioni
Parli inglese?,Eego o hanasemasu ka?,英語を話せますか？,Lingue
Prendo questo.,Kore o kudasai.,これをください。,Ristorante/Negozio
Buon appetito.,Itadakimasu.,いただきます。,Cibo
Grazie per il pasto.,Gochisoosama deshita.,ごちそうさまでした。,Cibo
Dove vai?,Doko e ikimasu ka?,どこへ行きますか？,Verbi
Vado a scuola.,Gakkou e ikimasu.,学校へ行きます。,Verbi
Cosa fai nel weekend?,Shuumatsu wa nani o shimasu ka?,週末は何をしますか？,Tempo Libero
Guardo un film.,Eiga o mimasu.,映画を見ます。,Tempo Libero
`;

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
        let mazzoFrasi = []; // NUOVO PER ASCOLTO
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
        const KEY_MAZZO_FRASI = 'mioMazzoFrasi'; // NUOVA KEY
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
            caricaFrasi(); // Carica le frasi
            setupEventListeners();
            costruisciTabelleKana();
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
            document.getElementById('reset-frasi-btn').addEventListener('click', ripristinaDatiFrasi); // Nuovo listener
            document.getElementById('form-aggiungi').addEventListener('submit', gestisciSalvataggioForm);
            document.getElementById('form-aggiungi-frase').addEventListener('submit', gestisciSalvataggioFrase); // Nuovo listener
            document.getElementById('import-csv-file').addEventListener('change', gestisciImportaCSV);
            document.getElementById('update-url-btn').addEventListener('click', gestisciAggiornaDaUrl);
            document.getElementById('svuota-tutto-btn').addEventListener('click', svuotaMazziTotali);
            document.getElementById('svuota-lista-btn').addEventListener('click', svuotaMazziTotali);
            document.getElementById('copia-vocaboli-btn').addEventListener('click', copiaVocaboli);
        }

        // --- GESTIONE FRASI (NUOVA) ---
        function caricaFrasi() {
            const df = localStorage.getItem(KEY_MAZZO_FRASI);
            mazzoFrasi = df ? JSON.parse(df) : [];
            
            if (mazzoFrasi.length === 0) {
                // Carica default se vuoto
                importaFrasiDaStringa(DATI_FRASI_CSV);
                return;
            }
            aggiornaFiltroFrasi();
            mostraListaFrasi();
        }

        function ripristinaDatiFrasi() {
             if(confirm("Vuoi ripristinare le frasi di default?")) {
                 importaFrasiDaStringa(DATI_FRASI_CSV);
             }
        }

        function importaFrasiDaStringa(csvText) {
             const lines = csvText.split('\n');
             // Se ricarichiamo default, svuotiamo prima per evitare duplicati
             mazzoFrasi = []; 
             lines.forEach(line => {
                if(!line.trim()) return;
                const c = parseCSVLine(line);
                if(c.length < 3) return; // Minimo Ita, Jpn, Romaji
                mazzoFrasi.push({
                    ita:c[0], jpn:c[1], romaji:c[2], tag:c[3]||"Generale"
                });
             });
             salvaMazzoFrasi();
             aggiornaFiltroFrasi();
             mostraListaFrasi();
        }

        function salvaMazzoFrasi() {
            localStorage.setItem(KEY_MAZZO_FRASI, JSON.stringify(mazzoFrasi));
        }

        function aggiornaFiltroFrasi() {
            const filtro = document.getElementById('filtro-frasi');
            const allTags = new Set();
            mazzoFrasi.forEach(f => { if(f.tag) allTags.add(f.tag); });
            filtro.innerHTML = '<option value="TUTTI">🎧 TUTTE LE FRASI</option>';
            Array.from(allTags).sort().forEach(tag => {
                const opt = document.createElement('option');
                opt.value = tag; opt.textContent = `📂 ${tag}`;
                filtro.appendChild(opt);
            });
        }

        function mostraListaFrasi() {
            const container = document.getElementById('lista-frasi-container');
            const filtro = document.getElementById('filtro-frasi').value;
            const spoilerMode = document.getElementById('spoiler-mode').checked;
            
            container.innerHTML = '';
            
            let frasiDaMostrare = mazzoFrasi;
            if (filtro !== 'TUTTI') {
                frasiDaMostrare = mazzoFrasi.filter(f => f.tag === filtro);
            }

            if (frasiDaMostrare.length === 0) {
                container.innerHTML = '<p style="text-align:center; color:#666;">Nessuna frase in questa categoria.</p>';
                return;
            }

            frasiDaMostrare.forEach((f, index) => {
                const div = document.createElement('div');
                div.className = 'frase-entry';
                if(spoilerMode) div.classList.add('spoiler-active');

                // Clean JPN for audio (remove punctuation mostly handled by browser but safe to check)
                const textToSpeak = f.jpn; 

                div.innerHTML = `
                    <button class="play-btn" onclick="parla('${textToSpeak.replace(/'/g, "\\'")}')">🔊</button>
                    <div class="frase-text">
                        <span class="frase-jpn">${f.jpn}</span>
                        <span class="frase-romaji">${f.romaji}</span>
                        <span class="frase-ita" onclick="this.parentElement.parentElement.classList.remove('spoiler-active')">${f.ita}</span>
                        <span class="frase-tag">${f.tag}</span>
                    </div>
                    <button class="delete-vocab-btn" onclick="eliminaFrase('${f.ita.replace(/'/g, "\\'")}')">X</button>
                `;
                container.appendChild(div);
            });
        }

        function eliminaFrase(keyIta) {
            if(confirm("Eliminare questa frase?")) {
                mazzoFrasi = mazzoFrasi.filter(f => f.ita !== keyIta);
                salvaMazzoFrasi();
                mostraListaFrasi();
                aggiornaFiltroFrasi(); // Aggiorna i tag se una categoria si svuota
            }
        }
        
        function gestisciSalvataggioFrase(e) {
            e.preventDefault();
            const n = {
                ita: document.getElementById('f-ita').value,
                jpn: document.getElementById('f-jpn').value,
                romaji: document.getElementById('f-romaji').value,
                tag: document.getElementById('f-tag').value || "Generale"
            };
            mazzoFrasi.push(n);
            salvaMazzoFrasi();
            // Resetta form e aggiorna UI
            e.target.reset();
            aggiornaFiltroFrasi();
            mostraListaFrasi();
            alert("Frase aggiunta!");
        }

        // --- FINE NUOVA SEZIONE FRASI ---

        function ripristinaDatiVocaboli() {
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
        
        // FUNZIONE COSTRUZIONE TABELLE
        function costruisciTabelleKana() {
            // Helper per riempire le tabelle
            const fillTable = (tableId, dataset, rowIndices) => {
                const table = document.getElementById(tableId);
                const colHeaders = tableId.includes('yoon') ? ['','YA','YU','YO'] : ['','A','I','U','E','O'];
                
                // Header Row
                let html = '<tr>';
                colHeaders.forEach(h => html += `<th>${h}</th>`);
                html += '</tr>';
                
                // Data Rows
                rowIndices.forEach(idx => {
                    // Logic to reconstruct rows based on dataset structure
                    // Questo è semplificato, in produzione userei una struttura dati migliore per le righe
                    // Per ora, lascio vuoto o statico se complesso da generare dinamicamente senza refactoring massiccio
                });
                
                // NOTA: Per brevità e per non rompere il layout, ho hardcodato le tabelle nell'HTML sopra.
                // Questa funzione servirebbe per generarle dinamicamente, ma l'HTML statico è più sicuro per il layout fisso.
            };
        }

        function caricaMazzi() {
            const dP = localStorage.getItem(KEY_MAZZO_PRINCIPALE);
            mazzoPrincipale = dP ? JSON.parse(dP) : [];
            const dE = localStorage.getItem(KEY_MAZZO_ERRORI);
            mazzoErroriPrioritari = dE ? JSON.parse(dE) : [];
            mazzoPrincipale.forEach(c => { if(typeof c.level==='undefined') c.level=0; c.type = 'vocab'; });

            // Se vuoto, carica default
            if (mazzoPrincipale.length === 0) { importaDatiDaStringa(DATI_INIZIALI_CSV); return; }
            
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
            if (id === 'ascolto') mostraListaFrasi(); // Aggiorna lista ascolto
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

        function svuotaMazziTotali(){ 
            if(confirm("ATTENZIONE: Questo cancellerà i tuoi progressi e ricaricherà i dati predefiniti. Continuare?")) { 
                localStorage.clear(); 
                location.reload(); 
            } 
        }
        
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
