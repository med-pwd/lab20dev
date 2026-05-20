# Number Book - Documentation Détaillée

Number Book est une application Android "Full-Stack" qui illustre la gestion des données système et la communication client/serveur.

## 🛠️ Architecture Technique

### 1. Le Frontend (Android / Java)
L'application est construite autour de plusieurs composants clés :

- **MainActivity.java** : Le cerveau de l'application. Elle gère :
  - La demande de permission `READ_CONTACTS` au moment de l'exécution.
  - La récupération des données système via `ContactsContract`.
  - La logique de filtrage pour la recherche locale.
- **ContactAdapter.java** : Assure le lien entre la liste de données (`List<Contact>`) et les éléments visuels (`item_contact.xml`).
- **Retrofit (Prêt pour activation)** : Client HTTP performant pour transformer l'API PHP en interface Java simple.

### 2. Le Backend (PHP 8.x / PDO)
Le serveur est organisé de manière modulaire :

- **Database.php** : Utilise l'extension **PDO** (PHP Data Objects) pour une connexion robuste et sécurisée.
- **ContactService.php** : Centralise les requêtes SQL. L'utilisation de `prepare()` et `execute()` garantit une protection contre les injections SQL.
- **JSON** : Toutes les réponses sont envoyées avec le header `application/json`, permettant une interopérabilité parfaite avec n'importe quel client (Android, iOS ou Web).
<img width="882" height="484" alt="Screenshot 2026-05-09 184725" src="https://github.com/user-attachments/assets/cb41630c-8a6e-47b6-a29c-1efba5eee5ec" />

---

## 📋 Guide de Fonctionnement (Pas à Pas)

### Étape A : Lecture des contacts système
Lorsque vous cliquez sur "Charger Contacts" :
1. L'application vérifie la permission.
2. Un `Cursor` parcourt le répertoire système : `content://com.android.contacts/data/phones`.
3. Chaque contact est transformé en objet `Contact` Java et ajouté à la liste.

### Étape B : Recherche locale
Grâce au `TextWatcher` sur l'EditText :
1. À chaque touche pressée, une nouvelle liste `filteredList` est créée.
2. On compare (en minuscules) le texte saisi avec le nom et le téléphone.
3. `adapter.notifyDataSetChanged()` rafraîchit l'écran instantanément.

### Étape C : Synchronisation Distante (Si activée)
1. L'objet Java `Contact` est sérialisé en chaîne JSON par **Gson**.
2. **Retrofit** envoie cette chaîne via une requête `POST` vers `insertContact.php`.
3. Le serveur décode le JSON, valide les champs et insère le contact dans MySQL.

---

## ⚙️ Configuration Avancée

### Accès réseau Android (Émulateur)
Pour que l'émulateur puisse "voir" votre serveur local (XAMPP/WAMP), utilisez toujours l'adresse **`10.0.2.2`**. L'adresse `127.0.0.1` désignerait l'émulateur lui-même !

### Permissions Manifest
N'oubliez pas que depuis Android 6.0, déclarer la permission dans le `AndroidManifest.xml` ne suffit pas, elle doit être demandée à l'utilisateur pendant que l'application tourne.
