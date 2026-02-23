<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>OpenSU Project | Root Environment</title>
    <style>
        :root { --accent: #0070f3; --bg: #000; --green: #00ff41; --red: #ff0000; }
        body { background: var(--bg); color: #fff; font-family: 'Inter', sans-serif; margin: 0; display: flex; align-items: center; justify-content: center; min-height: 100vh; overflow: hidden; }
        
        /* WEB INTERFACE */
        .web-panel { width: 90%; max-width: 450px; text-align: center; background: #0a0a0a; border: 1px solid #222; padding: 40px; border-radius: 20px; box-shadow: 0 0 30px rgba(0,112,243,0.1); }
        h1 { font-size: 2.2rem; margin: 0; font-weight: 900; }
        .upload-slot { border: 2px dashed #333; margin: 30px 0; padding: 50px 20px; border-radius: 12px; cursor: pointer; position: relative; }
        .upload-slot:hover { border-color: var(--accent); }
        input[type="file"] { position: absolute; inset: 0; opacity: 0; cursor: pointer; }
        
        /* STATUS & REBOOT */
        #check-box { display: none; padding: 15px; border-radius: 8px; font-weight: bold; margin-top: 15px; }
        .valid { border: 1px solid var(--green); color: var(--green); background: rgba(0,255,65,0.05); }
        .invalid { border: 1px solid var(--red); color: var(--red); background: rgba(255,0,0,0.05); }

        /* BOOT INTERFACE (OPENSU PROJECT) */
        #boot-screen { display: none; position: fixed; inset: 0; background: #000; flex-direction: column; padding: 30px; z-index: 10000; }
        .recovery-title { color: var(--accent); font-size: 24px; font-weight: 800; text-align: center; margin-bottom: 20px; letter-spacing: 2px; }
        .search-area { width: 100%; padding: 15px; background: #111; border: 1px solid #333; color: #fff; border-radius: 8px; margin-bottom: 20px; }
        .file-scroller { flex-grow: 1; border: 1px solid #222; border-radius: 8px; overflow-y: auto; background: #050505; }
        .file-node { padding: 18px; border-bottom: 1px solid #111; cursor: pointer; display: flex; justify-content: space-between; font-family: monospace; }
        .file-node:hover { background: #111; color: var(--accent); }
        .action-btn { width: 100%; padding: 20px; background: var(--accent); color: #fff; border: none; border-radius: 8px; font-weight: bold; font-size: 1.2rem; cursor: pointer; }
        .install-box { display: none; flex-direction: column; flex-grow: 1; }
    </style>
</head>
<body>

    <div class="web-panel" id="web-site">
        <h1>OpenSU</h1>
        <p style="color:#555; font-size: 11px;">VERIFYING GALE_GLOBAL COMPATIBILITY</p>
        
        <div class="upload-slot">
            <p id="label-text">Place the file here</p>
            <input type="file" id="file-trigger" onchange="startCheck()">
        </div>

        <div id="check-box"></div>
    </div>

    <div id="boot-screen">
        <div class="recovery-title">OPENSU PROJECT</div>
        
        <button class="action-btn" id="pre-install" onclick="openFileSelector()">INSTALL</button>

        <div class="install-box" id="selector-box">
            <input type="text" class="search-area" id="search-input" placeholder="Search for file to root...">
            <div class="file-scroller" id="file-list">
                <div class="file-node">gale_global_v14.img <span>(COMPATIBLE)</span></div>
                <div class="file-node">boot_patched.zip <span>(ENGINE)</span></div>
                <div class="file-node">Download/root_script.zip <span>(READY)</span></div>
                <div class="file-node">stock_kernel.img <span>(UNPATCHED)</span></div>
            </div>
        </div>
    </div>

    <script>
        function startCheck() {
            const file = document.getElementById('file-trigger').files[0];
            const box = document.getElementById('check-box');
            const label = document.getElementById('label-text');
            
            if (!file) return;

            box.style.display = "block";
            // Verifica se o arquivo é gale_global para ativar o Root
            if (file.name.toLowerCase().includes("gale_global")) {
                box.className = "valid";
                box.innerHTML = "ROOT COMPATIBLE: YES<br>Please turn off your device or restart now.";
                label.innerText = "VERIFIED";

                // Simula o celular desligando e ligando (Boot)
                setTimeout(() => {
                    document.getElementById('web-site').style.display = 'none';
                    document.body.style.background = "#000"; // Tela apaga
                    
                    setTimeout(() => {
                        document.getElementById('boot-screen').style.display = 'flex';
                    }, 2500); // Celular "ligando" com a tela do projeto
                }, 5000);

            } else {
                box.className = "invalid";
                box.innerHTML = "ROOT COMPATIBLE: NO<br>Invalid kernel file.";
                label.innerText = "REJECTED";
            }
        }

        function openFileSelector() {
            // Quando clica no botão Install, abre a busca e os arquivos
            document.getElementById('pre-install').style.display = 'none';
            document.getElementById('selector-box').style.display = 'flex';
        }

        // Sistema de busca nos arquivos
        document.getElementById('search-input').addEventListener('input', function(e) {
            let filter = e.target.value.toLowerCase();
            let nodes = document.querySelectorAll('.file-node');
            nodes.forEach(node => {
                node.style.display = node.innerText.toLowerCase().includes(filter) ? "flex" : "none";
            });
        });

        // Simulação de clique no arquivo para dar Root
        document.querySelectorAll('.file-node').forEach(node => {
            node.onclick = function() {
                const name = this.innerText.split(' ')[0];
                alert("OPENSU PROJECT: Patching kernel " + name + "...");
                alert("ROOT INSTALLED SUCCESSFULLY!");
                location.reload();
            };
        });
    </script>
</body>
</html>
