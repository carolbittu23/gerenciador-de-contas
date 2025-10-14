<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerenciador de Contas - Multi-Mês Avançado</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --cor-primaria: #1A73E8; /* Azul moderno */
            --cor-secundaria: #34A853; /* Verde para Sucesso/Pagamento */
            --cor-alerta: #EA4335;    /* Vermelho para Perigo/A pagar */
            --cor-fundo: #F5F7FA;     /* Fundo extra-claro */
            --cor-sombra-suave: rgba(0, 0, 0, 0.08);
            --cor-borda-clara: #E0E4E8;
            --cor-edicao: #fff7e6;
            --cor-recorrencia: #FF9800;
        }

        body {
            font-family: 'Poppins', sans-serif;
            margin: 0;
            padding: 20px;
            background-color: var(--cor-fundo);
            color: #3C4043;
        }

        h1, h2 {
            color: #202124;
            font-weight: 600;
            padding-bottom: 0;
            border-bottom: none;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: #fff;
            padding: 30px;
            box-shadow: 0 4px 12px var(--cor-sombra-suave);
            border-radius: 12px;
        }

        /* --- Seletor de Mês/Ano --- */
        .month-selector {
            display: flex;
            align-items: center;
            gap: 20px;
            margin-bottom: 30px;
            padding: 15px 20px;
            border: 1px solid var(--cor-borda-clara);
            border-radius: 8px;
            background-color: #ffffff;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
        }

        .month-selector label {
            font-weight: 600;
            color: #202124;
            margin-right: -10px;
        }

        .month-selector select {
            padding: 10px 12px;
            border: 1px solid #ccc;
            border-radius: 6px;
            background-color: white;
            transition: border-color 0.3s;
            appearance: none;
            background-image: url('data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%22292.4%22%20height%3D%22292.4%22%3E%3Cpath%20fill%3D%22%231A73E8%22%20d%3D%22M287%20197.35L146.2%2056.6%205.4%20197.35z%22%2F%3E%3C%2Fsvg%3E');
            background-repeat: no-repeat;
            background-position: right 10px center;
            background-size: 10px;
        }
        
        /* --- Abas de Navegação --- */
        .tabs {
            display: flex;
            margin-bottom: 30px;
            border-bottom: 2px solid var(--cor-borda-clara);
        }

        .tab-button {
            padding: 12px 20px;
            cursor: pointer;
            border: none;
            background-color: transparent;
            font-weight: 500;
            color: #616161;
            transition: all 0.3s ease;
            position: relative;
        }

        .tab-button.active {
            color: var(--cor-primaria);
            font-weight: 600;
        }

        .tab-button.active::after {
            content: '';
            position: absolute;
            bottom: -2px;
            left: 0;
            width: 100%;
            height: 3px;
            background-color: var(--cor-primaria);
            border-radius: 2px 2px 0 0;
        }

        /* --- VISUAL NOVO: Formulário de Cadastro --- */
        .card-form {
            background-color: #ffffff;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
            border: 1px solid var(--cor-borda-clara);
        }

        #cadastro-form {
            display: grid;
            grid-template-columns: 1fr 1fr; 
            gap: 25px;
        }

        #cadastro-form button[type="submit"] {
            grid-column: 1 / -1; 
            padding: 14px;
            background-color: var(--cor-primaria); 
            font-weight: 600;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            margin-top: 15px;
            transition: background-color 0.3s, transform 0.1s;
            font-size: 1.1em;
        }

        #cadastro-form button[type="submit"]:hover {
            background-color: #155bbd;
            transform: translateY(-1px);
        }

        #cadastro-form label {
            display: block;
            margin-bottom: 8px;
            font-size: 0.9em;
            color: #495057;
            font-weight: 600;
        }

        #cadastro-form input, #cadastro-form select {
            width: 100%;
            padding: 12px;
            border: 1px solid #d4d4d4;
            border-radius: 8px;
            box-sizing: border-box;
            background-color: #fcfcfc;
            transition: border-color 0.3s, box-shadow 0.3s;
        }
        
        #cadastro-form input:focus, #cadastro-form select:focus {
            border-color: var(--cor-primaria);
            box-shadow: 0 0 0 3px rgba(26, 115, 232, 0.2);
            background-color: white;
            outline: none;
        }
        
        /* --- Filtros e Totalizadores --- */
        .filter-section {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
            margin-bottom: 20px;
            padding: 15px 0;
            border-bottom: 1px solid var(--cor-borda-clara);
        }
        .filter-section label {
            font-size: 0.9em;
            font-weight: 500;
            display: block;
            margin-bottom: 5px;
        }
        .filter-section select {
            width: 100%;
            padding: 8px;
            border: 1px solid #ccc;
            border-radius: 6px;
        }

        .summary-bar {
            display: flex;
            justify-content: space-around;
            background-color: #fff;
            padding: 10px;
            border: 1px solid var(--cor-borda-clara);
            border-radius: 8px;
            margin-bottom: 20px;
        }
        .summary-item {
            text-align: center;
            padding: 0 10px;
            font-size: 0.9em;
        }
        .summary-item strong {
            display: block;
            font-size: 1.2em;
            font-weight: 700;
        }
        .summary-item #total-filtrado { color: var(--cor-primaria); }
        .summary-item #pago-filtrado { color: var(--cor-secundaria); }
        .summary-item #apagar-filtrado { color: var(--cor-alerta); }
        
        /* --- Tabela de Contas --- */
        #tabela-contas {
            width: 100%;
            border-collapse: collapse;
        }

        #tabela-contas th, #tabela-contas td {
            padding: 12px 10px;
            text-align: left;
            border-bottom: 1px solid #f0f0f0;
            vertical-align: middle;
        }

        #tabela-contas th {
            background-color: var(--cor-fundo);
            color: #495057;
            font-weight: 600;
            font-size: 0.9em;
            text-transform: uppercase;
        }

        /* Status Quick Toggle e Botões de Ação */
        .action-cell {
            white-space: nowrap; 
            min-width: 250px;
        }
        .quick-status-cell {
            min-width: 120px;
            text-align: center;
        }
        .quick-status-btn {
            background: var(--cor-secundaria);
            color: white;
            border: none;
            padding: 6px 10px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 0.8em;
            font-weight: 600;
            transition: background 0.3s;
            width: 100%;
        }
        .quick-status-btn.pago { background-color: var(--cor-secundaria); }
        .quick-status-btn.apagar { background-color: var(--cor-alerta); }

        .edit-btn, .delete-btn, .duplicate-btn, .recorrencia-btn {
            background: #6c757d; 
            color: white;
            border: none;
            padding: 6px 8px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 0.8em;
            font-weight: 600;
            transition: background 0.3s;
            margin-right: 4px; 
        }
        .edit-btn { background: var(--cor-primaria); }
        .duplicate-btn { background: #6f42c1; } 
        .recorrencia-btn { background: var(--cor-recorrencia); } 
        .delete-btn { background: var(--cor-alerta); }
        
        .status-apagar {
            color: var(--cor-alerta);
            font-weight: 600;
            background-color: #feebe8;
            padding: 4px 8px;
            border-radius: 4px;
            display: inline-block;
        }

        .status-pago {
            color: var(--cor-secundaria);
            font-weight: 600;
            background-color: #e7f4e9;
            padding: 4px 8px;
            border-radius: 4px;
            display: inline-block;
        }

        /* --- VISUAL NOVO: Relatórios --- */
        
        .report-section {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 20px;
            margin-bottom: 40px;
        }

        .report-card {
            background-color: #ffffff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            border-left: 5px solid; 
            transition: transform 0.3s;
        }

        .report-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
        }

        .report-card h3 {
            margin-top: 0;
            font-size: 0.9em;
            color: #616161;
            font-weight: 500;
        }

        .report-card p {
            font-size: 1.8em;
            font-weight: 700;
            margin: 5px 0 0;
        }
        
        .report-card.total { border-left-color: #202124; }
        .report-card.pago { border-left-color: var(--cor-secundaria); }
        .report-card.apagar { border-left-color: var(--cor-alerta); }
        .report-card.contas { border-left-color: var(--cor-primaria); }

        .report-card.pago p { color: var(--cor-secundaria); }
        .report-card.apagar p { color: var(--cor-alerta); }
        
        /* Seções de Detalhe (Categoria/Cartão) */

        .detail-report h3 {
            border-bottom: 2px solid var(--cor-borda-clara);
            padding-bottom: 10px;
            margin-top: 30px;
        }
        
        .detail-report ul {
            list-style: none;
            padding: 0;
        }

        .detail-report li {
            padding: 10px 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px dotted #e0e0e0;
        }

        .category-name, .card-name {
            font-weight: 500;
            flex-basis: 30%;
        }
        
        .category-value, .card-value {
            font-weight: 600;
            color: #202124;
            min-width: 100px;
            text-align: right;
        }
        
        /* Simulação de Barra de Progresso/Gráfico */
        .progress-bar {
            flex-grow: 1;
            height: 10px;
            margin: 0 15px;
            background-color: var(--cor-fundo);
            border-radius: 5px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background-color: var(--cor-primaria);
            border-radius: 5px;
        }
        
        /* --- Modal (Duplicar Mês) --- */
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.4);
        }

        .modal-content {
            background-color: #fefefe;
            margin: 15% auto; 
            padding: 20px;
            border: 1px solid #888;
            width: 80%; 
            max-width: 400px;
            border-radius: 10px;
            box-shadow: 0 4px 12px var(--cor-sombra-suave);
        }

        .close-btn {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
        }

        .close-btn:hover,
        .close-btn:focus {
            color: #000;
            text-decoration: none;
            cursor: pointer;
        }

        .modal-content label {
            display: block;
            margin-top: 10px;
            font-weight: 500;
        }
        .modal-content select, .modal-content input[type="number"] {
            width: 100%;
            padding: 8px;
            margin-top: 5px;
            border: 1px solid #ccc;
            border-radius: 6px;
            box-sizing: border-box;
        }
        .modal-content button {
            margin-top: 20px;
            padding: 10px 15px;
            background-color: var(--cor-recorrencia);
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            width: 100%;
        }

    </style>
