 <html lang="tg">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MKT SHOP - Professional Edition</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.17.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.17.1/firebase-database-compat.js"></script>

    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
    
    <style>
        :root { --blue: #1a3a5f; --orange: #d35400; --bg: #e2e8f0; --green: #25D366; --gold: #f1c40f; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        body { background: var(--bg); display: flex; justify-content: center; min-height: 100vh; padding: 20px 0; }

        .app-container { 
            width: 100%; max-width: 480px; background: #f8fafc; min-height: 90vh; 
            position: relative; padding-bottom: 80px; box-shadow: 0 10px 30px rgba(0,0,0,0.15);
            border-radius: 30px; overflow: hidden;
        }
        
        header { background: var(--blue); padding: 20px; color: white; border-radius: 0 0 25px 25px; position: sticky; top: 0; z-index: 1000; }
        .header-top { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; }
        
        .search-box { background: white; border-radius: 15px; display: flex; padding: 12px; align-items: center; }
        .search-box input { border: none; width: 100%; outline: none; padding-left: 10px; color: #333; font-size: 15px; }

        #notif-panel { 
            display: none; position: absolute; top: 75px; right: 15px; width: 300px; 
            background: white; border-radius: 20px; padding: 15px; color: #333; 
            box-shadow: 0 15px 35px rgba(0,0,0,0.2); z-index: 2000; max-height: 400px; overflow-y: auto; 
        }
        .news-item { border-bottom: 1px solid #f1f5f9; padding: 12px 0; position: relative; }
        .news-del-btn { position: absolute; right: 5px; top: 12px; color: #ef4444; cursor: pointer; padding: 5px; }

        .categories { display: flex; gap: 10px; overflow-x: auto; padding: 20px 15px; scrollbar-width: none; }
        .cat-btn { padding: 10px 22px; background: white; border-radius: 15px; border: none; font-size: 14px; cursor: pointer; white-space: nowrap; box-shadow: 0 4px 6px rgba(0,0,0,0.05); font-weight: 600; color: #475569; }
        .cat-btn.active { background: var(--orange); color: white; }

        .page { display: none; padding: 15px; animation: fadeIn 0.4s ease-out; }
        .page.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 18px; }
        .card { background: white; border-radius: 20px; padding: 15px; text-align: center; position: relative; box-shadow: 0 4px 12px rgba(0,0,0,0.04); border: 1px solid #f1f5f9; }
        .card.pinned { border: 2px solid var(--gold); background: #fffdf2; }
        .pin-icon { position: absolute; top: 10px; left: 10px; color: var(--gold); font-size: 14px; }
        
        .card img { width: 100%; height: 140px; object-fit: contain; border-radius: 12px; cursor: pointer; background: #fafafa; }
        .price { color: var(--orange); font-weight: 700; font-size: 16px; margin: 5px 0; }
        .old-price { text-decoration: line-through; color: #94a3b8; font-size: 13px; margin-right: 5px; }
        
        .fav-btn { position: absolute; top: 10px; right: 10px; font-size: 20px; cursor: pointer; color: #cbd5e1; z-index: 5; }
        .fav-btn.active { color: #ef4444; }

        .admin-section { background: white; padding: 20px; border-radius: 20px; margin-bottom: 20px; border: 2px dashed #cbd5e1; }
        .auth-input { width: 100%; padding: 14px; border: 1.5px solid #e2e8f0; border-radius: 12px; margin-bottom: 12px; outline: none; }

        nav { position: absolute; bottom: 0; width: 100%; background: white; display: flex; justify-content: space-around; padding: 15px; border-top: 1px solid #f1f5f9; z-index: 1000; }
        .nav-link { color: #94a3b8; font-size: 26px; cursor: pointer; }
        .nav-link.active { color: var(--blue); }

        .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.8); z-index: 3000; align-items: center; justify-content: center; padding: 20px; }
        .modal-content { background: white; width: 100%; max-width: 400px; border-radius: 30px; padding: 25px; text-align: center; position: relative; }
        .social-btn { display: flex; align-items: center; justify-content: center; gap: 10px; padding: 14px; color: white; text-decoration: none; border-radius: 15px; margin-top: 10px; font-weight: 700; }
        .admin-btns { display: flex; flex-direction: column; gap: 5px; margin-top: 10px; }
        .btn-sm { border: none; padding: 6px; border-radius: 8px; font-size: 11px; cursor: pointer; font-weight: 600; color: white; }
    </style>
</head>
<body>

<div class="app-container">
    <header>
        <div class="header-top">
            <b style="font-size: 24px;">MKT SHOP</b>
            <div onclick="toggleNotif()" style="position:relative; cursor:pointer; padding: 5px;">
                <i class="fas fa-bell" style="font-size: 24px;"></i>
                <span id="badge" style="display:none; position:absolute; top:0; right:0; background:#ef4444; color:white; font-size:11px; min-width:18px; height:18px; border-radius:50%; display:flex; align-items:center; justify-content:center;">0</span>
            </div>
        </div>
        <div class="search-box">
            <i class="fas fa-search" style="color:#94a3b8"></i>
            <input type="text" id="searchInput" placeholder="Ҷустуҷӯи мол..." onkeyup="liveSearch()">
        </div>
        <div id="notif-panel">
            <h4 style="margin-bottom:10px; color:var(--blue);">📢 Хабарҳо</h4>
            <div id="news-list-content"></div>
        </div>
    </header>

    <div class="categories">
        <button class="cat-btn active" onclick="filterCat('Ҳама', this)">Ҳама</button>
        <button class="cat-btn" onclick="filterCat('Телефон', this)">Телефон</button>
        <button class="cat-btn" onclick="filterCat('Техника', this)">Техника</button>
        <button class="cat-btn" onclick="filterCat('Либос', this)">Либос</button>
    </div>

    <div id="home" class="page active"><div id="productGrid" class="grid"></div></div>
    <div id="favs" class="page"><h3 style="margin-bottom:20px;">Дилхоҳ ❤️</h3><div id="favGrid" class="grid"></div></div>

    <div id="admin" class="page">
        <div class="admin-section">
            <h4>📦 Иловаи Мол</h4><br>
            <input type="text" id="p-name" class="auth-input" placeholder="Номи мол">
            <input type="number" id="p-price" class="auth-input" placeholder="Нархи ҳозира (TJS)">
            <input type="number" id="p-old-price" class="auth-input" placeholder="Нархи куҳна (ихтиёрӣ)">
            <select id="p-cat" class="auth-input">
                <option value="Телефон">Телефон</option>
                <option value="Техника">Техника</option>
                <option value="Либос">Либос</option>
            </select>
            <textarea id="p-desc" class="auth-input" placeholder="Тавсиф..." style="height:80px;"></textarea>
            <input type="file" id="p-file" hidden onchange="processImg(this)">
            <label for="p-file" style="display:block; background:#f1f5f9; padding:15px; border-radius:12px; text-align:center; cursor:pointer; margin-bottom:15px; font-weight:600;">📸 Интихоби сурат</label>
            <button onclick="saveProduct()" style="width:100%; background:#22c55e; color:white; border:none; padding:16px; border-radius:15px; font-weight:700;">ЗАХИРА</button>
        </div>
        <div class="admin-section" style="border-color: orange;">
            <h4>🚀 Нашри Хабар</h4><br>
            <textarea id="n-text" class="auth-input" placeholder="Матни хабар..."></textarea>
            <button onclick="saveNews()" style="width:100%; background:orange; color:white; border:none; padding:14px; border-radius:15px; font-weight:700;">ФИРИСТОДАН</button>
        </div>
    </div>

    <nav>
        <div class="nav-link active" onclick="navigate('home', this)"><i class="fas fa-home"></i></div>
        <div class="nav-link" onclick="navigate('favs', this); renderFavs()"><i class="fas fa-heart"></i></div>
        <div class="nav-link" onclick="handleAdminLogin(this)"><i class="fas fa-user-shield"></i></div>
    </nav>
</div>

<div id="p-modal" class="modal">
    <div class="modal-content">
        <span onclick="closeModal()" style="position:absolute; top:20px; right:25px; font-size:35px; cursor:pointer;">&times;</span>
        <img id="m-img" style="width:100%; height:200px; object-fit:contain; border-radius:20px; background:#f8fafc;">
        <h2 id="m-name" style="margin-top:15px;"></h2>
        <h3 id="m-price" style="color:var(--orange); margin:10px 0;"></h3>
        <p id="m-desc" style="font-size:14px; color:#64748b; margin-bottom:20px;"></p>
        
        <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
            <a id="m-wa" href="#" target="_blank" class="social-btn" style="background:var(--green); margin-top:0;"><i class="fab fa-whatsapp"></i> WhatsApp</a>
            <a href="https://t.me/habib_2026" target="_blank" class="social-btn" style="background:#0ea5e9; margin-top:0;"><i class="fab fa-telegram-plane"></i> Telegram</a>
        </div>
        <a href="instagram://user?username=habib_2026" class="social-btn" onclick="window.open('https://instagram.com/habib_2026', '_blank'); return false;" style="background:linear-gradient(45deg, #f09433, #dc2743, #cc2366);"><i class="fab fa-instagram"></i> Instagram</a>
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
    let tempImg = "", allProducts = [], isAdmin = false;
    let favorites = JSON.parse(localStorage.getItem('mkt_favs')) || [];

    function handleAdminLogin(el) {
        if(isAdmin) navigate('admin', el);
        else if(prompt("Пароли админ:") === "ANONYMOUS*2009") { isAdmin = true; renderUI(allProducts); db.ref('news').once('value', renderNews); navigate('admin', el); }
    }

    function processImg(input) {
        const reader = new FileReader();
        reader.onload = (e) => tempImg = e.target.result;
        reader.readAsDataURL(input.files[0]);
    }

    function saveProduct() {
        const name = document.getElementById('p-name').value, price = document.getElementById('p-price').value,
              oldP = document.getElementById('p-old-price').value, cat = document.getElementById('p-cat').value,
              desc = document.getElementById('p-desc').value;
        if(name && price && tempImg) {
            db.ref('products').push({ name, price, oldPrice: oldP || "", cat, desc, img: tempImg, pinned: false }).then(() => location.reload());
        }
    }

    function togglePin(id, currentStatus) {
        db.ref('products/' + id).update({ pinned: !currentStatus });
    }

    function saveNews() {
        const txt = document.getElementById('n-text').value;
        if(txt) db.ref('news').push({ txt, date: new Date().toLocaleDateString() }).then(() => document.getElementById('n-text').value="");
    }

    window.onload = () => {
        db.ref('products').on('value', (snap) => {
            allProducts = []; const data = snap.val();
            for(let id in data) allProducts.push({ id, ...data[id] });
            allProducts.sort((a, b) => (b.pinned || false) - (a.pinned || false));
            renderUI(allProducts);
        });
        db.ref('news').on('value', renderNews);
    };

    function renderNews(snap) {
        const data = snap.val(); let html = "", count = 0;
        for(let id in data) {
            count++;
            html += `<div class="news-item">
                <p>${data[id].txt}</p><small>${data[id].date}</small>
                ${isAdmin ? `<i class="fas fa-trash-alt news-del-btn" onclick="deleteItem('news/${id}')"></i>` : ''}
            </div>`;
        }
        document.getElementById('news-list-content').innerHTML = html || "Хабар нест.";
        document.getElementById('badge').style.display = count > 0 ? 'flex' : 'none';
        document.getElementById('badge').innerText = count;
    }

    function renderUI(list) {
        let html = "";
        list.forEach(p => {
            const isFav = favorites.includes(p.id);
            const op = p.oldPrice ? `<span class="old-price">${p.oldPrice} TJS</span>` : "";
            const isPinned = p.pinned || false;
            html += `<div class="card ${isPinned ? 'pinned' : ''}">
                ${isPinned ? '<i class="fas fa-thumbtack pin-icon"></i>' : ''}
                <i class="fa-heart fav-btn ${isFav?'fas active':'far'}" onclick="toggleFav('${p.id}')"></i>
                <img src="${p.img}" onclick="openModal('${p.img}', '${p.name}', '${p.price}', '${p.desc}')">
                <h4>${p.name}</h4><p class="price">${op}${p.price} TJS</p>
                ${isAdmin ? `<div class="admin-btns">
                    <button onclick="togglePin('${p.id}', ${isPinned})" class="btn-sm" style="background:var(--gold)">${isPinned ? '📌 Открепить' : '📌 Закрепить'}</button>
                    <button onclick="deleteItem('products/${p.id}')" class="btn-sm" style="background:red">Удалить</button>
                </div>` : ''}
            </div>`;
        });
        document.getElementById('productGrid').innerHTML = html || "<p style='grid-column:1/3; text-align:center;'>Мол нест.</p>";
    }

    function toggleFav(id) {
        if(!favorites.includes(id)) favorites.push(id); else favorites = favorites.filter(f => f !== id);
        localStorage.setItem('mkt_favs', JSON.stringify(favorites));
        renderUI(allProducts); if(document.getElementById('favs').classList.contains('active')) renderFavs();
    }

    function renderFavs() {
        const favList = allProducts.filter(p => favorites.includes(p.id));
        let html = "";
        favList.forEach(p => {
            html += `<div class="card"><i class="fas fa-heart fav-btn active" onclick="toggleFav('${p.id}')"></i>
                <img src="${p.img}" onclick="openModal('${p.img}', '${p.name}', '${p.price}', '${p.desc}')">
                <h4>${p.name}</h4><p class="price">${p.price} TJS</p></div>`;
        });
        document.getElementById('favGrid').innerHTML = html || "<p style='grid-column:1/3; text-align:center;'>Холӣ аст.</p>";
    }

    function filterCat(cat, btn) {
        document.querySelectorAll('.cat-btn').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        renderUI(cat === 'Ҳама' ? allProducts : allProducts.filter(p => p.cat === cat));
    }

    function liveSearch() {
        const term = document.getElementById('searchInput').value.toLowerCase();
        renderUI(allProducts.filter(p => p.name.toLowerCase().includes(term)));
    }

    function openModal(img, name, price, desc) {
        document.getElementById('m-img').src = img;
        document.getElementById('m-name').innerText = name;
        document.getElementById('m-price').innerText = price + " TJS";
        document.getElementById('m-desc').innerText = desc || "Тавсиф надорад.";
        document.getElementById('m-wa').href = `https://wa.me/992900000000?text=Салом! Ман мехоҳам инро харам: ${name}`;
        document.getElementById('p-modal').style.display = 'flex';
    }

    function closeModal() { document.getElementById('p-modal').style.display = 'none'; }
    function deleteItem(path) { if(confirm("Нест карда шавад?")) db.ref(path).remove(); }
    function toggleNotif() { const p = document.getElementById('notif-panel'); p.style.display = p.style.display === 'block' ? 'none' : 'block'; }
    function navigate(id, el) {
        document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
        document.getElementById(id).classList.add('active');
        document.querySelectorAll('.nav-link').forEach(n => n.classList.remove('active'));
        el.classList.add('active');
    }
</script>
</body>
</html>
