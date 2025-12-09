<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<title>Formulário Igreja</title>
<style>
    body { font-family: Arial; padding: 20px; }
    .hidden { display: none; }
    .pessoa { margin-bottom: 8px; }
</style>
</head>
<body>

<h2>Formulário de Evento</h2>

<label>Nome da Igreja:</label><br>
<input type="text" id="igreja" required><br>

<input type="checkbox" id="semMinisterio">
<label for="semMinisterio">Não possuo ministério</label>
<br><br>

<h3>Nomes dos Irmãos(ãs)</h3>

<div id="pessoas">
    <!-- Gerado automaticamente via JS -->
</div>

<br>

<label>Tipo de Evento:</label><br>
<select id="tipoEvento">
    <option value="" disabled selected>Selecione</option>
    <option value="Festividade">Festividade</option>
    <option value="Culto">Culto</option>
</select>

<br><br>

<div id="festividadeCampos" class="hidden">
    <label>Nome do Grupo:</label><br>
    <input type="text" id="grupo"><br><br>

    <label>Louvor:</label><br>
    <input type="text" id="louvor"><br><br>
</div>

<button id="enviarBtn">Enviar</button>

<script>
    // Gerar 10 campos de irmãos
    let container = document.getElementById("pessoas");
    for (let i = 1; i <= 10; i++) {
        container.innerHTML += `
            <div class="pessoa">
                Irmão(a) ${i}: <input type="text" id="irmao${i}">
            </div>
        `;
    }

    // Checkbox que remove obrigatoriedade da igreja
    document.getElementById("semMinisterio").addEventListener("change", function() {
        document.getElementById("igreja").required = !this.checked;
    });

    // Exibir campos extras caso festividade
    document.getElementById("tipoEvento").addEventListener("change", function() {
        if (this.value === "Festividade") {
            document.getElementById("festividadeCampos").classList.remove("hidden");
        } else {
            document.getElementById("festividadeCampos").classList.add("hidden");
        }
    });

    // Enviar para WhatsApp
    document.getElementById("enviarBtn").addEventListener("click", function () {
        let numeroWhatsApp = "+55119969831289"; // <- altere aqui

        let igreja = document.getElementById("igreja").value;
        let semMinisterio = document.getElementById("semMinisterio").checked ? "Sim" : "Não";
        let tipoEvento = document.getElementById("tipoEvento").value;

        let pessoas = "";
        for (let i = 1; i <= 10; i++) {
            let nome = document.getElementById("irmao" + i).value;
            if (nome.trim() !== "") {
                pessoas += `- ${nome}\n`;
            }
        }

        let msg = `*Formulário Enviado*\n\n` +
                  `*Possui Ministério:* ${semMinisterio === "Sim" ? "Não" : "Sim"}\n` +
                  (semMinisterio === "Não" ? `*Nome da Igreja:* ${igreja}\n` : ``) +
                  `\n*Irmãos(as):*\n${pessoas}\n` +
                  `*Tipo de Evento:* ${tipoEvento}\n`;

        if (tipoEvento === "Festividade") {
            let grupo = document.getElementById("grupo").value;
            let louvor = document.getElementById("louvor").value;
            msg += `*Nome do Grupo:* ${grupo}\n*Louvor:* ${louvor}\n`;
        }

        let link = `https://wa.me/${numeroWhatsApp}?text=` + encodeURIComponent(msg);

        window.open(link, "_blank");
    });
</script>

</body>
</html>
