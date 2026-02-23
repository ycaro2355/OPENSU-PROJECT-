<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>OpenSU Project | Kernel Toolchain</title>
    <style>
        :root { --blue: #0070f3; --bg: #000; --card: #111; --text: #eaeaea; }
        body { background: var(--bg); color: var(--text); font-family: 'Segoe UI', sans-serif; margin: 0; display: flex; justify-content: center; align-items: center; min-height: 100vh; overflow: hidden; }
        
        /* MODERN SITE DESIGN */
        .container { width: 90%; max-width: 500px; background: var(--card); padding: 40px; border-radius: 20px; border: 1px solid #333; text-align: center; box-shadow: 0 0 50px rgba(0, 112, 243, 0.2); }
        h1 { font-size: 2.5rem; margin: 0; background: linear-gradient(to right, #0070f3, #00dfd8); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .step-info { color: #888; font-size: 14px; margin-bottom: 30px; text-transform: uppercase; }
        
        /* THE "PLACE THE FILE" BUTTON */
        .upload-area { border: 2px dashed #444; padding: 30px; border-radius: 12px; cursor: pointer; transition: 0.3s; position: relative; }
        .upload-area:hover { border-color: var(--blue); background: rgba(0, 112, 243, 0.05); }
        input[type="file"] { position: absolute; width: 100%; height: 100%; top: 0; left: 0; opacity: 0; cursor: pointer; }

        /* RECOVERY INTERFACE (HIDDEN INITIALLY) */
        #recovery-screen { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: #000; z-index: 1000; flex-direction: column; padding: 20px; box-sizing: border-box; }
        .search-bar { width: 100%; padding: 15px; border-radius: 8px; border: 1px solid #333; background: #111; color: white; margin-top: 20px; box-sizing: border-box; }
        .file-list { flex-grow: 1; border: 1px solid #222; margin-top: 15px; border-radius: 8px; overflow-y: auto; text-align: left; }
        .file-item { padding: 15px; border-bottom: 1px solid #222; cursor: pointer; }
        .file-item:hover { background: #1a1a1a; color: var(--blue); }
        .install-btn { width: 100%; padding: 20px; background: var(--blue); color: white; border: none; border-radius: 8px; font-weight: bold; font-size: 18px; margin-top: 15px; cursor: pointer; }

        .loading-bar { height: 4px; width: 0%; background: var(--blue); position: fixed; top: 0; left: 0; transition: 0.5s; }
    </style>
</head>
<body>

    <div id="loading" class="loading-bar"></div>

    <div class="container" id="web-site">
        <h1>OPENSU</h1>
        <div class="step-info">Engine: Systemless SU Toolchain</div>
        
        <div class="upload-area">
            <p id="upload-text">Place the file here</p>
            <input type="file" id="boot-input" onchange="processFile()">
        </div>
        <p style="font-size: 12px; color: #555; margin-top: 20px;">Upload your boot.img to inject OpenSU motor.</p>
    </div>

    <div id="recovery-screen">
        <h2 style="color: var(--blue); margin: 0;">OPENSU PROJECT</h2>
        <p style="font-size: 10px; color: #888;">RECOVERY INSTALLER v1.0</p>
        
        <input type="text" class="search-bar" placeholder="Search files (.zip, .img)..." id="search">
        
        <div class="file-list" id="files">
            <div class="file-item">opensu_patched_gale.zip</div>
            <div class="file-item">boot_original.img</div>
            <div class="file-item">Download/custom_mod.zip</div>
            <div class="file-item">TWRP/backups/stock.img</div>
        </div>

        <button class="install-btn" onclick="startInstall()">INSTALL</button>
    </div>

    <script>
        // Simulação do Motor e Processamento
        function processFile() {
            const input = document.getElementById('boot-input');
            const text = document.getElementById('upload-text');
            const loader = document.getElementById('loading');
            
            if (input.files.length > 0) {
                text.innerText = "Patching: " + input.files[0].name;
                loader.style.width = "100%";
                
                setTimeout(() => {
                    alert("Patch Successful! Now reboot your device to OpenSU Recovery.");
                    // Simula o "ligar e desligar" entrando na interface de instalação
                    document.getElementById('web-site').style.display = 'none';
                    document.getElementById('recovery-screen').style.display = 'flex';
                }, 2500);
            }
        }

        function startInstall() {
            const btn = document.querySelector('.install-btn');
            btn.innerText = "FLASHING SU MOTOR...";
            btn.style.background = "#222";
            
            setTimeout(() => {
                alert("OPENSU PROJECT: Root Activated Successfully!");
                location.reload();
            }, 3000);
        }

        // Lógica da Barra de Pesquisa
        document.getElementById('search').addEventListener('input', function(e) {
            let filter = e.target.value.toLowerCase();
            let items = document.querySelectorAll('.file-item');
            items.forEach(item => {
                if(item.innerText.toLowerCase().includes(filter)) {
                    item.style.display = "block";
                } else {
                    item.style.display = "none";
                }
            });
        });
    </script>
</body>
</html>
