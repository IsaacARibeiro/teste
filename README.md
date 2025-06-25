<?php
// rss.php

// URL do feed RSS
$rssUrl = "https://www.google.com.br/alerts/feeds/10347195646859231749/1802926607955157163";

// Pega o conteúdo do feed RSS
$rssContent = file_get_contents($rssUrl);
if (!$rssContent) {
    echo "Erro ao carregar o RSS.";
    exit;
}

// Carrega o XML
$xml = simplexml_load_string($rssContent);
if (!$xml) {
    echo "Erro ao analisar XML.";
    exit;
}

// Filtra itens dos últimos 3 dias
$threeDaysAgo = strtotime("-3 days");
$items = [];

foreach ($xml->channel->item as $item) {
    $pubDate = strtotime((string)$item->pubDate);
    if ($pubDate >= $threeDaysAgo) {
        $items[] = [
            'title' => (string)$item->title,
            'link' => (string)$item->link,
            'pubDate' => date('d/m/Y', $pubDate),
            'description' => (string)$item->description,
        ];
    }
}

// Passa os itens para o JavaScript
?>

<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8" />
    <title>RSS Slider Simples</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 600px; margin: 30px auto; }
        .news-item { border: 1px solid #ccc; padding: 20px; margin-bottom: 20px; border-radius: 8px; background: #f9f9f9; }
        .news-title { font-size: 1.2em; font-weight: bold; }
        .news-date { color: #666; font-size: 0.9em; margin-bottom: 10px; }
        .controls button { margin: 0 5px; padding: 5px 15px; cursor: pointer; }
    </style>
</head>
<body>

<div id="rss-container">
    <p>Carregando notícias...</p>
</div>

<div class="controls" style="text-align:center;">
    <button onclick="prevItem()">⬅️ Anterior</button>
    <button onclick="nextItem()">Próximo ➡️</button>
</div>

<script>
    const items = <?php echo json_encode($items); ?>;
    let currentIndex = 0;

    function showItem(index) {
        if (items.length === 0) {
            document.getElementById('rss-container').innerHTML = "<p>Não há notícias recentes.</p>";
            return;
        }

        const item = items[index];
        document.getElementById('rss-container').innerHTML = 
            <div class="news-item">
                <div class="news-title">${item.title}</div>
                <div class="news-date">${item.pubDate}</div>
                <div class="news-desc">${item.description}</div>
                <a href="${item.link}" target="_blank" rel="noopener noreferrer">Leia mais</a>
            </div>
        ;
    }

    function nextItem() {
        currentIndex = (currentIndex + 1) % items.length;
        showItem(currentIndex);
    }

    function prevItem() {
        currentIndex = (currentIndex - 1 + items.length) % items.length;
        showItem(currentIndex);
    }

    // Mostrar primeiro item ao carregar
    showItem(currentIndex);

    // Avançar automaticamente a cada 5 segundos
    setInterval(nextItem, 5000);
</script>

</body>
</html>
