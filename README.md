# SICOSA - Système d'Information Commerciale de Sandiara

## Description

SICOSA est une solution numérique complète permettant la gestion, le recensement et le suivi des unités commerciales dans la commune de Sandiara. Elle vise à moderniser la fiscalité locale.

## Fonctionnalités
- **Plateforme Web** : Gestion des collecteurs, unités commerciales, paiements, rapports PDF, carte interactive.
- **Application Mobile** : Recensement des commerçants, collecte des paiements, consultation des cartes.
- **Intégration KoboToolbox** : Importation des données de collecte terrain.
- **Géolocalisation** : Localisation des unités commerciales sur une carte interactive.

## Technologies Utilisées
- **Backend Web** : PHP natif avec PDO
- **Frontend Web** : HTML, CSS, JavaScript, LeafletJS
- **Application Mobile** : Flutter (Android uniquement)
- **Base de Données** : MySQL
- **Collection Terrain** : KoboToolbox via API REST

## Installation
1. Clonez ce dépôt :
   ```bash
   git clone https://github.com/sultan2096/sicosa.git
   ```
2. Configurez la base de données MySQL :
   - Importez le fichier `sql/create_tables.sql`.
3. Configurez le fichier `config/database.php` avec vos informations MySQL.

4. Exécutez un serveur local pour la plateforme web :
   ```bash
   php -S localhost:8000 -t web/public
   ```

## Utilisation
- Accédez à la plateforme web via `http://localhost:8000`.
- Utilisez l'application mobile Flutter pour les collecteurs.

## Contributeurs
-

## Affichage du "Type de taxe"

Pour afficher le "Type de taxe" dans votre interface de gestion des unités commerciales, vous devez vous assurer que le champ est correctement intégré dans votre formulaire et que les données sont récupérées et affichées correctement. Voici comment procéder :

### 1. Mettre à Jour le Formulaire HTML

Assurez-vous que le formulaire d'ajout/modification d'une unité commerciale inclut un menu déroulant pour le "Type de taxe". Voici un exemple de code HTML pour le formulaire :

```html
<form action="votre_action.php" method="post">
    <label for="nom">Nom:</label>
    <input type="text" id="nom" name="nom" required>

    <label for="prenom">Prénom:</label>
    <input type="text" id="prenom" name="prenom" required>

    <label for="telephone">Téléphone:</label>
    <input type="text" id="telephone" name="telephone">

    <label for="cin">CIN:</label>
    <input type="text" id="cin" name="cin">

    <label for="nature_taxe">Nature de la taxe:</label>
    <input type="text" id="nature_taxe" name="nature_taxe" required>

    <label for="type_taxe_id">Type de taxe:</label>
    <select id="type_taxe_id" name="type_taxe_id">
        <!-- Les options seront remplies dynamiquement depuis la base de données -->
    </select>

    <label for="geolocalisation">Géolocalisation:</label>
    <input type="text" id="geolocalisation" name="geolocalisation">

    <input type="submit" value="Enregistrer">
</form>
```

### 2. Remplir Dynamiquement le Menu Déroulant

Pour remplir le menu déroulant avec les types de taxes depuis la base de données, vous pouvez utiliser PHP pour récupérer les données et les afficher. Voici un exemple de code PHP :

```php
<?php
// Connexion à la base de données
$pdo = new PDO('mysql:host=localhost;dbname=sicosa', 'votre_utilisateur', 'votre_mot_de_passe');

// Récupérer les types de taxes
$query = $pdo->query("SELECT id, libelle FROM taxes");
$types_de_taxes = $query->fetchAll(PDO::FETCH_ASSOC);
?>

<select id="type_taxe_id" name="type_taxe_id">
    <?php foreach ($types_de_taxes as $taxe): ?>
        <option value="<?php echo htmlspecialchars($taxe['id']); ?>">
            <?php echo htmlspecialchars($taxe['libelle']); ?>
        </option>
    <?php endforeach; ?>
</select>
```

### 3. Mettre à Jour le Code Backend

Assurez-vous que votre code backend est configuré pour gérer ces nouveaux champs lors de l'insertion et de la mise à jour des données dans la table `commerces`. Voici un exemple de requête d'insertion :

```php
$nom = $_POST['nom'];
$prenom = $_POST['prenom'];
$telephone = $_POST['telephone'];
$cin = $_POST['cin'];
$type_taxe_id = $_POST['type_taxe_id'];
$nature_taxe = $_POST['nature_taxe'];
$geolocalisation = $_POST['geolocalisation'];

// Assurez-vous de bien échapper les valeurs pour éviter les injections SQL
$sql = "INSERT INTO commerces (nom, prenom, telephone, cin, type_taxe_id, nature_taxe, geolocalisation) VALUES (?, ?, ?, ?, ?, ?, POINT(?, ?))";
$stmt = $pdo->prepare($sql);
$stmt->execute([$nom, $prenom, $telephone, $cin, $type_taxe_id, $nature_taxe, $latitude, $longitude]);
```

### 4. Vérifier l'Affichage

Assurez-vous que le champ "Type de taxe" est bien affiché dans votre interface utilisateur. Si vous ne le voyez pas, vérifiez que le code HTML et PHP est correctement intégré et que les données sont récupérées correctement depuis la base de données.

En suivant ces étapes, vous devriez être en mesure d'afficher le "Type de taxe" dans votre interface de gestion des unités commerciales. Si vous avez besoin d'aide supplémentaire, n'hésitez pas à demander !# Sig
