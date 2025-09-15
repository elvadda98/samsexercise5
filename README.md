<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Italian Vocabulary Game</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #6a93cb 0%, #a4bfef 100%);
            min-height: 100vh;
            padding: 20px;
            color: #2c3e50;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .container {
            max-width: 1000px;
            width: 100%;
            background: white;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.2);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            padding: 30px;
            text-align: center;
            position: relative;
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            position: relative;
            z-index: 1;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
        }

        .header p {
            font-size: 1.2em;
            opacity: 0.9;
            position: relative;
            z-index: 1;
        }

        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 10px;
            padding: 20px;
            background: #f8f9fa;
            flex-wrap: wrap;
        }

        .nav-btn {
            padding: 12px 24px;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .nav-btn.active {
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(74, 111, 165, 0.4);
        }

        .nav-btn:not(.active) {
            background: white;
            color: #666;
        }

        .nav-btn:hover:not(.active) {
            background: #e9ecef;
            transform: translateY(-1px);
        }

        .game-section {
            display: none;
            padding: 30px;
            min-height: 400px;
        }

        .game-section.active {
            display: block;
            animation: fadeIn 0.5s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .word-bank {
            background: linear-gradient(135deg, #f5f7fa, #e8f4f2);
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 25px;
            border: 2px solid #4a6fa5;
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.2);
        }

        .word-bank h3 {
            color: #4a6fa5;
            margin-bottom: 15px;
            text-align: center;
            font-size: 1.4em;
        }

        .word-options {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            justify-content: center;
        }

        .word-option {
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            padding: 10px 18px;
            border-radius: 20px;
            font-weight: bold;
            font-size: 16px;
            box-shadow: 0 3px 10px rgba(74, 111, 165, 0.3);
            transition: all 0.3s ease;
            cursor: default;
        }

        .word-option:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(74, 111, 165, 0.4);
        }

        .question {
            background: #f8f9fa;
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 20px;
            border-left: 5px solid #4a6fa5;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
        }

        .question h3 {
            color: #4a6fa5;
            margin-bottom: 15px;
            font-size: 1.3em;
        }

        .fill-blank {
            background: #fff;
            border: 2px solid #ddd;
            border-radius: 8px;
            padding: 8px 12px;
            font-size: 16px;
            margin: 0 5px;
            min-width: 120px;
            transition: border-color 0.3s ease;
        }

        .fill-blank.correct {
            border-color: #4CAF50;
            background: #e8f5e8;
        }

        .fill-blank.incorrect {
            border-color: #f44336;
            background: #ffeaea;
        }

        .options {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }

        .option {
            padding: 15px 20px;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
        }

        .option:hover {
            border-color: #4a6fa5;
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.2);
        }

        .option.selected {
            background: #4a6fa5;
            color: white;
            border-color: #4a6fa5;
        }

        .option.correct {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
        }

        .option.incorrect {
            background: #f44336;
            color: white;
            border-color: #f44336;
        }

        .matching-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            margin-top: 20px;
        }

        .match-column h4 {
            text-align: center;
            margin-bottom: 15px;
            color: #4a6fa5;
            font-size: 1.2em;
        }

        .match-item {
            padding: 15px;
            margin: 8px 0;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
        }

        .match-item:hover {
            border-color: #4a6fa5;
            transform: translateY(-1px);
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.2);
        }

        .match-item.selected {
            background: #e8f4f2;
            border-color: #4a6fa5;
        }

        .match-item.matched {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
            cursor: default;
        }

        .check-btn {
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            margin: 20px auto;
            display: block;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.3);
        }

        .check-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(74, 111, 165, 0.4);
        }

        .feedback {
            margin-top: 15px;
            padding: 15px;
            border-radius: 10px;
            font-weight: bold;
            text-align: center;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from { transform: translateX(-20px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .feedback.correct {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .feedback.incorrect {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .score {
            position: fixed;
            top: 20px;
            right: 20px;
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            padding: 15px 20px;
            border-radius: 25px;
            font-weight: bold;
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.3);
            z-index: 1000;
        }

        .icon {
            font-size: 24px;
            margin-right: 10px;
            vertical-align: middle;
        }

        /* Vocabulary List Styles */
        .vocabulary-list {
            padding: 20px;
        }

        .vocabulary-item {
            margin-bottom: 25px;
            padding: 20px;
            border-radius: 10px;
            background: #f8f9fa;
            box-shadow: 0 4px 10px rgba(0,0,0,0.05);
        }

        .vocabulary-item h3 {
            color: #4a6fa5;
            margin-bottom: 10px;
            font-size: 1.4em;
            border-bottom: 2px solid #4a6fa5;
            padding-bottom: 8px;
        }

        .vocabulary-item p {
            margin-bottom: 8px;
            line-height: 1.6;
        }

        .example {
            font-style: italic;
            color: #555;
            padding-left: 15px;
            border-left: 3px solid #4a6fa5;
            margin-top: 10px;
        }

        @media (max-width: 768px) {
            .matching-container {
                grid-template-columns: 1fr;
                gap: 20px;
            }
            
            .nav-buttons {
                flex-direction: column;
                align-items: center;
            }
            
            .nav-btn {
                width: 200px;
            }
            
            .score {
                position: static;
                margin: 20px auto;
                display: block;
                width: fit-content;
            }
        }
    </style>
</head>
<body>
    <div class="score">Score: <span id="score">0</span>/12</div>
    
    <div class="container">
        <div class="header">
            <h1><i class="fas fa-book icon"></i> Italian Vocabulary Game</h1>
            <p>Learn these useful Italian words and expressions!</p>
        </div>

        <div class="nav-buttons">
            <button class="nav-btn active" onclick="showSection('vocabulary')">Vocabulary List</button>
            <button class="nav-btn" onclick="showSection('fill-gaps')">Fill in the Gaps</button>
            <button class="nav-btn" onclick="showSection('matching')">Match Definitions</button>
            <button class="nav-btn" onclick="showSection('multiple-choice')">Multiple Choice</button>
        </div>

        <!-- Vocabulary List Section -->
        <div id="vocabulary" class="game-section active">
            <h2 style="text-align: center; margin-bottom: 20px; color: #4a6fa5;">Italian Vocabulary</h2>
            
            <div class="vocabulary-list">
                <div class="vocabulary-item">
                    <h3>come mai</h3>
                    <p><strong>Definition:</strong> Why, how come</p>
                    <p><strong>Usage:</strong> Used to ask for an explanation or reason.</p>
                    <p class="example"><strong>Example:</strong> "Come mai non sei venuto alla festa?" (Why didn't you come to the party?)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3>volte</h3>
                    <p><strong>Definition:</strong> Times (as in occurrences)</p>
                    <p><strong>Usage:</strong> Used to indicate how many times something happens.</p>
                    <p class="example"><strong>Example:</strong> "Sono stato a Roma tre volte." (I've been to Rome three times.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3>dappertutto</h3>
                    <p><strong>Definition:</strong> Everywhere</p>
                    <p><strong>Usage:</strong> Indicates that something is in all places.</p>
                    <p class="example"><strong>Example:</strong> "Ho cercato le chiavi dappertutto." (I looked for the keys everywhere.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3>potessi</h3>
                    <p><strong>Definition:</strong> Could (subjunctive form of potere)</p>
                    <p><strong>Usage:</strong> Used to express possibility or ability in hypothetical situations.</p>
                    <p class="example"><strong>Example:</strong> "Se potessi, viaggerei il mondo." (If I could, I would travel the world.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3>di solito</h3>
                    <p><strong>Definition:</strong> Usually, generally</p>
                    <p><strong>Usage:</strong> Indicates a habitual action or regular occurrence.</p>
                    <p class="example"><strong>Example:</strong> "Di solito mi sveglio alle sette." (I usually wake up at seven.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3>costoso</h3>
                    <p><strong>Definition:</strong> Expensive, costly</p>
                    <p><strong>Usage:</strong> Describes something that has a high price.</p>
                    <p class="example"><strong>Example:</strong> "Questo ristorante è troppo costoso." (This restaurant is too expensive.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3>funziona</h3>
                    <p><strong>Definition:</strong> It works, functions</p>
                    <p><strong>Usage:</strong> Indicates that something is operating correctly.</p>
                    <p class="example"><strong>Example:</strong> "Il mio telefono non funziona bene." (My phone doesn't work well.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3>valigia</h3>
                    <p><strong>Definition:</strong> Suitcase</p>
                    <p><strong>Usage:</strong> A bag for carrying belongings during travel.</p>
                    <p class="example"><strong>Example:</strong> "Devo fare la valigia per il viaggio." (I need to pack my suitcase for the trip.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3>previsioni</h3>
                    <p><strong>Definition:</strong> Forecasts, predictions</p>
                    <p><strong>Usage:</strong> Often used for weather forecasts.</p>
                    <p class="example"><strong>Example:</strong> "Le previsioni del tempo dicono che pioverà domani." (The weather forecast says it will rain tomorrow.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3>stanco</h3>
                    <p><strong>Definition:</strong> Tired, weary</p>
                    <p><strong>Usage:</strong> Describes a state of fatigue.</p>
                    <p class="example"><strong>Example:</strong> "Sono stanco dopo una lunga giornata di lavoro." (I'm tired after a long day of work.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3>palco</h3>
                    <p><strong>Definition:</strong> Stage (for performances)</p>
                    <p><strong>Usage:</strong> A raised platform for performances.</p>
                    <p class="example"><strong>Example:</strong> "I musicisti sono saliti sul palco." (The musicians went up on stage.)</p>
                </div>
            </div>
        </div>

        <!-- Fill in the Gaps Section -->
        <div id="fill-gaps" class="game-section">
            <div class="word-bank">
                <h3><i class="fas fa-list-ul icon"></i> Word Bank - Choose from these words:</h3>
                <div class="word-options">
                    <span class="word-option">come mai</span>
                    <span class="word-option">volte</span>
                    <span class="word-option">dappertutto</span>
                    <span class="word-option">potessi</span>
                    <span class="word-option">di solito</span>
                    <span class="word-option">costoso</span>
                    <span class="word-option">funziona</span>
                    <span class="word-option">valigia</span>
                    <span class="word-option">previsioni</span>
                    <span class="word-option">stanco</span>
                    <span class="word-option">palco</span>
                </div>
            </div>

            <!-- SHUFFLED SENTENCES WITH ITALIAN WORDS -->
            <div class="question">
                <h3>Question 1:</h3>
                <p>Dopo la corsa, mi sento molto <input type="text" class="fill-blank" data-answer="stanco" placeholder="answer">. (After the run, I feel very tired.)</p>
                <div class="feedback" id="feedback-1" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 2:</h3>
                <p>Ho cercato le mie chiavi <input type="text" class="fill-blank" data-answer="dappertutto" placeholder="answer"> ma non le trovo. (I looked for my keys everywhere but I can't find them.)</p>
                <div class="feedback" id="feedback-2" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 3:</h3>
                <p>Questo ristorante è troppo <input type="text" class="fill-blank" data-answer="costoso" placeholder="answer"> per il mio budget. (This restaurant is too expensive for my budget.)</p>
                <div class="feedback" id="feedback-3" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 4:</h3>
                <p>Devo fare la <input type="text" class="fill-blank" data-answer="valigia" placeholder="answer"> per il mio viaggio in Italia. (I need to pack my suitcase for my trip to Italy.)</p>
                <div class="feedback" id="feedback-4" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 5:</h3>
                <p>Il cantante è salito sul <input type="text" class="fill-blank" data-answer="palco" placeholder="answer"> tra gli applausi del pubblico. (The singer went up on stage amid audience applause.)</p>
                <div class="feedback" id="feedback-5" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 6:</h3>
                <p>Ho visitato Parigi tre <input type="text" class="fill-blank" data-answer="volte" placeholder="answer"> nella mia vita. (I've visited Paris three times in my life.)</p>
                <div class="feedback" id="feedback-6" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 7:</h3>
                <p>Le <input type="text" class="fill-blank" data-answer="previsioni" placeholder="answer"> del tempo dicono che sarà soleggiato domani. (The weather forecast says it will be sunny tomorrow.)</p>
                <div class="feedback" id="feedback-7" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 8:</h3>
                <p>Se <input type="text" class="fill-blank" data-answer="potessi" placeholder="answer">, andrei in vacanza tutto l'anno. (If I could, I would go on vacation all year.)</p>
                <div class="feedback" id="feedback-8" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 9:</h3>
                <p>Il mio computer non <input type="text" class="fill-blank" data-answer="funziona" placeholder="answer"> bene oggi. (My computer isn't working well today.)</p>
                <div class="feedback" id="feedback-9" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 10:</h3>
                <p><input type="text" class="fill-blank" data-answer="come mai" placeholder="answer"> non sei venuto alla festa ieri sera? (Why didn't you come to the party last night?)</p>
                <div class="feedback" id="feedback-10" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 11:</h3>
                <p><input type="text" class="fill-blank" data-answer="di solito" placeholder="answer"> prendo il caffè alle 8 del mattino. (I usually have coffee at 8 in the morning.)</p>
                <div class="feedback" id="feedback-11" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 12:</h3>
                <p>Quel negozio è <input type="text" class="fill-blank" data-answer="costoso" placeholder="answer"> ma vende prodotti di alta qualità. (That store is expensive but sells high-quality products.)</p>
                <div class="feedback" id="feedback-12" style="display: none;"></div>
            </div>

            <button class="check-btn" onclick="checkFillBlanks()">Check Answers</button>
        </div>

        <!-- Matching Section -->
        <div id="matching" class="game-section">
            <h2 style="text-align: center; margin-bottom: 20px; color: #4a6fa5;">Match the Italian words with their English definitions</h2>
            <div class="matching-container">
                <div class="match-column">
                    <h4>Italian Words</h4>
                    <div class="match-item" data-word="come mai" onclick="selectMatch(this)">come mai</div>
                    <div class="match-item" data-word="volte" onclick="selectMatch(this)">volte</div>
                    <div class="match-item" data-word="dappertutto" onclick="selectMatch(this)">dappertutto</div>
                    <div class="match-item" data-word="potessi" onclick="selectMatch(this)">potessi</div>
                    <div class="match-item" data-word="di solito" onclick="selectMatch(this)">di solito</div>
                    <div class="match-item" data-word="costoso" onclick="selectMatch(this)">costoso</div>
                    <div class="match-item" data-word="funziona" onclick="selectMatch(this)">funziona</div>
                    <div class="match-item" data-word="valigia" onclick="selectMatch(this)">valigia</div>
                    <div class="match-item" data-word="previsioni" onclick="selectMatch(this)">previsioni</div>
                    <div class="match-item" data-word="stanco" onclick="selectMatch(this)">stanco</div>
                    <div class="match-item" data-word="palco" onclick="selectMatch(this)">palco</div>
                </div>
                <div class="match-column">
                    <h4>English Definitions</h4>
                    <div id="meanings-container">
                        <!-- Meanings will be dynamically inserted here -->
                    </div>
                </div>
            </div>
            <div class="feedback" id="matching-feedback" style="display: none;"></div>
            <button class="check-btn" onclick="checkMatching()">Check Matching</button>
        </div>

        <!-- Multiple Choice Section -->
        <div id="multiple-choice" class="game-section">
            <div class="question">
                <h3>Question 1: What does "come mai" mean?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">How much</div>
                    <div class="option" onclick="selectOption(this, true)">Why, how come</div>
                    <div class="option" onclick="selectOption(this, false)">Good morning</div>
                    <div class="option" onclick="selectOption(this, false)">Maybe</div>
                </div>
                <div class="feedback" id="mc-feedback-1" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 2: "Volte" is used to indicate:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Places</div>
                    <div class="option" onclick="selectOption(this, true)">Times or occurrences</div>
                    <div class="option" onclick="selectOption(this, false)">People</div>
                    <div class="option" onclick="selectOption(this, false)">Feelings</div>
                </div>
                <div class="feedback" id="mc-feedback-2" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 3: If something is "dappertutto", it is:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Nowhere</div>
                    <div class="option" onclick="selectOption(this, true)">Everywhere</div>
                    <div class="option" onclick="selectOption(this, false)">Somewhere</div>
                    <div class="option" onclick="selectOption(this, false)">Anywhere</div>
                </div>
                <div class="feedback" id="mc-feedback-3" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 4: "Potessi" is the subjunctive form of:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Dovere (must)</div>
                    <div class="option" onclick="selectOption(this, true)">Potere (can)</div>
                    <div class="option" onclick="selectOption(this, false)">Volere (want)</div>
                    <div class="option" onclick="selectOption(this, false)">Andare (go)</div>
                </div>
                <div class="feedback" id="mc-feedback-4" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 5: "Di solito" means:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Sometimes</div>
                    <div class="option" onclick="selectOption(this, true)">Usually</div>
                    <div class="option" onclick="selectOption(this, false)">Never</div>
                    <div class="option" onclick="selectOption(this, false)">Always</div>
                </div>
                <div class="feedback" id="mc-feedback-5" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 6: Something that is "costoso" is:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Cheap</div>
                    <div class="option" onclick="selectOption(this, true)">Expensive</div>
                    <div class="option" onclick="selectOption(this, false)">Beautiful</div>
                    <div class="option" onclick="selectOption(this, false)">Useful</div>
                </div>
                <div class="feedback" id="mc-feedback-6" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 7: If something "funziona", it:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Is broken</div>
                    <div class="option" onclick="selectOption(this, true)">Works or functions</div>
                    <div class="option" onclick="selectOption(this, false)">Is new</div>
                    <div class="option" onclick="selectOption(this, false)">Is old</div>
                </div>
                <div class="feedback" id="mc-feedback-7" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 8: A "valigia" is used for:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Cooking</div>
                    <div class="option" onclick="selectOption(this, true)">Traveling</div>
                    <div class="option" onclick="selectOption(this, false)">Sleeping</div>
                    <div class="option" onclick="selectOption(this, false)">Studying</div>
                </div>
                <div class="feedback" id="mc-feedback-8" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 9: "Previsioni" are often about:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">The past</div>
                    <div class="option" onclick="selectOption(this, true)">The future</div>
                    <div class="option" onclick="selectOption(this, false)">The present</div>
                    <div class="option" onclick="selectOption(this, false)">History</div>
                </div>
                <div class="feedback" id="mc-feedback-9" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 10: If you are "stanco", you need to:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Eat</div>
                    <div class="option" onclick="selectOption(this, true)">Rest</div>
                    <div class="option" onclick="selectOption(this, false)">Work</div>
                    <div class="option" onclick="selectOption(this, false)">Study</div>
                </div>
                <div class="feedback" id="mc-feedback-10" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 11: A "palco" is typically found in:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Kitchens</div>
                    <div class="option" onclick="selectOption(this, true)">Theaters</div>
                    <div class="option" onclick="selectOption(this, false)">Bedrooms</div>
                    <div class="option" onclick="selectOption(this, false)">Gardens</div>
                </div>
                <div class="feedback" id="mc-feedback-11" style="display: none;"></div>
            </div>
        </div>
    </div>

    <script>
        let score = 0;
        let selectedWord = null;
        let selectedMeaning = null;
        let matchedPairs = [];
        
        // Definitions for the matching game
        const definitions = [
            { meaning: "come mai", text: "Why, how come" },
            { meaning: "volte", text: "Times (occurrences)" },
            { meaning: "dappertutto", text: "Everywhere" },
            { meaning: "potessi", text: "Could (subjunctive form)" },
            { meaning: "di solito", text: "Usually, generally" },
            { meaning: "costoso", text: "Expensive, costly" },
            { meaning: "funziona", text: "It works, functions" },
            { meaning: "valigia", text: "Suitcase" },
            { meaning: "previsioni", text: "Forecasts, predictions" },
            { meaning: "stanco", text: "Tired, weary" },
            { meaning: "palco", text: "Stage (for performances)" }
        ];

        // Function to shuffle array (Fisher-Yates algorithm)
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        // Initialize the matching game with shuffled definitions
        function initMatchingGame() {
            const meaningsContainer = document.getElementById('meanings-container');
            meaningsContainer.innerHTML = '';
            
            // Shuffle the definitions
            const shuffledDefinitions = shuffleArray([...definitions]);
            
            // Create and append the definition elements
            shuffledDefinitions.forEach(def => {
                const div = document.createElement('div');
                div.className = 'match-item';
                div.setAttribute('data-meaning', def.meaning);
                div.onclick = function() { selectMatch(this); };
                div.textContent = def.text;
                meaningsContainer.appendChild(div);
            });
        }

        function showSection(sectionId) {
            // Hide all sections
            document.querySelectorAll('.game-section').forEach(section => {
                section.classList.remove('active');
            });
            
            // Remove active class from all buttons
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // Show selected section
            document.getElementById(sectionId).classList.add('active');
            
            // Add active class to clicked button
            event.target.classList.add('active');
            
            // If showing matching section, reinitialize with shuffled definitions
            if (sectionId === 'matching') {
                initMatchingGame();
                // Reset matching game state
                document.querySelectorAll('.match-item').forEach(item => {
                    item.classList.remove('selected', 'matched');
                });
                selectedWord = null;
                selectedMeaning = null;
                matchedPairs = [];
                document.getElementById('matching-feedback').style.display = 'none';
            }
        }

        function checkFillBlanks() {
            const blanks = document.querySelectorAll('.fill-blank');
            let correctCount = 0;
            
            blanks.forEach((blank, index) => {
                const userAnswer = blank.value.toLowerCase().trim();
                const correctAnswer = blank.dataset.answer.toLowerCase();
                
                if (userAnswer === correctAnswer) {
                    blank.classList.remove('incorrect');
                    blank.classList.add('correct');
                    correctCount++;
                } else {
                    blank.classList.remove('correct');
                    blank.classList.add('incorrect');
                }
                
                // Show feedback for each question
                const feedback = document.getElementById(`feedback-${index+1}`);
                if (userAnswer === correctAnswer) {
                    feedback.textContent = '✅ Correct!';
                    feedback.className = 'feedback correct';
                } else {
                    feedback.textContent = `❌ Incorrect. The correct answer is: "${blank.dataset.answer}"`;
                    feedback.className = 'feedback incorrect';
                }
                feedback.style.display = 'block';
            });
            
            updateScore();
        }

        function selectMatch(element) {
            if (element.classList.contains('matched')) return;
            
            if (element.dataset.word) {
                // Word selected
                if (selectedWord) selectedWord.classList.remove('selected');
                selectedWord = element;
                element.classList.add('selected');
            } else {
                // Meaning selected
                if (selectedMeaning) selectedMeaning.classList.remove('selected');
                selectedMeaning = element;
                element.classList.add('selected');
            }
            
            // Check if we have both word and meaning selected
            if (selectedWord && selectedMeaning) {
                checkMatch();
            }
        }

        function checkMatch() {
            const feedback = document.getElementById('matching-feedback');
            
            if (selectedWord.dataset.word === selectedMeaning.dataset.meaning) {
                // Correct match
                selectedWord.classList.remove('selected');
                selectedWord.classList.add('matched');
                selectedMeaning.classList.remove('selected');
                selectedMeaning.classList.add('matched');
                
                matchedPairs.push(selectedWord.dataset.word);
                
                feedback.textContent = '✅ Correct match!';
                feedback.className = 'feedback correct';
                
                // Check if all matches are complete
                if (matchedPairs.length === definitions.length) {
                    feedback.textContent = '🎉 Congratulations! You matched all terms correctly!';
                }
            } else {
                // Incorrect match
                feedback.textContent = '❌ Incorrect match. Try again.';
                feedback.className = 'feedback incorrect';
            }
            
            feedback.style.display = 'block';
            selectedWord = null;
            selectedMeaning = null;
            
            updateScore();
        }

        function checkMatching() {
            const allMatches = document.querySelectorAll('.match-item[data-word]');
            let allCorrect = true;
            
            allMatches.forEach(item => {
                if (!item.classList.contains('matched')) {
                    allCorrect = false;
                }
            });
            
            const feedback = document.getElementById('matching-feedback');
            if (allCorrect) {
                feedback.textContent = '🎉 Excellent! All matches are correct!';
                feedback.className = 'feedback correct';
            } else {
                feedback.textContent = 'Keep trying! Some matches are still incorrect.';
                feedback.className = 'feedback incorrect';
            }
            feedback.style.display = 'block';
        }

        function selectOption(element, isCorrect) {
            // Remove selection from all options in this question
            const options = element.parentElement.querySelectorAll('.option');
            options.forEach(opt => {
                opt.classList.remove('selected');
                opt.classList.remove('correct');
                opt.classList.remove('incorrect');
            });
            
            // Mark the selected option
            element.classList.add('selected');
            
            const questionNumber = element.closest('.question').querySelector('h3').textContent.match(/\d+/)[0];
            const feedback = document.getElementById(`mc-feedback-${questionNumber}`);
            
            if (isCorrect) {
                element.classList.add('correct');
                feedback.textContent = '✅ Correct!';
                feedback.className = 'feedback correct';
            } else {
                element.classList.add('incorrect');
                feedback.textContent = '❌ Incorrect. Try again.';
                feedback.className = 'feedback incorrect';
                
                // Show the correct answer
                options.forEach(opt => {
                    if (opt.onclick.toString().includes('true')) {
                        opt.classList.add('correct');
                    }
                });
            }
            
            feedback.style.display = 'block';
            updateScore();
        }

        function updateScore() {
            // Calculate score for fill-in-the-blank
            let fillScore = 0;
            document.querySelectorAll('.fill-blank.correct').forEach(blank => {
                if (!blank.classList.contains('counted')) {
                    fillScore++;
                    blank.classList.add('counted');
                }
            });
            
            // Calculate score for matching (each pair is 1 point)
            const matchScore = matchedPairs.length;
            
            // Calculate score for multiple choice (each correct is 1 point)
            let mcScore = 0;
            document.querySelectorAll('.option.correct.selected').forEach(opt => {
                mcScore++;
            });
            
            // Total score
            score = fillScore + matchScore + mcScore;
            document.getElementById('score').textContent = score;
        }
        
        // Initialize the page
        window.onload = function() {
            initMatchingGame();
        }
    </script>
</body>
</html>
