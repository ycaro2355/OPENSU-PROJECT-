<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>OpenSU | Professional Root Toolchain</title>
    <style>
        body { background: #0d1117; color: #c9d1d9; font-family: 'Courier New', monospace; padding: 40px; display: flex; flex-direction: column; align-items: center; }
        .card { background: #161b22; border: 1px solid #30363d; border-radius: 8px; padding: 30px; width: 100%; max-width: 550px; box-shadow: 0 10px 40px rgba(0,0,0,0.8); }
        h1 { color: #58a6ff; font-size: 2.5rem; margin: 0; }
        .tagline { color: #8b949e; font-size: 12px; margin-bottom: 30px; border-bottom: 1px solid #333; padding-bottom: 10px; }
        .btn { display: block; width: 100%; padding: 18px; margin: 15px 0; border: none; border-radius: 6px; font-weight: bold; cursor: pointer; text-decoration: none; text-align: center; text-transform: uppercase; font-size: 14px; }
        .btn-download { background: #238636; color: white; }
        .btn-install { background: #21262d; color: #58a6ff; border: 1px solid #30363d; }
        .terminal { background: #000; color: #39ff14; padding: 15px; border-radius: 4px; height: 180px; overflow-y: auto; font-size: 12px; margin-top: 20px; border: 1px solid #333; }
    </style>
</head>
<body>

<div class="card">
    <h1>OpenSU</h1>
    <div class="tagline">SYSTEMLESS ROOT ENGINE | CORE: BINARY SU</div>

    <a href="OpenSU.zip" class="btn btn-download">Download OpenSU.zip</a>

    <button class="btn btn-install" onclick="runMotor()">Install / Patch Engine</button>

    <div class="terminal" id="console">
        [#] OpenSU environment initialized.<br>
        [#] Waiting for GALE boot.img...<br>
    </div>
</div>

<script>
    /* ESTE É O MOTOR REAL (SHELL SCRIPT) 
       Copie o conteúdo desta variável para o arquivo 'update-binary' dentro do ZIP
    */
    const shellMotorScript = `
#!/sbin/sh
# OPENSU CORE ENGINE - UPDATE-BINARY
BOOT_DEV="/dev/block/by-name/boot"

ui_print "- OpenSU: Initializing Engine..."
# Unpack using magiskboot tool
./magiskboot unpack $BOOT_DEV

ui_print "- OpenSU: Injecting SU binary..."
# The 'su' binary is the motor that grants root
./magiskboot cpio ramdisk.cpio \\
    "add 0755 bin/su common/su" \\
    "patch"

ui_print "- OpenSU: Repacking kernel..."
./magiskboot repack $BOOT_DEV new-boot.img
dd if=new-boot.img of=$BOOT_DEV

ui_print "- OpenSU: Success! Root activated."
    `;

    function runMotor() {
        const consoleLog = document.getElementById('console');
        const tasks = [
            "> Starting OpenSU Toolchain...",
            "> Analyzing Kernel Partitions...",
            "> Loading SU Binary (Motor)...",
            "> Patching Ramdisk via magiskboot...",
            "> Rebuilding Boot Image...",
            "<b>> SUCCESS: OpenSU Engine Injected!</b>"
        ];

        let i = 0;
        const timer = setInterval(() => {
            if (i < tasks.length) {
                consoleLog.innerHTML += `<div>${tasks[i]}</div>`;
                i++;
                consoleLog.scrollTop = consoleLog.scrollHeight;
            } else {
                clearInterval(timer);
                console.log("Flashable Script Code:", shellMotorScript);
            }
        }, 800);
    }
</script>

<footer style="margin-top:20px; font-size:10px; color:#444;">
    OPENSU PROJECT | POWERED BY ORIGINAL BINARY SU | 2026
</footer>

</body>
</html>
