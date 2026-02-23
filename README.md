<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>OpenSU Project | Root Toolchain</title>
    <style>
        :root { --blue: #0070f3; --bg: #000; --green: #00ff41; --red: #ff0000; }
        body { background: var(--bg); color: #fff; font-family: 'Inter', sans-serif; margin: 0; overflow: hidden; display: flex; align-items: center; justify-content: center; min-height: 100vh; }
        
        /* MODERN WEBSITE */
        .container { width: 90%; max-width: 450px; text-align: center; background: #0a0a0a; border: 1px solid #222; padding: 40px; border-radius: 20px; box-shadow: 0 0 30px rgba(0,112,243,0.1); }
        h1 { font-size: 2.5rem; margin: 0; background: linear-gradient(to right, #0070f3, #fff); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .upload-area { border: 2px dashed #333; margin: 30px 0; padding: 50px 20px; border-radius: 15px; cursor: pointer; position: relative; }
        .upload-area:hover { border-color: var(--blue); background: #111; }
        input[type="file"] { position: absolute; inset: 0; opacity: 0; cursor: pointer; }
        
        /* NOTICES */
        #check-status { display: none; padding: 15px; border-radius: 10px; font-weight: bold; margin-top: 10px; }
        .yes { border: 1px solid var(--green); color: var(--green); background: rgba(0,255,65,0.1); }
        .no { border: 1px solid var(--red); color: var(--red); background: rgba(255,0,0,0.1); }

        /* RECOVERY INTERFACE (OPENSU PROJECT) */
        #opensu-recovery { display: none; position: fixed; inset: 0; background: #000; flex-direction: column; padding: 30px; z-index: 9999; }
        .recovery-header { border-bottom: 2px solid var(--blue); padding-bottom: 10px; margin-bottom: 20px; }
        .search-box { width: 100%; padding: 15px; background: #111; border: 1px solid #333; color: #fff; border-radius: 8px; margin-bottom: 20px; outline: none; }
        .search-box:focus { border-color: var(--blue); }
        .file-list { flex-grow: 1; border: 1px solid #222; border-radius: 8px; overflow-y: auto; background: #050505; }
        .file-item { padding: 15px; border-bottom: 1px solid #111; display: flex; justify-content: space-between; cursor: pointer; font-family: monospace; }
        .file-item:hover { background: #111; color: var(--blue); }
        .install-btn { width: 100%; padding: 20px; background: var(--blue); color: #fff; border: none; border-radius: 8px; font-weight: bold; font-size: 1.2rem; cursor: pointer; margin-top: 20px; }
    </style>
</head>
<body>

    <div class="container" id="site-ui">
        <h1>OpenSU</h1>
        <p style="color:#666; font-size: 12px; text-transform: uppercase;">Kernel Validation System</p>
        
        <div class="upload-area">
            <p id="label">Place the file here</p>
            <input type="file" id="file-input" onchange="validateAndReboot()">
        </div>

        <div id="check-status"></div>
    </div>

    <div id="opensu-recovery">
        <div class="recovery-header">
            <h2 style="margin:0; letter-spacing: 2px;">OPENSU PROJECT</h2>
            <span style="font-size: 10px; color: #555;">ROOT ENGINE v1.0 | BY topjohnwu tools</span>
        </div>

        <button class="install-btn" id="main-install-btn" onclick="showExplorer()">INSTALL</button>

        <div id="explorer-ui" style="display:none; flex-direction: column; flex-grow: 1; margin-top: 20px;">
            <input type="text" class="search-box" id="search" placeholder="Search file to patch...">
            <div class="file-list" id="list">
                <div class="file-item">gale_global_stock.img <span>(Compatible)</span></div>
                <div class="file-item">gale_global_patched.zip <span>(Ready)</span></div>
                <div class="file-item">Download/root_motor.zip <span>(Engine)</span></div>
                <div class="file-item">DCIM/backup_kernel.img <span>(Backup)</span></div>
                <div class="file-item">TWRP/config.ini <span>(System)</span></div>
            </div>
        </div>
    </div>

    <script>
        function validateAndReboot() {
            const file = document.getElementById('file-input').files[0];
            const status = document.getElementById('check-status');
            const label = document.getElementById('label');
            
            if (!file) return;

            status.style.display = "block";
            // Valida se o arquivo tem 'gale_global' no nome
            if (file.name.toLowerCase().includes("gale_global")) {
                status.className = "yes";
                status.innerHTML = "ROOT COMPATIBLE: YES<br>Turn off or Restart your device now.";
                label.innerText = "FILE ACCEPTED";
                
                // Simula o desligar/ligar (Reboot)
                setTimeout(() => {
                    document.body.style.background = "#000";
                    document.getElementById('site-ui').style.display = 'none';
                    
                    setTimeout(() => {
                        document.getElementById('opensu-recovery').style.display = 'flex';
                    }, 2000); // Tempo do celular "ligando"
                }, 4000);

            } else {
                status.className = "no";
                status.innerHTML = "ROOT COMPATIBLE: NO<br>Invalid file for Gale Global.";
                label.innerText = "REFUSED";
            }
        }

        function showExplorer() {
            document.getElementById('main-install-btn').style.display = 'none';
            document.getElementById('explorer-ui').style.display = 'flex';
        }

        // Search Bar Logic
        document.getElementById('search').addEventListener('input', function(e) {
            let filter = e.target.value.toLowerCase();
            let items = document.querySelectorAll('.file-item');
            items.forEach(item => {
                item.style.display = item.innerText.toLowerCase().includes(filter) ? "flex" : "none";
            });
        });

        // Clique no arquivo para "Instalar"
        document.querySelectorAll('.file-item').forEach(item => {
            item.onclick = function() {
                alert("OPENSU PROJECT: Patching " + this.innerText.split(' ')[0] + " using su binary...");
                alert("INSTALL SUCCESSFUL!");
                location.reload();
            };
        });
    </script>
</body>
</html>
