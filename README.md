<html lang="tg">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MKT SHOP | Premium Soft Edition</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.17.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.17.1/firebase-database-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --glass: rgba(255, 255, 255, 0.08);
            --glass-border: rgba(255, 255, 255, 0.15);
            --accent: #00f2fe;
            --premium-gold: #f1c40f;
            --bg-dark: #0f172a;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; -webkit-tap-highlight-color: transparent; }
        
        body { 
            background: var(--bg-dark);
            background-image: radial-gradient(at 0% 0%, rgba(0, 242, 254, 0.12) 0, transparent 50%), 
                              radial-gradient(at 100% 100%, rgba(79, 172, 254, 0.12) 0, transparent 50%);
            color: white;
            min-height: 100vh;
            display: flex;
            justify-content: center;
        }

        .app-container {
            width: 100%;
            max-width: 450px;
            background: rgba(15, 23, 42, 0.9);
            min-height: 100vh;
            position: relative;
            padding-bottom: 110px;
            border-left: 1px solid var(--glass-border);
            border-right: 1px solid var(--glass-border);
        }

        header { 
            padding: 20px; 
            position: sticky; 
            top: 0; 
            z-index: 1000; 
            background: rgba(15, 23, 42, 0.92);
            backdrop-filter: blur(25px);
            border-bottom: 1px solid var(--glass-border);
        }

        .header-top { display: flex; justify-content: space-between; align-items: center; }
        .logo { font-size: 28px; font-weight: 900; background: linear-gradient(45deg, #fff, var(--accent)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        
        .search-box {
            margin-top: 15px; 
            background: var(--glass); 
            border-radius: 20px; 
            padding: 12px 18px; 
            display: flex; 
            align-items: center;
            border: 1px solid var(--glass-border);
            box-shadow: inset 2px 2px 5px rgba(0,0,0,0.2);
        }
        .search-box input { background: none; border: none; color: white; margin-left: 10px; outline: none; width: 100%; font-size: 15px; }

        .cat-container {
            display: flex; gap: 12px; overflow-x: auto; padding: 20px; 
            scrollbar-width: none; background: transparent;
        }
        .cat-btn { 
            padding: 10px 22px; 
            background: #1e293b; 
            border: 1px solid rgba(255,255,255,0.05); 
            border-radius: 18px; 
            color: #94a3b8; 
            cursor: pointer; 
            white-space: nowrap; 
            font-weight: 600;
            transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            box-shadow: 4px 4px 10px rgba(0,0,0,0.3), -2px -2px 10px rgba(255,255,255,0.02);
        }
        .cat-btn.active { 
            background: var(--accent); 
            color: #0f172a; 
            transform: scale(1.05);
            box-shadow: 0 0 15px var(--accent);
            border: none;
        }

        .product-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; padding: 15px; }
        .card { 
            background: var(--glass); 
            border: 1px solid var(--glass-border); 
            border-radius: 28px; 
            padding: 10px; 
            transition: 0.3s; 
            position: relative;
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
        }
        .card:hover { transform: translateY(-5px); border-color: var(--accent); }
        .card img { width: 100%; height: 170px; object-fit: cover; border-radius: 22px; cursor: pointer; }
        
        /* Ислоҳи нарх дар карточка */
        .price-tag { color: var(--accent); font-weight: 800; font-size: 17px; margin-top: 8px; display: flex; align-items: baseline; gap: 5px; flex-wrap: wrap; }
        .old-price { font-size: 12px; color: #94a3b8; text-decoration: line-through; font-weight: 400; }

        .pin-tag { position: absolute; top: 15px; left: 15px; background: var(--premium-gold); color: #000; padding: 4px 10px; border-radius: 10px; font-size: 10px; font-weight: 900; z-index: 10; }

        .slider-wrapper { position: relative; width: 100%; height: 250px; border-radius: 25px; overflow: hidden; margin-bottom: 20px; box-shadow: 0 10px 30px rgba(0,0,0,0.5); }
        #m-img { width: 100%; height: 100%; object-fit: cover; transition: 0.4s ease; }
        .s-btn-nav { 
            position: absolute; top: 50%; transform: translateY(-50%); 
            background: rgba(255,255,255,0.1); backdrop-filter: blur(10px);
            color: white; border: 1px solid rgba(255,255,255,0.2); 
            width: 42px; height: 42px; border-radius: 50%; cursor: pointer; z-index: 10;
            display: flex; align-items: center; justify-content: center; transition: 0.3s;
        }
        .s-prev { left: 12px; } .s-next { right: 12px; }

        .social-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-top: 20px; }
        .s-btn { 
            padding: 14px; border-radius: 18px; text-decoration: none; color: white; 
            font-size: 14px; font-weight: 700; display: flex; align-items: center; 
            justify-content: center; gap: 8px; transition: 0.3s;
            border: 1px solid rgba(255,255,255,0.1);
        }
        .wa { background: #22c55e; } .tg { background: #3b82f6; }
        
        nav { 
            position: fixed; bottom: 25px; left: 50%; transform: translateX(-50%); 
            width: 85%; max-width: 380px; background: rgba(255,255,255,0.1); 
            backdrop-filter: blur(30px); border-radius: 30px; border: 1px solid var(--glass-border); 
            display: flex; justify-content: space-around; padding: 18px; z-index: 1000;
        }
        .nav-btn { color: rgba(255,255,255,0.3); font-size: 22px; cursor: pointer; }
        .nav-btn.active { color: var(--accent); text-shadow: 0 0 15px var(--accent); }

        .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.9); backdrop-filter: blur(15px); z-index: 2000; align-items: center; justify-content: center; padding: 20px; }
        .modal-content { background: #1e293b; border-radius: 35px; padding: 30px; width: 100%; max-width: 420px; text-align: center; border: 1px solid var(--glass-border); }

        #news-panel { display: none; position: absolute; top: 90px; left: 20px; right: 20px; background: #1e293b; border: 1px solid var(--glass-border); border-radius: 25px; padding: 20px; z-index: 1100; }
    </style>
</head>
<body>

<div class="app-container">
    <header>
        <div class="header-top">
            <div class="logo">MKT SHOP</div>
            <div onclick="toggleNews()" style="cursor:pointer; position:relative; padding: 5px;">
                <i class="fas fa-bell" style="font-size: 24px;"></i>
                <span id="badge" style="display:none; position:absolute; top:2px; right:2px; background:#ff4757; width:10px; height:10px; border-radius:50%; border:2px solid #0f172a;"></span>
            </div>
        </div>
        
        <div id="news-panel">
            <h3 style="margin-bottom:15px; color:var(--accent); display:flex; align-items:center; gap:10px;"><i class="fas fa-bullhorn"></i> Хабарҳо</h3>
            <div id="news-content" style="max-height:250px; overflow-y:auto; font-size:14px; text-align:left;"></div>
        </div>

        <div class="search-box">
            <i class="fas fa-search" style="opacity:0.5;"></i>
            <input type="text" id="searchInput" onkeyup="liveSearch()" placeholder="Ҷустуҷӯи мол...">
        </div>
    </header>

    <div class="cat-container">
        <button class="cat-btn active" onclick="filterCat('Ҳама', this)">Ҳама</button>
        <button class="cat-btn" onclick="filterCat('Телефон', this)">Телефон</button>
        <button class="cat-btn" onclick="filterCat('Техника', this)">Техника</button>
        <button class="cat-btn" onclick="filterCat('Либос', this)">Либос</button>
    </div>

    <div id="home-page" class="page">
        <div id="productGrid" class="product-grid"></div>
    </div>

    <div id="fav-page" class="page" style="display:none; padding:20px;">
        <h2 style="margin-bottom:20px;"><i class="fas fa-heart" style="color:#ff4757;"></i> Дилхоҳ</h2>
        <div id="favGrid" class="product-grid"></div>
    </div>

    <nav>
        <div class="nav-btn active" onclick="showPage('home-page', this)"><i class="fas fa-home"></i></div>
        <div class="nav-btn" onclick="showPage('fav-page', this); renderFavs()"><i class="fas fa-heart"></i></div>
    </nav>
</div>

<div id="p-modal" class="modal">
    <div class="modal-content">
        <div class="slider-wrapper">
            <button class="s-btn-nav s-prev" onclick="moveImg(-1)"><i class="fas fa-chevron-left"></i></button>
            <img id="m-img" src="">
            <button class="s-btn-nav s-next" onclick="moveImg(1)"><i class="fas fa-chevron-right"></i></button>
        </div>
        <h2 id="m-name" style="font-weight:800;"></h2>
        <div id="m-price-box" class="price-tag" style="font-size:26px; margin:10px 0; justify-content:center;"></div>
        <p id="m-desc" style="opacity:0.6; font-size:14px; margin-bottom:20px;"></p>
        
        <div class="social-grid">
            <a id="m-wa" href="#" target="_blank" class="s-btn wa"><i class="fab fa-whatsapp"></i> WhatsApp</a>
            <a id="m-tg" href="https://t.me/habib_2026" target="_blank" class="s-btn tg"><i class="fab fa-telegram-plane"></i> Telegram</a>
        </div>
        <button onclick="closeModal()" style="background:none; border:none; color:white; margin-top:25px; cursor:pointer; opacity:0.4;">БАРГАШТ</button>
    </div>
</div>

<script>
    const firebaseConfig = {
        apiKey: "AIzaSyD0bqFLASYLqZG3c0Xn08_Y1HiwSHVWwj4",
        authDomain: "habibjon2026-c7df4.firebaseapp.com",
        projectId: "habibjon2026-c7df4",
        storageBucket: "habibjon2026-c7df4.firebasestorage.app",
        messagingSenderId: "378177007395",
        appId: "1:378177007395:web:e4cfb100c4e3b5a0d52696"
    };
    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();
    
    let allProducts = [];
    let favorites = JSON.parse(localStorage.getItem('mkt_favs')) || [];
    let modalImgs = [];
    let modalIdx = 0;

    window.onload = () => {
        db.ref('products').on('value', snap => {
            allProducts = []; const data = snap.val();
            for(let id in data) allProducts.push({ id, ...data[id] });
            allProducts.sort((a,b) => (b.pinned || false) - (a.pinned || false));
            renderUI(allProducts);
        });

        db.ref('news').on('value', snap => {
            const data = snap.val(); let html = ""; let count = 0;
            for(let id in data) { 
                count++; 
                html += `<div style="padding:12px; border-bottom:1px solid rgba(255,255,255,0.05); background:rgba(255,255,255,0.02); border-radius:15px; margin-bottom:8px;">
                            <p>${data[id].txt}</p>
                            <small style="opacity:0.4; font-size:10px;">${data[id].date || ''}</small>
                         </div>`; 
            }
            document.getElementById('news-content').innerHTML = html || "Хабар нест.";
            document.getElementById('badge').style.display = count > 0 ? 'block' : 'none';
        });
    };

    function renderUI(list) {
        let html = "";
        list.forEach(p => {
            const isFav = favorites.includes(p.id);
            const mainImg = Array.isArray(p.images) ? p.images[0] : (p.img || "");
            
            // Намоиши нархи кӯҳна агар бошад
            let priceDisplay = p.oldPrice 
                ? `<span class="old-price">${p.oldPrice} TJS</span> <span>${p.price} TJS</span>`
                : `<span>${p.price} TJS</span>`;

            html += `<div class="card">
                ${p.pinned ? '<div class="pin-tag"><i class="fas fa-thumbtack"></i> TOP</div>' : ''}
                <i class="fa-heart ${isFav ? 'fas' : 'far'}" onclick="toggleFav('${p.id}')" style="position:absolute; top:15px; right:15px; z-index:10; cursor:pointer; font-size:20px; color:${isFav?'#ff4757':'rgba(255,255,255,0.3)'}"></i>
                <img src="${mainImg}" onclick="openModal('${p.id}')">
                <h4 style="font-size:14px; margin-top:10px; font-weight:500; color:#e2e8f0;">${p.name}</h4>
                <div class="price-tag">${priceDisplay}</div>
            </div>`;
        });
        document.getElementById('productGrid').innerHTML = html || "<p style='grid-column:1/3; text-align:center; opacity:0.5; padding:50px;'>Мол ёфт нашуд.</p>";
    }

    function openModal(id) {
        const p = allProducts.find(x => x.id === id);
        modalImgs = Array.isArray(p.images) ? p.images : [p.img];
        modalIdx = 0;
        document.getElementById('m-name').innerText = p.name;
        
        // Нархи модал бо скидка
        let modalPrice = p.oldPrice 
            ? `<span class="old-price" style="font-size:18px;">${p.oldPrice} TJS</span> <span>${p.price} TJS</span>`
            : `<span>${p.price} TJS</span>`;
        document.getElementById('m-price-box').innerHTML = modalPrice;

        document.getElementById('m-desc').innerText = p.desc || "Тавсиф барои ин мол мавҷуд нест.";
        document.getElementById('m-wa').href = `https://wa.me/992900000000?text=Салом! Ман мехоҳам инро харам: ${p.name}`;
        updateSlider();
        document.getElementById('p-modal').style.display = 'flex';
    }

    function moveImg(s) {
        modalIdx += s;
        if(modalIdx < 0) modalIdx = modalImgs.length - 1;
        if(modalIdx >= modalImgs.length) modalIdx = 0;
        updateSlider();
    }

    function updateSlider() { document.getElementById('m-img').src = modalImgs[modalIdx]; }
    function closeModal() { document.getElementById('p-modal').style.display = 'none'; }
    
    function toggleFav(id) {
        if(favorites.includes(id)) favorites = favorites.filter(f => f !== id);
        else favorites.push(id);
        localStorage.setItem('mkt_favs', JSON.stringify(favorites));
        renderUI(allProducts);
    }

    function showPage(pId, btn) {
        document.querySelectorAll('.page').forEach(p => p.style.display = 'none');
        document.getElementById(pId).style.display = 'block';
        document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
    }

    function toggleNews() { 
        const p = document.getElementById('news-panel'); 
        p.style.display = p.style.display === 'block' ? 'none' : 'block'; 
    }

    function liveSearch() { 
        const v = document.getElementById('searchInput').value.toLowerCase(); 
        renderUI(allProducts.filter(p => p.name.toLowerCase().includes(v))); 
    }

    function filterCat(c, b) { 
        document.querySelectorAll('.cat-btn').forEach(x => x.classList.remove('active')); 
        b.classList.add('active'); 
        renderUI(c === 'Ҳама' ? allProducts : allProducts.filter(p => p.cat === c)); 
    }

    function renderFavs() {
        const fData = allProducts.filter(p => favorites.includes(p.id));
        let h = "";
        fData.forEach(p => {
            const mImg = Array.isArray(p.images) ? p.images[0] : (p.img || "");
            h += `<div class="card"><img src="${mImg}" onclick="openModal('${p.id}')"><h4>${p.name}</h4><div class="price-tag">${p.price} TJS</div></div>`;
        });
        document.getElementById('favGrid').innerHTML = h || "<p style='grid-column:1/3; text-align:center; padding:50px;'>Рӯйхат холӣ аст.</p>";
    }
</script>
</body>
</html>
