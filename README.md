<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>OpenSU Project | Root Toolchain</title>
    <style>
        :root { --accent: #0070f3; --bg: #000; --green: #00ff41; --red: #ff0000; }
        body { background: var(--bg); color: #fff; font-family: 'Segoe UI', sans-serif; margin: 0; overflow: hidden; display: flex; align-items: center; justify-content: center; min-height: 100vh; }
        
        /* STAGE 1: WEB PORTAL (ONLY VALIDATION) */
        #web-portal { width: 90%; max-width: 400px; text-align: center; background: #0a0a0a; border: 1px solid #222; padding: 40px; border-radius: 20px; }
        .upload-area { border: 2px dashed #333; margin: 30px 0; padding: 50px 20px; border-radius: 12px; cursor: pointer; position: relative; }
        .upload-area:hover { border-color: var(--accent); }
        input[type="file"] { position: absolute; inset: 0; opacity: 0; cursor: pointer; }
        #status-box { display: none; padding: 15px; border-radius: 8px; font-weight: bold; margin-top: 20px; font-size: 14px; }
        .yes { border: 1px solid var(--green); color: var(--green); background: rgba(0, 255, 65, 0.1); }
        .no { border: 1px solid var(--red); color: var(--red); background: rgba(255, 0, 0, 0.1); }

        /* STAGE 2: BOOT LOADER (OPENSU PROJECT INTERFACE) */
        #boot-screen { display: none; position: fixed; inset: 0; background: #000; flex-direction: column; align-items: center; justify-content: center; z-index: 9999; padding: 30px; }
        .logo { font-size: 30px; font-weight: 900; color: var(--accent); margin-bottom: 40px; letter-spacing: 2px; }
        
        /* INSTALLER COMPONENTS (ONLY VISIBLE AFTER REBOOT) */
        #installer-ui { display: none; width: 100%; max-width: 480px; flex-direction: column; }
        .search-bar { width: 100%; padding: 15px; background: #111; border: 1px solid #333; color: #fff; border-radius: 8px; margin-bottom: 20px; outline: none; box-sizing: border-box; }
        .file-list { height: 300px; border: 1px solid #222; border-radius: 8px; overflow-y: auto; background: #050505; }
        .file-item { padding: 15px; border-bottom: 1px solid #111; cursor: pointer; font-family: monospace; font-size: 13px; display: flex; justify-content: space-between; }
        .file-item:hover { background: #111; color: var(--accent); }
        
        .btn-main { width: 100%; padding: 22px; background: var(--accent); color: #fff; border: none; border-radius: 10px; font-weight: bold; font-size: 18px; cursor: pointer; }
    </style>
</head>
<body>

    <div id="web-portal">
        <h1 style="margin:0;">OpenSU</h1>
        <p style="color:#555; font-size: 10px; text-transform: uppercase;">Kernel Root Validator</p>
        
        <div class="upload-area">
            <p id="label-text">Place the file here</p>
            <input type="file" id="file-input" onchange="validateFile()">
        </div>

        <div id="status-box"></div>
    </div>

    <div id="boot-screen">
        <div class="logo" id="project-title">OPENSU PROJECT</div>
        
        <button class="btn-main" id="install-trigger" onclick="openInstaller()">INSTALL</button>

        <div id="installer-ui">
            <input type="text" class="search-bar" id="search-box" placeholder="Search files to activate root...">
            <div class="file-list" id="files">
                <div class="file-item">gale_global_v14_stock.img <span>(YES)</span></div>
                <div class="file-item">gale_global_patched.zip <span>(YES)</span></div>
                <div class="file-item">boot_original.img <span>(YES)</span></div>
                <div class="file-item">Download/opensu_module.zip <span>(YES)</span></div>
                <div class="file-item">DCIM/backup_camera.img <span>(NO)</span></div>
                <div class="file-item">system_config.img <span>(NO)</span></div>
            </div>
        </div>
    </div>

    <script>
        function validateFile() {
            const file = document.getElementById('file-input').files[0];
            const status = document.getElementById('status-box');
            const label = document.getElementById('label-text');

            if (file) {
                const isGale = file.name.toLowerCase().includes("gale_global");
                status.style.display = "block";

                if (isGale) {
                    status.className = "yes";
                    status.innerHTML = "ROOT COMPATIBLE: YES<br><span style='color:white; font-size:11px;'>Please Power Off or Restart your device now.</span>";
                    label.innerText = "FILE VERIFIED";

                    // START REBOOT SIMULATION
                    setTimeout(() => {
                        document.getElementById('web-portal').style.display = 'none';
                        document.body.style.background = "#000"; // Screen goes black (Off)
                        
                        setTimeout(() => {
                            // SCREEN TURNS ON IN BOOT MODE
                            document.getElementById('boot-screen').style.display = 'flex';
                        }, 3000); // 3 seconds "Off"
                    }, 5000); // 5 seconds of warning

                } else {
                    status.className = "no";
                    status.innerHTML = "ROOT COMPATIBLE: NO<br><span style='color:white; font-size:11px;'>gale_global signature not found.</span>";
                    label.innerText = "ERROR";
                }
            }
        }

        function openInstaller() {
            // Hide Install button and show Search + File List
            document.getElementById('install-trigger').style.display = 'none';
            document.getElementById('project-title').style.fontSize = '20px';
            document.getElementById('project-title').style.marginBottom = '20px';
            document.getElementById('installer-ui').style.display = 'flex';
        }

        // Search Bar Filtering
        document.getElementById('search-box').addEventListener('input', function(e) {
            let filter = e.target.value.toLowerCase();
            let items = document.querySelectorAll('.file-item');
            items.forEach(item => {
                item.style.display = item.innerText.toLowerCase().includes(filter) ? "flex" : "none";
            });
        });

        // File Selection
        document.querySelectorAll('.file-item').forEach(item => {
            item.onclick = function() {
                if (this.innerText.includes("(YES)")) {
                    alert("OPENSU PROJECT: Patching selected file... Initializing su motor.");
                    alert("ROOT ACTIVATED SUCCESSFULLY!");
                } else {
                    alert("ERROR: This file is not compatible for Root activation.");
                }
                location.reload();
            };
        });
    </script>
</body>
</html>
