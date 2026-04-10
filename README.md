<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>AIDE ETR N1 – TEST</title>
</head>
<body>

<h1>AIDE ETR N1 – TEST OK</h1>

<button id="btnTest">Tester le bouton</button>

<script>
document.getElementById("btnTest").addEventListener("click", function () {
  document.body.insertAdjacentHTML(
    "beforeend",
    "<p>✅ JavaScript exécuté correctement</p>"
  );
});
</script>

</body>
</html>
