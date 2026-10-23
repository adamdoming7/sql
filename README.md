# sql

DROP DATABASE IF EXISTS gestion_commandes;
CREATE DATABASE gestion_commandes;
USE gestion_commandes;

CREATE TABLE profil_fidele (
    id_profil INT PRIMARY KEY AUTO_INCREMENT,
    profil VARCHAR(30) NOT NULL
);

CREATE TABLE client (
    id_client INT PRIMARY KEY AUTO_INCREMENT,
    nom_cl VARCHAR(30) NOT NULL,
    adresse VARCHAR(50),
    idprofil INT,
    FOREIGN KEY (idprofil) REFERENCES profil_fidele(id_profil)
);

CREATE TABLE categorie (
    id_categorie INT PRIMARY KEY AUTO_INCREMENT,
    intitule VARCHAR(40) NOT NULL
);

CREATE TABLE produit (
    id_produit INT PRIMARY KEY AUTO_INCREMENT,
    nom_produit VARCHAR(50) NOT NULL,
    prix DECIMAL(10,2) NOT NULL CHECK (prix > 0),
    stock INT DEFAULT 0 CHECK (stock >= 0),
    idcat INT,
    FOREIGN KEY (idcat) REFERENCES categorie(id_categorie)
);

CREATE TABLE commande (
    id_commande INT PRIMARY KEY AUTO_INCREMENT,
    date_commande DATE,
    statut VARCHAR(20) DEFAULT 'En cours',
    id_cl INT,
    FOREIGN KEY (id_cl) REFERENCES client(id_client)
);

CREATE TABLE lignecommande (
    id_commande INT,
    id_produit INT,
    quantite INT NOT NULL CHECK (quantite > 0),
    prixUnitaire DECIMAL(10,2) CHECK (prixUnitaire > 0),
    PRIMARY KEY (id_commande, id_produit),
    FOREIGN KEY (id_commande) REFERENCES commande(id_commande),
    FOREIGN KEY (id_produit) REFERENCES produit(id_produit)
);

INSERT INTO profil_fidele (id_profil, profil)
VALUES
(1, 'jeans'),
(2, 'T-shirt'),
(3, 'veste');


INSERT INTO client (id_client, nom_cl, adresse, idprofil)
VALUES
(1, 'Sara', 'Sale', 1),
(2, 'Aya', 'Rabat', 2),
(3, 'Imane', 'Temara', 3);


INSERT INTO categorie (id_categorie, intitule)
VALUES
(1, 'Ordinateur'),
(2, 'Telephone'),
(3, 'Accessoires');


INSERT INTO produit
(id_produit, nom_produit, prix, stock, idcat)
VALUES
(1, 'PC Lenovo', 7000, 10, 1),
(2, 'iPhone 15', 9000, 5, 2),
(3, 'Souris Logitech', 250, 20, 3),
(4, 'Clavier HP', 400, 15, 3),
(5, 'Samsung S24', 8500, 7, 2);


INSERT INTO commande
(id_commande, date_commande, statut, id_cl)
VALUES
(1, '2026-09-20', 'En cours', 1),
(2, '2026-09-21', 'Confirme', 2),
(3, '2026-09-22', 'En cours', 1),
(4, '2027-03-12', 'Confirme', 2),
(5, '2030-09-12', 'Confirme', 3);


INSERT INTO lignecommande
(id_commande, id_produit, quantite, prixUnitaire)
VALUES
(1, 1, 1, 7000),
(1, 3, 2, 250),
(2, 2, 1, 9000),
(2, 4, 1, 400),
(3, 5, 1, 8500),
(3, 3, 3, 250);


CREATE VIEW produits_stock AS
SELECT *
FROM produit
WHERE stock > 10;


CREATE VIEW montant_commandes AS
SELECT
    id_commande,
    SUM(quantite * prixUnitaire) AS montantTotal
FROM lignecommande
GROUP BY id_commande;


CREATE VIEW produits_vendus AS
SELECT
    produit.nom_produit,
    SUM(lignecommande.quantite) AS totalVendu
FROM produit
JOIN lignecommande
    ON produit.id_produit = lignecommande.id_produit
GROUP BY produit.id_produit, produit.nom_produit
ORDER BY totalVendu DESC;


SELECT * FROM profil_fidele;

SELECT * FROM client;

SELECT * FROM categorie;

SELECT * FROM produit;

SELECT * FROM commande;

SELECT * FROM lignecommande;


SELECT * FROM produits_stock;

SELECT * FROM montant_commandes;

SELECT * FROM produits_vendus;


