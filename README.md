<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Avaliação TAD</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        .classificacao-box {
            display: inline-block;
            padding: 10px;
            font-weight: bold;
            font-size: 1.2em;
            color: white;
            border-radius: 5px;
        }
        .verde { background-color: green; }
        .amarelo { background-color: orange; }
        .vermelho { background-color: red; }
    </style>
    <script>
        function atualizarAvaliacao() {
            const pesosFonte = { "sim": 2, "nao": 0, "alta": 2, "media": 1, "baixa": 0 };
            const pesosConteudo = { "alta": 2, "media": 1, "baixa": 0 };
            
            let autenticidade = document.querySelector('select[name="autenticidade"]').value;
            let confianca = document.querySelector('select[name="confianca"]').value;
            let competencia = document.querySelector('select[name="competencia"]').value;
            let coerencia = document.querySelector('select[name="coerencia"]').value;
            let compatibilidade = document.querySelector('select[name="compatibilidade"]').value;
            let semelhanca = document.querySelector('select[name="semelhanca"]').value;
            
            let pontuacaoFonte = pesosFonte[autenticidade] + pesosFonte[confianca] + pesosFonte[competencia];
            let classificacaoFonte, motivoFonte, letra;
            
            if (pontuacaoFonte >= 5) {
                classificacaoFonte = "A - Inteiramente idônea";
                motivoFonte = "A fonte foi considerada autêntica, altamente confiável e competente.";
                letra = "A";
            } else if (pontuacaoFonte === 4) {
                classificacaoFonte = "B - Normalmente idônea";
                motivoFonte = "A fonte é autêntica e possui um nível médio de confiança e competência.";
                letra = "B";
            } else if (pontuacaoFonte === 3) {
                classificacaoFonte = "C - Regularmente idônea";
                motivoFonte = "A fonte é autêntica, mas apresenta limitações na confiabilidade e competência.";
                letra = "C";
            } else {
                classificacaoFonte = "E - Inidônea";
                motivoFonte = "A fonte não pôde ser autenticada ou apresenta pouca confiabilidade.";
                letra = "E";
            }
            
            let pontuacaoConteudo = pesosConteudo[coerencia] + pesosConteudo[compatibilidade] + pesosConteudo[semelhanca];
            let classificacaoConteudo, motivoConteudo, numero;
            
            if (pontuacaoConteudo >= 5) {
                classificacaoConteudo = "1 - Confirmado por outras fontes";
                motivoConteudo = "O conteúdo foi coerente, compatível e corroborado por outras fontes.";
                numero = "1";
            } else if (pontuacaoConteudo === 4) {
                classificacaoConteudo = "2 - Provavelmente verdadeiro";
                motivoConteudo = "O conteúdo apresenta coerência e compatibilidade, mas sem total confirmação.";
                numero = "2";
            } else if (pontuacaoConteudo === 3) {
                classificacaoConteudo = "3 - Possivelmente verdadeiro";
                motivoConteudo = "O conteúdo faz sentido, mas carece de mais fontes confirmatórias.";
                numero = "3";
            } else if (pontuacaoConteudo === 2) {
                classificacaoConteudo = "4 - Duvidoso";
                motivoConteudo = "O conteúdo apresenta algumas inconsistências e poucas confirmações.";
                numero = "4";
            } else {
                classificacaoConteudo = "5 - Improvável";
                motivoConteudo = "O conteúdo apresenta baixa coerência, tornando sua veracidade improvável.";
                numero = "5";
            }
            
            let corClassificacao = (letra === "A" && numero === "1") ? "verde" :
                                  (letra === "B" || numero === "2") ? "amarelo" : "vermelho";
            
            document.getElementById("resultado").innerHTML = `
                <h3 class="text-center">Resultado da Avaliação</h3>
                <p><strong>Classificação da Fonte:</strong> ${classificacaoFonte}</p>
                <p>${motivoFonte}</p>
                <p><strong>Classificação do Conteúdo:</strong> ${classificacaoConteudo}</p>
                <p>${motivoConteudo}</p>
                <div class="classificacao-box ${corClassificacao}">${letra}${numero}</div>
            `;
        }
    </script>
</head>
<body class="bg-light">
    <div class="container mt-5">
        <div class="card shadow p-4">
            <h2 class="text-center mb-4">Formulário de Avaliação TAD</h2>
            <form>
                <h3>Avaliação da Fonte</h3>
                <div class="mb-3">
                    <label class="form-label">Autenticidade da fonte:</label>
                    <select name="autenticidade" class="form-select" onchange="atualizarAvaliacao()">
                        <option value="sim">Sim</option>
                        <option value="nao">Não</option>
                    </select>
                </div>
                <div class="mb-3">
                    <label class="form-label">Confiança da fonte:</label>
                    <select name="confianca" class="form-select" onchange="atualizarAvaliacao()">
                        <option value="alta">Alta</option>
                        <option value="media">Média</option>
                        <option value="baixa">Baixa</option>
                    </select>
                </div>
                <div class="mb-3">
                    <label class="form-label">Competência da fonte:</label>
                    <select name="competencia" class="form-select" onchange="atualizarAvaliacao()">
                        <option value="alta">Alta</option>
                        <option value="media">Média</option>
                        <option value="baixa">Baixa</option>
                    </select>
                </div>
                <h3>Avaliação do Conteúdo</h3>
                <div class="mb-3">
                    <label class="form-label">Coerência do conteúdo:</label>
                    <select name="coerencia" class="form-select" onchange="atualizarAvaliacao()">
                        <option value="alta">Alta</option>
                        <option value="media">Média</option>
                        <option value="baixa">Baixa</option>
                    </select>
                </div>
            </form>
        </div>
        <div class="card shadow p-4 mt-4" id="resultado">
            <h3 class="text-center">Resultado da Avaliação</h3>
            <p>Selecione as opções acima para visualizar a avaliação.</p>
        </div>
    </div>
</body>
</html>
