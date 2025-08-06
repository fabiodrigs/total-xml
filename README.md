<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Teste Múltiplos XMLs</title>
</head>
<body>
  <h2>Teste de Múltiplos Arquivos</h2>
  <input type="file" id="xmlFiles" accept=".xml" multiple />
  <ul id="fileList"></ul>

  <script>
    document.getElementById("xmlFiles").addEventListener("change", function (e) {
      const files = e.target.files;
      const list = document.getElementById("fileList");
      list.innerHTML = "";
      Array.from(files).forEach(file => {
        const item = document.createElement("li");
        item.textContent = file.name;
        list.appendChild(item);
      });
    });
  </script>
</body>
</html>
