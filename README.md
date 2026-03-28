<html lang="tg">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MKT SHOP - Ultimate Edition</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.17.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.17.1/firebase-database-compat.js"></script>

    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
    
    <style>
        :root { --blue: #1a3a5f; --orange: #d35400; --bg: #f4f7f9; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        body { background: #cbd5e0; display: flex; justify-content: center; min-height: 100vh; }

        .app-container { width: 100%; max-width: 450px; background: var(--bg); min-height: 100vh; position: relative; padding-bottom: 80px; box-shadow: 0 0 20px rgba(0,0,0,0.1); }
        
        header { background: var(--blue); padding: 15px; color: white; border-radius: 0 0 20px 20px; position: sticky; top: 0; z-index: 1000; }
        .header-top { display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px; }
        
        .search-box { background: white; border-radius: 12px; display: flex; padding: 10px; align-items: center; }
        .search-box input { border: none; width: 100%; outline: none; padding-left: 10px; color: #333; font-size: 14px; }

        /* NOTIFICATION PANEL */
        #notif-panel { display: none; position: absolute; top: 60px; right: 10px; width: 280px; background: white; border-radius: 15px; padding: 15px; color: #333; box-shadow: 0 10px 25px rgba(0,0,0,0.2); z-index: 2000; max-height: 400px; overflow-y: auto; }
        .news-item { border-bottom: 1px solid #eee; padding: 10px 0; position: relative; }
        .news-item p { font-size: 13px; color: #444; margin-right: 20px; }
        .news-item small { color: var(--orange); font-size: 11px; }
        .del-btn-news { position: absolute; top: 10px; right: 0; color: #ff4757; cursor: pointer; display: none; }

        .categories { display: flex; gap: 10px; overflow-x: auto; padding: 15px 10px; scrollbar-width: none; }
        .cat-btn { padding: 8px 18px; background: white; border-radius: 20px; border: none; font-size: 13px; cursor: pointer; white-space: nowrap; box-shadow: 0 2px 5px rgba(0,0,0,0.05); font-weight: 600; color: #555; }
        .cat-btn.active { background: var(--orange); color: white; }

        .page { display: none; padding: 15px; animation: fadeIn 0.3s; }
        .page.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
        .card { background: white; border-radius: 15px; padding: 12px; text-align: center; position: relative; box-shadow: 0 4px 10px rgba(0,0,0,0.03); }
        .card img { width: 100%; height: 120px; object-fit: contain; border-radius: 10px; cursor: pointer; }
        .price { color: var(--orange); font-weight: 700; margin-top: 5px; }
        .fav-btn { position: absolute; top: 10px; right: 10px; font-size: 18px; cursor: pointer; color: #ddd; z-index: 5; }
        .fav-btn.active { color: #ff4757; }
        .admin-del-prod { background: #ff4757; color: white; border: none; padding: 5px 10px; border-radius: 5px; font-size: 10px; margin-top: 10px; cursor: pointer; display: none; }

        .admin-section { background: white; padding: 15px; border-radius: 15px; margin-bottom: 20px; border: 2px dashed var(--blue); }
        .auth-input { width: 100%; padding: 12px; border: 1px solid #ddd; border-radius: 10px; margin-bottom: 10px; outline: none; }

        nav { position: fixed; bottom: 0; width: 100%; max-width: 450px; background: white; display: flex; justify-content: space-around; padding: 12px; border-top: 1px solid #eee; z-index: 1000; }
        .nav-link { color: #bbb; font-size: 24px; cursor: pointer; }
        .nav-link.active { color: var(--blue); }

        .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.85); z-index: 3000; align-items: center; justify-content: center; padding: 20px; }
        .modal-content { background: white; width: 100%; border-radius: 25px; padding: 20px; text-align: center; position: relative; }
        .social-btn { display: flex; align-items: center; justify-content: center; gap: 10px; padding: 12px; color: white; text-decoration: none; border-radius: 15px; margin-top: 10px; font-weight: bold; }
    </style>
</head>
<body>

<div class="app-container">
    <header>
        <div class="header-top">
            <b style="font-size: 22px;">MKT SHOP</b>
            <div onclick="toggleNotif()" style="position:relative; cursor:pointer; padding: 5px;">
                <i class="fas fa-bell" style="font-size: 22px;"></i>
                <span id="badge" style="display:none; position:absolute; top:0; right:0; background:red; color:white; font-size:10px; min-width:16px; height:16px; border-radius:50%; display:flex; align-items:center; justify-content:center;">0</span>
            </div>
        </div>
        <div class="search-box">
            <i class="fas fa-search" style="color:#aaa"></i>
            <input type="text" id="searchInput" placeholder="Ҷустуҷӯи мол..." onkeyup="liveSearch()">
        </div>
        <div id="notif-panel">
            <h4 style="border-bottom:2px solid #eee; padding-bottom:8px; margin-bottom:10px;">📢 Хабарҳо</h4>
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
    <div id="favs" class="page"><h3>Дилхоҳ ❤️</h3><br><div id="favGrid" class="grid"></div></div>

    <div id="admin" class="page">
        <div class="admin-section">
            <h4>📦 Иловаи Мол</h4><br>
            <input type="text" id="p-name" class="auth-input" placeholder="Номи мол">
            <input type="number" id="p-price" class="auth-input" placeholder="Нарх">
            <select id="p-cat" class="auth-input">
                <option value="Телефон">Телефон</option>
                <option value="Техника">Техника</option>
                <option value="Либос">Либос</option>
            </select>
            <textarea id="p-desc" class="auth-input" placeholder="Тавсиф..."></textarea>
            <input type="file" id="p-file" hidden onchange="processImg(this)">
            <label for="p-file" style="display:block; background:#eee; padding:12px; border-radius:10px; text-align:center; cursor:pointer; margin-bottom:10px;">📸 Интихоби сурат</label>
            <button onclick="saveProduct()" style="width:100%; background:green; color:white; border:none; padding:14px; border-radius:12px; font-weight:bold;">ЗАХИРА</button>
        </div>
        <div class="admin-section" style="border-color: orange;">
            <h4>🚀 Нашри Хабар</h4><br>
            <textarea id="n-text" class="auth-input" placeholder="Матни хабар..."></textarea>
            <button onclick="saveNews()" style="width:100%; background:orange; color:white; border:none; padding:12px; border-radius:12px; font-weight:bold;">ФИРИСТОДАН</button>
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
        <span onclick="closeModal()" style="position:absolute; top:15px; right:20px; font-size:30px; cursor:pointer;">&times;</span>
        <img id="m-img" style="width:100%; height:180px; object-fit:contain; border-radius:15px;">
        <h2 id="m-name" style="margin-top:10px;"></h2>
        <h3 id="m-price" style="color:orange;"></h3>
        <p id="m-desc" style="font-size:13px; color:#666; margin-bottom:15px; text-align:left;"></p>
        
        <a href="https://t.me/habib_2026" target="_blank" class="social-btn" style="background:#0088cc;"><i class="fab fa-telegram-plane"></i> Telegram</a>
        
        <a href="instagram://user?username=habib_2026" class="social-btn" 
           onclick="window.open('https://instagram.com/habib_2026', '_blank'); return false;"
           style="background:linear-gradient(45deg, #f09433, #dc2743, #cc2366);">
           <i class="fab fa-instagram"></i> Instagram
        </a>
        
        <a href="https://tiktok.com/@habib_2026" target="_blank" class="social-btn" style="background:black;"><i class="fab fa-tiktok"></i> TikTok</a>
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
        if(isAdmin) { navigate('admin', el); } 
        else {
            if(prompt("Пароли админ:") === "12") {
                isAdmin = true;
                renderUI(allProducts); 
                db.ref('news').once('value', renderNews);
                navigate('admin', el);
            }
        }
    }

    function processImg(input) {
        const reader = new FileReader();
        reader.onload = (e) => tempImg = e.target.result;
        reader.readAsDataURL(input.files[0]);
    }

    function saveProduct() {
        const name = document.getElementById('p-name').value, price = document.getElementById('p-price').value,
              cat = document.getElementById('p-cat').value, desc = document.getElementById('p-desc').value;
        if(name && price && tempImg) {
            db.ref('products').push({ name, price, cat, desc, img: tempImg });
            alert("Илова шуд!"); location.reload();
        }
    }

    function saveNews() {
        const txt = document.getElementById('n-text').value;
        if(txt) {
            db.ref('news').push({ txt, date: new Date().toLocaleDateString() });
            document.getElementById('n-text').value = "";
        }
    }

    window.onload = () => {
        db.ref('products').on('value', (snap) => {
            allProducts = []; const data = snap.val();
            for(let id in data) allProducts.push({ id, ...data[id] });
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
                <i class="fas fa-trash del-btn-news" style="display:${isAdmin?'block':'none'}" onclick="deleteItem('news/${id}')"></i>
            </div>`;
        }
        document.getElementById('news-list-content').innerHTML = html || "Хабар нест.";
        const badge = document.getElementById('badge');
        if(count > 0) { badge.style.display = 'flex'; badge.innerText = count; }
        else { badge.style.display = 'none'; }
    }

    function renderUI(list) {
        let html = "";
        list.forEach(p => {
            const isFav = favorites.includes(p.id);
            html += `<div class="card">
                <i class="fa-heart fav-btn ${isFav?'fas active':'far'}" onclick="toggleFav('${p.id}')"></i>
                <img src="${p.img}" onclick="openModal('${p.img}', '${p.name}', '${p.price}', '${p.desc}')">
                <h4>${p.name}</h4><p class="price">${p.price} TJS</p>
                <button class="admin-del-prod" style="display:${isAdmin?'inline-block':'none'}" onclick="deleteItem('products/${p.id}')">Удалить</button>
            </div>`;
        });
        document.getElementById('productGrid').innerHTML = html || "Мол нест.";
    }

    function toggleFav(id) {
        if(!favorites.includes(id)) favorites.push(id);
        else favorites = favorites.filter(f => f !== id);
        localStorage.setItem('mkt_favs', JSON.stringify(favorites));
        renderUI(allProducts);
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

    function toggleNotif() {
        const p = document.getElementById('notif-panel');
        p.style.display = p.style.display === 'block' ? 'none' : 'block';
    }

    function openModal(img, name, price, desc) {
        document.getElementById('m-img').src = img;
        document.getElementById('m-name').innerText = name;
        document.getElementById('m-price').innerText = price + " TJS";
        document.getElementById('m-desc').innerText = desc || "Тавсиф надорад.";
        document.getElementById('p-modal').style.display = 'flex';
    }

    function closeModal() { document.getElementById('p-modal').style.display = 'none'; }
    function deleteItem(path) { if(confirm("Нест карда шавад?")) db.ref(path).remove(); }
    function navigate(id, el) {
        document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
        document.getElementById(id).classList.add('active');
        document.querySelectorAll('.nav-link').forEach(n => n.classList.remove('active'));
        el.classList.add('active');
    }
</script>
</body>
</html>
