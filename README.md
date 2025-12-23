<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Giapponese SRS v5.0 (Stable)</title>
    <style>
        /* --- Stile Base --- */
        body { font-family: -apple-system, sans-serif; background-color: #f2f2f7; color: #333; margin: 0; padding: 10px; display: flex; flex-direction: column; align-items: center; min-height: 100vh; }
        h1 { color: #1c1c1e; text-align: center; margin: 10px 0 20px; font-size: 1.5rem; }
        h2 { color: #007aff; border-bottom: 1px solid #ddd; padding-bottom: 10px; margin-top: 0; }
        h4 { color: #555; margin: 20px 0 10px; border-left: 4px solid #34c759; padding-left: 10px; }

        .container { width: 100%; max-width: 600px; margin-bottom: 40px; }
        .card { background: #fff; padding: 20px; border-radius: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); margin-bottom: 20px; }

        /* --- Navigazione --- */
        #main-nav { display: flex; gap: 8px; overflow-x: auto; padding-bottom: 5px; margin-bottom: 15px; width: 100%; max-width: 600px; }
        .nav-btn { flex: 1; min-width: 80px; padding: 10px; border: none; border-radius: 8px; background: #e5e5ea; color: #007aff; font-weight: 600; cursor: pointer; white-space: nowrap; }
        .nav-btn.active { background: #007aff; color: white; }
        
        .modulo-content { display: none; } /* Nascosti di default */
        .modulo-content.active-module { display: block; } /* Classe per mostrare */

        /* --- Quiz Vocaboli --- */
        .quiz-area { text-align: center; }
        #prompt-principale { font-size: 2.2rem; margin: 15px 0; min-height: 50px; }
        #prompt-secondario { color: #666; font-style: italic; min-height: 20px; }
        .input-box { width: 100%; padding: 12px; font-size: 1.1rem; border: 2px solid #ddd; border-radius: 8px; text-align: center; box-sizing: border-box; margin: 15px 0; }
        .btn-row { display: flex; gap: 10px; }
        .btn { flex: 1; padding: 12px; border: none; border-radius: 8px; font-weight: bold; color: white; cursor: pointer; font-size: 1rem; }
        .btn-blue { background: #007aff; } .btn-green { background: #34c759; } .btn-red { background: #ff3b30; } .btn-orange { background: #ff9500; }
        .feedback { font-weight: bold; margin: 10px 0; min-height: 24px; }
        .corretto { color: #34c759; } .sbagliato { color: #ff3b30; }

        /* --- Sezione Ascolto --- */
        .frase-item { background: #f9f9f9; border: 1px solid #eee; border-radius: 8px; padding: 15px; margin-bottom: 10px; display: flex; align-items: center; gap: 15px; }
        .audio-btn { width: 45px; height: 45px; border-radius: 50%; background: #34c759; color: white; border: none; font-size: 1.2rem; cursor: pointer; flex-shrink: 0; display:flex; justify-content:center; align-items:center;}
        .frase-content { flex-grow: 1; }
        .f-jpn { font-weight: bold; font-size: 1.1rem; display: block; color: #333; }
        .f-rom { color: #888; font-size: 0.85rem; display: block; margin-bottom: 4px; }
        .f-ita { color: #007aff; font-weight: 500; cursor: pointer; }
        .spoiler .f-ita { background: #ccc; color: transparent; border-radius: 3px; }
        .spoiler .f-ita:hover { background: transparent; color: #007aff; }

        /* --- Tabelle Kana --- */
        .table-wrap { overflow-x: auto; }
        .k-table { width: 100%; border-collapse: collapse; min-width: 350px; text-align: center; }
        .k-table th { background: #f2f2f7; padding: 8px; font-size: 0.8rem; color: #666; }
        .k-table td { border: 1px solid #e5e5ea; padding: 10px 5px; vertical-align: middle; }
        .kc { font-size: 1.3rem; font-weight: bold; display: block; color: #333; }
        .kr { font-size: 0.75rem; color: #999; display: block; }
        
        /* --- Lista Vocaboli --- */
        .v-item { display: flex; justify-content: space-between; padding: 10px; border-bottom: 1px solid #eee; }
        .v-main { font-weight: bold; }
        .v-meta { text-align: right; }
        .v-jp { color: #007aff; font-weight: bold; display: block; }
        .v-ro { color: #aaa; font-size: 0.8rem; }

        /* Helper */
        .hidden { display: none !important; }
    </style>
</head>
<body>

    <h1>Set di Studio Giapponese</h1>

    <nav id="main-nav">
        <button class="nav-btn active" onclick="cambiaTab('quiz')">Quiz Vocaboli</button>
        <button class="nav-btn" onclick="cambiaTab('ascolto')">🎧 Ascolto</button>
        <button class="nav-btn" onclick="cambiaTab('hiragana')">Hiragana</button>
        <button class="nav-btn" onclick="cambiaTab('katakana')">Katakana</button>
        <button class="nav-btn" onclick="cambiaTab('lista')">Lista</button>
    </nav>

    <div class="container">

        <div id="mod-quiz" class="modulo-content active-module">
            <div class="card">
                <div style="text-align:center; margin-bottom:10px;">
                    <span id="score-display" style="color:#007aff; font-weight:bold;"></span>
                </div>
                
                <select id="filtro-cat" class="input-box" onchange="initQuizSession()" style="padding:8px; font-weight:bold;">
                    <option value="TUTTI">Tutte le Categorie</option>
                </select>

                <div class="quiz-area">
                    <div id="q-label" style="font-size:0.8rem; color:#888; font-weight:bold; text-transform:uppercase;"></div>
                    <div id="prompt-principale">Caricamento...</div>
                    <div id="prompt-secondario"></div>
                    
                    <input type="text" id="user-input" class="input-box" placeholder="Risposta..." autocomplete="off">
                    
                    <div id="feedback-msg" class="feedback"></div>
                    <div id="example-msg" style="font-style:italic; color:#555; margin-bottom:10px; min-height:20px;"></div>

                    <div class="btn-row">
                        <button id="btn-check" class="btn btn-blue" onclick="checkAnswer()">Controlla</button>
                        <button id="btn-next" class="btn btn-green hidden" onclick="nextCard()">Prossima</button>
                    </div>
                </div>
            </div>

            <div class="card">
                <h2>Gestione</h2>
                <div class="btn-row" style="margin-bottom:10px;">
                    <button class="btn btn-green" onclick="forceResetData()">🔄 Ripristina Dati Default</button>
                </div>
                <div class="btn-row">
                    <button class="btn btn-red" onclick="clearAllData()">🗑 CANCELLA TUTTO</button>
                </div>
            </div>
        </div>

        <div id="mod-ascolto" class="modulo-content">
            <div class="card">
                <h2>Ascolto Frasi</h2>
                <select id="filtro-frasi" class="input-box" onchange="renderFrasi()">
                    <option value="TUTTI">Tutte le Frasi</option>
                </select>
                <div style="text-align:center; margin-bottom:15px;">
                    <label><input type="checkbox" id="spoiler-check" onchange="renderFrasi()"> Nascondi Italiano</label>
                </div>
                <div id="frasi-list"></div>
            </div>
        </div>

        <div id="mod-hiragana" class="modulo-content">
            <div class="card">
                <h2>Hiragana</h2>
                <h4>Suoni Base (Gojūon)</h4>
                <div class="table-wrap">
                    <table class="k-table">
                        <tr><th>-</th><th>K</th><th>S</th><th>T</th><th>N</th><th>H</th><th>M</th><th>Y</th><th>R</th><th>W</th><th>N</th></tr>
                        <tr>
                            <td><span class="kc">あ</span><span class="kr">a</span></td>
                            <td><span class="kc">か</span><span class="kr">ka</span></td>
                            <td><span class="kc">さ</span><span class="kr">sa</span></td>
                            <td><span class="kc">た</span><span class="kr">ta</span></td>
                            <td><span class="kc">な</span><span class="kr">na</span></td>
                            <td><span class="kc">は</span><span class="kr">ha</span></td>
                            <td><span class="kc">ま</span><span class="kr">ma</span></td>
                            <td><span class="kc">や</span><span class="kr">ya</span></td>
                            <td><span class="kc">ら</span><span class="kr">ra</span></td>
                            <td><span class="kc">わ</span><span class="kr">wa</span></td>
                            <td><span class="kc">ん</span><span class="kr">n</span></td>
                        </tr>
                        <tr>
                            <td><span class="kc">い</span><span class="kr">i</span></td>
                            <td><span class="kc">き</span><span class="kr">ki</span></td>
                            <td><span class="kc">し</span><span class="kr">shi</span></td>
                            <td><span class="kc">ち</span><span class="kr">chi</span></td>
                            <td><span class="kc">に</span><span class="kr">ni</span></td>
                            <td><span class="kc">ひ</span><span class="kr">hi</span></td>
                            <td><span class="kc">み</span><span class="kr">mi</span></td>
                            <td></td>
                            <td><span class="kc">り</span><span class="kr">ri</span></td>
                            <td></td>
                            <td></td>
                        </tr>
                        <tr>
                            <td><span class="kc">う</span><span class="kr">u</span></td>
                            <td><span class="kc">く</span><span class="kr">ku</span></td>
                            <td><span class="kc">す</span><span class="kr">su</span></td>
                            <td><span class="kc">つ</span><span class="kr">tsu</span></td>
                            <td><span class="kc">ぬ</span><span class="kr">nu</span></td>
                            <td><span class="kc">ふ</span><span class="kr">fu</span></td>
                            <td><span class="kc">む</span><span class="kr">mu</span></td>
                            <td><span class="kc">ゆ</span><span class="kr">yu</span></td>
                            <td><span class="kc">る</span><span class="kr">ru</span></td>
                            <td></td>
                            <td></td>
                        </tr>
                        <tr>
                            <td><span class="kc">え</span><span class="kr">e</span></td>
                            <td><span class="kc">け</span><span class="kr">ke</span></td>
                            <td><span class="kc">せ</span><span class="kr">se</span></td>
                            <td><span class="kc">て</span><span class="kr">te</span></td>
                            <td><span class="kc">ね</span><span class="kr">ne</span></td>
                            <td><span class="kc">へ</span><span class="kr">he</span></td>
                            <td><span class="kc">め</span><span class="kr">me</span></td>
                            <td></td>
                            <td><span class="kc">れ</span><span class="kr">re</span></td>
                            <td></td>
                            <td></td>
                        </tr>
                        <tr>
                            <td><span class="kc">お</span><span class="kr">o</span></td>
                            <td><span class="kc">こ</span><span class="kr">ko</span></td>
                            <td><span class="kc">そ</span><span class="kr">so</span></td>
                            <td><span class="kc">と</span><span class="kr">to</span></td>
                            <td><span class="kc">の</span><span class="kr">no</span></td>
                            <td><span class="kc">ほ</span><span class="kr">ho</span></td>
                            <td><span class="kc">も</span><span class="kr">mo</span></td>
                            <td><span class="kc">よ</span><span class="kr">yo</span></td>
                            <td><span class="kc">ろ</span><span class="kr">ro</span></td>
                            <td><span class="kc">を</span><span class="kr">wo</span></td>
                            <td></td>
                        </tr>
                    </table>
                </div>
                <h4>Dakuten (G, Z, D, B, P)</h4>
                <div class="table-wrap">
                    <table class="k-table">
                         <tr><th>G</th><th>Z</th><th>D</th><th>B</th><th>P</th></tr>
                         <tr>
                             <td><span class="kc">が</span><span class="kr">ga</span></td>
                             <td><span class="kc">ざ</span><span class="kr">za</span></td>
                             <td><span class="kc">だ</span><span class="kr">da</span></td>
                             <td><span class="kc">ば</span><span class="kr">ba</span></td>
                             <td><span class="kc">ぱ</span><span class="kr">pa</span></td>
                         </tr>
                         <tr>
                             <td><span class="kc">ぎ</span><span class="kr">gi</span></td>
                             <td><span class="kc">じ</span><span class="kr">ji</span></td>
                             <td><span class="kc">ぢ</span><span class="kr">ji</span></td>
                             <td><span class="kc">び</span><span class="kr">bi</span></td>
                             <td><span class="kc">ぴ</span><span class="kr">pi</span></td>
                         </tr>
                         <tr>
                             <td><span class="kc">ぐ</span><span class="kr">gu</span></td>
                             <td><span class="kc">ず</span><span class="kr">zu</span></td>
                             <td><span class="kc">づ</span><span class="kr">zu</span></td>
                             <td><span class="kc">ぶ</span><span class="kr">bu</span></td>
                             <td><span class="kc">ぷ</span><span class="kr">pu</span></td>
                         </tr>
                         <tr>
                             <td><span class="kc">げ</span><span class="kr">ge</span></td>
                             <td><span class="kc">ぜ</span><span class="kr">ze</span></td>
                             <td><span class="kc">で</span><span class="kr">de</span></td>
                             <td><span class="kc">べ</span><span class="kr">be</span></td>
                             <td><span class="kc">ぺ</span><span class="kr">pe</span></td>
                         </tr>
                         <tr>
                             <td><span class="kc">ご</span><span class="kr">go</span></td>
                             <td><span class="kc">ぞ</span><span class="kr">zo</span></td>
                             <td><span class="kc">ど</span><span class="kr">do</span></td>
                             <td><span class="kc">ぼ</span><span class="kr">bo</span></td>
                             <td><span class="kc">ぽ</span><span class="kr">po</span></td>
                         </tr>
                    </table>
                </div>
            </div>
        </div>

        <div id="mod-katakana" class="modulo-content">
            <div class="card">
                <h2>Katakana</h2>
                <h4>Suoni Base (Gojūon)</h4>
                <div class="table-wrap">
                    <table class="k-table">
                        <tr><th>-</th><th>K</th><th>S</th><th>T</th><th>N</th><th>H</th><th>M</th><th>Y</th><th>R</th><th>W</th><th>N</th></tr>
                        <tr>
                            <td><span class="kc">ア</span><span class="kr">a</span></td>
                            <td><span class="kc">カ</span><span class="kr">ka</span></td>
                            <td><span class="kc">サ</span><span class="kr">sa</span></td>
                            <td><span class="kc">タ</span><span class="kr">ta</span></td>
                            <td><span class="kc">ナ</span><span class="kr">na</span></td>
                            <td><span class="kc">ハ</span><span class="kr">ha</span></td>
                            <td><span class="kc">マ</span><span class="kr">ma</span></td>
                            <td><span class="kc">ヤ</span><span class="kr">ya</span></td>
                            <td><span class="kc">ラ</span><span class="kr">ra</span></td>
                            <td><span class="kc">ワ</span><span class="kr">wa</span></td>
                            <td><span class="kc">ン</span><span class="kr">n</span></td>
                        </tr>
                        <tr>
                            <td><span class="kc">イ</span><span class="kr">i</span></td>
                            <td><span class="kc">キ</span><span class="kr">ki</span></td>
                            <td><span class="kc">シ</span><span class="kr">shi</span></td>
                            <td><span class="kc">チ</span><span class="kr">chi</span></td>
                            <td><span class="kc">ニ</span><span class="kr">ni</span></td>
                            <td><span class="kc">ヒ</span><span class="kr">hi</span></td>
                            <td><span class="kc">ミ</span><span class="kr">mi</span></td>
                            <td></td>
                            <td><span class="kc">リ</span><span class="kr">ri</span></td>
                            <td></td>
                            <td></td>
                        </tr>
                        <tr>
                            <td><span class="kc">ウ</span><span class="kr">u</span></td>
                            <td><span class="kc">ク</span><span class="kr">ku</span></td>
                            <td><span class="kc">ス</span><span class="kr">su</span></td>
                            <td><span class="kc">ツ</span><span class="kr">tsu</span></td>
                            <td><span class="kc">ヌ</span><span class="kr">nu</span></td>
                            <td><span class="kc">フ</span><span class="kr">fu</span></td>
                            <td><span class="kc">ム</span><span class="kr">mu</span></td>
                            <td><span class="kc">ユ</span><span class="kr">yu</span></td>
                            <td><span class="kc">ル</span><span class="kr">ru</span></td>
                            <td></td>
                            <td></td>
                        </tr>
                        <tr>
                            <td><span class="kc">エ</span><span class="kr">e</span></td>
                            <td><span class="kc">ケ</span><span class="kr">ke</span></td>
                            <td><span class="kc">セ</span><span class="kr">se</span></td>
                            <td><span class="kc">テ</span><span class="kr">te</span></td>
                            <td><span class="kc">ネ</span><span class="kr">ne</span></td>
                            <td><span class="kc">ヘ</span><span class="kr">he</span></td>
                            <td><span class="kc">メ</span><span class="kr">me</span></td>
                            <td></td>
                            <td><span class="kc">レ</span><span class="kr">re</span></td>
                            <td></td>
                            <td></td>
                        </tr>
                        <tr>
                            <td><span class="kc">オ</span><span class="kr">o</span></td>
                            <td><span class="kc">コ</span><span class="kr">ko</span></td>
                            <td><span class="kc">ソ</span><span class="kr">so</span></td>
                            <td><span class="kc">ト</span><span class="kr">to</span></td>
                            <td><span class="kc">ノ</span><span class="kr">no</span></td>
                            <td><span class="kc">ホ</span><span class="kr">ho</span></td>
                            <td><span class="kc">モ</span><span class="kr">mo</span></td>
                            <td><span class="kc">ヨ</span><span class="kr">yo</span></td>
                            <td><span class="kc">ロ</span><span class="kr">ro</span></td>
                            <td><span class="kc">ヲ</span><span class="kr">wo</span></td>
                            <td></td>
                        </tr>
                    </table>
                </div>
            </div>
        </div>

        <div id="mod-lista" class="modulo-content">
            <div class="card">
                <h2>Lista Vocaboli (<span id="total-count">0</span>)</h2>
                <div id="vocab-list"></div>
            </div>
        </div>

    </div>

    <script>
        // --- 1. DATI STATICI (VOCABOLI) ---
        const RAW_VOCAB = `
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
Sì (informale),yes (informal),ええ,ee,L3 - Aggettivi e Espressioni
`;

        // --- 2. DATI FRASI ---
        const RAW_FRASI = `
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

        // --- 3. LOGICA APP ---
        
        let vocaboli = [];
        let frasi = [];
        let queue = [];
        let currentCard = null;
        let score = { ok: 0, err: 0 };

        // Inizializzazione sicura
        document.addEventListener('DOMContentLoaded', () => {
            console.log("App avviata.");
            try {
                loadData();
                renderVocabList();
                renderFrasi();
                cambiaTab('quiz'); // Default tab
            } catch (e) {
                console.error("Errore critico avvio:", e);
                alert("Errore caricamento dati. Reset automatico in corso...");
                forceResetData();
            }
        });

        function cambiaTab(tabId) {
            // Nascondi tutto
            document.querySelectorAll('.modulo-content').forEach(el => el.classList.remove('active-module'));
            document.querySelectorAll('.nav-btn').forEach(el => el.classList.remove('active'));

            // Mostra target
            const target = document.getElementById('mod-' + tabId);
            if(target) target.classList.add('active-module');
            
            // Attiva bottone (ricerca semplice per testo, migliorabile con ID)
            const btns = document.querySelectorAll('.nav-btn');
            btns.forEach(btn => {
                if(btn.onclick.toString().includes(tabId)) btn.classList.add('active');
            });

            if(tabId === 'quiz') initQuizSession();
        }

        // --- GESTIONE DATI ---
        function loadData() {
            const vData = localStorage.getItem('srs_vocaboli_v5');
            const fData = localStorage.getItem('srs_frasi_v5');

            if(vData) {
                try { vocaboli = JSON.parse(vData); } catch(e) { vocaboli = []; }
            }
            if(fData) {
                try { frasi = JSON.parse(fData); } catch(e) { frasi = []; }
            }

            // Se vuoti, carica default
            if(vocaboli.length === 0) parseVocabCSV(RAW_VOCAB);
            if(frasi.length === 0) parseFrasiCSV(RAW_FRASI);

            updateCatFilter();
        }

        function forceResetData() {
            if(!confirm("Sicuro di voler ripristinare i dati originali?")) return;
            localStorage.removeItem('srs_vocaboli_v5');
            localStorage.removeItem('srs_frasi_v5');
            location.reload();
        }

        function clearAllData() {
            if(!confirm("ATTENZIONE: Questo cancellerà TUTTO e lascerà l'app vuota. Continuare?")) return;
            vocaboli = [];
            frasi = [];
            saveData();
            location.reload();
        }

        function saveData() {
            localStorage.setItem('srs_vocaboli_v5', JSON.stringify(vocaboli));
            localStorage.setItem('srs_frasi_v5', JSON.stringify(frasi));
        }

        function parseVocabCSV(csv) {
            vocaboli = [];
            const lines = csv.trim().split('\n');
            lines.forEach(l => {
                const p = l.split(','); // Semplice split, non gestisce virgole nelle frasi (ok per questo dataset)
                if(p.length >= 4) {
                    vocaboli.push({ ita: p[0], eng: p[1], jpn: p[2], rom: p[3], tag: p[4] || 'Generale', lvl: 0 });
                }
            });
            saveData();
        }

        function parseFrasiCSV(csv) {
            frasi = [];
            const lines = csv.trim().split('\n');
            lines.forEach(l => {
                const p = l.split(','); 
                if(p.length >= 3) {
                    frasi.push({ ita: p[0], rom: p[1], jpn: p[2], tag: p[3] || 'Generale' });
                }
            });
            saveData();
        }

        // --- LOGICA QUIZ ---
        function updateCatFilter() {
            const sel = document.getElementById('filtro-cat');
            const current = sel.value;
            const tags = new Set(vocaboli.map(v => v.tag));
            sel.innerHTML = '<option value="TUTTI">Tutte le Categorie</option>';
            Array.from(tags).sort().forEach(t => {
                const opt = document.createElement('option');
                opt.value = t;
                opt.textContent = t;
                sel.appendChild(opt);
            });
            sel.value = current;
        }

        function initQuizSession() {
            const cat = document.getElementById('filtro-cat').value;
            let pool = (cat === 'TUTTI') ? vocaboli : vocaboli.filter(v => v.tag === cat);
            
            // Semplice shuffle
            queue = pool.sort(() => 0.5 - Math.random()).slice(0, 10); // Batch di 10
            
            if(queue.length === 0) {
                document.getElementById('q-label').innerText = "Nessuna carta disponibile";
                document.getElementById('prompt-principale').innerText = "---";
                document.getElementById('input-risposta').disabled = true;
                return;
            }

            score = { ok:0, err:0 };
            updateScore();
            nextCard();
        }

        function nextCard() {
            if(queue.length === 0) {
                document.getElementById('prompt-principale').innerText = "Sessione Finita!";
                document.getElementById('prompt-secondario').innerText = "";
                document.getElementById('btn-check').classList.add('hidden');
                document.getElementById('btn-next').classList.add('hidden');
                return;
            }

            currentCard = queue.pop();
            const mode = Math.random() > 0.5 ? 'JtoI' : 'ItoJ';
            currentCard.mode = mode;

            document.getElementById('btn-check').classList.remove('hidden');
            document.getElementById('btn-next').classList.add('hidden');
            document.getElementById('input-risposta').value = '';
            document.getElementById('input-risposta').disabled = false;
            document.getElementById('input-risposta').focus();
            document.getElementById('feedback-msg').innerText = '';
            document.getElementById('example-msg').innerText = '';

            if(mode === 'JtoI') {
                document.getElementById('q-label').innerText = "TRADUCI IN ITALIANO";
                document.getElementById('prompt-principale').innerHTML = currentCard.jpn + ` <span style='font-size:1rem; cursor:pointer;' onclick="speak('${currentCard.jpn}')">🔊</span>`;
                document.getElementById('prompt-secondario').innerText = currentCard.rom;
            } else {
                document.getElementById('q-label').innerText = "TRADUCI IN GIAPPONESE (ROMAJI)";
                document.getElementById('prompt-principale').innerText = currentCard.ita;
                document.getElementById('prompt-secondario').innerText = currentCard.eng;
            }
        }

        function checkAnswer() {
            const user = document.getElementById('input-risposta').value.trim().toLowerCase();
            let correct = false;

            if(currentCard.mode === 'JtoI') {
                // Accetta ita o eng
                if(user === currentCard.ita.toLowerCase() || user === currentCard.eng.toLowerCase()) correct = true;
            } else {
                // Accetta romaji o jpn
                if(user === currentCard.rom.toLowerCase() || user === currentCard.jpn) correct = true;
            }

            const fb = document.getElementById('feedback-msg');
            if(correct) {
                fb.innerHTML = "<span class='corretto'>Esatto!</span>";
                score.ok++;
                speak(currentCard.jpn);
            } else {
                fb.innerHTML = `<span class='sbagliato'>Errato. Era: ${currentCard.rom} / ${currentCard.jpn}</span>`;
                score.err++;
            }
            
            updateScore();
            document.getElementById('input-risposta').disabled = true;
            document.getElementById('btn-check').classList.add('hidden');
            document.getElementById('btn-next').classList.remove('hidden');
            document.getElementById('btn-next').focus();
        }

        function updateScore() {
            document.getElementById('score-display').innerText = `Punteggio: ${score.ok} | Errori: ${score.err}`;
        }

        // --- LOGICA ASCOLTO ---
        function renderFrasi() {
            const list = document.getElementById('frasi-list');
            const filtroBox = document.getElementById('filtro-frasi');
            const spoiler = document.getElementById('spoiler-check').checked;
            
            // Popola filtro categorie se necessario (una tantum o dynamic)
            const tags = new Set(frasi.map(f => f.tag));
            if(filtroBox.options.length === 1) {
                Array.from(tags).sort().forEach(t => {
                    const o = document.createElement('option'); o.value = t; o.textContent = t;
                    filtroBox.appendChild(o);
                });
            }

            const cat = filtroBox.value;
            const items = (cat === 'TUTTI') ? frasi : frasi.filter(f => f.tag === cat);
            
            list.innerHTML = '';
            if(items.length === 0) { list.innerHTML = '<p style="text-align:center">Nessuna frase.</p>'; return; }

            items.forEach(f => {
                const el = document.createElement('div');
                el.className = `frase-item ${spoiler ? 'spoiler' : ''}`;
                el.innerHTML = `
                    <button class="audio-btn" onclick="speak('${f.jpn.replace(/'/g, "\\'")}')">▶</button>
                    <div class="frase-content">
                        <span class="f-jpn">${f.jpn}</span>
                        <span class="f-rom">${f.rom}</span>
                        <span class="f-ita" onclick="this.parentElement.parentElement.classList.remove('spoiler')">${f.ita}</span>
                    </div>
                `;
                list.appendChild(el);
            });
        }

        // --- LOGICA LISTA ---
        function renderVocabList() {
            const c = document.getElementById('lista-vocaboli-container');
            document.getElementById('total-count').innerText = vocaboli.length;
            c.innerHTML = '';
            // Mostra solo primi 100 per performance se troppi, o tutti
            vocaboli.slice(0, 500).forEach(v => {
                const d = document.createElement('div');
                d.className = 'v-item';
                d.innerHTML = `
                    <div class="v-main">${v.ita}<br><small style="color:#888">${v.eng}</small></div>
                    <div class="v-meta">
                        <span class="v-jp">${v.jpn}</span>
                        <span class="v-ro">${v.rom}</span>
                        <span style="font-size:0.7rem; background:#eee; padding:2px;">${v.tag}</span>
                    </div>
                `;
                c.appendChild(d);
            });
        }

        // --- UTILS ---
        function speak(text) {
            if(!window.speechSynthesis) return;
            const u = new SpeechSynthesisUtterance(text);
            u.lang = 'ja-JP';
            window.speechSynthesis.speak(u);
        }
    </script>
</body>
</html>
