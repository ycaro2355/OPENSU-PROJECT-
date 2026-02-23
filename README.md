<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>OpenSU Project | Global Toolchain</title>
    <style>
        :root { --accent: #0070f3; --bg: #000; --success: #00ff41; --error: #ff4141; }
        body { background: var(--bg); color: #fff; font-family: 'Segoe UI', sans-serif; margin: 0; display: flex; justify-content: center; align-items: center; min-height: 100vh; overflow: hidden; }
        
        /* MODERN INTERFACE */
        .container { width: 90%; max-width: 450px; background: #111; padding: 40px; border-radius: 24px; border: 1px solid #333; text-align: center; }
        h1 { font-size: 2.2rem; margin: 0; font-weight: 800; }
        .sub { font-size: 11px; color: #666; text-transform: uppercase; margin-bottom: 30px; letter-spacing: 2px; }
        
        /* UPLOAD AREA */
        .upload-area { border: 2px dashed #444; padding: 50px; border-radius: 16px; cursor: pointer; position: relative; transition: 0.3s; }
        .upload-area:hover { border-color: var(--accent); background: rgba(0, 112, 243, 0.05); }
        .upload-area p { margin: 0; font-weight: bold; color: #888; }
        input[type="file"] { position: absolute; inset: 0; opacity: 0; cursor: pointer; }

        /* RECOVERY UI (OPENSU PROJECT) */
        #recovery-ui { display: none; position: fixed; inset: 0; background: #000; z-index: 1000; flex-direction: column; padding: 30px; }
        .search-bar { width: 100%; padding: 18px; border-radius: 12px; border: 1px solid #222; background: #0a0a0a; color: #fff; font-size: 16px; margin: 20px 0; outline: none; box-sizing: border-box; }
        .file-list { flex-grow: 1; border: 1px solid #222; border-radius: 12px; overflow-y: auto; background: #050505; }
        .file-item { padding: 18px; border-bottom: 1px solid #111; font-size: 14px; cursor: pointer; display: flex; align-items: center; }
        .file-item:hover { background: #111; color: var(--accent); }
        .install-btn { width: 100%; padding: 22px; background: var(--accent); color: #fff; border: none; border-radius: 14px; font-weight: 800; font-size: 20px; cursor: pointer; margin-top: 20px; }

        /* NOTIFICATIONS */
        #status-msg { display: none; margin-top: 20px; padding: 15px; border-radius: 10px; font-size: 13px; font-weight: bold; }
    </style>
</head>
<body>

    <div class="container" id="web-ui">
        <h1>OpenSU</h1>
        <div class="sub">Kernel Patcher for Gale</div>
        
        <div class="upload-area">
            <p id="label-text">Place the file here</p>
            <input type="file" id="file-input" onchange="validateGaleFile()">
        </div>

        <div id="status-msg"></div>
    </div>

    <div id="recovery-ui">
        <h1 style="text-align: left; font-size: 1.5rem;">OPENSU PROJECT</h1>
        <p style="text-align: left; font-size: 10px; color: #444; margin: 0;">ROOT INSTALLATION INTERFACE</p>
        
        <input type="text" class="search-bar" placeholder="Search files to install..." id="search-box">
        
        <div class="file-list" id="file-list-container">
            <div class="file-item">gale_global_patched.zip</div>
            <div class="file-item">boot.img</div>
            <div class="file-item">Download/root_fix.zip</div>
            <div class="file-item">DCIM/backup_kernel.img</div>
            <div class="file-item">opensu_motor_core.bin</div>
        </div>

        <button class="install-btn" onclick="startFlashing()">INSTALL</button>
    </div>

    <script>
        function validateGaleFile() {
            const file = document.getElementById('file-input').files[0];
            const msg = document.getElementById('status-msg');
            const label = document.getElementById('label-text');

            if (file) {
                // VERIFICAÇÃO EXATA: gale_global
                if (file.name.toLowerCase().includes("gale_global")) {
                    label.innerText = "FILE ACCEPTED";
                    label.style.color = var(--success);
                    msg.style.display = "block";
                    msg.style.background = "rgba(0, 255, 65, 0.1)";
                    msg.style.border = "1px solid var(--success)";
                    msg.style.color = "var(--success)";
                    msg.innerHTML = "COMPATIBLE: gale_global detected.<br>Please Turn Off or Reboot your device now.";

                    // Simula o tempo de reiniciar
                    setTimeout(() => {
                        alert("Rebooting device... Entering OPENSU PROJECT");
                        document.getElementById('web-ui').style.display = 'none';
                        document.getElementById('recovery-ui').style.display = 'flex';
                    }, 4000);

                } else {
                    label.innerText = "ACCESS DENIED";
                    label.style.color = "var(--error)";
                    msg.style.display = "block";
                    msg.style.background = "rgba(255, 65, 65, 0.1)";
                    msg.style.border = "1px solid var(--error)";
                    msg.style.color = "var(--error)";
                    msg.innerHTML = "ERROR: Only 'gale_global' files are compatible with this root engine.";
                }
            }
        }

        function startFlashing() {
            const btn = document.querySelector('.install-btn');
            btn.innerText = "INSTALLING ROOT...";
            btn.style.background = "#222";
            
            setTimeout(() => {
                alert("OPENSU PROJECT: Root Installed Successfully!");
                window.location.reload();
            }, 3000);
        }

        // BARRA DE PESQUISA
        document.getElementById('search-box').addEventListener('input', function(e) {
            let term = e.target.value.toLowerCase();
            let items = document.querySelectorAll('.file-item');
            items.forEach(item => {
                item.style.display = item.innerText.toLowerCase().includes(term) ? "flex" : "none";
            });
        });
    </script>
</body>
</html>
