function generateScript() {

    let jours = document.getElementById("joursDisplay").innerText;
    let pluscode = document.getElementById("pluscode").value.trim();
    let plusLink = pluscode ? `Maps: https://maps.google.com/?q=${encodeURIComponent(pluscode)}` : "";

    let autorouteBloc = "";
    if (document.getElementById("autoroute").checked) {
        autorouteBloc =
            `Autoroute: ${document.getElementById("numAutoroute").value}, `
          + `PK ${document.getElementById("pk").value}, `
          + `Dir ${document.getElementById("direction").value}`;
    }

    let specBloc = "";
    if (document.getElementById("grosVehicule").checked) {
        specBloc = `Spécificités véhicule: ${document.getElementById("specVehicule").value}`;
    }

    let script =
`📅 Séjour: ${document.getElementById("dateEntree").value} (${jours})
🗣 Langues: ${document.getElementById("langues").value}
🎯 Destination: ${document.getElementById("destination").value}
💥 Évènement: ${document.getElementById("evenement").value}
📞 Second numéro: ${document.getElementById("numero2").value}
📍 Localisation: ${document.getElementById("pays").value} – ${document.getElementById("localisation").value}
${autorouteBloc}
🌍 Plus Code: ${pluscode} ${plusLink}
🚐 Véhicule: ${document.getElementById("vehicule").value}
${specBloc}
📝 Notes: ${document.getElementById("notes").value}`;

    // Nettoyage lignes vides
    script = script.replace(/^\s*$/gm, "");

    document.getElementById("output").value = script;
    generateChecklist();
}
