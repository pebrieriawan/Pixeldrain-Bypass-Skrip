# PixelDrain Bypass Download Limit Script

## Cara Menggunakan

### 1. Buka Halaman PixelDrain
- Kunjungi halaman file (contoh: https://pixeldrain.com/u/xxxxxxxx), halaman gallery (contoh: https://pixeldrain.com/l/xxxxxxxx), atau halaman lain yang berisi file yang ingin kamu download.

### 2. Buka DevTools Console
- Tekan `F12` atau `Ctrl + Shift + I` (Windows/Linux) atau `Cmd + Option + I` (Mac).
- Pilih tab **Console**.

### 3. Izinkan Paste
Karena browser akan memperingatkan saat paste kode ke Console, ketik:

```bash
allow pasting
```
lalu tekan Enter.

### 4. Paste Script
Salin seluruh script berikut ini dan paste ke Console, lalu tekan Enter.
```bash
(function() {
    const bypassBaseUrl = "https://pd.cybar.xyz/";
    const idRegex = /\/api\/file\/(\w+)\//;

    function getBypassUrls(type) {
        const url = window.location.href;
        if (type === "file") {
            const id = url.replace("https://pixeldrain.com/u/", "");
            return bypassBaseUrl + id;
        }
        if (type === "gallery") {
            const links = document.querySelectorAll('a.file');
            const urls = [];
            links.forEach(link => {
                const bgUrl = link.querySelector('div').style.backgroundImage;
                const match = bgUrl.match(idRegex);
                if (match) {
                    urls.push(bypassBaseUrl + match[1]);
                }
            });
            return urls;
        }
    }

    function startDownload(link) {
        const a = document.createElement("a");
        a.href = link;
        a.click();
    }

    function handleDownloadClick() {
        const url = window.location.href;
        if (url.includes("/u/")) {
            startDownload(getBypassUrls("file"));
        } else if (url.includes("/l/")) {
            getBypassUrls("gallery").forEach(link => startDownload(link));
        }
    }

    function handleShowLinksClick() {
        const popup = document.getElementById('popupBox') || createPopupBox();
        popup.innerHTML = "";

        const closeBtn = document.createElement('span');
        closeBtn.innerHTML = '&times;';
        Object.assign(closeBtn.style, {
            position: 'absolute',
            top: '5px',
            right: '10px',
            cursor: 'pointer'
        });
        closeBtn.onclick = () => popup.style.display = 'none';
        popup.appendChild(closeBtn);

        const url = window.location.href;
        if (url.includes("/u/")) {
            const link = getBypassUrls("file");
            const a = document.createElement("a");
            a.href = link;
            a.textContent = link;
            popup.appendChild(a);
        } else if (url.includes("/l/")) {
            const links = getBypassUrls("gallery");
            const container = document.createElement("div");
            Object.assign(container.style, {
                maxHeight: "60vh",
                overflowY: "auto",
                marginTop: "10px"
            });

            links.forEach(link => {
                const a = document.createElement("a");
                a.href = link;
                a.textContent = link;
                a.style.display = "block";
                container.appendChild(a);
            });

            popup.appendChild(container);
        }

        popup.style.display = "block";
    }

    function createPopupBox() {
        const box = document.createElement("div");
        box.id = "popupBox";
        Object.assign(box.style, {
            zIndex: 1000,
            whiteSpace: "pre-line",
            position: "fixed",
            top: "50%",
            left: "50%",
            transform: "translate(-50%, -50%)",
            padding: "20px",
            background: "#2f3541",
            border: "2px solid #a4be8c",
            color: "#d7dde8",
            borderRadius: "10px",
            width: "30%",
            maxWidth: "600px",
            maxHeight: "80vh",
            overflowY: "auto"
        });
        document.body.appendChild(box);
        return box;
    }

    function init() {
        const downloadButton = document.createElement("button");
        downloadButton.textContent = "Download Bypass";
        downloadButton.style.margin = "5px";
        downloadButton.onclick = handleDownloadClick;

        const showLinksButton = document.createElement("button");
        showLinksButton.textContent = "Show Bypass Link";
        showLinksButton.style.margin = "5px";
        showLinksButton.onclick = handleShowLinksClick;

        const labels = document.querySelectorAll('div.label');
        labels.forEach(label => {
            if (label.textContent.trim() === 'Size') {
                const next = label.nextElementSibling;
                if (next) {
                    next.insertAdjacentElement('afterend', showLinksButton);
                    next.insertAdjacentElement('afterend', downloadButton);
                }
            }
        });
    }

    init();
})();
```

### 5. Cara Pakai
Setelah di enter dan jalan, Akan muncul dua tombol baru di halaman PixelDrain:

- **Download Bypass**  
  ➔ Untuk langsung memulai download tanpa limit.
- **Show Bypass Links**  
  ➔ Untuk menampilkan link bypass.
