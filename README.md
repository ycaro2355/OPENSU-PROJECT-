<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>OpenSU Project | Root Engine</title>
    <style>
        :root { --accent: #0070f3; --bg: #000; --green: #00ff41; --red: #ff0000; }
        body { background: var(--bg); color: #fff; font-family: 'Segoe UI', sans-serif; margin: 0; overflow: hidden; display: flex; align-items: center; justify-content: center; min-height: 100vh; }
        
        /* STAGE 1: WEB PORTAL */
        #web-portal { width: 90%; max-width: 400px; text-align: center; background: #0a0a0a; border: 1px solid #222; padding: 40px; border-radius: 20px; }
        .upload-area { border: 2px dashed #333; margin: 30px 0; padding: 50px 20px; border-radius: 12px; cursor: pointer; position: relative; }
        input[type="file"] { position: absolute; inset: 0; opacity: 0; cursor: pointer; }
        
        #status-box { display: none; padding: 15px; border-radius: 8px; font-weight: bold; margin-top: 20px; font-size: 14px; }
        .yes { border: 1px solid var(--green); color: var(--green); background: rgba(0, 255, 65, 0.1); }
        .no { border: 1px solid var(--red); color: var(--red); background: rgba(255, 0, 0, 0.1); }

        /* STAGE 2: BOOT LOADER (OPENSU PROJECT) */
        #boot-screen { display: none; position: fixed; inset: 0; background: #000; flex-direction: column; align-items: center; justify-content: center; z-index: 9999; padding: 30px; }
        .logo { font-size: 30px; font-weight: 900; color: var(--accent); margin-bottom: 40px; letter-spacing: 2px; }
        
        #installer-ui { display: none; width: 100%; max-width: 480px; flex-direction: column; }
        .search-bar { width: 100%; padding: 15px; background: #111; border: 1px solid #333; color: #fff; border-radius: 8px; margin-bottom: 20px; outline: none; box-sizing: border-box; }
        .file-list { height: 250px; border: 1px solid #222; border-radius: 8px; overflow-y: auto; background: #050505; }
        .file-item { padding: 15px; border-bottom: 1px solid #111; cursor: pointer; font-family: monospace; display: flex; justify-content: space-between; }
        
        .btn-main { width: 100%; padding: 22px; background: var(--accent); color: #fff; border: none; border-radius: 10px; font-weight: bold; font-size: 18px; cursor: pointer; }
    </style>
</head>
<body>

    <div id="web-portal">
        <h1>OpenSU</h1>
        <p style="color:#555; font-size: 10px; text-transform: uppercase;">Kernel Toolchain</p>
        
        <div class="upload-area">
            <p id="label-text">Place the file here</p>
            <input type="file" id="file-input" onchange="validateKernel()">
        </div>

        <div id="status-box"></div>
    </div>

    <div id="boot-screen">
        <div class="logo" id="project-title">OPENSU PROJECT</div>
        <button class="btn-main" id="install-trigger" onclick="openInstaller()">INSTALL</button>

        <div id="installer-ui">
            <input type="text" class="search-bar" id="search-box" placeholder="Search files...">
            <div class="file-list" id="files">
                <div class="file-item">gale global boot.img <span style="color:var(--green)">[ROOT READY]</span></div>
                <div class="file-item">boot.img <span style="color:var(--green)">[ROOT READY]</span></div>
                <div class="file-item">recovery.img <span style="color:var(--red)">[NO ROOT]</span></div>
                <div class="file-item">Download/system.zip <span style="color:var(--red)">[NO ROOT]</span></div>
            </div>
        </div>
    </div>

    <script>
        function validateKernel() {
            const file = document.getElementById('file-input').files[0];
            const status = document.getElementById('status-box');
            const label = document.getElementById('label-text');

            if (file) {
                const name = file.name.toLowerCase();
                status.style.display = "block";

                // VALIDAÇÃO CORRIGIDA PARA "GALE GLOBAL BOOT.IMG"
                if (name.includes("gale") || name.includes("boot") || name.includes("global")) {
                    status.className = "yes";
                    status.innerHTML = "This file activates Root<br><span style='color:white; font-size:12px; display:block; margin-top:10px;'>The phone has to be turned off and on again to activate Root</span>";
                    label.innerText = "FILE COMPATIBLE";

                    // SIMULAÇÃO DO REBOOT
                    setTimeout(() => {
                        document.getElementById('web-portal').style.display = 'none';
                        document.body.style.background = "#000"; 
                        setTimeout(() => {
                            document.getElementById('boot-screen').style.display = 'flex';
                        }, 3000); 
                    }, 6000); // Tempo para o usuário ler o aviso de desligar
                } else {
                    status.className = "no";
                    status.innerHTML = "This file cannot enable Root<br><span style='color:white; font-size:11px;'>Unsupported kernel signature.</span>";
                    label.innerText = "INCOMPATIBLE";
                }
            }
        }

        function openInstaller() {
            document.getElementById('install-trigger').style.display = 'none';
            document.getElementById('installer-ui').style.display = 'flex';
        }

        // Search and Select
        document.getElementById('search-box').addEventListener('input', function(e) {
            let filter = e.target.value.toLowerCase();
            document.querySelectorAll('.file-item').forEach(item => {
                item.style.display = item.innerText.toLowerCase().includes(filter) ? "flex" : "none";
            });
        });

        document.querySelectorAll('.file-item').forEach(item => {
            item.onclick = function() {
                if (this.innerText.includes("READY")) {
                    alert("OPENSU PROJECT: Running su toolchain... Injecting su motor...");
                    alert("SUCCESS: Root activated via OPENSU system tool.");
                } else {
                    alert("ERROR: This file cannot enable Root.");
                }
                location.reload();
            };
        });
    </script>
</body>
</html>
