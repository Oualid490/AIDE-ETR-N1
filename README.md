<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>AIDE ETR N1 – Ouverture dossier étranger</title>

<style>
body {
  font-family: Arial, sans-serif;
  background:#eef2f7;
  margin:0;
  padding:0;
}

.wrapper {
  max-width: 780px;
  margin: 40px auto;
  background:#fff;
  padding:30px;
  border-radius:12px;
  box-shadow:0 4px 12px rgba(0,0,0,.15);
}

h1 {
  text-align:center;
  color:#154360;
  margin-bottom:5px;
}

.subtitle {
  text-align:center;
  color:#566573;
  font-size:14px;
  margin-bottom:25px;
}

h2 {
  font-size:18px;
  margin-top:25px;
  color:#1b4f72;
  border-left:5px solid #1b4f72;
  padding-left:10px;
}

label {
  font-weight:600;
  display:block;
  margin-top:15px;
}

.help {
  font-size:12px;
  font-style:italic;
  color:#5f6a6a;
  margin-top:3px;
}

input, textarea {
  width:100%;
  padding:8px;
  margin-top:5px;
  box-sizing:border-box;
}

textarea { min-height:60px; }

button {
  margin-top:20px;
  padding:12px;
  width:100%;
  font-size:16px;
  cursor:pointer;
}

#output {
  height:200px;
  margin-top:15px;
  white-space:pre-line;
}
</style>
</head>

<body>

<div class="wrapper">

<h1>AIDE ETR N1</h1>
<div class="subtitle">
Outil d’aide à l’ouverture de dossier étranger – N1  
<br>Objectif : ne rien oublier dès le premier contact
</div>

<h2>1. Contexte client</h2>

<label>Date d’entrée dans le pays</label>
<div class="help">Date à laquelle le client est arrivé dans le pays étranger</div>
<input type="date" id="dateEntree">

<label>Langues parlées par le client</label>
<div class="help">Ex : Français / Anglais – utile pour le choix du prestataire</div>
<input type="text" id="langues">

<label>Destination finale</label>
<div class="help">Ville, hôtel, famille, lieu de séjour prévu</div>
<input type="text" id="destination">

<label>Détail précis de l’évènement</label>
<div class="help">Ce qu’il s’est passé exactement : panne, accident, immobilisation…</div>
<textarea id="evenement"></textarea>

<label>Second numéro de téléphone</label>
<div class="help">Numéro local si possible, ou autre contact joignable</div>
<input type="text" id="numero2">

<label>Détails importants (passagers, enfants, animaux, bagages…)</label>
<div class="help">Toute information utile pour la suite du dossier</div>
<textarea id="notes"></textarea>

<h2>2. Localisation</h2>

<label>Pays</label>
<div class="help">Pays où se trouve le client au moment de l’évènement</div>
<input type="text" id="pays">

<label>Localisation précise</label>
<div class="help">
Adresse exacte ou description précise.  
Si autoroute : numéro, PK et direction.
</div>
<textarea id="localisation"></textarea>

<h2>3. Véhicule</h2>

<label>Type de véhicule</label>
<div class="help">VL, utilitaire, camping-car, moto, location…</div>
<input type="text" id="vehicule">

<label>Spécificités du véhicule (si nécessaire)</label>
<div class="help">Gabarit, poids, équipements, remorque, aménagements…</div>
<textarea id="specVehicule"></textarea>

<h2>4. Génération du script</h2>

<button id="btnGenerate">Générer le script</button>

<textarea id="output" readonly></textarea>

<button id="btnCopy">Copier le script</button>

</div>

<script>
document.addEventListener("DOMContentLoaded", function () {

  const btnGenerate = document.getElementById("btnGenerate");
  const btnCopy = document.getElementById("btnCopy");
  const output = document.getElementById("output");

  btnGenerate.addEventListener("click", function () {

    const script =
`📅 Date entrée pays : ${dateEntree.value}
🗣 Langues parlées : ${langues.value}
🎯 Destination finale : ${destination.value}

💥 Évènement :
${evenement.value}

📍 Localisation :
${pays.value} – ${localisation.value}

🚐 Véhicule : ${vehicule.value}
Spécificités : ${specVehicule.value}

📞 Second numéro : ${numero2.value}

📝 Infos complémentaires :
${notes.value}`;

    output.value = script;
  });

  btnCopy.addEventListener("click", function () {
    navigator.clipboard.writeText(output.value)
      .then(() => alert("✅ Script copié"))
      .catch(() => alert("❌ Copie impossible"));
  });

});
</script>

</body>
</html>
