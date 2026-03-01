# Scraping-Pokaa
Code réalisé par Mattéo CORREGER et Elia HUREL-LUCEF-LOISEL. 

## Trouvez une nouvelle sortie à faire à Strasbourg selon vos dates ! 
Ce projet est un scraper intelligent du site pokaa.fr, le média strasbourgeois de référence pour les sorties et événements culturels.
Il permet de trouver automatiquement toutes les sorties publiées sur Pokaa pour une date donnée. 

### Objectif
Pokaa publie régulièrement des articles listant des événements à Strasbourg.
Le problème : les dates des événements ne sont pas structurées dans le site, elles sont écrites en texte libre dans les articles ("ce week-end", "du 7 au 9 mars", "samedi 8 mars"...).

Notre outil résout ce problème en :

- *Scrappant* automatiquement les articles de la catégorie "Sorties" de Pokaa
- *Extrayant* les dates grâce à des expressions régulières (Regex)
- *Filtrant* les articles qui correspondent à la date recherchée
- *Affichant* le titre et le lien de chaque sortie trouvée
  
### Pour pouvoir excétuter notre code, il est nécessaire d'avoir les bons packages
En travaillant avec votre environnement python vous aurez besoin d'installer le package requests beautifulsoup4 ipykernel. 

```
pip install requests beautifulsoup4 ipykernel
```
- requests : Pour récupérer le contenu des pages web.

- beautifulsoup4 : Pour extraire les informations du code HTML.

- ipykernel : Indispensable pour exécuter le code dans un environnement de type "Notebook" (.ipynb).

Pour les autres packages commme sys, re et datetime, ce sont des modules de la bibliothèque standard de Python. Ils sont pré-installés avec l'exécutable Python lui-même. 

## Fonctionnement

### Structure du notebook

Le notebook est composé de 4 cellules à exécuter dans l'ordre :

**Cellule 1 — Date cible**

C'est la seule cellule à modifier. Tu y entres la date que tu veux chercher.
```
DATE_CIBLE = "JJ/MM/AAAA"
```
**Cellule 2 — Configuration**

Chargement des librairies et définition des paramètres globaux : URL de base, nombre de pages à scraper, headers HTTP, dictionnaire des mois en français.

**Cellule 3 — Fonctions**

Définition de toutes les fonctions du scraper :

- *parse_input_date()* : convertit la date saisie en objet Python datetime

- *extract_event_dates()* : utilise des Regex pour détecter les dates dans le texte des articles (plages "du X au Y", dates isolées "le 7 mars", listes "3 et 10 mars")

- *date_is_covered()* : vérifie si la date cible est couverte par une date ou période trouvée

- *fetch_soup()* : récupère et parse le HTML d'une page avec requests + BeautifulSoup

- *get_listing_articles()* : extrait les métadonnées (titre, lien, catégorie) de chaque article dans le listing

- *get_article_event_dates()* : visite chaque article et extrait les dates de l'événement depuis son contenu

**Cellule 4 — Lancement et résultats**

Boucle principale qui parcourt les pages du listing, filtre les articles de la catégorie "Sorties", analyse chaque article et affiche les résultats.

##  La détection de dates par Regex

C'est le cœur du projet. Les dates dans les articles Pokaa peuvent prendre de nombreuses formes. Nous avons défini 3 patterns Regex pour les couvrir :

**Pattern 1 — Plage de dates**
```
"du 7 au 9 mars"  →  (07/03, 09/03)
"du 20 mars au 2 avril"  →  (20/03, 02/04)
```
**Pattern 2 — Dates multiples**
```
"les 3 et 10 mars"  →  (03/03) et (10/03)
```
**Pattern 3 — Date unique**
```
"vendredi 7 mars"  →  (07/03)
"le 8 mars"  →  (08/03)
 ```

