# Calculadora-de-custos-
Cálcule seus custos 
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calcule seu Custo - Stecca Recheios</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;600;700&family=Playfair+Display:wght@700&display=swap" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <style>
        :root {
            --primary-color: #A0522D; /* Tom de marrom terra/confeitaria */
            --secondary-color: #F5DEB3; /* Creme suave */
            --text-color: #333;
            --light-text-color: #666;
            --background-gradient: linear-gradient(135deg, #FDFBFB 0%, #EBEDEE 100%);
            --button-hover-bg: #8B4513;
        }

        body {
            font-family: 'Montserrat', sans-serif;
            margin: 0;
            padding: 0;
            background: var(--background-gradient);
            color: var(--text-color);
            line-height: 1.6;
            display: flex;
            justify-content: center;
            align-items: flex-start; /* Alinha ao topo para evitar overflow em telas pequenas */
            min-height: 100vh;
            padding: 20px 0;
            overflow-x: hidden;
            position: relative;
        }

        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: url('https://images.unsplash.com/photo-1579606138676-e9d6d3d9d3d3?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=M3w0NTc1MjN8MHwxfHNlYXJjaHwyMHx8Y29uZmVpdGVpcm8lMjBjb25mZWl0YW5kb3xlbnwwfHx8fDE3MTkyNTM2MjZ8MA&ixlib=rb-4.0.3&q=80&w=1920'); /* Imagem de confeiteiro */
            background-size: cover;
            background-position: center;
            filter: brightness(0.6) grayscale(0.5); /* Escurece e dessatura a imagem */
            z-index: -1;
        }

        .container {
            background-color: rgba(255, 255, 255, 0.95); /* Fundo branco semi-transparente */
            padding: 40px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            width: 90%;
            max-width: 800px;
            box-sizing: border-box;
            position: relative;
            z-index: 1;
            margin-top: 20px; /* Espaçamento superior para mobile */
            margin-bottom: 20px; /* Espaçamento inferior para mobile */
        }

        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
            border-bottom: 1px solid #eee;
            padding-bottom: 15px;
            flex-wrap: wrap; /* Permite quebrar linha em telas pequenas */
        }

        h1 {
            font-family: 'Playfair Display', serif;
            color: var(--primary-color);
            margin: 0;
            font-size: 2.5em;
            text-align: center;
            flex-grow: 1;
            min-width: 200px; /* Garante que o título não fique muito pequeno */
        }

        .logo {
            font-family: 'Playfair Display', serif;
            font-weight: 700;
            font-size: 1.2em;
            color: var(--primary-color);
            text-transform: uppercase;
            letter-spacing: 1px;
            white-space: nowrap;
        }

        .input-group {
            margin-bottom: 20px;
        }

        .input-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: var(--text-color);
        }

        .input-group input[type="text"],
        .input-group input[type="number"] {
            width: calc(100% - 20px);
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 1em;
            transition: border-color 0.3s ease;
        }

        .input-group input[type="text"]:focus,
        .input-group input[type="number"]:focus {
            border-color: var(--primary-color);
            outline: none;
        }

        .ingredients-section h2, .pricing-section h2 {
            font-family: 'Playfair Display', serif;
            color: var(--primary-color);
            margin-top: 30px;
            margin-bottom: 20px;
            font-size: 1.8em;
            border-bottom: 1px dashed #eee;
            padding-bottom: 10px;
        }

        .ingredient-item {
            display: grid;
            grid-template-columns: 2fr 1fr 1fr 1.5fr 0.1fr; /* Ajustado para 5 colunas */
            gap: 15px;
            margin-bottom: 15px;
            align-items: center;
            background-color: var(--secondary-color);
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
        }

        .ingredient-item label {
            font-size: 0.9em;
            color: var(--light-text-color);
            margin-bottom: 5px;
            display: block;
        }

        .ingredient-item input {
            width: 100%;
            padding: 8px;
            border: 1px solid #ccc;
            border-radius: 5px;
            font-size: 0.95em;
        }
        
        .ingredient-item input[type="number"] {
            -moz-appearance: textfield; /* Firefox */
        }
        .ingredient-item input[type="number"]::-webkit-outer-spin-button,
        .ingredient-item input[type="number"]::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }

        .remove-ingredient {
            background: none;
            border: none;
            color: #ff6347;
            font-size: 1.5em;
            cursor: pointer;
            padding: 0;
            transition: color 0.3s ease;
        }

        .remove-ingredient:hover {
            color: #cc0000;
        }

        .add-ingredient-btn {
            background-color: var(--primary-color);
            color: white;
            padding: 12px 25px;
            border: none;
            border-radius: 8px;
            font-size: 1.1em;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.2s ease;
            display: block;
            width: fit-content;
            margin: 20px auto 30px auto;
        }

        .add-ingredient-btn:hover {
            background-color: var(--button-hover-bg);
            transform: translateY(-2px);
        }

        .calculate-btn {
            background-color: var(--primary-color);
            color: white;
            padding: 15px 30px;
            border: none;
            border-radius: 10px;
            font-size: 1.3em;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.2s ease;
            width: 100%;
            box-sizing: border-box;
            margin-top: 20px;
        }

        .calculate-btn:hover {
            background-color: var(--button-hover-bg);
            transform: translateY(-2px);
        }

        #results-section {
            background-color: rgba(255, 255, 255, 0.9);
            padding: 30px;
            border-radius: 15px;
            margin-top: 40px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.08);
            display: none; /* Escondido por padrão */
            text-align: left;
        }

        #results-section h2 {
            font-family: 'Playfair Display', serif;
            color: var(--primary-color);
            font-size: 2em;
            text-align: center;
            margin-bottom: 25px;
        }

        #results-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #eee;
            padding-bottom: 15px;
            margin-bottom: 20px;
            flex-wrap: wrap; /* Permite quebrar linha em telas pequenas */
        }
        
        #results-header .logo {
            font-size: 1em;
            color: var(--light-text-color);
            order: 1; /* Para manter a logo no canto */
        }
        #results-header #result-recipe-name {
            order: 2; /* Nome da receita no centro */
            flex-grow: 1;
            text-align: center;
            font-size: 1.8em;
        }


        .results-section-title {
            font-family: 'Montserrat', sans-serif;
            font-weight: 600;
            color: var(--primary-color);
            margin-top: 25px;
            margin-bottom: 15px;
            font-size: 1.3em;
            border-bottom: 1px dashed #ddd;
            padding-bottom: 8px;
        }

        .result-item {
            margin-bottom: 10px;
            font-size: 1.1em;
        }

        .result-item strong {
            color: var(--primary-color);
            font-weight: 700;
        }

        .ingredient-summary-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
            padding-bottom: 5px;
            border-bottom: 1px dotted #eee;
            font-size: 1em;
            color: var(--light-text-color);
        }
        .ingredient-summary-item:last-child {
            border-bottom: none;
        }
        .ingredient-summary-item span:first-child {
            font-weight: 600;
            color: var(--text-color);
        }

        .pdf-btn {
            background-color: #007bff; /* Azul para PDF */
            color: white;
            padding: 12px 25px;
            border: none;
            border-radius: 8px;
            font-size: 1.1em;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.2s ease;
            display: block;
            width: fit-content;
            margin: 30px auto 0 auto;
        }

        .pdf-btn:hover {
            background-color: #0056b3;
            transform: translateY(-2px);
        }

        /* --- Media Queries para Responsividade --- */
        @media (max-width: 768px) {
            .container {
                padding: 20px;
                margin-top: 10px;
                margin-bottom: 10px;
            }

            header {
                flex-direction: column; /* Empilha logo e título */
                align-items: center;
                text-align: center;
            }

            .logo {
                margin-bottom: 10px;
            }
            
            h1 {
                font-size: 1.8em;
                margin-top: 0;
            }

            .ingredient-item {
                grid-template-columns: 1fr; /* Cada item ocupa a largura total */
                gap: 10px; /* Menos espaçamento entre itens empilhados */
                padding: 10px;
            }

            .ingredient-item div {
                grid-column: 1 / -1; /* Garante que cada div ocupe a largura total */
            }

            .remove-ingredient {
                margin-top: 10px; /* Espaçamento do botão de remover */
                width: 100%;
                text-align: right;
            }
            
            .add-ingredient-btn, .calculate-btn, .pdf-btn {
                padding: 10px 20px;
                font-size: 1em;
            }
            
            #results-header {
                flex-direction: column;
                align-items: center;
                text-align: center;
            }

            #results-header .logo {
                margin-bottom: 10px;
            }

            #results-header #result-recipe-name {
                font-size: 1.5em;
            }

            .results-section-title, .result-item {
                font-size: 1em;
            }
            .ingredient-summary-item {
                font-size: 0.95em;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <span class="logo">Stecca Recheios</span>
            <h1>Calcule seu Custo</h1>
        </header>

        <div class="input-group">
            <label for="recipe-name">Nome da Receita:</label>
            <input type="text" id="recipe-name" placeholder="Ex: Bolo de Fubá Cremoso da Vovó">
        </div>

        <div class="ingredients-section">
            <h2>Ingredientes</h2>
            <div id="ingredients-list">
                </div>
            <button type="button" class="add-ingredient-btn" id="add-ingredient">Adicionar Ingrediente</button>
        </div>

        <div class="input-group">
            <label for="loss-percentage">Porcentagem de Perda na Produção (%):</label>
            <input type="number" id="loss-percentage" value="10" min="0" max="100" step="0.1">
        </div>

        <div class="pricing-section">
            <h2>Precificação</h2>
            <div class="input-group">
                <label for="markup-percentage">Markup Desejado (%):</label>
                <input type="number" id="markup-percentage" value="100" min="0" step="1">
            </div>
            <div class="input-group">
                <label for="unit-grammage">Gramatura por Unidade/Porção (g/ml):<br><small>(Ex: 100 para um brigadeiro de 100g)</small></label>
                <input type="number" id="unit-grammage" value="0" min="0" step="1">
            </div>
        </div>

        <button type="button" class="calculate-btn" id="calculate-btn">Calcular Custos e Preços</button>

        <div id="results-section">
            <div id="results-header">
                <span class="logo">Stecca Recheios</span>
                <h2 id="result-recipe-name"></h2>
            </div>
            
            <div class="results-section-title">Ingredientes Utilizados:</div>
            <div id="ingredients-summary">
                </div>

            <div class="results-section-title">Resultados de Custo:</div>
            <div class="result-item"><strong>Custo Total dos Ingredientes:</strong> <span id="total-ingredients-cost"></span></div>
            <div class="result-item"><strong>Porcentagem de Perda na Produção:</strong> <span id="display-loss-percentage"></span>%</div>
            <div class="result-item"><strong>Peso Inicial dos Ingredientes (total):</strong> <span id="initial-weight"></span></div>
            <div class="result-item"><strong>Peso Final da Receita (após perda):</strong> <span id="final-weight"></span></div>
            <div class="result-item"><strong>Custo por Quilo (kg) da Receita:</strong> <span id="cost-per-kg"></span></div>
            <div class="result-item"><strong>Custo Final da Receita Completa:</strong> <span id="final-recipe-cost"></span></div>
            
            <div class="results-section-title">Resultados de Venda:</div>
            <div class="result-item"><strong>Markup Aplicado:</strong> <span id="display-markup-percentage"></span>%</div>
            <div class="result-item"><strong>Rendimento em Unidades:</strong> <span id="yield-units"></span></div>
            <div class="result-item"><strong>Preço de Venda Sugerido por Kg:</strong> <span id="suggested-price-per-kg"></span></div>
            <div class="result-item"><strong>Preço de Venda Sugerido por Unidade:</strong> <span id="suggested-price-per-unit"></span></div>


            <button type="button" class="pdf-btn" id="generate-pdf">Gerar PDF</button>
        </div>
    </div>

    <script>
        const ingredientsList = document.getElementById('ingredients-list');
        const addIngredientBtn = document.getElementById('add-ingredient');
        const calculateBtn = document.getElementById('calculate-btn');
        const resultsSection = document.getElementById('results-section');
        const generatePdfBtn = document.getElementById('generate-pdf');

        // Adiciona um ingrediente inicial ao carregar a página
        addIngredientInput(); 

        function addIngredientInput() {
            const ingredientDiv = document.createElement('div');
            ingredientDiv.classList.add('ingredient-item');
            ingredientDiv.innerHTML = `
                <div>
                    <label>Nome do Ingrediente:</label>
                    <input type="text" class="ingredient-name" placeholder="Farinha de Trigo">
                </div>
                <div>
                    <label>Preço Emb. (R$):</label>
                    <input type="number" class="ingredient-price-pack" min="0" step="0.01" value="0">
                </div>
                <div>
                    <label>Peso/Quant. Emb. (g/ml/un):<br><small>(ex: 1000 para 1kg, 500 para 500g)</small></label>
                    <input type="number" class="ingredient-weight-pack" min="0" step="0.01" value="0">
                </div>
                <div>
                    <label>Quant. Usada (g/ml/un):<br><small>(ex: 250 para 250g, 500 para 500ml)</small></label>
                    <input type="number" class="ingredient-quantity-used" min="0" step="0.01" value="0">
                </div>
                <div>
                    <button type="button" class="remove-ingredient" aria-label="Remover ingrediente">&times;</button>
                </div>
            `;
            ingredientsList.appendChild(ingredientDiv);

            ingredientDiv.querySelector('.remove-ingredient').addEventListener('click', () => {
                ingredientDiv.remove();
            });
        }

        addIngredientBtn.addEventListener('click', addIngredientInput);

        calculateBtn.addEventListener('click', () => {
            const recipeName = document.getElementById('recipe-name').value || 'Sua Receita';
            const lossPercentage = parseFloat(document.getElementById('loss-percentage').value.replace(',', '.')) / 100 || 0;
            const markupPercentage = parseFloat(document.getElementById('markup-percentage').value.replace(',', '.')) / 100 || 0;
            const unitGrammage = parseFloat(document.getElementById('unit-grammage').value.replace(',', '.'));

            let totalCost = 0;
            let totalWeightUsed = 0;
            const ingredientsSummary = [];

            let hasInvalidInput = false;

            document.querySelectorAll('.ingredient-item').forEach(item => {
                const name = item.querySelector('.ingredient-name').value.trim();
                const pricePack = parseFloat(item.querySelector('.ingredient-price-pack').value.replace(',', '.'));
                const weightPack = parseFloat(item.querySelector('.ingredient-weight-pack').value.replace(',', '.'));
                const quantityUsed = parseFloat(item.querySelector('.ingredient-quantity-used').value.replace(',', '.'));

                // Validação para campos de ingrediente
                if (!name || isNaN(pricePack) || isNaN(weightPack) || isNaN(quantityUsed) || weightPack <= 0) {
                    item.style.border = '2px solid red';
                    hasInvalidInput = true;
                    return; 
                } else {
                    item.style.border = 'none';
                }

                const costPerUnit = pricePack / weightPack;
                const ingredientCost = costPerUnit * quantityUsed;

                totalCost += ingredientCost;
                totalWeightUsed += quantityUsed;

                ingredientsSummary.push({
                    name: name,
                    quantityUsed: quantityUsed,
                    cost: ingredientCost
                });
            });

            // Validação para campos de precificação
            if (isNaN(markupPercentage) || markupPercentage < 0 || (isNaN(unitGrammage) && unitGrammage !== 0)) { // unitGrammage pode ser 0
                alert('Por favor, preencha o Markup e a Gramatura por Unidade corretamente. O Markup deve ser um número positivo e a Gramatura um número válido.');
                hasInvalidInput = true;
            }

            if (hasInvalidInput) {
                alert('Por favor, preencha todos os campos corretamente. Verifique se os valores numéricos são válidos e o "Peso/Quant. Emb." dos ingredientes não é zero.');
                resultsSection.style.display = 'none';
                return;
            }

            // --- Cálculos de Custo ---
            document.getElementById('result-recipe-name').innerText = recipeName;
            document.getElementById('total-ingredients-cost').innerText = `R$ ${totalCost.toFixed(2)}`;
            document.getElementById('display-loss-percentage').innerText = (lossPercentage * 100).toFixed(1);

            document.getElementById('initial-weight').innerText = `${totalWeightUsed.toFixed(2)} g/ml`;
            
            const finalWeightGramsMl = totalWeightUsed * (1 - lossPercentage);
            const finalWeightKg = finalWeightGramsMl / 1000; 
            document.getElementById('final-weight').innerText = `${finalWeightKg.toFixed(3)} kg (aprox. ${finalWeightGramsMl.toFixed(0)} g/ml)`;

            const costPerKg = finalWeightKg > 0 ? (totalCost / finalWeightKg) : 0;
            document.getElementById('cost-per-kg').innerText = `R$ ${costPerKg.toFixed(2)}`;
            document.getElementById('final-recipe-cost').innerText = `R$ ${totalCost.toFixed(2)}`;

            // Resumo dos ingredientes
            const ingredientsSummaryDiv = document.getElementById('ingredients-summary');
            ingredientsSummaryDiv.innerHTML = '';
            ingredientsSummary.forEach(ing => {
                const p = document.createElement('div');
                p.classList.add('ingredient-summary-item');
                p.innerHTML = `<span><strong>${ing.name}:</strong> ${ing.quantityUsed.toFixed(2)} g/ml/un</span><span>R$ ${ing.cost.toFixed(2)}</span>`;
                ingredientsSummaryDiv.appendChild(p);
            });

            // --- Cálculos de Venda ---
            document.getElementById('display-markup-percentage').innerText = (markupPercentage * 100).toFixed(0);

            const suggestedPricePerKg = costPerKg * (1 + markupPercentage);
            document.getElementById('suggested-price-per-kg').innerText = `R$ ${suggestedPricePerKg.toFixed(2)}`;

            let yieldUnits = 0;
            let suggestedPricePerUnit = 0;

            if (unitGrammage > 0 && finalWeightGramsMl > 0) {
                yieldUnits = Math.floor(finalWeightGramsMl / unitGrammage); // Arredonda para baixo para unidades inteiras
                suggestedPricePerUnit = (totalCost / yieldUnits) * (1 + markupPercentage);
                // Arredonda para baixo para 2 casas decimais
                suggestedPricePerUnit = Math.floor(suggestedPricePerUnit * 100) / 100; 
            } else {
                yieldUnits = 0; // Se gramatura for 0, não rende unidades
                suggestedPricePerUnit = 0;
            }

            document.getElementById('yield-units').innerText = `${yieldUnits} unidades`;
            document.getElementById('suggested-price-per-unit').innerText = `R$ ${suggestedPricePerUnit.toFixed(2)}`;


            resultsSection.style.display = 'block';
            resultsSection.scrollIntoView({ behavior: 'smooth' });
        });

        generatePdfBtn.addEventListener('click', () => {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();

            const recipeName = document.getElementById('recipe-name').value || 'Sua Receita';
            const totalIngredientsCost = document.getElementById('total-ingredients-cost').innerText;
            const displayLossPercentage = document.getElementById('display-loss-percentage').innerText;
            const initialWeight = document.getElementById('initial-weight').innerText;
            const finalWeight = document.getElementById('final-weight').innerText;
            const costPerKg = document.getElementById('cost-per-kg').innerText;
            const finalRecipeCost = document.getElementById('final-recipe-cost').innerText;
            
            const displayMarkupPercentage = document.getElementById('display-markup-percentage').innerText;
            const yieldUnits = document.getElementById('yield-units').innerText;
            const suggestedPricePerKg = document.getElementById('suggested-price-per-kg').innerText;
            const suggestedPricePerUnit = document.getElementById('suggested-price-per-unit').innerText;

            doc.setFont('helvetica', 'bold');
            doc.setFontSize(10);
            doc.text('Stecca Recheios', doc.internal.pageSize.width - 40, 15, { align: 'right' });

            doc.setFont('helvetica', 'normal');
            doc.setDrawColor(200); // Cor da linha
            doc.line(10, 20, 200, 20); // Linha superior

            doc.setFont('Playfair Display', 'bold');
            doc.setFontSize(18);
            doc.text(`Resumo Detalhado da Receita: ${recipeName}`, 105, 30, { align: 'center' });

            doc.line(10, 35, 200, 35); // Linha inferior cabeçalho

            doc.setFont('Montserrat', 'bold');
            doc.setFontSize(14);
            doc.text('Ingredientes Utilizados:', 15, 45);

            doc.setFont('Montserrat', 'normal');
            doc.setFontSize(10);
            let y = 55;
            document.querySelectorAll('#ingredients-summary .ingredient-summary-item').forEach(item => {
                const nameText = item.querySelector('span:first-child').innerText.trim();
                const costText = item.querySelector('span:last-child').innerText.trim();
                doc.text(nameText, 15, y);
                doc.text(costText, doc.internal.pageSize.width - 15, y, { align: 'right' });
                y += 7;
            });
            
            y += 10;
            doc.line(10, y, 200, y); // Linha divisória

            y += 10;
            doc.setFont('Montserrat', 'bold');
            doc.setFontSize(14);
            doc.text('Resultados de Custo:', 15, y);

            doc.setFont('Montserrat', 'normal');
            doc.setFontSize(10);
            y += 10;
            doc.text(`Custo Total dos Ingredientes: ${totalIngredientsCost}`, 15, y);
            y += 7;
            doc.text(`Porcentagem de Perda na Produção: ${displayLossPercentage}%`, 15, y);
            y += 7;
            doc.text(`Peso Inicial dos Ingredientes (total): ${initialWeight}`, 15, y);
            y += 7;
            doc.text(`Peso Final da Receita (após perda): ${finalWeight}`, 15, y);
            y += 7;
            doc.text(`Custo por Quilo (kg) da Receita: ${costPerKg}`, 15, y);
            y += 7;
            doc.text(`Custo Final da Receita Completa: ${finalRecipeCost}`, 15, y);

            y += 15;
            doc.line(10, y, 200, y); // Linha divisória
            y += 10;
            doc.setFont('Montserrat', 'bold');
            doc.setFontSize(14);
            doc.text('Resultados de Venda:', 15, y);

            doc.setFont('Montserrat', 'normal');
            doc.setFontSize(10);
            y += 10;
            doc.text(`Markup Aplicado: ${displayMarkupPercentage}%`, 15, y);
            y += 7;
            doc.text(`Rendimento em Unidades: ${yieldUnits}`, 15, y);
            y += 7;
            doc.text(`Preço de Venda Sugerido por Kg: ${suggestedPricePerKg}`, 15, y);
            y += 7;
            doc.text(`Preço de Venda Sugerido por Unidade: ${suggestedPricePerUnit}`, 15, y);


            doc.save(`${recipeName.replace(/ /g, '_')}_custo_venda.pdf`);
        });
    </script>
</body>
</html>