</head>
<body>

<div class="container">
    <h1>Gerenciador Financeiro Pessoal</h1>

    <div class="month-selector">
        <label for="mes-selector">Mês de Referência:</label>
        <select id="mes-selector" onchange="mudarMesReferencia()">
            <option value="01">Janeiro</option>
            <option value="02">Fevereiro</option>
            <option value="03">Março</option>
            <option value="04">Abril</option>
            <option value="05">Maio</option>
            <option value="06">Junho</option>
            <option value="07">Julho</option>
            <option value="08">Agosto</option>
            <option value="09">Setembro</option>
            <option value="10" selected>Outubro</option>
            <option value="11">Novembro</option>
            <option value="12">Dezembro</option>
        </select>
        <label for="ano-selector">Ano:</label>
        <select id="ano-selector" onchange="mudarMesReferencia()">
            <option value="2024">2024</option>
            <option value="2025" selected>2025</option>
            <option value="2026">2026</option>
        </select>
    </div>

    <div class="tabs">
        <button class="tab-button active" onclick="openTab(event, 'visualizacao')">Contas</button>
        <button class="tab-button" onclick="openTab(event, 'cadastro')">Novo Lançamento</button>
        <button class="tab-button" onclick="openTab(event, 'relatorios')">Resumo/Relatórios</button>
        <button class="tab-button" onclick="exportToCSV()">Exportar (CSV)</button>
    </div>

    <div id="visualizacao" class="tab-content active">
        <h2 id="titulo-visualizacao">Contas de Outubro de 2025</h2>
        
        <div class="filter-section">
            <div>
                <label for="filter-status">Filtrar por Status:</label>
                <select id="filter-status" onchange="aplicarFiltros()">
                    <option value="todos">Todos os Status</option>
                    <option value="A pagar">A pagar</option>
                    <option value="Pago">Pago</option>
                </select>
            </div>
            <div>
                <label for="filter-cat">Filtrar por Categoria:</label>
                <select id="filter-cat" onchange="aplicarFiltros()">
                    <option value="todos">Todas as Categorias</option>
                </select>
            </div>
            <div>
                <label for="filter-card">Filtrar por Cartão:</label>
                <select id="filter-card" onchange="aplicarFiltros()">
                    <option value="todos">Todos os Cartões</option>
                </select>
            </div>
        </div>

        <div class="summary-bar">
            <div class="summary-item">Total Filtrado: <strong id="total-filtrado">R$ 0,00</strong></div>
            <div class="summary-item">Pago Filtrado: <strong id="pago-filtrado">R$ 0,00</strong></div>
            <div class="summary-item">A Pagar Filtrado: <strong id="apagar-filtrado">R$ 0,00</strong></div>
        </div>

        <div class="table-container">
            <table id="tabela-contas">
                <thead>
                    <tr>
                        <th>Status Rápido</th>
                        <th>Descrição Despesa</th>
                        <th>Categoria</th>
                        <th>Parcelas</th>
                        <th>Valor</th>
                        <th>Cartão</th>
                        <th>Status</th>
                        <th>Ação</th>
                    </tr>
                </thead>
                <tbody></tbody>
            </table>
        </div>
    </div>

    <div id="cadastro" class="tab-content">
        <h2>Cadastrar Novo Lançamento</h2>
        
        <div class="card-form">
            <form id="cadastro-form">
                <div>
                    <label for="descricao">Descrição Despesa:</label>
                    <input type="text" id="descricao" required>
                </div>
                <div>
                    <label for="valor">Valor:</label>
                    <input type="number" id="valor" step="0.01" required>
                </div>
                
                <div>
                    <label for="categoria">Categoria:</label>
                    <input type="text" id="categoria" required>
                </div>
                <div>
                    <label for="cartao">Cartão:</label>
                    <input type="text" id="cartao" placeholder="Ex: Cartão Nubank" required>
                </div>
                
                <div>
                    <label for="parcelas">Parcelas (Ex: 1/5 ou Fixo):</label>
                    <input type="text" id="parcelas" placeholder="Ex: 1/5 ou Fixo" value="1/1">
                </div>
                <div>
                    <label for="status">Status Inicial:</label>
                    <select id="status" required>
                        <option value="A pagar">A pagar</option>
                        <option value="Pago">Pago</option>
                    </select>
                </div>
                
                <button type="submit">CADASTRAR NOVA CONTA</button>
            </form>
        </div>
    </div>

    <div id="relatorios" class="tab-content">
        <h2 id="titulo-relatorio">Resumo Mensal de Outubro de 2025</h2>
        
        <div class="report-section">
            <div class="report-card total">
                <h3>Total de Despesas</h3>
                <p id="total-despesas">R$ 0,00</p>
            </div>
            <div class="report-card pago">
                <h3>Total Pago</h3>
                <p id="total-pago">R$ 0,00</p>
            </div>
            <div class="report-card apagar">
                <h3>Total A Pagar</h3>
                <p id="total-a-pagar">R$ 0,00</p>
            </div>
            <div class="report-card contas">
                <h3>Contas Cadastradas</h3>
                <p id="total-contas">0</p>
            </div>
        </div>
        
        <div class="detail-report">
            <h3>Distribuição por Categoria</h3>
            <ul id="resumo-por-categoria"></ul>
        </div>

        <div class="detail-report">
            <h3>Distribuição por Cartão/Meio de Pagamento</h3>
            <ul id="resumo-por-cartao"></ul>
        </div>
    </div>
