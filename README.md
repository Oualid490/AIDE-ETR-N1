<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>AIDE ETR N1 – Ouverture de dossier étranger</title>

<style>
    body {
        font-family: Arial, sans-serif;
        background: #eef2f7;
        margin: 0;
        padding: 0;
    }

    .wrapper {
        max-width: 820px;
        margin: 40px auto;
        background: #ffffff;
        padding: 30px 35px;
        border-radius: 14px;
        box-shadow: 0 4px 15px rgba(0,0,0,0.18);
    }

    h1 {
        text-align: center;
        color: #154360;
        font-size: 30px;
        margin-bottom: 5px;
        font-weight: 800;
    }

    .subtitle {
        text-align: center;
        color: #566573;
        margin-bottom: 25px;
        font-size: 15px;
    }

    h2 {
        font-size: 18px;
        margin-top: 25px;
        color: #1b4f72;
        border-left: 5px solid #1b4f72;
        padding-left: 10px;
    }

    label {
        font-weight: 700;
        margin-top: 15px;
        display: block;
        color: #1f2d3d;
    }

    .small-help {
        font-size: 12px;
        color: #5f6a6a;
        margin-bottom: 6px;
    }

    input, textarea, select {
        width: 100%;
        padding: 11px;
        margin-top: 4px;
        border-radius: 6px;
        border: 1px solid #cbd5e1;
        background: #f9fbfc;
        font-size: 15px;
        box-sizing: border-box;
    }

    textarea {
        resize: vertical;
        min-height: 70px;
    }

    button {
        margin-top: 18px;
        width: 100%;
        padding: 13px;
        background: #154360;
        color: white;
        border: none;
        border-radius: 6px;
        font-size: 18px;
        cursor: pointer;
        font-weight: 600;
    }

    button:hover {
        background: #0d2c42;
    }

    .reset-btn {
        background: #a93226;
    }
    .reset-btn:hover {
        background: #7b241c;
    }

    .copy-btn {
        background: #1e8449;
    }
    .copy-btn:hover {
        background: #145a32;
    }

    #output {
        height: 200px;
        background: #f4f6f9;
        margin-top: 15px;
        white-space: pre-line;
    }

    .status {
        padding: 10px;
        margin-top: 10px;
        border-radius: 8px;
        font-weight: bold;
        font-size: 14px;
    }
    .status-green { background:#d4efdf;color:#145a32; }
    .status-orange { background:#fdebd0;color:#af601a; }
    .status-red { background:#f5b7b1;color:#922b21; }

    ul.checklist { list-style-type:none; padding-left:0; }
    ul.checklist li { margin:4px 0; }
</style>
</head>

<body>

<div class="wrapper">

<h1>AIDE ETR N1</h1>
<div class="subtitle">Ouverture de dossier étranger – pour une prise en charge complète et sans erreur.</div>

<!-- SECTION 1 -->
<h2>1. Informations générales</h2>

<label>Date d’entrée dans le pays</label>
<input type="date" id="dateEntree">
<div id="joursDisplay" class="status"></div>
<label><input type="checkbox" id="panneRecente"> Panne récente (&lt; 30 jours)</label>

<label>Langues parlées</label>
<input type="text" id="langues">

<label>Destination finale</label>
<input type="text" id="destination">

<label>Raison de la panne</label>
<textarea id="evenement"></textarea>

<label>Second numéro</label>
<input type="text" id="numero2">

<label>Notes internes</label>
<textarea id="notes"></textarea>

<!-- SECTION 2 -->
<h2>2. Localisation</h2>

<label>Pays</label>
<input list="listePays" id="pays" placeholder="Commence à taper…">
<datalist id="listePays">
<option>France</option><option>Espagne</option><option>Portugal</option>
<option>Italie</option><option>Allemagne</option><option>Belgique</option>
<option>Luxembourg</option><option>Suisse</option><option>Pays-Bas</option>
<option>Autriche</option><option>Royaume-Uni</option>
</datalist>

<label>Localisation précise</label>
<div class="small-help">Si autoroute : PK + direction. Si Espagne : PK indispensable.</div>
<textarea id="localisation"></textarea>

<label><input type="checkbox" id="autoroute"> Sur autoroute</label>

<div id="zoneAutoroute" style="display:none;">
<label>Autoroute</label><input id="numAutoroute">
<label>PK</label><input id="pk">
<label>Direction</label><input id="direction">
</div>

<label>Google Plus Code</label>
<input type="text" id="pluscode" placeholder="Ex : 87G8+4W">
<div id="pluscodeStatus"></div>

<!-- SECTION 3 -->
<h2>3. Véhicule</h2>

<label>Type véhicule</label>
<input list="listeVehicules" id="vehicule" placeholder="Commence à taper…">
<datalist id="listeVehicules">
<option>Véhicule léger</option><option>Véhicule utilitaire</option>
<option>Camping-car</option><option>Moto</option><option>Taxi</option>
<option>Caravane</option><option>Remorque</option><option>Voiture de location</option>
</datalist>

<label><input type="checkbox" id="grosVehicule"> Gros véhicule ou spécificités</label>
<div id="zoneSpecVehicule" style="display:none;">
<label>Spécificités</label>
<textarea id="specVehicule"></textarea>
</div>

<!-- SECTION 4 -->
<h2>4. Script</h2>

<button onclick="generateScript()">Générer</button>
<textarea id="output" readonly></textarea>

<button class="copy-btn" onclick="copyScript()">Copier</button>
<button class="reset-btn" onclick="resetForm()">Réinitialiser</button>

<!-- SECTION 5 -->
<h2>5. Checklist N1</h2>
<div id="checklist"></div>

</div>

<script>
// -------- CALCUL JOURS --------
document.getElementById("dateEntree").addEventListener("change", calculateDays);
document.getElementById("panneRecente").addEventListener("change", calculateDays);

function calculateDays() {
    let d = document.getElementById("dateEntree").value;
    let disp = document.getElementById("joursDisplay");
    if (!d) { disp.innerHTML=""; return; }

    let entree = new Date(d);
    let now = new Date();
    let diff = Math.floor((now - entree)/(1000*60*60*24));
    let panne = document.getElementById("panneRecente").checked;

    if (diff<60) disp.className="status status-green";
    else if (diff<=90) disp.className="status status-orange";
    else disp.className= panne ? "status status-orange" : "status status-red";

    let assist = (diff>90 && !panne)?"❌ non assistable (>90j)":"✅ assistable";
    disp.innerHTML=`Séjour: ${diff}j – ${assist}`;
}

// -------- AUTOROUTE --------
document.getElementById("autoroute").addEventListener("change",()=>{
document.getElementById("zoneAutoroute").style.display=
document.getElementById("autoroute").checked?"block":"none";});

// -------- GROS VEHICULE --------
document.getElementById("grosVehicule").addEventListener("change",()=>{
document.getElementById("zoneSpecVehicule").style.display=
document.getElementById("grosVehicule").checked?"block":"none";});

// -------- PLUS CODE --------
document.getElementById("pluscode").addEventListener("input",()=>{
let pc=document.getElementById("pluscode").value.trim();
let s=document.getElementById("pluscodeStatus");
let r=/^[A-Z0-9]{4}\+[A-Z0-9]{2}$/i;
if(r.test(pc)){s.innerHTML="✅ valide";s.style.color="green";}
else{s.innerHTML="❌ invalide";s.style.color="red";}
});

// -------- GENERER SCRIPT (VERSION COMPACTE) --------
function generateScript(){
    let jours = document.getElementById("joursDisplay").innerText;
    let pc = document.getElementById("pluscode").value.trim();
    let pcLink = pc?`Maps: https://maps.google.com/?q=${encodeURIComponent(pc)}`:"";

    let autoroute="";
    if(document.getElementById("autoroute").checked){
        autoroute = `Autoroute: ${document.getElementById("numAutoroute").value}, `
                  + `PK ${document.getElementById("pk").value}, `
                  + `Dir ${document.getElementById("direction").value}`;
    }

    let spec="";
    if(document.getElementById("grosVehicule").checked){
        spec=`Spécificités: ${document.getElementById("specVehicule").value}`;
    }

    let script =
`📅 Séjour: ${document.getElementById("dateEntree").value} (${jours})
🗣 Langues: ${document.getElementById("langues").value}
🎯 Destination: ${document.getElementById("destination").value}
💥 Panne: ${document.getElementById("evenement").value}
📞 Numéro 2: ${document.getElementById("numero2").value}
📍 Localisation: ${document.getElementById("pays").value} – ${document.getElementById("localisation").value}
${autoroute}
🌍 Plus Code: ${pc} ${pcLink}
🚐 Véhicule: ${document.getElementById("vehicule").value}
${spec}
📝 Notes: ${document.getElementById("notes").value}`;

    script = script.replace(/^\s*$/gm,"");
    document.getElementById("output").value=script;

    generateChecklist();
}

// -------- CHECKLIST --------
function generateChecklist(){
    let out="<ul class='checklist'>";
    function c(t,ok){ out+=`<li>${ok?"✅":"❌"} ${t}</li>`; }

    c("Date entrée", document.getElementById("dateEntree").value!=="");
    c("Langues", document.getElementById("langues").value!=="");
    c("Destination", document.getElementById("destination").value!=="");
    c("Localisation", document.getElementById("localisation").value!=="");
    c("Panne", document.getElementById("evenement").value!=="");
    c("Second numéro", document.getElementById("numero2").value!=="");
    c("Type véhicule", document.getElementById("vehicule").value!=="");

    let pc=document.getElementById("pluscode").value.trim();
    let r=/^[A-Z0-9]{4}\+[A-Z0-9]{2}$/i;
    c("Plus Code OK", pc==="" || r.test(pc));

    out+="</ul>";
    document.getElementById("checklist").innerHTML=out;
}

// -------- COPIER --------
function copyScript(){
    let txt=document.getElementById("output").value;
    navigator.clipboard.writeText(txt)
        .then(()=>alert("✅ Script copié !"))
        .catch(()=>alert("❌ Impossible de copier"));
}

// -------- RESET --------
function resetForm(){
    document.querySelectorAll("input, textarea").forEach(x=>x.value="");
    document.getElementById("autoroute").checked=false;
    document.getElementById("grosVehicule").checked=false;
    document.getElementById("zoneAutoroute").style.display="none";
    document.getElementById("zoneSpecVehicule").style.display="none";
    document.getElementById("joursDisplay").innerHTML="";
    document.getElementById("pluscodeStatus").innerHTML="";
    document.getElementById("output").value="";
    document.getElementById("checklist").innerHTML="";
}
</script>

</body>
</html>

