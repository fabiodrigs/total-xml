<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Somador de XMLs</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center p-6">
  <div class="bg-white shadow-xl rounded-2xl p-8 max-w-xl w-full">
    <h1 class="text-2xl font-bold text-center text-gray-800 mb-6">📄 Somador de Valores XML</h1>
    
    <label class="block text-sm font-medium text-gray-700 mb-2">Selecione os arquivos XML:</label>
    <input type="file" id="xmlFiles" accept=".xml" multiple 
           class="block w-full text-sm text-gray-900 border border-gray-300 rounded-lg cursor-pointer bg-gray-50 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 p-2 mb-4" />

    <div id="resultado" class="text-xl font-semibold text-green-600 mt-4 text-center"></div>

    <ul id="lista" class="mt-4 space-y-2 text-sm text-gray-700 list-disc list-inside"></ul>
  </div>

  <script>
    document.getElementById('xmlFiles').addEventListener('change', function (e) {
      const files = e.target.files;
      let total = 0;
      let lidos = 0;
      let listaValores = '';

      const lista = document.getElementById('lista');
      const resultado = document.getElementById('resultado');
      lista.innerHTML = '';
      resultado.textContent = '';

      if (files.length === 0) {
        resultado.textContent = "Nenhum arquivo selecionado.";
        return;
      }

      Array.from(files).forEach(file => {
        const reader = new FileReader();

        reader.onload = function (event) {
          try {
            const xmlString = event.target.result;
            const parser = new DOMParser();
            const xmlDoc = parser.parseFromString(xmlString, "text/xml");

            // Aqui você pode trocar "vNF" por outro campo (ex: "vProd")
            const valorTag = xmlDoc.getElementsByTagName("vNF")[0];

            if (valorTag && valorTag.textContent) {
              const valor = parseFloat(valorTag.textContent.replace(',', '.'));
              if (!isNaN(valor)) {
                total += valor;
                listaValores += `<li>${file.name}: R$ ${valor.toFixed(2)}</li>`;
              } else {
                listaValores += `<li>${file.name}: Valor inválido</li>`;
              }
            } else {
              listaValores += `<li>${file.name}: Tag &lt;vNF&gt; não encontrada</li>`;
            }
          } catch (err) {
            listaValores += `<li>${file.name}: Erro ao processar</li>`;
          }

          lidos++;
          if (lidos === files.length) {
            resultado.textContent = "Total Somado: R$ " + total.toFixed(2);
            lista.innerHTML = listaValores;
          }
        };

        reader.readAsText(file);
      });
    });
  </script>
</body>
</html>