</div>

<div id="duplicarModal" class="modal">
    <div class="modal-content">
        <span class="close-btn" onclick="document.getElementById('duplicarModal').style.display='none'">&times;</span>
        <h2>Duplicar Despesa para Outros Meses</h2>
        <p id="modal-descricao-conta">Conta: </p>
        <form id="form-duplicar-mes">
            <label for="dest-mes-selector">Mês/Ano de Destino:</label>
            <select id="dest-mes-selector" required>
                </select>
            <label for="parcelas-avancar">Quantidade de meses para avançar:</label>
            <input type="number" id="parcelas-avancar" value="1" min="1" required>
            <button type="submit">Duplicar Lançamento(s)</button>
        </form>
    </div>
</div>

<script>
    // --- Variáveis Globais ---
    let contas = [];
    let nextId = 1;
    let mesAtual = '10'; 
    let anoAtual = '2025';
    let linhaEmEdicao = null; 
    let contaParaDuplicar = null; 
    
    const meses = [
        "Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho",
        "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"
    ];

    // Dados de exemplo (Outubro 2025)
    const DADOS_OUTUBRO_2025 = [
        { desc: "Anuidade caixa", cat: "Tarifas/Serviços", parc: "Fixo", valor: 10.50, cartao: "Cartão Caixa", status: "A pagar" },
        { desc: "Apple (Caixa)", cat: "Assinaturas", parc: "Fixo", valor: 10.90, cartao: "Cartão Caixa", status: "A pagar" },
        { desc: "Amazon (Caixa)", cat: "Compras Online", parc: "Fixo", valor: 59.80, cartao: "Cartão Caixa", status: "A pagar" },
        { desc: "Netflix (Caixa)", cat: "Lazer/Entretenimento", parc: "Fixo", valor: 22.50, cartao: "Cartão Caixa", status: "Pago" },
        { desc: "Academia (Caixa)", cat: "Saúde/Esportes", parc: "Fixo", valor: 59.90, cartao: "Cartão Caixa", status: "A pagar" },
        { desc: "Terreno", cat: "Investimento/Imóvel", parc: "25/200", valor: 615.00, cartao: "Sem cartão", status: "A pagar" },
        { desc: "Facilpa Ze neto", cat: "Serviços", parc: "6/6", valor: 71.15, cartao: "Cartão C6", status: "Pago" },
        { desc: "Mariella", cat: "Vestuário", parc: "1/5", valor: 90.03, cartao: "Cartão Nubank", status: "A pagar" },
        { desc: "Jorge", cat: "Vestuário", parc: "3/5", valor: 526.52, cartao: "Cartão Nubank", status: "A pagar" },
        { desc: "Pneu (Nubank)", cat: "Veículos", parc: "3/8", valor: 60.00, cartao: "Cartão Nubank", status: "A pagar" },
        { desc: "Guarda roupa", cat: "Móveis/Decoração", parc: "6/10", valor: 250.00, cartao: "Mercado Pago", status: "A pagar" },
        { desc: "Colchão", cat: "Móveis/Decoração", parc: "2/6", valor: 928.00, cartao: "Cartão Inter", status: "A pagar" },
        { desc: "Yuli (Inter)", cat: "Vestuário", parc: "3/5", valor: 276.20, cartao: "Cartão Inter", status: "Pago" },
        { desc: "Seguro carro", cat: "Veículos", parc: "2/6", valor: 157.00, cartao: "Cartão Caixa", status: "A pagar" },
        { desc: "Panela", cat: "Móveis/Decoração", parc: "1/10", valor: 36.50, cartao: "Mercado Pago", status: "A pagar" },
        { desc: "Pix verdura", cat: "Alimentação/Mercado", parc: "", valor: 51.00, cartao: "Sem cartão", status: "Pago" },
        { desc: "Parcelamento c6", cat: "Dívidas/Empréstimos", parc: "4/12", valor: 947.71, cartao: "Cartão C6", status: "A pagar" },
        { desc: "Gympass (Andre)", cat: "Saúde/Esportes", parc: "", valor: 89.90, cartao: "Cartão C6", status: "A pagar" },
        { desc: "Mes passado", cat: "Dívidas/Empréstimos", parc: "", valor: 2778.00, cartao: "Sem cartão", status: "A pagar" },
    ];


    // --- Funções de Utilitário ---
    function getStorageKey(mes = mesAtual, ano = anoAtual) { return `contas_${mes}_${ano}`; }
    function getMonthName(monthNumber) {
        const index = parseInt(monthNumber) - 1;
        return meses[index];
    }
    function formatarMoeda(valor) {
        return parseFloat(valor || 0).toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
    }
    function getNextId() {
        let maxId = 0;
        contas.forEach(conta => {
            if (conta.id > maxId) maxId = conta.id;
        });
        nextId = maxId + 1;
        return nextId;
    }

    // --- Funções Principais (Load/Save/Render) ---

    function carregarContas() {
        fecharEdicao(); 
        const STORAGE_KEY = getStorageKey();
        const storedContas = localStorage.getItem(STORAGE_KEY);
        const mes = parseInt(mesAtual);

        if (storedContas) {
            contas = JSON.parse(storedContas);
        } else if (mes === 10 && anoAtual === '2025') {
            contas = DADOS_OUTUBRO_2025.map((item, index) => ({...item, id: index + 1}));
            salvarContas(contas);
        } else {
            contas = [];
        }
        
        getNextId();
        popularFiltros(); 
        aplicarFiltros(); 
        atualizarRelatorios(contas);
        atualizarTitulos();
    }

    function salvarContas(contasArray) {
        const STORAGE_KEY = getStorageKey();
        localStorage.setItem(STORAGE_KEY, JSON.stringify(contasArray));
    }
    
    function popularFiltros() {
        const cats = [...new Set(contas.map(c => c.cat))].sort();
        const cards = [...new Set(contas.map(c => c.cartao))].sort();
        
        const filterCat = document.getElementById('filter-cat');
        const filterCard = document.getElementById('filter-card');

        const currentCat = filterCat.value;
        const currentCard = filterCard.value;
        
        filterCat.innerHTML = '<option value="todos">Todas as Categorias</option>';
        filterCard.innerHTML = '<option value="todos">Todos os Cartões</option>';

        cats.forEach(cat => { filterCat.innerHTML += `<option value="${cat}">${cat}</option>`; });
        cards.forEach(card => { filterCard.innerHTML += `<option value="${card}">${card}</option>`; });

        filterCat.value = currentCat;
        filterCard.value = currentCard;

        popularModalDuplicar();
    }

    function aplicarFiltros() {
        const statusFilter = document.getElementById('filter-status').value;
        const catFilter = document.getElementById('filter-cat').value;
        const cardFilter = document.getElementById('filter-card').value;

        const contasFiltradas = contas.filter(conta => {
            const statusMatch = statusFilter === 'todos' || conta.status === statusFilter;
            const catMatch = catFilter === 'todos' || conta.cat === catFilter;
            const cardMatch = cardFilter === 'todos' || conta.cartao === cardFilter;
            return statusMatch && catMatch && cardMatch;
        });

        renderizarTabela(contasFiltradas);
        atualizarTotalizadorFiltrado(contasFiltradas);
    }

    function atualizarTotalizadorFiltrado(contasFiltradas) {
        let total = 0;
        let pago = 0;
        let apagar = 0;

        contasFiltradas.forEach(c => {
            total += c.valor;
            if (c.status === 'Pago') {
                pago += c.valor;
            } else {
                apagar += c.valor;
            }
        });

        document.getElementById('total-filtrado').textContent = formatarMoeda(total);
        document.getElementById('pago-filtrado').textContent = formatarMoeda(pago);
        document.getElementById('apagar-filtrado').textContent = formatarMoeda(apagar);
    }


    function renderizarTabela(contasArray) {
        const tbody = document.querySelector('#tabela-contas tbody');
        tbody.innerHTML = '';
        
        contasArray.forEach(conta => {
            const row = tbody.insertRow();
            row.setAttribute('data-id', conta.id);
            
            // Coluna: Status Rápido (0)
            const quickStatusCell = row.insertCell();
            quickStatusCell.classList.add('quick-status-cell');
            const statusBtn = document.createElement('button');
            const newStatus = conta.status === 'Pago' ? 'A pagar' : 'Pago';
            statusBtn.textContent = conta.status === 'Pago' ? 'Estornar' : 'Pagar';
            statusBtn.classList.add('quick-status-btn', conta.status === 'Pago' ? 'apagar' : 'pago');
            statusBtn.onclick = (e) => { e.stopPropagation(); toggleStatus(conta.id, newStatus); };
            quickStatusCell.appendChild(statusBtn);

            // Outras Colunas (Desc(1), Cat(2), Parc(3), Valor(4), Cartão(5))
            row.insertCell().textContent = conta.desc;
            row.insertCell().textContent = conta.cat;
            row.insertCell().textContent = conta.parc;
            row.insertCell().textContent = formatarMoeda(conta.valor);
            row.insertCell().textContent = conta.cartao;
            
            // Coluna: Status (Texto) (6)
            const statusCell = row.insertCell();
            statusCell.textContent = conta.status;
            statusCell.classList.add(conta.status === 'A pagar' ? 'status-apagar' : 'status-pago');

            // Coluna: Ação (Botões) (7)
            const actionCell = row.insertCell();
            actionCell.classList.add('action-cell');
            
            const editBtn = document.createElement('button');
            editBtn.textContent = 'Editar';
            editBtn.classList.add('edit-btn');
            editBtn.onclick = (e) => { e.stopPropagation(); iniciarEdicao(e, conta.id); };

            const duplicateBtn = document.createElement('button');
            duplicateBtn.textContent = 'Duplicar';
            duplicateBtn.classList.add('duplicate-btn');
            duplicateBtn.onclick = (e) => { e.stopPropagation(); duplicarConta(conta.id); };

            const recorrenciaBtn = document.createElement('button');
            recorrenciaBtn.textContent = 'Duplicar Mês';
            recorrenciaBtn.classList.add('recorrencia-btn');
            recorrenciaBtn.onclick = (e) => { e.stopPropagation(); abrirModalRecorrencia(conta.id); };
            
            const deleteBtn = document.createElement('button');
            deleteBtn.textContent = 'Excluir';
            deleteBtn.classList.add('delete-btn');
            deleteBtn.onclick = (e) => { e.stopPropagation(); excluirConta(conta.id); };
            
            actionCell.appendChild(editBtn);
            actionCell.appendChild(duplicateBtn);
            actionCell.appendChild(recorrenciaBtn);
            actionCell.appendChild(deleteBtn);
        });
    }

    function mudarMesReferencia() {
        mesAtual = document.getElementById('mes-selector').value;
        anoAtual = document.getElementById('ano-selector').value;
        carregarContas();
        openTab(null, 'visualizacao');
    }
    
    function atualizarTitulos() {
        const nomeMes = getMonthName(mesAtual);
        document.getElementById('titulo-visualizacao').textContent = `Contas de ${nomeMes} de ${anoAtual}`;
        document.getElementById('titulo-relatorio').textContent = `Resumo Mensal de ${nomeMes} de ${anoAtual}`;
    }
    
    function toggleStatus(id, newStatus) {
        const contaIndex = contas.findIndex(c => c.id === id);
        if (contaIndex !== -1) {
            contas[contaIndex].status = newStatus;
            salvarContas(contas);
            aplicarFiltros(); 
            atualizarRelatorios(contas);
        }
    }

    function duplicarConta(id) {
        const contaOriginal = contas.find(c => c.id === id);
        if (!contaOriginal) return;

        const novaConta = { ...contaOriginal };
        novaConta.id = getNextId();
        novaConta.desc = `(Cópia) ${novaConta.desc}`; 
        
        contas.push(novaConta);
        salvarContas(contas);
        aplicarFiltros();
        atualizarRelatorios(contas);
        alert(`Conta duplicada no mês atual como: ${novaConta.desc}`);
    }


    // --- Funções de Duplicação Recorrente (Mês/Ano) ---

    function popularModalDuplicar() {
        const mesSelector = document.getElementById('dest-mes-selector');
        mesSelector.innerHTML = '';

        const currentMonthIndex = parseInt(mesAtual) - 1;
        const currentYear = parseInt(anoAtual);

        // Popula Meses e Anos (próximos 24 meses)
        for (let i = 1; i <= 24; i++) {
            let targetMonthIndex = (currentMonthIndex + i) % 12;
            let targetYear = currentYear + Math.floor((currentMonthIndex + i) / 12);
            
            const monthValue = String(targetMonthIndex + 1).padStart(2, '0');
            const monthName = meses[targetMonthIndex];
            
            // Opção de Mês/Ano (Simplificando para um único select como solicitado)
            let mesOption = document.createElement('option');
            // Formato valor: Mês_Ano (Ex: 11_2025)
            mesOption.value = `${monthValue}_${targetYear}`;
            mesOption.textContent = `${monthName} de ${targetYear}`;
            mesSelector.appendChild(mesOption);
        }

        // Seleciona o primeiro mês/ano futuro como padrão
        if (mesSelector.options.length > 0) mesSelector.selectedIndex = 0;
    }

    function abrirModalRecorrencia(id) {
        contaParaDuplicar = contas.find(c => c.id === id);
        if (!contaParaDuplicar) return;

        document.getElementById('modal-descricao-conta').textContent = `Conta: ${contaParaDuplicar.desc} (Valor: ${formatarMoeda(contaParaDuplicar.valor)})`;
        popularModalDuplicar(); // Garante que o seletor esteja atualizado
        document.getElementById('duplicarModal').style.display = 'block';
    }

    document.getElementById('form-duplicar-mes').addEventListener('submit', function(e) {
        e.preventDefault();
        
        const numParcelas = parseInt(document.getElementById('parcelas-avancar').value);
        
        if (!contaParaDuplicar || numParcelas < 1) return;

        let baseParcNum = 0;
        let baseParcTotal = 0;
        
        const match = contaParaDuplicar.parc.match(/(\d+)\/(\d+)/);
        if (match) {
            baseParcNum = parseInt(match[1]);
            baseParcTotal = parseInt(match[2]);
        }
        
        const contasDestino = [];
        let [currentMonthNum, currentYearNum] = [parseInt(mesAtual), parseInt(anoAtual)];
        
        for (let i = 0; i < numParcelas; i++) {
            // Calcula o próximo mês
            let targetMonthNum = (currentMonthNum + i);
            let targetYearNum = currentYearNum + Math.floor((targetMonthNum - 1) / 12);
            targetMonthNum = (targetMonthNum - 1) % 12 + 1;

            const targetMonthStr = String(targetMonthNum).padStart(2, '0');
            const targetYearStr = String(targetYearNum);

            // O primeiro destino é o próximo mês (i=0, avanço de 1 mês)
            if (i >= 0) { 
                const novaConta = { ...contaParaDuplicar };
                novaConta.id = null; 
                novaConta.status = 'A pagar'; 
                
                let novaParc = contaParaDuplicar.parc;
                if (baseParcNum > 0 && baseParcTotal > 0) {
                    const nextParcNum = baseParcNum + i + 1; 
                    if (nextParcNum <= baseParcTotal) {
                        novaParc = `${nextParcNum}/${baseParcTotal}`;
                    } else { 
                         novaParc = `Fixo (Encerrado em ${baseParcTotal}/${baseParcTotal})`;
                         novaConta.desc = `(Encerrada) ${novaConta.desc}`; 
                    }
                }
                novaConta.parc = novaParc;

                salvarNovaContaRecorrente(novaConta, targetMonthStr, targetYearStr);

                contasDestino.push(`${getMonthName(targetMonthStr)}/${targetYearStr} (${novaParc})`);
            }
        }
        
        document.getElementById('duplicarModal').style.display = 'none';
        alert(`Despesa duplicada com sucesso para ${numParcelas} mês(es) seguintes: \n${contasDestino.join('\n')}`);
    });

    function salvarNovaContaRecorrente(novaConta, mesDestino, anoDestino) {
        const storageKeyDestino = getStorageKey(mesDestino, anoDestino);
        let contasDestino = JSON.parse(localStorage.getItem(storageKeyDestino)) || [];
        
        let maxIdDestino = 0;
        contasDestino.forEach(c => { if (c.id > maxIdDestino) maxIdDestino = c.id; });
        novaConta.id = maxIdDestino + 1;

        contasDestino.push(novaConta);
        localStorage.setItem(storageKeyDestino, JSON.stringify(contasDestino));
    }


    // --- Funções de Edição Inline ---
    
    function fecharEdicao() {
        if (linhaEmEdicao) {
            renderizarTabela(contas); 
            linhaEmEdicao = null;
            aplicarFiltros(); 
        }
    }

    function iniciarEdicao(event, id) {
        fecharEdicao(); 

        const row = document.querySelector(`tr[data-id="${id}"]`); 
        linhaEmEdicao = row;
        linhaEmEdicao.classList.add('editing');

        const conta = contas.find(c => c.id === id);
        if (!conta) return;

        const cells = row.cells;
        
        const camposEditaveis = [
            { index: 1, key: 'desc', type: 'text' },
            { index: 2, key: 'cat', type: 'text' },
            { index: 3, key: 'parc', type: 'text' },
            { index: 4, key: 'valor', type: 'number' },
            { index: 5, key: 'cartao', type: 'text' },
            { index: 6, key: 'status', type: 'select', options: ['A pagar', 'Pago'] }
        ];

        camposEditaveis.forEach(({ index, key, type, options }) => {
            const cell = cells[index];

            if (type === 'select') {
                const select = document.createElement('select');
                options.forEach(optionText => {
                    const option = document.createElement('option');
                    option.value = optionText;
                    option.textContent = optionText;
                    if (optionText === conta.status) {
                        option.selected = true;
                    }
                    select.appendChild(option);
                });
                cell.innerHTML = '';
                cell.appendChild(select);
            } else {
                const input = document.createElement('input');
                input.type = type;
                input.value = key === 'valor' ? parseFloat(conta.valor).toFixed(2) : conta[key];
                
                if (key === 'valor') {
                    input.step = "0.01";
                }

                cell.innerHTML = '';
                cell.appendChild(input);
            }
        });

        cells[0].innerHTML = ''; 
        const actionCell = cells[7];
        actionCell.innerHTML = ''; 

        const saveBtn = document.createElement('button');
        saveBtn.textContent = 'Salvar';
        saveBtn.classList.add('save-btn');
        saveBtn.onclick = (e) => { e.stopPropagation(); salvarEdicao(id, cells); };

        const cancelBtn = document.createElement('button');
        cancelBtn.textContent = 'Cancelar';
        cancelBtn.classList.add('cancel-btn');
        cancelBtn.onclick = (e) => { e.stopPropagation(); fecharEdicao(); };

        actionCell.appendChild(saveBtn);
        actionCell.appendChild(cancelBtn);
    }

    function salvarEdicao(id, cells) {
        const contaIndex = contas.findIndex(c => c.id === id);
        if (contaIndex === -1) return;

        const novaConta = { ...contas[contaIndex] };

        novaConta.desc = cells[1].querySelector('input').value;
        novaConta.cat = cells[2].querySelector('input').value;
        novaConta.parc = cells[3].querySelector('input').value;
        novaConta.valor = parseFloat(cells[4].querySelector('input').value);
        novaConta.cartao = cells[5].querySelector('input').value;
        novaConta.status = cells[6].querySelector('select').value;

        if (isNaN(novaConta.valor) || novaConta.valor <= 0) {
            alert('Valor inválido. Insira um número maior que zero.');
            return;
        }

        contas[contaIndex] = novaConta;
        salvarContas(contas);
        fecharEdicao(); 
        aplicarFiltros(); 
        atualizarRelatorios(contas);
        popularFiltros(); 
    }
    
    // --- Funções CRUD e Relatórios ---

    function excluirConta(id) {
        if (confirm('Tem certeza que deseja excluir esta conta?')) {
            contas = contas.filter(conta => conta.id !== id);
            salvarContas(contas);
            aplicarFiltros();
            atualizarRelatorios(contas);
            popularFiltros();
        }
    }

    document.getElementById('cadastro-form').addEventListener('submit', function(e) {
        e.preventDefault();

        const novaConta = {
            id: getNextId(),
            desc: document.getElementById('descricao').value,
            cat: document.getElementById('categoria').value,
            parc: document.getElementById('parcelas').value,
            valor: parseFloat(document.getElementById('valor').value),
            cartao: document.getElementById('cartao').value,
            status: document.getElementById('status').value
        };

        contas.push(novaConta);
        salvarContas(contas);
        
        popularFiltros(); 
        aplicarFiltros();
        atualizarRelatorios(contas);
        openTab(e, 'visualizacao');

        e.target.reset();
        document.getElementById('parcelas').value = '1/1';
    });


    // --- FUNÇÃO ATUALIZADA: Renderização dos Relatórios ---
    function atualizarRelatorios(contasArray) {
        let totalDespesas = 0;
        let totalPago = 0;
        let totalAPagar = 0;
        const resumoPorCartao = {};
        const resumoPorCategoria = {};

        contasArray.forEach(conta => {
            totalDespesas += conta.valor;

            if (conta.status === 'Pago') {
                totalPago += conta.valor;
            } else {
                totalAPagar += conta.valor;
            }

            if (!resumoPorCartao[conta.cartao]) { resumoPorCartao[conta.cartao] = 0; }
            resumoPorCartao[conta.cartao] += conta.valor;

            if (!resumoPorCategoria[conta.cat]) { resumoPorCategoria[conta.cat] = 0; }
            resumoPorCategoria[conta.cat] += conta.valor;
        });

        document.getElementById('total-despesas').textContent = formatarMoeda(totalDespesas);
        document.getElementById('total-pago').textContent = formatarMoeda(totalPago);
        document.getElementById('total-a-pagar').textContent = formatarMoeda(totalAPagar);
        document.getElementById('total-contas').textContent = contasArray.length;

        const renderResumo = (ulId, resumo, totalGeral) => {
            const ul = document.getElementById(ulId);
            ul.innerHTML = '';
            const ordenado = Object.keys(resumo).sort((a, b) => resumo[b] - resumo[a]);
            const maiorValor = ordenado.length > 0 ? resumo[ordenado[0]] : 0;
            
            ordenado.forEach(key => {
                const valor = resumo[key];
                const percentual = maiorValor > 0 ? (valor / maiorValor) * 100 : 0;
                
                const li = document.createElement('li');
                li.innerHTML = `
                    <span class="${ulId.includes('categoria') ? 'category-name' : 'card-name'}">${key}</span>
                    <div class="progress-bar">
                        <div class="progress-fill" style="width: ${percentual.toFixed(2)}%;"></div>
                    </div>
                    <span class="${ulId.includes('categoria') ? 'category-value' : 'card-value'}">${formatarMoeda(valor)}</span>
                `;
                ul.appendChild(li);
            });
        };
        
        renderResumo('resumo-por-categoria', resumoPorCategoria, totalDespesas);
        renderResumo('resumo-por-cartao', resumoPorCartao, totalDespesas);
    }

    // --- Funções Auxiliares (Aba, CSV, Inicialização) ---

    function openTab(evt, tabName) {
        fecharEdicao();

        const tabContents = document.getElementsByClassName("tab-content");
        for (let i = 0; i < tabContents.length; i++) {
            tabContents[i].style.display = "none";
            tabContents[i].classList.remove("active");
        }

        const tabButtons = document.getElementsByClassName("tab-button");
        for (let i = 0; i < tabButtons.length; i++) {
            tabButtons[i].classList.remove("active");
        }

        document.getElementById(tabName).style.display = "block";
        document.getElementById(tabName).classList.add("active");
        
        const targetButton = evt && evt.currentTarget ? evt.currentTarget : document.querySelector(`.tabs button[onclick*='${tabName}']`);
        if (targetButton) { targetButton.classList.add("active"); }

        if (tabName === 'visualizacao') {
            aplicarFiltros(); 
        } else if (tabName === 'relatorios') {
            atualizarRelatorios(contas);
        }
    }
    
    function convertToCSV(data) {
        const header = ["ID", "Descricao", "Categoria", "Parcelas", "Valor", "Cartao", "Status"];
        const rows = data.map(conta => [
            conta.id,
            conta.desc.replace(/;/g, '').replace(/,/g, ''),
            conta.cat,
            conta.parc,
            conta.valor.toFixed(2).replace('.', ','),
            conta.cartao,
            conta.status
        ]);
        
        let csvContent = header.join(";") + "\n";
        rows.forEach(row => {
            csvContent += row.join(";") + "\n";
        });
        return csvContent;
    }

    function exportToCSV() {
        const csv = convertToCSV(contas);
        const nomeMes = getMonthName(mesAtual);
        const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
        const url = URL.createObjectURL(blob);
        const link = document.createElement("a");
        link.setAttribute("href", url);
        link.setAttribute("download", `contas_${nomeMes}_${anoAtual}.csv`);
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
    }

    window.onload = function() {
        const today = new Date();
        const currentMonth = String(today.getMonth() + 1).padStart(2, '0');
        const currentYear = String(today.getFullYear());

        const mesSelector = document.getElementById('mes-selector');
        const anoSelector = document.getElementById('ano-selector');

        for (let y = today.getFullYear() - 2; y <= today.getFullYear() + 5; y++) {
             const option = document.createElement('option');
             option.value = String(y);
             option.textContent = String(y);
             anoSelector.appendChild(option);
        }

        if (mesSelector) mesSelector.value = currentMonth;
        if (anoSelector) anoSelector.value = currentYear;

        mesAtual = currentMonth;
        anoAtual = currentYear;

        carregarContas();
        
        popularModalDuplicar(); 
    };
</script>

</body>
</html>
