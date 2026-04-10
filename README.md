<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>AIDE ETR N1 – Générateur</title>

<style>
body { font-family: Arial, sans-serif; background:#eef2f7; }
.wrapper {
  max-width: 750px;
  margin: 40px auto;
  background: #fff;
  padding: 25px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,.15);
}
label { font-weight:600; display:block; margin-top:15px; }
input, textarea {
  width:100%;
  padding:8px;
  margin-top:5px;
}
button {
  margin-top:20px;
  padding:12px;
  width:100%;
  font-size:16px;
  cursor:pointer;
}
#output { height:180px; margin-top:15px; }
</style>
</head>

<body>

<div class="wrapper">
  <h2>AIDE ETR N1 – Ouverture dossier</h2>

  <label>Date d’entrée dans le pays</label>
  <input type="date" id="dateEntree">

  <label>Langues parlées</label>
  <input type="text" id="langues">

  <label>Destination finale</label>
  <input type="text" id="destination">

  <label>Localisation précise</label>
  <textarea id="localisation"></textarea>

  <label>Évènement / panne</label>
  <textarea id="evenement"></textarea>

  <button id="btnGenerate">Générer le script</button>

  <textarea id="output" readonly></textarea>

  <button id="btnCopy">Copier le script</button>
</div>

<script>
document.addEventListener("DOMContentLoaded", function () {

  document.getElementById("btnGenerate").addEventListener("click", function () {
    const script =
`📅 Entrée pays : ${dateEntree.value}
🗣 Langues : ${langues.value}
🎯 Destination : ${destination.value}
📍 Localisation : ${localisation.value}
💥 Évènement : ${evenement.value}`;

    output.value = script;
  });

  document.getElementById("btnCopy").addEventListener("click", function () {
    navigator.clipboard.writeText(output.value)
      .then(() => alert("✅ Script copié"))
      .catch(() => alert("❌ Copie impossible"));
  });

});
</script>

</body>
</html>
