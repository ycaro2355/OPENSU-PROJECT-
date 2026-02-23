<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>OpenSU | Official Toolchain</title>
    <style>
        :root { --blue: #0070f3; --green: #00ff41; --bg: #000; }
        body { background: var(--bg); color: #fff; font-family: 'Segoe UI', sans-serif; margin: 0; overflow: hidden; display: flex; align-items: center; justify-content: center; min-height: 100vh; }
        
        /* SITE INICIAL - VALIDATION STAGE */
        #web-site { width: 90%; max-width: 420px; text-align: center; background: #0a0a0a; border: 1px solid #222; padding: 40px; border-radius: 20px; transition: 0.5s; }
        h1 { font-size: 2.5rem; letter-spacing: -1px; margin: 0; }
        .upload-zone { border: 2px dashed #333; margin: 30px 0; padding: 40px; border-radius: 12px; cursor: pointer; position: relative; }
        .upload-zone:hover { border-color: var(--blue); }
        input[type="file"] { position: absolute; inset: 0; opacity: 0; cursor: pointer; }
        #status-info { display: none; padding: 15px; border-radius: 8px; font-weight: bold; font-size: 13px; margin-top: 20px; }
        .yes { border: 1px solid var(--green); color: var(--green); background: rgba(0, 255, 65, 0.1); }

        /* INTERFACE DE BOOT (OPENSU PROJECT) - SÓ NO REBOOT */
        #boot-loader { display: none; position: fixed; inset: 0; background: #000; flex-direction: column; align-items: center; justify-content: center; z-index: 9999; padding: 20px; }
        .project-logo { font-size: 35px; font-weight: 900; color: var(--blue); margin-bottom: 50px; text-shadow: 0 0 20px rgba(0,112,243,0.5); }
        
        /* ELEMENTOS DO INSTALADOR */
        #install-interface { display: none; width: 100%; max-width: 500px; flex-direction: column; }
        .search-input { width: 100%; padding: 15px; background: #111; border: 1px solid #333; color: #fff; border-radius: 10px; margin-bottom: 15px; outline: none; box-sizing: border-box; }
        .file-scroller { height: 250px; border: 1px solid #222; border-radius: 10px; overflow-y: auto; background: #050505; margin-bottom: 20px; }
        .file-card { padding: 15px; border-bottom: 1px solid #111; cursor: pointer; font-family: monospace; font-size: 14px; display: flex; justify-content: space-between; }
        .file-card:hover { background: #111; color: var(--blue); }
        
        .btn-install { width: 100%; padding: 20px; background: var(--blue); color: #fff; border: none; border-radius: 10px; font-weight: bold; font-size: 18px; cursor: pointer; }
    </style>
</head>
<body>

    <div id="web-site">
        <h1>OpenSU</h1>
        <p style="color:#555; font-size: 11px; text-transform: uppercase;">Systemless Root Patcher</p>
        
        <div class="upload-zone">
            <p id="label">Place the file here</p>
            <input type="file" id="file-check" onchange="runValidation()">
        </div>

        <div id="status-info"></div>
    </div>

    <div id="boot-loader">
        <div class="project-logo" id="logo-text">OPENSU PROJECT</div>
        
        <button class="btn-install" id="init-install" onclick="showFileSelector()">INSTALL</button>

        <div id="install-interface">
            <input type="text" class="search-input" id="search" placeholder="Search for file to activate root...">
            <div class="file-scroller" id="explorer">
                <div class="file-card">gale_global_boot.img <span>(COMPATIBLE: YES)</span></div>
                <div class="file-card">boot_original.img <span>(COMPATIBLE: YES)</span></div>
                <div class="file-card">Download/opensu_v1.zip <span>(ENGINE)</span></div>
                <div class="file-card">DCIM/camera_backup.img <span>(COMPATIBLE: NO)</span></div>
            </div>
        </div>
    </div>

    <script>
        function runValidation() {
            const file = document.getElementById('file-check').files[0];
            const status = document.getElementById('status-info');
            const label = document.getElementById('label');

            if (file && file.name.toLowerCase().includes("gale_global")) {
                status.className = "yes";
                status.style.display = "block";
                status.innerHTML = "ROOT COMPATIBLE: YES<br><span style='color:white'>Please Power Off or Restart your device now.</span>";
                label.innerText = "FILE VERIFIED";

                // SIMULAÇÃO DO REBOOT (Desligar e Ligar)
                setTimeout(() => {
                    document.getElementById('web-site').style.display = 'none';
                    document.body.style.background = "#000"; // Tela preta (Desligado)
                    
                    setTimeout(() => {
                        // TELA DO OPENSU PROJECT APARECE AO LIGAR
                        document.getElementById('boot-loader').style.display = 'flex';
                    }, 3000); // 3 segundos desligado
                }, 5000); // 5 segundos após o aviso

            } else {
                alert("COMPATIBILITY ERROR: This is not a gale_global file.");
            }
        }

        function showFileSelector() {
            // O botão de Install some e abre a busca + arquivos
            document.getElementById('init-install').style.display = 'none';
            document.getElementById('logo-text').style.fontSize = '20px';
            document.getElementById('logo-text').style.marginBottom = '20px';
            document.getElementById('install-interface').style.display = 'flex';
        }

        // Lógica da Barra de Pesquisa
        document.getElementById('search').addEventListener('input', function(e) {
            let filter = e.target.value.toLowerCase();
            let items = document.querySelectorAll('.file-card');
            items.forEach(item => {
                item.style.display = item.innerText.toLowerCase().includes(filter) ? "flex" : "none";
            });
        });

        // Clique final no arquivo
        document.querySelectorAll('.file-card').forEach(item => {
            item.onclick = function() {
                if(this.innerText.includes("YES")) {
                    alert("OPENSU PROJECT: Patching kernel... Injecting su motor...");
                    alert("ROOT ACTIVATED!");
                } else {
                    alert("ERROR: This file cannot activate Root.");
                }
                location.reload();
            };
        });
    </script>
</body>
</html>
