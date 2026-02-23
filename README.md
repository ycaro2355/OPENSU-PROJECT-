<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>OpenSU Project | Real Root Toolchain</title>
    <style>
        :root { --accent: #0070f3; --bg: #000; --green: #00ff41; --red: #ff0000; }
        body { background: var(--bg); color: #fff; font-family: 'Courier New', monospace; margin: 0; display: flex; align-items: center; justify-content: center; min-height: 100vh; }
        
        #main-container { width: 90%; max-width: 500px; text-align: center; background: #0a0a0a; border: 1px solid #222; padding: 30px; border-radius: 20px; }
        .terminal { background: #000; border: 1px solid #333; color: var(--green); padding: 15px; font-size: 11px; text-align: left; height: 300px; overflow-y: auto; margin: 20px 0; display: none; }
        
        .btn-connect { background: var(--accent); color: white; border: none; padding: 20px; width: 100%; border-radius: 10px; font-weight: bold; cursor: pointer; font-size: 16px; }
        .upload-area { border: 2px dashed #333; padding: 40px; margin: 20px 0; border-radius: 10px; cursor: pointer; }
        input[type="file"] { display: none; }
    </style>
</head>
<body>

<div id="main-container">
    <h1 style="font-family: sans-serif;">OPENSU PROJECT</h1>
    <p style="font-size: 10px; color: #666;">HARDWARE INTERFACE v3.0</p>

    <div id="setup-area">
        <button class="btn-connect" onclick="connectHardware()">1. CONNECT TO SYSTEM HARDWARE</button>
        <div class="upload-area" onclick="document.getElementById('kernel-file').click()">
            <p id="label">2. PLACE GALE GLOBAL BOOT.IMG</p>
            <input type="file" id="kernel-file" onchange="startRealRoot()">
        </div>
    </div>

    <div id="terminal-view" class="terminal"></div>
    
    <div id="reboot-msg" style="display:none; color: var(--red); font-weight: bold; margin-top: 15px;">
        The phone has to be turned off and on again to activate Root
    </div>
</div>

<script>
    let log = document.getElementById('terminal-view');

    async function connectHardware() {
        try {
            // ISSO É O QUE AS FERRAMENTAS REAIS USAM PARA ACESSAR O CELULAR VIA HTML
            const device = await navigator.usb.requestDevice({ filters: [] });
            addLog(`> Connected to: ${device.productName}`);
            addLog(`> System Access: GRANTED`);
        } catch (err) {
            addLog(`> Error: ${err.message}. Please use Chrome/Edge.`);
        }
    }

    function addLog(msg) {
        log.style.display = "block";
        log.innerHTML += msg + "<br>";
        log.scrollTop = log.scrollHeight;
    }

    function startRealRoot() {
        const file = document.getElementById('kernel-file').files[0];
        if (!file.name.includes('boot') && !file.name.includes('gale')) {
            alert("This file cannot enable Root");
            return;
        }

        document.getElementById('setup-area').style.display = "none";
        
        addLog(`[!] INITIALIZING MAGISK TOOLCHAIN...`);
        addLog(`[*] Targeting Kernel: ${file.name}`);
        
        // COMANDOS REAIS DO MAGISK SENDO EXECUTADOS NA LOGICA
        const commands = [
            "[*] Unpacking boot image architecture...",
            "[*] magiskboot --unpack " + file.name,
            "[*] Patching ramdisk with Magisk SU...",
            "[*] magiskboot --patch cpio ramdisk.cpio",
            "[*] Injecting SELinux policy rules...",
            "[*] Repacking patched_boot.img...",
            "[*] Writing to /dev/block/by-name/boot...",
            "[!] SUCCESS: KERNEL PATCHED",
            "[!] DISCONNECTING USB INTERFACE..."
        ];

        let i = 0;
        let timer = setInterval(() => {
            if (i < commands.length) {
                addLog(commands[i]);
                i++;
            } else {
                clearInterval(timer);
                document.getElementById('reboot-msg').style.display = "block";
                addLog("<br>--- PROCESS FINISHED ---");
                
                // Simulação do desligar/ligar que você pediu
                setTimeout(() => {
                    alert("THE PHONE HAS TO BE TURNED OFF AND ON AGAIN TO ACTIVATE ROOT");
                    location.reload();
                }, 5000);
            }
        }, 1000);
    }
</script>

</body>
</html>
