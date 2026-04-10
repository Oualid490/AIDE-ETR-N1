<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>AIDE ETR N1 – Ouverture dossier étranger</title>
<p style="color:red;">VERSION UX 2026‑04‑TEST</p>

<style>
body {
  font-family: Arial, sans-serif;
  background:#eef2f7;
  margin:0;
}

/* HEADER */
.header {
  background:#0b4ea2;
  color:#fff;
  padding:20px;
  display:flex;
  align-items:center;
  gap:20px;
}

.header img {
  width:70px;
  height:auto;
}

.header h1 {
  margin:0;
  font-size:26px;
}

.header p {
  margin:5px 0 0;
  font-size:14px;
  opacity:0.95;
}

/* CONTAINER */
.wrapper {
  max-width: 820px;
  margin: 30px auto;
  background:#fff;
  padding:30px;
  border-radius:12px;
  box-shadow:0 4px 12px rgba(0,0,0,.15);
}

.note {
  background:#f0f6ff;
  border-left:5px solid #0b4ea2;
  padding:15px;
  font-size:14px;
  margin-bottom:25px;
}

h2 {
  font-size:18px;
  margin-top:30px;
  color:#0b4ea2;
  border-left:4px solid #0b4ea2;
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

.status {
  margin-top:10px;
  padding:10px;
  border-radius:6px;
  font-weight:bold;
}

.green { background:#d4efdf; color:#145a32; }
.orange { background:#fdebd0; color:#af601a; }
.red { background:#f5b7b1; color:#922b21; }

button {
  margin-top:20px;
  padding:12px;
  width:100%;
  font-size:16px;
  cursor:pointer;
}

.actions {
  display:flex;
  gap:10px;
  margin-top:15px;
}

.actions button {
  flex:1;
}

#output {
  height:180px;
  margin-top:15px;
  white-space:pre-line;
}

.checklist {
  list-style:none;
  padding-left:0;
}

.checklist li {
  margin:6px 0;
}
</style>
</head>

<body>

<div class="header">
  <img src="ICONE ETR.png" alt="ETR">
  <div>
    <h1>AIDE ETR N1</h1>
    <p>Outil d’aide à l’ouverture de dossier étranger – Niveau 1</p>
  </div>
</div>

<div class="wrapper">

<div class="note">
<b>⚠️ Enjeu d’un dossier étranger</b><br>
Un dossier ETR mal ouvert en N1 est souvent à l’origine de retards, surcoûts et insatisfactions client.<br>
L’objectif de cet outil est de <b>collecter dès le premier contact toutes les informations utiles</b>
pour sécuriser la prise en charge à l’étranger.
</div>

<h2>1. Contexte client</h2>

<label>Date d’entrée dans le pays</label>
<div class="help">Date d’arrivée effective du client dans le pays étranger</div>
<input type="date" id="dateEntree">
<div id="joursInfo" class="status"></div>

<label>
<input type="checkbox" id="panneRecente"> Panne récente (&lt; 30 jours)
</label>
<div class="help">À cocher si la panne est récente même si le séjour est long</div>

<label>Langues parlées</label>
<div class="help">Utile pour la coordination avec les prestataires étrangers</div>
<input type="text" id="langues">

<label>Destination finale</label>
<div class="help">Ville, hôtel, famille, lieu de séjour prévu</div>
<input type="text" id="destination">

<label>Détail précis de l’évènement</label>
<div class="help">Décrire précisément la panne ou la situation</div>
<textarea id="evenement"></textarea>

<label>Second numéro de téléphone</label>
<div class="help">Numéro local ou autre contact joignable</div>
<input type="text" id="numero2">

<label>Informations importantes</label>
<div class="help">Passagers, enfants, animaux, bagages, contraintes particulières</div>
<textarea id="notes"></textarea>

<h2>2. Localisation</h2>

<label>Pays</label>
<input type="text" id="pays">

<label>Localisation précise</label>
<div class="help">Adresse exacte. Si autoroute : numéro, PK et direction</div>
<textarea id="localisation"></textarea>

<label>
<input type="checkbox" id="autoroute"> Client sur autoroute
</label>

<div id="zoneAutoroute" style="display:none;">
  <label>Autoroute</label><input type="text" id="numAutoroute">
  <label>PK</label><input type="text" id="pk">
  <label>Direction</label><input type="text" id="direction">
</div>

<h2>3. Véhicule – uniquement si nécessaire</h2>

<label>Spécificités du véhicule</label>
<div class="help">
À remplir uniquement si le véhicule nécessite un traitement particulier :
camping-car, utilitaire, remorque, gabarit, équipements spécifiques…
</div>
<textarea id="specVehicule"></textarea>

<h2>4. Script</h2>

<button id="btnGenerate">Générer le script</button>
<textarea id="output" readonly></textarea>

<div class="actions">
  <button id="btnCopy">Copier</button>
  <button id="btnReset">Réinitialiser</button>
</div>

<h2>5. Checklist N1</h2>
<ul class="checklist" id="checklist"></ul>

</div>

<script>
document.addEventListener("DOMContentLoaded", function () {

const dateEntree = document.getElementById("dateEntree");
const panneRecente = document.getElementById("panneRecente");
const joursInfo = document.getElementById("joursInfo");

function updateDays() {
  if (!dateEntree.value) {
    joursInfo.textContent = "";
    joursInfo.className = "status";
    return;
  }
  const start = new Date(dateEntree.value);
  const now = new Date();
  const days = Math.floor((now - start) / (1000*60*60*24));

  if (days < 60) {
    joursInfo.className = "status green";
    joursInfo.textContent = `Séjour : ${days} jours – OK`;
  } else if (days <= 90) {
    joursInfo.className = "status orange";
    joursInfo.textContent = `Séjour : ${days} jours – vigilance`;
  } else {
    joursInfo.className = panneRecente.checked ? "status orange" : "status red";
    joursInfo.textContent = panneRecente.checked
      ? `Séjour : ${days} jours – panne récente`
      : `Séjour : ${days} jours – hors conditions`;
  }
}

dateEntree.addEventListener("change", updateDays);
panneRecente.addEventListener("change", updateDays);

document.getElementById("autoroute").addEventListener("change", e => {
  document.getElementById("zoneAutoroute").style.display = e.target.checked ? "block" : "none";
});

document.getElementById("btnGenerate").addEventListener("click", function () {

const script =
`📅 Entrée pays : ${dateEntree.value}
🗣 Langues : ${langues.value}
🎯 Destination : ${destination.value}

💥 Évènement :
${evenement.value}

📍 Localisation :
${pays.value} – ${localisation.value}
${autoroute.checked ? `Autoroute ${numAutoroute.value}, PK ${pk.value}, ${direction.value}` : ""}

🚐 Spécificités véhicule :
${specVehicule.value}

📞 Second numéro : ${numero2.value}

📝 Infos complémentaires :
${notes.value}`;

output.value = script;
updateChecklist();
});

document.getElementById("btnCopy").addEventListener("click", function () {
  navigator.clipboard.writeText(output.value)
    .then(() => alert("✅ Script copié"));
});

document.getElementById("btnReset").addEventListener("click", function () {
  document.querySelectorAll("input, textarea").forEach(el => el.value = "");
  document.querySelectorAll("input[type=checkbox]").forEach(el => el.checked = false);
  document.getElementById("zoneAutoroute").style.display = "none";
  joursInfo.textContent = "";
  joursInfo.className = "status";
  output.value = "";
  checklist.innerHTML = "";
});

function updateChecklist() {
  checklist.innerHTML = "";
  const items = [
    ["Date entrée", dateEntree.value],
    ["Localisation", localisation.value],
    ["Destination", destination.value],
    ["Évènement", evenement.value],
    ["Second numéro", numero2.value]
  ];
  items.forEach(i => {
    const li = document.createElement("li");
    li.textContent = (i[1] ? "✅ " : "❌ ") + i[0];
    checklist.appendChild(li);
  });
}

});
</script>

</body>
</html>
