# Office Add-in - Gestion BDD Excel

Application CRUD intégrée à Excel pour gérer les Contacts, Projets et leurs Liaisons.

## Structure du fichier Excel attendue

Le fichier BDD.xlsx doit avoir cette structure sur la feuille active :

| Colonne A | Colonne B | Colonne C | Colonne D | Colonne E | Colonne F |
|-----------|-----------|-----------|-----------|-----------|-----------|
| Contacts  |           | Projets   |           | Contacts  | Projets   |
| Jérémy    |           | PJT1      |           | Jérémy    | PJT1      |
| Florian   |           | PJT2      |           | Jérémy    | PJT2      |
|           |           | PJT3      |           | Florian   | PJT2      |

- **Colonne A** : Liste des contacts
- **Colonne C** : Liste des projets
- **Colonnes E-F** : Table de liaison (Contact ↔ Projet)

---

## Installation

### Méthode 1 : Excel Online (Recommandée)

Cette méthode fonctionne directement avec votre fichier sur SharePoint.

#### Étape 1 : Démarrer le serveur local

```bash
cd /Users/jeremy/Documents/AppExcel/addin
npm install
npm run start-http
```

Le serveur démarre sur `http://localhost:3000`

#### Étape 2 : Charger l'add-in dans Excel Online

1. Ouvrez votre fichier **BDD.xlsx** sur SharePoint dans Excel Online
2. Cliquez sur **Insertion** > **Compléments** > **Compléments Office**
3. Cliquez sur **Charger mon complément** (ou "Upload My Add-in")
4. Cliquez sur **Parcourir** et sélectionnez le fichier `manifest.xml`
5. Cliquez sur **Charger**

L'add-in apparaît dans le ruban sous l'onglet **Accueil** avec le bouton **Gestion BDD**.

#### Important pour Excel Online

Pour Excel Online, vous devez modifier le manifest.xml pour utiliser HTTP au lieu de HTTPS :

Remplacez toutes les occurrences de :
```
https://localhost:3000
```
par :
```
http://localhost:3000
```

---

### Méthode 2 : Excel Desktop (Windows/Mac)

#### Étape 1 : Générer les certificats SSL

```bash
cd /Users/jeremy/Documents/AppExcel/addin
npm install
npm run generate-cert
```

#### Étape 2 : Démarrer le serveur HTTPS

```bash
npm start
```

Le serveur démarre sur `https://localhost:3000`

#### Étape 3 : Charger l'add-in

**Sur Windows :**
1. Ouvrez Excel
2. Allez dans **Fichier** > **Options** > **Centre de gestion de la confidentialité**
3. Cliquez sur **Paramètres du Centre de gestion de la confidentialité**
4. Sélectionnez **Catalogues de compléments approuvés**
5. Ajoutez le chemin du dossier contenant le manifest.xml
6. Redémarrez Excel
7. Allez dans **Insertion** > **Mes compléments** > **Dossier partagé**

**Sur Mac :**
1. Ouvrez Excel
2. Allez dans **Insertion** > **Compléments** > **Mes compléments**
3. Cliquez sur le menu déroulant et sélectionnez **Charger mon complément**
4. Parcourez et sélectionnez le fichier `manifest.xml`

---

## Utilisation

1. **Ouvrez** votre fichier BDD.xlsx (depuis SharePoint ou en local)
2. **Cliquez** sur le bouton **Gestion BDD** dans le ruban
3. Un panneau latéral s'ouvre avec l'interface CRUD
4. **Naviguez** entre les onglets : Contacts, Projets, Liaisons
5. **Ajoutez/Modifiez/Supprimez** des éléments avec les boutons correspondants
6. Les modifications sont **automatiquement enregistrées** dans le fichier Excel

---

## Fonctionnalités

- **Contacts** : Ajouter, modifier, supprimer des contacts
- **Projets** : Ajouter, modifier, supprimer des projets
- **Liaisons** : Associer des contacts à des projets via des listes déroulantes
- **Statistiques** : Vue rapide du nombre d'éléments
- **Rafraîchissement** : Recharger les données depuis Excel

---

## Dépannage

### L'add-in ne se charge pas

1. Vérifiez que le serveur est bien démarré (`npm run start-http`)
2. Vérifiez que le manifest.xml pointe vers la bonne URL
3. Pour Excel Online, utilisez HTTP (pas HTTPS)

### Les données ne s'affichent pas

1. Vérifiez que la feuille active contient les données
2. Vérifiez que les en-têtes sont en ligne 1
3. Cliquez sur **Rafraîchir** dans l'add-in

### Erreur de certificat (Excel Desktop)

Exécutez `npm run generate-cert` et acceptez le certificat dans votre système.

---

## Structure des fichiers

```
addin/
├── manifest.xml      # Configuration de l'add-in
├── taskpane.html     # Interface utilisateur
├── package.json      # Dépendances npm
├── README.md         # Ce fichier
└── assets/
    ├── icon-16.png
    ├── icon-32.png
    ├── icon-64.png
    └── icon-80.png
```
