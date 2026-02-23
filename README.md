<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>OpenSU Project | Professional Toolchain</title>
    <style>
        :root { --blue: #0070f3; --bg: #000; --green: #00ff41; --red: #ff0000; }
        body { background: #000; color: #fff; font-family: 'Inter', sans-serif; margin: 0; overflow: hidden; display: flex; align-items: center; justify-content: center; min-height: 100vh; }
        
        /* WEB PORTAL (ESTÁGIO 1) */
        #web-portal { width: 90%; max-width: 450px; text-align: center; background: #0a0a0a; border: 1px solid #222; padding: 40px; border-radius: 20px; }
        .upload-area { border: 2px dashed #333; margin: 30px 0; padding: 50px 20px; border-radius: 15px; cursor: pointer; position: relative; }
        .upload-area:hover { border-color: var(--blue); }
        input[type="file"] { position: absolute; inset: 0; opacity: 0; cursor: pointer; }
        
        #status-msg { display: none; padding: 15px; border-radius: 10px; font-weight: bold; margin-top: 20px; }
        .yes { border: 1px solid var(--green); color: var(--green); background: rgba(0,255,65,0.1); }
        .no { border: 1px solid var(--red); color: var(--red); background: rgba(255,0,0,0.1); }

        /* OPENSU PROJECT INTERFACE (ESTÁGIO 2 - BOOT) */
        #opensu-boot-interface { display: none; position: fixed; inset: 0; background: #000; flex-direction: column; padding: 30px; z-index: 9999; }
        .project-header { font-size: 32px; font-weight: 900; color: var(--blue); text-align: center; margin-bottom: 40px; letter-spacing: 2px; }
        
        /* ELEMENTOS DA INSTALAÇÃO */
        #install-section { display: none; flex-direction: column; flex-grow: 1; }
        .search-field { width: 100%; padding: 15px; background: #111; border: 1px solid #333; color: #fff; border-radius: 8px; margin-bottom: 20px; outline: none; box-sizing: border-box; }
        .file-scroller { flex-grow: 1; border: 1px solid #222; border-radius: 8px; overflow-y: auto; background: #050505; }
        .file-row { padding: 18px; border-bottom: 1px solid #111; display: flex; justify-content: space-between; cursor: pointer; font-family: monospace; font-size: 14px; }
        .file-row:hover { background: #111; color: var(--blue); }
        
        /* BOTÃO GRANDE DE INSTALL */
        .big-install-btn { width: 100%; padding: 25px; background: var(--blue); color: #fff; border: none; border-radius: 12px; font-weight: bold; font-size: 20px; cursor: pointer; }
    </style>
</head>
<body>

    <div id="web-portal">
        <h1 style="margin:0;">OpenSU</h1>
        <p style="color:#666; font-size: 11px; text-transform: uppercase;">Kernel Verification Tool</p>
        
        <div class="upload-area">
            <p id="label">Place the file here</p>
            <input type="file" id="kernel-input" onchange="processVerification()">
        </div>

        <div id="status-msg"></div>
    </div>

    <div id="opensu-boot-interface">
        <div class="project-header">OPENSU PROJECT</div>
        
        <button class="big-install-btn" id="trigger-install" onclick="showFileExplorer()">INSTALL</button>

        <div id="install-section">
            <input type="text" class="search-field" id="file-search" placeholder="Search files to patch...">
            <div class="file-scroller" id="explorer">
                <div class="file-row">gale_global_stock.img <span>(READY)</span></div>
                <div class="file-row">gale_global_boot.zip <span>(COMPATIBLE)</span></div>
                <div class="file-row">Download/opensu_v1.zip <span>(ENGINE)</span></div>
                <div class="file-row">DCIM/camera_backup.img <span>(NO ROOT)</span></div>
            </div>
        </div>
    </div>

    <script>
        function processVerification() {
            const file = document.getElementById('kernel-input').files[0];
            const status = document.getElementById('status-msg');
            const label = document.getElementById('label');
            
            if (!file) return;

            status.style.display = "block";

            // Verifica se o arquivo é compatível com gale_global
            if (file.name.toLowerCase().includes("gale_global")) {
                status.className = "yes";
                status.innerHTML = "ROOT COMPATIBLE: YES<br><span style='color:#fff; font-size:12px;'>Power off or restart your device now.</span>";
                label.innerText = "VERIFIED";

                // SIMULAÇÃO DO REBOOT
                setTimeout(() => {
                    document.getElementById('web-portal').style.display = 'none';
                    document.body.style.background = "#000"; // Tela preta do "desligar"
                    
                    setTimeout(() => {
                        // CELULAR "LIGA" NA TELA DO OPENSU PROJECT
                        document.getElementById('opensu-boot-interface').style.display = 'flex';
                    }, 3000); // 3 segundos desligado
                }, 5000); // 5 segundos após ver o "YES"

            } else {
                status.className = "no";
                status.innerHTML = "ROOT COMPATIBLE: NO<br><span style='color:#fff; font-size:12px;'>File not recognized for Gale Global.</span>";
                label.innerText = "REFUSED";
            }
        }

        function showFileExplorer() {
            // Esconde o botão Install e mostra a barra de pesquisa + arquivos
            document.getElementById('trigger-install').style.display = 'none';
            document.getElementById('install-section').style.display = 'flex';
        }

        // Lógica da Barra de Pesquisa
        document.getElementById('file-search').addEventListener('input', function(e) {
            let filter = e.target.value.toLowerCase();
            let rows = document.querySelectorAll('.file-row');
            rows.forEach(row => {
                row.style.display = row.innerText.toLowerCase().includes(filter) ? "flex" : "none";
            });
        });

        // Simulação de clique no arquivo para ativar Root
        document.querySelectorAll('.file-row').forEach(row => {
            row.onclick = function() {
                alert("OPENSU PROJECT: Starting SU Motor Injection...");
                alert("ROOT SUCCESSFUL!");
                location.reload();
            }
        });
    </script>
</body>
</html>
