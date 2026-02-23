<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>OpenSU Project | Universal Toolchain</title>
    <style>
        :root { --accent: #0070f3; --bg: #000; --card: #111; --text: #eaeaea; --success: #00ff41; }
        body { background: var(--bg); color: var(--text); font-family: 'Inter', -apple-system, sans-serif; margin: 0; display: flex; justify-content: center; align-items: center; min-height: 100vh; overflow: hidden; }
        
        /* MODERN INTERFACE */
        .container { width: 90%; max-width: 450px; background: var(--card); padding: 40px; border-radius: 24px; border: 1px solid #333; text-align: center; transition: 0.5s; }
        h1 { font-size: 2.2rem; margin: 0; font-weight: 800; letter-spacing: -1px; color: #fff; }
        .sub { font-size: 12px; color: #666; text-transform: uppercase; margin-bottom: 30px; }
        
        /* UPLOAD BUTTON */
        .upload-box { border: 2px dashed #444; padding: 40px; border-radius: 16px; cursor: pointer; position: relative; transition: 0.3s; }
        .upload-box:hover { border-color: var(--accent); background: rgba(0, 112, 243, 0.05); }
        .upload-box p { margin: 0; font-weight: 600; color: #888; }
        input[type="file"] { position: absolute; width: 100%; height: 100%; top: 0; left: 0; opacity: 0; cursor: pointer; }

        /* RECOVERY/INSTALL SCREEN */
        #recovery-ui { display: none; position: fixed; inset: 0; background: #000; z-index: 1000; flex-direction: column; padding: 25px; box-sizing: border-box; }
        .search-input { width: 100%; padding: 18px; border-radius: 12px; border: 1px solid #222; background: #0a0a0a; color: #fff; font-size: 16px; margin-top: 20px; outline: none; }
        .search-input:focus { border-color: var(--accent); }
        .file-browser { flex-grow: 1; border: 1px solid #222; margin-top: 20px; border-radius: 12px; overflow-y: auto; text-align: left; background: #050505; }
        .file { padding: 18px; border-bottom: 1px solid #111; font-size: 14px; cursor: pointer; display: flex; align-items: center; }
        .file:before { content: "📄"; margin-right: 12px; opacity: 0.5; }
        .file:hover { background: #111; color: var(--accent); }
        .install-btn { width: 100%; padding: 22px; background: var(--accent); color: #fff; border: none; border-radius: 14px; font-weight: 700; font-size: 18px; margin-top: 20px; cursor: pointer; }

        /* WARNING NOTIFICATION */
        #reboot-notice { display: none; margin-top: 20px; padding: 15px; border-radius: 10px; background: rgba(0, 255, 65, 0.1); border: 1px solid var(--success); color: var(--success); font-size: 13px; }
    </style>
</head>
<body>

    <div class="container" id="web-ui">
        <h1>OpenSU</h1>
        <div class="sub">Kernel Root Patcher</div>
        
        <div class="upload-box" id="drop-zone">
            <p id="label-text">Place the file here</p>
            <input type="file" id="file-input" onchange="checkFile()">
        </div>

        <div id="reboot-notice">
            <b>FILE VERIFIED!</b> Root is compatible.<br>
            Please Reboot or Turn Off your device to continue.
        </div>
    </div>

    <div id="recovery-ui">
        <h2 style="margin: 0; letter-spacing: -1px;">OPENSU PROJECT</h2>
        <p style="font-size: 11px; color: #444; text-transform: uppercase;">Recovery Installation Mode</p>
        
        <input type="text" class="search-input" placeholder="Search files..." id="search-box">
        
        <div class="file-browser" id="file-list">
            <div class="file">opensu_motor_v1.zip</div>
            <div class="file">boot_gale_global.img</div>
            <div class="file">recovery_backup.img</div>
            <div class="file">Download/root_package.zip</div>
            <div class="file">System/kernel_mod.img</div>
        </div>

        <button class="install-btn" onclick="finishInstall()">INSTALL</button>
    </div>

    <script>
        function checkFile() {
            const input = document.getElementById('file-input');
            const label = document.getElementById('label-text');
            const notice = document.getElementById('reboot-notice');
            
            if (input.files.length > 0) {
                const fileName = input.files[0].name.toLowerCase();
                
                // VERIFICAÇÃO SE O ARQUIVO É COMPATÍVEL (Root compatible check)
                if (fileName.includes("boot") || fileName.includes(".img") || fileName.includes(".zip")) {
                    label.innerHTML = "FILE COMPATIBLE";
                    label.style.color = "#00ff41";
                    notice.style.display = "block";
                    
                    // Simula o tempo de "Reiniciar / Ligar de novo"
                    setTimeout(() => {
                        alert("Device is rebooting into OPENSU PROJECT...");
                        document.getElementById('web-ui').style.display = 'none';
                        document.getElementById('recovery-ui').style.display = 'flex';
                    }, 4000);
                } else {
                    label.innerHTML = "INCOMPATIBLE FILE";
                    label.style.color = "#ff4141";
                    alert("This file cannot be used for Root.");
                }
            }
        }

        function finishInstall() {
            const btn = document.querySelector('.install-btn');
            btn.innerText = "PATCHING KERNEL...";
            btn.style.background = "#111";
            btn.style.color = "#555";
            
            setTimeout(() => {
                alert("OPENSU PROJECT: Install Successful! Root is active.");
                window.location.reload();
            }, 3500);
        }

        // BARRA DE PESQUISA (Search Bar Logic)
        document.getElementById('search-box').addEventListener('input', function(e) {
            let filter = e.target.value.toLowerCase();
            let files = document.querySelectorAll('.file');
            files.forEach(f => {
                f.style.display = f.innerText.toLowerCase().includes(filter) ? "flex" : "none";
            });
        });
    </script>
</body>
</html>
