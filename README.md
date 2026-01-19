// Create initial data

INSERT INTO clienti ( nome, cognome, anno_di_nascita, regione_residenza) VALUES
('Aldo', 'Rossi', 1985, 'Lombardia'),
('Laura', 'Baglio', 1990, 'Lazio'),
('Giacomino', 'Verdi', 1978, 'Toscana'),
('Paolo', 'Neri', 1982, 'Veneto'),
('Francesca', 'Conti', 1995, 'Emilia-Romagna');
('Aldo', 'Menenghini', 1985, 'Abruzzo'),
('Mario', 'Bassu', 1991, 'Calabria'),
('Giacomino', 'Verdi', 1978, 'Toscana'),
('Mario', 'Neri', 1982, 'Molise'),
('Francesca', 'Corti', 1975, 'Umbria');

INSERT INTO prodotti ( descrizione, in_produzione, in_commercio, data_attivazione, data_disattivazione) VALUES
('Software gestionale', TRUE, TRUE, '2022-01-01', NULL),
('App mobile', TRUE, TRUE, '2022-06-15', NULL),
('Servizio cloud', TRUE, FALSE, '2021-03-10', '2024-01-01'),
('Licenza analytics', FALSE, FALSE, '2020-09-01', '2023-06-30'),
('Piattaforma e-commerce', TRUE, TRUE, '2023-02-01', NULL);
('Software gestionale', TRUE, TRUE, '2026-01-01', NULL),
('App mobile', TRUE, TRUE, '2012-06-15', NULL),
('Servizio cloud', TRUE, FALSE, '2017-03-10', '2024-01-01'),
('Licenza analytics', FALSE, FALSE, '2017-09-01', '2023-06-30'),
('Piattaforma e-commerce', TRUE, TRUE, '2017-02-01', NULL);


INSERT INTO fornitori ( denominazione, regione_residenza) VALUES
('Tech Solutions Srl', 'Lombardia'),
('Cloud Italia Spa', 'Piemonte'),
('Digital Services Srl', 'Lazio'),
('Innovatech Spa', 'Emilia-Romagna'),
('Data Systems Srl', 'Veneto');

INSERT INTO fatture (tipologia, importo, iva, id_cliente, data_fattura, numero_fornitore) VALUES
('Vendita', 1200.00, 22, 1, '2024-01-15', 1),
('Vendita', 850.00, 22, 2, '2024-01-20', 1),
('Abbonamento', 300.00, 22, 3, '2024-02-01', 2),
('Consulenza', 1500.00, 22, 1, '2024-02-10', 3),
('Vendita', 500.00, 22, 4, '2024-02-18', 4),
('Vendita', 100.00, 18, 1, '2024-01-15', 1),
('Vendita', 550.00, 20, 2, '2024-01-20', 1),
('Abbonamento', 250.00, 25, 3, '2024-02-01', 2),
('Consulenza', 500.00, 20, 1, '2024-02-10', 3),
('Vendita', 700.00, 20, 4, '2024-02-18', 5);


// Estrai tutti I client con nome Mario
SELECT * FROM clienti
WHERE nome = 'Mario'
ORDER BY cognome ASC 

// Estrai il nome e cognomen dei client nati nel 1982
SELECT nome,cognome FROM clienti
WHERE anno_di_nascita = 1982
ORDER BY nome ASC 


//Numero di fatture con IVA al 20%
SELECT COUNT(*) FROM fatture
WHERE iva = 20


//Prodotti attivati nel 2017 e in produzione oppure in commercio
SELECT * FROM prodotti
WHERE EXTRACT(YEAR FROM data_attivazione) = 2017 AND (in_produzione = TRUE OR in_comercio = TRUE);

//Fatture con importo < 1000 e dati dei clienti collegati
SELECT  f.*, c.nome, c.cognome, c.regione_residenza
FROM fatture f
JOIN clienti c ON f.id_cliente = c.numero_cliente
WHERE f.importo < 1000
ORDER by  f.numero_fattura ASC

//Elenco fatture (numero, importo, iva, data) + nome fornitore
SELECT f.numero_fattura, f.importo, f.iva, f.data_fattura, fo.denominazione
FROM fatture f
JOIN fornitori fo ON f. numero_fornitore = fo.numero_fornitore

//Conteggio fatture con IVA 20% per anno
SELECT EXTRACT(YEAR FROM data_fattura) AS anno, COUNT(*) AS numero_fatture
FROM fatture
WHERE iva = 20
GROUP BY anno
ORDER BY anno;

//Numero fatture e somma importi per anno di fatturazione
SELECT EXTRACT(YEAR FROM data_fattura) as anno, COUNT(*) AS numero_fatture, SUM(importo)
FROM fatture
GROUP BY anno
ORDER BY anno;

//Anni con più di 2 fatture di tipologia "Vendita"
SELECT EXTRACT(YEAR FROM data_fattura) AS anno
FROM fatture
WHERE tipologia = 'Vendita'
GROUP BY anno
HAVING COUNT(*) > 2
ORDER BY anno;
