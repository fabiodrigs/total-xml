<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Somar Valores de Múltiplos XMLs</title>
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
      max-width: 600px;
      margin: auto;
    }
    #resultado {
      margin-top: 20px;
      font-weight: bold;
      font-size: 1.2em;
      color: green;
    }
    #lista {
      margin-top: 15px;
      font-size: 0.95em;
      text-align: left;
    }
  </style>
</head>
<body>
  <div class="box">
    <h2>Somar Total de Vários Arquivos XML</h2>
    <input type="file" id="xmlFiles" accept=".xml" multiple />
    <div id="resultado"></div>
    <div id="lista"></div>
  </div>

  <script>
    document.getElementById('xmlFiles').addEventListener('change', function (e) {
      const files = e.target.files;
      let total = 0;
      let lidos = 0;
      let listaValores = '';

      if (files.length === 0) {
        document.getElementById("resultado").textContent = "Nenhum arquivo selecionado.";
        return;
      }

      Array.from(files).forEach(file => {
        const reader = new FileReader();

        reader.onload = function (event) {
          const xmlString = event.target.result;
          const parser = new DOMParser();
          const xmlDoc = parser.parseFromString(xmlString, "text/xml");

          // Tente encontrar a tag <vTot> ou <vNF> (padrão de NF-e)
          const valorTag = xmlDoc.querySelector("vTot, vNF");

          if (valorTag) {
            const valor = parseFloat(valorTag.textContent.replace(',', '.'));
            if (!isNaN(valor)) {
              total += valor;
              listaValores += `<li>${file.name}: R$ ${valor.toFixed(2)}</li>`;
            } else {
              listaValores += `<li>${file.name}: Valor inválido</li>`;
            }
          } else {
            listaValores += `<li>${file.name}: Tag <vTot> ou <vNF> não encontrada</li>`;
          }

          lidos++;
          if (lidos === files.length) {
            document.getElementById("resultado").textContent = "Total Somado: R$ " + total.toFixed(2);
            document.getElementById("lista").innerHTML = "<ul>" + listaValores + "</ul>";
          }
        };

        reader.readAsText(file);
      });
    });
  </script>
</body>
</html>
