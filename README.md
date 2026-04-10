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
  max-width: 820px;
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

.status {
  margin-top:10px;
  padding:10px;
  border-radius:6px;
  font-weight:bold;
}

.green { background:#d4efdf; color:#145a32; }
.orange { background:#fdebd0; color:#af601a; }
.red { background:#f5b7b1; color:#922b21; }

#output {
  height:220px;
  margin-top:15px;
  white-space:pre-line;
}

.checklist {
  margin-top:15px;
  padding-left:0;
  list-style:none;
}

.checklist li {
  margin:5px 0;
}
</style>
</head>

<body>

<div class="wrapper">

<h1>AIDE ETR N1</h1>
<div class="subtitle">
Outil d’aide à l’ouverture de dossier étranger – N1<br>
Objectif : ouverture complète dès le premier contact
</div>

<h2>1. Contexte client</h2>

<label>Date d’entrée dans le pays</label>
<div class="help">Date d’arrivée effective du client dans le pays étranger</div>
<input type="date" id="dateEntree">
<div id="joursInfo" class="status"></div>

<label>
<input type="checkbox" id="panneRecente">
 Panne récente (&lt; 30 jours)
</label>
<div class="help">À cocher si la panne est récente même si le séjour est long</div>

<label>Langues parlées</label>
<div class="help">Ex : Français / Anglais – utile pour prestataires</div>
<input type="text" id="langues">

<label>Destination finale</label>
<div class="help">Ville, hôtel, famille, lieu de séjour</div>
<input type="text" id="destination">

<label>Détail précis de l’évènement</label>
<div class="help">Ce qu’il s’est passé exactement</div>
<textarea id="evenement"></textarea>

<label>Second numéro de téléphone</label>
<div class="help">Numéro local ou autre contact joignable</div>
<input type="text" id="numero2">

<label>Détails importants (passagers, enfants, animaux, bagages…)</label>
<div class="help">Toute information utile pour la suite du dossier</div>
<textarea id="notes"></textarea>

<h2>2. Localisation</h2>

<label>Pays</label>
<div class="help">Pays où se trouve le client au moment de l’évènement</div>
<input type="text" id="pays">

<label>Localisation précise</label>
<div class="help">Adresse exacte ou description. Si autoroute : PK + direction</div>
<textarea id="localisation"></textarea>

<label>
<input type="checkbox" id="autoroute">
 Client sur autoroute
</label>

<div id="zoneAutoroute" style="display:none;">
  <label>Autoroute</label>
  <input type="text" id="numAutoroute">

  <label>PK</label>
  <input type="text" id="pk">

  <label>Direction</label>
  <input type="text" id="direction">
</div>

<h2>3. Véhicule</h2>

<label>Type de véhicule</label>
<div class="help">VL, utilitaire, camping-car, moto, location…</div>
<input type="text" id="vehicule">

<label>
<input type="checkbox" id="grosVehicule">
 Gros véhicule ou spécificités
</label>

<div id="zoneSpecVehicule" style="display:none;">
  <label>Spécificités du véhicule</label>
  <div class="help">Gabarit, poids, équipements, remorque…</div>
  <textarea id="specVehicule"></textarea>
</div>

<h2>4. Génération du script</h2>

<button id="btnGenerate">Générer le script</button>
<textarea id="output" readonly></textarea>

<button id="btnCopy">Copier le script</button>
<button id="btnReset">Réinitialiser</button>

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
    if (panneRecente.checked) {
      joursInfo.className = "status orange";
      joursInfo.textContent = `Séjour : ${days} jours – panne récente`;
    } else {
      joursInfo.className = "status red";
      joursInfo.textContent = `Séjour : ${days} jours – hors conditions`;
    }
  }
}

dateEntree.addEventListener("change", updateDays);
panneRecente.addEventListener("change", updateDays);

document.getElementById("autoroute").addEventListener("change", e => {
  document.getElementById("zoneAutoroute").style.display = e.target.checked ? "block" : "none";
});

document.getElementById("grosVehicule").addEventListener("change", e => {
  document.getElementById("zoneSpecVehicule").style.display = e.target.checked ? "block" : "none";
});

document.getElementById("btnGenerate").addEventListener("click", function () {

  const script =
`📅 Date entrée pays : ${dateEntree.value}
🗣 Langues : ${langues.value}
🎯 Destination : ${destination.value}

💥 Évènement :
${evenement.value}

📍 Localisation :
${pays.value} – ${localisation.value}

🚐 Véhicule : ${vehicule.value}
${grosVehicule.checked ? "Spécificités : " + specVehicule.value : ""}

📞 Second numéro : ${numero2.value}

📝 Infos complémentaires :
${notes.value}`;

  output.value = script;

  updateChecklist();
});

document.getElementById("btnCopy").addEventListener("click", function () {
  navigator.clipboard.writeText(output.value)
    .then(() => alert("✅ Script copié"))
    .catch(() => alert("❌ Copie impossible"));
});

document.getElementById("btnReset").addEventListener("click", function () {
  document.querySelectorAll("input, textarea").forEach(el => el.value = "");
  document.querySelectorAll("input[type=checkbox]").forEach(el => el.checked = false);
  document.getElementById("zoneAutoroute").style.display = "none";
  document.getElementById("zoneSpecVehicule").style.display = "none";
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
    ["Véhicule", vehicule.value],
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
