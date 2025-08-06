<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Leitor de XML - Valor Total</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 40px;
    }
    .box {
      border: 2px dashed #ccc;
      padding: 30px;
      text-align: center;
      border-radius: 10px;
      max-width: 500px;
      margin: auto;
    }
    #resultado {
      margin-top: 20px;
      font-weight: bold;
      font-size: 1.2em;
      color: green;
    }
  </style>
</head>
<body>
  <div class="box">
    <h2>Enviar XML e Ver Valor Total</h2>
    <input type="file" id="xmlFile" accept=".xml" />
    <div id="resultado"></div>
  </div>

  <script>
    document.getElementById('xmlFile').addEventListener('change', function(e) {
      const file = e.target.files[0];
      if (!file) return;

      const reader = new FileReader();
      reader.onload = function(event) {
        const xmlString = event.target.result;
        const parser = new DOMParser();
        const xmlDoc = parser.parseFromString(xmlString, "text/xml");

        // Tenta encontrar a tag <vTot> ou <vNF> (comum em notas fiscais)
        const valorTag = xmlDoc.querySelector("vTot, vNF");

        if (valorTag) {
          const valor = valorTag.textContent;
          document.getElementById("resultado").textContent = "Valor Total: R$ " + valor;
        } else {
          document.getElementById("resultado").textContent = "Tag <vTot> ou <vNF> não encontrada no XML.";
        }
      };
      reader.readAsText(file);
    });
  </script>
</body>
</html>
