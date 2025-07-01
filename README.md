# Nomira

Nomira est un projet open source développé par [envel69](https://github.com/envel69).  
Ce projet vise à offrir une solution simple pour faire un tirage au sort parmi une liste d'utilisateurs.

## Fonctionnalités

- **Ajout, modification et suppression d'utilisateurs**
- **Tirage au sort aléatoire d'un nom**
- **Historique des tirages**
- **Sauvegarde locale des listes (localStorage)**

## Installation

Clonez ce dépôt puis installez les dépendances nécessaires :

```bash
git clone https://github.com/envel69/nomira.git
cd nomira
npm install
```

## Utilisation

Après avoir installé les dépendances, lance le projet avec la commande suivante :

```bash
npm run dev
```

Ouvre ensuite ton navigateur à l'adresse indiquée dans le terminal (par défaut : [http://localhost:5173](http://localhost:5173)).

Pour effectuer un tirage au sort :
1. Ajoute les utilisateurs dans la liste via l'interface.
2. Clique sur le bouton **"Tirer au sort"** pour sélectionner un nom aléatoirement.



## Structure du projet

- `src/App.vue` : Composant principal de l'application.
- `src/views/HomeView.vue` : Vue principale avec la logique de tirage.
- `src/components/AttachmentUsers.vue` : Gestion de la liste des utilisateurs (ajout, modification, suppression).
- `src/components/ListUsers.vue` : Affichage de la liste des utilisateurs.

## Tester 

[Nomira](https://nomira69.netlify.app/)
