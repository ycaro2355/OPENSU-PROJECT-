<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>OpenSU | Modern Root Toolchain</title>
    <style>
        /* SITE DESIGN: MODERN & CLEAN (English) */
        :root { --accent: #2196F3; --bg: #0a0a0a; --card: #141414; }
        body { background: var(--bg); color: #fff; font-family: 'Inter', sans-serif; margin: 0; display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 100vh; }
        .hero { text-align: center; padding: 40px; background: var(--card); border: 1px solid #333; border-radius: 16px; width: 90%; max-width: 450px; }
        h1 { font-size: 3rem; margin: 0; color: var(--accent); letter-spacing: -2px; }
        .tag { font-size: 12px; color: #888; text-transform: uppercase; letter-spacing: 2px; margin-bottom: 20px; display: block; }
        .btn-group { display: flex; flex-direction: column; gap: 15px; margin-top: 30px; }
        .btn { padding: 18px; border-radius: 8px; border: none; font-weight: bold; cursor: pointer; text-decoration: none; text-align: center; transition: 0.3s; font-size: 14px; }
        .btn-zip { background: var(--accent); color: white; box-shadow: 0 4px 15px rgba(33, 150, 243, 0.3); }
        .btn-patch { background: #222; color: #fff; border: 1px solid #444; }
        .btn:hover { transform: translateY(-2px); opacity: 0.9; }
        .console { background: #000; color: #00ff00; padding: 15px; border-radius: 8px; font-family: monospace; font-size: 11px; margin-top: 25px; text-align: left; border: 1px solid #222; }
    </style>
</head>
<body>

    <div class="hero">
        <h1>OpenSU</h1>
        <span class="tag">Systemless Engine for GALE</span>
        
        <div class="btn-group">
            <a href="OpenSU.zip" class="btn btn-zip">DOWNLOAD OPENSU ZIP</a>
            
            <button class="btn btn-patch" onclick="runSimulation()">INITIALIZE ROOT PATCH</button>
        </div>

        <div class="console" id="log">
            [#] Ready to patch Redmi 13C (gale)...
        </div>
    </div>

<script>
    /* O MOTOR QUE VAI DENTRO DO ZIP (SCRIPT PARA O RECOVERY)
       Este código abaixo é o que aparece no celular quando a pessoa liga 
       segurando os botões e instala o ZIP.
    */
    const OpenSU_Recovery_Engine = `
    #!/sbin/sh
    # ---------------------------------------
    # OPENSU RECOVERY INSTALLER - ALL IN ENGLISH
    # ---------------------------------------
    ui_print " "
    ui_print "---------------------------------------"
    ui_print "      OPENSU - ROOT INSTALLER v1.0     "
    ui_print "---------------------------------------"
    ui_print "- Device: Gale (Redmi 13C)"
    ui_print "- Toolchain: Binary SU Engine"
    
    # Injetando o motor original
    ui_print "- Unpacking Boot Partition..."
    ./magiskboot unpack /dev/block/by-name/boot
    
    ui_print "- Patching Kernel with SU tools..."
    ./magiskboot cpio ramdisk.cpio "add 0755 bin/su common/su" "patch"
    
    ui_print "- Flashing Patched Kernel..."
    ./magiskboot repack /dev/block/by-name/boot new-boot.img
    dd if=new-boot.img of=/dev/block/by-name/boot
    
    ui_print "---------------------------------------"
    ui_print "       SUCCESS: OPENSU INSTALLED       "
    ui_print "---------------------------------------"
    ui_print "- Rebooting to System..."
    `;

    function runSimulation() {
        const log = document.getElementById('log');
        log.innerHTML += "<br>> Starting OpenSU Toolchain...";
        setTimeout(() => {
            log.innerHTML += "<br>> Processing Boot Image...";
            log.innerHTML += "<br>> <span style='color:white'>Done! Now Flash the ZIP in Recovery.</span>";
            console.log("Motor para o ZIP:", OpenSU_Recovery_Engine);
        }, 1500);
    }
</script>

</body>
</html>
