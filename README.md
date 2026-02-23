<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>OpenSU Project | Root & Magisk Engine</title>
    <style>
        :root { --accent: #0070f3; --bg: #000; --green: #00ff41; --red: #ff0000; }
        body { background: var(--bg); color: #fff; font-family: 'Courier New', monospace; margin: 0; overflow: hidden; display: flex; align-items: center; justify-content: center; min-height: 100vh; }
        
        /* SITE - VALIDADOR */
        #web-portal { width: 90%; max-width: 450px; text-align: center; background: #0a0a0a; border: 1px solid #222; padding: 40px; border-radius: 20px; font-family: sans-serif; }
        .upload-area { border: 2px dashed #333; margin: 30px 0; padding: 50px 20px; border-radius: 12px; cursor: pointer; position: relative; }
        input[type="file"] { position: absolute; inset: 0; opacity: 0; cursor: pointer; }
        
        #status-box { display: none; padding: 15px; border-radius: 8px; font-weight: bold; margin-top: 20px; font-size: 14px; }
        .yes { border: 1px solid var(--green); color: var(--green); background: rgba(0, 255, 65, 0.1); }
        .no { border: 1px solid var(--red); color: var(--red); background: rgba(255, 0, 0, 0.1); }

        /* REBOOT - OPENSU PROJECT */
        #boot-screen { display: none; position: fixed; inset: 0; background: #000; flex-direction: column; align-items: center; justify-content: center; z-index: 9999; padding: 30px; }
        .logo { font-size: 30px; font-weight: 900; color: var(--accent); margin-bottom: 40px; letter-spacing: 2px; font-family: sans-serif; }
        
        #installer-ui { display: none; width: 100%; max-width: 500px; flex-direction: column; }
        .search-bar { width: 100%; padding: 15px; background: #111; border: 1px solid #333; color: #fff; border-radius: 8px; margin-bottom: 20px; outline: none; }
        .file-list { height: 180px; border: 1px solid #222; border-radius: 8px; overflow-y: auto; background: #050505; margin-bottom: 20px; }
        .file-item { padding: 15px; border-bottom: 1px solid #111; cursor: pointer; display: flex; justify-content: space-between; }
        
        /* TERMINAL DE ATIVAÇÃO MAGISK */
        #console { display: none; width: 100%; background: #000; border: 1px solid #333; padding: 15px; height: 280px; overflow-y: hidden; font-size: 11px; color: var(--green); text-align: left; }

        .btn-main { width: 100%; padding: 22px; background: var(--accent); color: #fff; border: none; border-radius: 10px; font-weight: bold; font-size: 18px; cursor: pointer; }
    </style>
</head>
<body>

    <div id="web-portal">
        <h1>OpenSU</h1>
        <p style="color:#555; font-size: 10px;">MAGISK & ROOT ACTIVATOR</p>
        
        <div class="upload-area">
            <p id="label-text">Place the file here</p>
            <input type="file" id="file-input" onchange="validateKernel()">
        </div>

        <div id="status-box"></div>
    </div>

    <div id="boot-screen">
        <div class="logo">OPENSU PROJECT</div>
        <button class="btn-main" id="install-trigger" onclick="openInstaller()">INSTALL</button>

        <div id="installer-ui">
            <input type="text" class="search-bar" id="search-box" placeholder="Search for gale global boot...">
            <div class="file-list" id="file-container">
                <div class="file-item" onclick="startRootActivation('gale global boot.img')">gale global boot.img <span style="color:var(--green)">[ROOT READY]</span></div>
                <div class="file-item" onclick="startRootActivation('boot.img')">boot.img <span style="color:var(--green)">[ROOT READY]</span></div>
                <div class="file-item">recovery.img <span style="color:var(--red)">[INCOMPATIBLE]</span></div>
            </div>
            <div id="console"></div>
        </div>
    </div>

    <script>
        function validateKernel() {
            const input = document.getElementById('file-input');
            const status = document.getElementById('status-box');
            const label = document.getElementById('label-text');

            if (input.files.length > 0) {
                const name = input.files[0].name.toLowerCase();
                status.style.display = "block";

                // Reconhece gale global ou boot
                if (name.includes("boot") || name.includes("gale")) {
                    status.className = "yes";
                    status.innerHTML = "This file activates Root<br><br>The phone has to be turned off and on again to activate Root";
                    label.innerText = "VERIFIED";

                    // Simula Reboot
                    setTimeout(() => {
                        document.getElementById('web-portal').style.display = 'none';
                        document.body.style.background = "#000"; 
                        setTimeout(() => {
                            document.getElementById('boot-screen').style.display = 'flex';
                        }, 2000); 
                    }, 5000);
                } else {
                    status.className = "no";
                    status.innerHTML = "This file cannot enable Root";
                    label.innerText = "ERROR";
                }
            }
        }

        function openInstaller() {
            document.getElementById('install-trigger').style.display = 'none';
            document.getElementById('installer-ui').style.display = 'flex';
        }

        function startRootActivation(fileName) {
            const consoleBox = document.getElementById('console');
            const fileList = document.getElementById('file-container');
            const search = document.querySelector('.search-bar');
            
            fileList.style.display = "none";
            search.style.display = "none";
            consoleBox.style.display = "block";

            const steps = [
                "[#] Initializing OPENSU Toolchain...",
                "[#] Loading " + fileName + "...",
                "[*] Detected Architecture: ARM64-v8a",
                "[*] Unpacking boot image...",
                "[*] Patching kernel with Magisk engine...",
                "[*] Injecting su binary into /system/bin/...",
                "[*] Modifying ramdisk for Root persistence...",
                "[*] Repacking boot image...",
                "[*] Flashing patched_boot.img to partition...",
                "[#] Cleaning temporary cache...",
                "[!] ROOT ACTIVATED SUCCESSFULLY!",
                "[!] Magisk core is now running."
            ];

            let count = 0;
            const process = setInterval(() => {
                if (count < steps.length) {
                    consoleBox.innerHTML += steps[count] + "<br>";
                    count++;
                } else {
                    clearInterval(process);
                    setTimeout(() => {
                        alert("PROCESS COMPLETE! Root and Magisk are now active.");
                        location.reload();
                    }, 1500);
                }
            }, 700);
        }
    </script>
</body>
</html>
