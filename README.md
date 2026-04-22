<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>AIDE‑ETR‑N1</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f6f7f9;
      margin: 0;
      padding: 20px;
    }

    h1 {
      text-align: center;
      color: #2c3e50;
    }

    h2 {
      color: #1f4ea8;
      margin-top: 0;
    }

    h3 {
      margin-top: 15px;
      font-size: 14px;
      color: #444;
    }

    .bloc {
      background: #ffffff;
      border: 1px solid #ddd;
      padding: 15px;
      margin-bottom: 20px;
      border-radius: 8px;
    }

    label {
      display: block;
      margin-top: 10px;
      font-weight: bold;
    }

    input, textarea {
      width: 100%;
      padding: 6px;
      margin-top: 4px;
      box-sizing: border-box;
    }

    textarea {
      min-height: 60px;
    }

    .checklist li {
      list-style: none;
      margin-bottom: 6px;
    }

    .info {
      font-size: 13px;
      color: #555;
      margin-top: 5px;
    }
  </style>
</head>

<body>

<h1>🧭 AIDE‑ETR‑N1  
<br><small>Ouverture d’un dossier Étranger – Niveau 1</small></h1>

<!-- IDENTIFICATION -->
<section class="bloc">
  <h2>👤 Identification bénéficiaire</h2>

  <label>Nom / Prénom</label>
  <input type="text">

  <label>N° contrat ou immatriculation</label>
  <input type="text">

  <label>Pays de souscription</label>
  <input type="text">

  <label>Numéro de contact principal</label>
  <input type="tel">

  <h3>ℹ️ Informations importantes</h3>
  <ul class="info">
    <li>Client facilement joignable</li>
    <li>Moyen de contact privilégié (appel / WhatsApp / SMS)</li>
    <li>Contraintes particulières (horaires, stress, urgence…)</li>
  </ul>
</section>

<!-- LOCALISATION -->
<section class="bloc">
  <h2>📍 Situation & localisation</h2>

  <label>Pays</label>
  <input type="text">

  <label>Ville</label>
  <input type="text">

  <label>Adresse précise / Point GPS</label>
  <textarea></textarea>

  <label>Lieu sécurisé</label>
  <input type="checkbox"> Oui

  <label>Véhicule roulant</label>
  <input type="checkbox"> Oui

  <p class="info">⚠️ Penser au décalage horaire</p>
</section>

<!-- VEHICULE -->
<section class="bloc">
  <h2>🚗 Véhicule</h2>

  <label>Marque / Modèle</label>
  <input type="text">

  <label>Immatriculation</label>
  <input type="text">

  <label>Année / motorisation (si connue)</label>
  <input type="text">

  <label>Véhicule accessible au dépanneur</label>
  <input type="checkbox"> Oui
</section>

<!-- EVENEMENT -->
<section class="bloc">
  <h2>⚠️ Évènement</h2>

  <label>Type d’évènement</label>
  <ul class="checklist">
    <li><input type="checkbox"> Panne</li>
    <li><input type="checkbox"> Accident</li>
    <li><input type="checkbox"> Crevaison</li>
    <li><input type="checkbox"> Autre</li>
  </ul>

  <label>Symptômes décrits par le client</label>
  <textarea></textarea>

  <label>Véhicule déjà vu par un professionnel</label>
  <input type="checkbox"> Oui

  <label>Nombre de personnes à bord lors de l’évènement</label>
  <input type="number" min="1">
</section>

<!-- PRESTATIONS -->
<section class="bloc">
  <h2>🧾 Prestations & contrat</h2>

  <p class="info">
    Prestations disponibles selon les garanties contractuelles.<br>
    Délais variables selon le pays et le réseau local.
  </p>

  <p class="info">
    🗣️ <strong>Phrase type :</strong><br>
    « Les délais et modalités peuvent varier selon le pays et les partenaires locaux. »
  </p>
</section>

<!-- FINANCIER -->
<section class="bloc">
  <h2>💰 Informations financières</h2>

  <ul class="info">
    <li>Avance de frais possible</li>
    <li>Reste à charge potentiel</li>
    <li>Moyen de paiement local en cas de dépassement</li>
  </ul>

  <p class="info">⚠️ Toujours annoncer un reste à charge potentiel</p>
</section>

<!-- LANGUE -->
<section class="bloc">
  <h2>🌍 Langue & communication</h2>

  <label>Langue parlée par le client</label>
  <input type="text">

  <label>Langue locale</label>
  <input type="text">

  <label>Besoin d’intermédiaire / traducteur</label>
  <input type="checkbox"> Oui

  <p class="info">📌 WhatsApp / SMS privilégiés à l’étranger</p>
</section>

<!-- ACTIONS -->
<section class="bloc">
  <h2>✅ Actions N1 obligatoires</h2>

  <ul class="checklist">
    <li><input type="checkbox"> Dossier ETR créé</li>
    <li><input type="checkbox"> Commentaires clairs et factuels</li>
    <li><input type="checkbox"> Localisation complète</li>
    <li><input type="checkbox"> Informations client vérifiées</li>
    <li><input type="checkbox"> Résumé structuré de l’évènement</li>
  </ul>
</section>

<!-- CHECKLIST FINALE -->
<section class="bloc">
  <h2>✅ Checklist finale N1</h2>

  <ul class="checklist">
    <li><input type="checkbox"> Bénéficiaire identifié et joignable</li>
    <li><input type="checkbox"> Localisation exacte confirmée</li>
    <li><input type="checkbox"> Véhicule identifié</li>
    <li><input type="checkbox"> Type d’évènement renseigné</li>
    <li><input type="checkbox"> Nombre de personnes à bord indiqué</li>
    <li><input type="checkbox"> Prestations expliquées</li>
    <li><input type="checkbox"> Plafonds / reste à charge annoncés</li>
    <li><input type="checkbox"> Langue et canal de contact adaptés</li>
    <li><input type="checkbox"> Traces claires dans le dossier</li>
    <li><input type="checkbox"> Escalade réalisée si nécessaire</li>
  </ul>

  <p class="info">
    🧠 <strong>Règle d’or N1 – ETR :</strong><br>
    Factuel – Pas de diagnostic – Pas de promesse – Traçabilité maximale
  </p>
</section>

</body>
</html>
