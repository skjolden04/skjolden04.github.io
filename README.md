<!DOCTYPE html>
<html lang="no">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Trade Tracker - Startside</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        h1 { color: #333; }
        ul { list-style-type: none; padding: 0; }
        li { background: #f9f9f9; margin: 10px 0; padding: 10px; border: 1px solid #ddd; }
    </style>
</head>
<body>
    <h1>Velkommen til Trade Tracker</h1>
    <p>Dette er en enkel startside som henter og viser siste trades-data. Dataen oppdateres når siden lastes.</p>

    <h2>Siste Innsider Trades (f.eks. CEO/CFO)</h2>
    <ul id="insider-list"></ul>

    <h2>Siste Bjellesauer Endringer (f.eks. store investorer)</h2>
    <ul id="bellwether-list"></ul>

    <h2>Siste Kongress Trades (f.eks. Nancy Pelosi)</h2>
    <ul id="congress-list"></ul>

    <script>
        // Erstatt med dine API-nøkler
        const fmpApiKey = 'DIN_FMP_API_NØKKEL'; // Fra financialmodelingprep.com
        const finnhubApiKey = 'DIN_FINNHUB_API_NØKKEL'; // Fra finnhub.io

        // Funksjon for å hente og vise data
        async function fetchTrades(url, listId) {
            try {
                const response = await fetch(url);
                if (!response.ok) throw new Error('Feil ved henting');
                const data = await response.json();
                const list = document.getElementById(listId);
                // Vis de siste 5 elementene (tilpass etter behov)
                (data.slice(0, 5) || []).forEach(item => {
                    const li = document.createElement('li');
                    li.textContent = `${item.symbol || 'Ukjent'} - ${item.transactionType || 'Endring'} av ${item.owner || item.name || 'Ukjent'} - Dato: ${item.date || 'Ukjent'} - Beløp/Antall: ${item.amount || item.shares || 'Ukjent'}`;
                    list.appendChild(li);
                });
            } catch (error) {
                console.error('Feil:', error);
                document.getElementById(listId).innerHTML = '<li>Kunne ikke hente data. Sjekk API-nøkkel eller nettverk.</li>';
            }
        }

        // Eksempler: Hent data for Apple (AAPL) – tilpass tickers eller filtre
        fetchTrades(`https://financialmodelingprep.com/api/v4/insider-trading?symbol=AAPL&apikey=${fmpApiKey}`, 'insider-list');
        fetchTrades(`https://financialmodelingprep.com/api/v3/institutional-holder/AAPL?apikey=${fmpApiKey}`, 'bellwether-list');
        fetchTrades(`https://finnhub.io/api/v1/stock/congressional-trading?symbol=AAPL&token=${finnhubApiKey}`, 'congress-list');
    </script>
</body>
</html>
