# xmanter.memehub 
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="description" content="Meme Hub - Upload, like, dan komen meme lucu bareng @Shadow_Zeroo!" />
  <title>Meme Hub by @Shadow_Zeroo</title>
  <link href="https://fonts.googleapis.com/css2?family=Comic+Neue:wght@400;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg: #f4f4f4;
      --text: #222;
      --card: #fff;
      --primary: #ff4757;
      --header: #111;
    }
    body.dark { --bg: #111; --text: #eee; --card: #1a1a1a; --header: #000; }
    * { margin:0; padding:0; box-sizing:border-box; }
    body { font-family:'Comic Neue',Arial,sans-serif; background:var(--bg); color:var(--text); transition:0.4s; }
    header { background:var(--header); padding:15px 20px; display:flex; justify-content:space-between; align-items:center; position:sticky; top:0; z-index:1000; box-shadow:0 4px 10px rgba(0,0,0,0.3); }
    .logo { font-size:28px; font-weight:bold; color:#ff4757; }
    nav a { color:#fff; margin:0 15px; text-decoration:none; font-weight:bold; }
    nav a:hover { color:#ff4757; }
    #dark-toggle { background:none; border:none; font-size:26px; cursor:pointer; color:#fff; }

    .hero { background:linear-gradient(rgba(0,0,0,0.75),rgba(0,0,0,0.75)), url('https://source.unsplash.com/random/1600x900/?meme,funny') center/cover; padding:120px 20px; text-align:center; color:#fff; }
    .hero h1 { font-size:56px; margin-bottom:15px; }
    .btn { background:#ff4757; color:#fff; padding:14px 32px; border:none; border-radius:50px; font-size:18px; cursor:pointer; margin:10px; transition:0.3s; }
    .btn:hover { background:#ff3742; transform:scale(1.05); }

    #upload-section { max-width:600px; margin:40px auto; padding:20px; background:var(--card); border-radius:16px; box-shadow:0 8px 25px rgba(0,0,0,0.2); text-align:center; }
    #drop-area { border:3px dashed #ff4757; border-radius:16px; padding:40px; background:rgba(255,71,87,0.05); cursor:pointer; transition:0.3s; }
    #drop-area.dragover { background:rgba(255,71,87,0.2); border-color:#ff3742; }
    #preview { max-width:100%; max-height:400px; margin:20px 0; border-radius:12px; display:none; }
    textarea { width:100%; padding:12px; margin:10px 0; border-radius:12px; border:1px solid #ddd; font-family:inherit; font-size:16px; }

    .content { max-width:1200px; margin:40px auto; padding:0 20px; display:grid; grid-template-columns:repeat(auto-fit, minmax(300px,1fr)); gap:30px; }
    .card { background:var(--card); border-radius:16px; overflow:hidden; box-shadow:0 8px 25px rgba(0,0,0,0.2); transition:0.3s; }
    .card:hover { transform:translateY(-10px); }
    .card img { width:100%; height:320px; object-fit:cover; }
    .card-body { padding:15px; }
    .caption { font-weight:bold; font-size:18px; margin-bottom:10px; text-align:center; }
    .actions { display:flex; justify-content:space-between; align-items:center; margin-bottom:10px; }
    .like-btn { background:none; border:none; font-size:28px; cursor:pointer; }
    .like-btn.liked { color:#ff4757; }
    .comments { margin-top:15px; }
    .comment { background:rgba(0,0,0,0.05); padding:8px 12px; margin:8px 0; border-radius:12px; font-size:14px; }
    .comment-input { width:100%; padding:10px; border-radius:12px; border:1px solid #ddd; margin-top:10px; }

    footer { background:var(--header); color:#aaa; text-align:center; padding:30px; margin-top:60px; }
  </style>
</head>
<body>

  <header>
    <div class="logo">MEME HUB</div>
    <nav>
      <a href="#">Beranda</a>
      <a href="#">Trending</a>
      <a href="#">Upload</a>
    </nav>
    <button id="dark-toggle">🌙</button>
  </header>

  <section class="hero">
    <h1>Meme Hub by @Shadow_Zeroo 😂🔥</h1>
    <p>Upload meme kamu sendiri, like & komen sepuasnya!</p>
    <button class="btn" onclick="document.getElementById('upload-section').scrollIntoView({behavior:'smooth'})">
      Upload Meme Sekarang
    </button>
  </section>

  <!-- Upload Section -->
  <div id="upload-section">
    <h2>Upload Meme Baru</h2>
    <div id="drop-area">
      <p>Drag & drop gambar di sini, atau klik untuk pilih file</p>
      <input type="file" id="file-input" accept="image/*" hidden>
      <img id="preview" src="" alt="Preview">
    </div>
    <textarea id="caption-input" placeholder="Kasih caption lucu dong..." rows="2"></textarea>
    <button class="btn" id="submit-meme">Post Meme!</button>
  </div>

  <!-- Meme Grid -->
  <section class="content" id="meme-container">
    <!-- meme akan muncul di sini -->
  </section>

  <footer>
    © 2025 Meme Hub by @Shadow_Zeroo<br>
    <small>Semua meme disimpan di browser kamu ⚡ No login needed!</small>
  </footer>

  <script>
    // Dark mode
    document.getElementById('dark-toggle').addEventListener('click', () => {
      document.body.classList.toggle('dark');
      toggle.textContent = document.body.classList.contains('dark') ? '☀️' : '🌙';
    });

    // Uploadana Upload
    const dropArea = document.getElementById('drop-area');
    const fileInput = document.getElementById('file-input');
    const preview = document.getElementById('preview');

    ['dragenter','dragover','dragleave','drop'].forEach(event => {
      dropArea.addEventListener(event, e => { e.preventDefault(); e.stopPropagation(); });
    });
    ['dragenter','dragover'].forEach(event => dropArea.addEventListener(event, () => dropArea.classList.add('dragover')));
    ['dragleave','drop'].forEach(event => dropArea.addEventListener(event, () => dropArea.classList.remove('dragover')));
    dropArea.addEventListener('drop', e => {
      const files = e.dataTransfer.files;
      if (files.length) handleFiles(files);
    });
    dropArea.addEventListener('click', () => fileInput.click());
    fileInput.addEventListener('change', () => handleFiles(fileInput.files));

    function handleFiles(files) {
      const file = files[0];
      if (file && file.type.startsWith('image/')) {
        const reader = new FileReader();
        reader.onload = e => {
          preview.src = e.target.result;
          preview.style.display = 'block';
        };
        reader.readAsDataURL(file);
      }
    }

    // Load & save memes dari localStorage
    let memes = JSON.parse(localStorage.getItem('memes')) || [];

    function saveMemes() {
      localStorage.setItem('memes', JSON.stringify(memes));
    }

    function renderMemes() {
      const container = document.getElementById('meme-container');
      container.innerHTML = '';
      memes.forEach((meme, index) => {
        const card = document.createElement('div');
        card.className = 'card';
        card.innerHTML = `
          <img src="${meme.src}" alt="Meme">
          <div class="card-body">
            <div class="caption">${meme.caption || 'Tanpa caption 🗿'}</div>
            <div class="actions">
              <button class="like-btn ${meme.liked ? 'liked' : ''}" data-index="${index}">
                ${meme.liked ? '❤️' : '🤍'} <span>${meme.likes || 0}</span>
              </button>
              <small>${new Date(meme.date).toLocaleString('id-ID')}</small>
            </div>
            <div class="comments" id="comments-${index}">
              ${meme.comments ? meme.comments.map(c => `<div class="comment"><strong>${c.name}:</strong> ${c.text}</div>`).join('') : ''}
            </div>
            <input type="text" class="comment-input" placeholder="Tulis komentar..." data-index="${index}">
          </div>
        `;
        container.appendChild(card);
      });

      // Like button
      document.querySelectorAll('.like-btn').forEach(btn => {
        btn.addEventListener('click', () => {
          const i = btn.dataset.index;
          if (memes[i].liked) {
            memes[i].likes--;
            memes[i].liked = false;
          } else {
            memes[i].likes++;
            memes[i].liked = true;
          }
          saveMemes();
          renderMemes();
        });
      });

      // Komentar
      document.querySelectorAll('.comment-input').forEach(input => {
        input.addEventListener('keypress', e => {
          if (e.key === 'Enter' && input.value.trim()) {
            const i = input.dataset.index;
            if (!memes[i].comments) memes[i].comments = [];
            memes[i].comments.push({
              name: prompt("Nama kamu siapa? (bisa pake nama samaran)", "Anon") || "Anon",
              text: input.value.trim()
            });
            saveMemes();
            renderMemes();
            input.value = '';
          }
        });
      });
    }

    // Submit meme
    document.getElementById('submit-meme').addEventListener('click', () => {
      const caption = document.getElementById('caption-input').value.trim();
      const src = preview.src;
      if (!src) return alert("Upload gambar dunya dulu dong bro!");
      
      memes.unshift({
        src,
        caption,
        likes: 0,
        liked: false,
        comments: [],
        date: new Date().toISOString()
      });
      saveMemes();
      renderMemes();

      // Reset form
      preview.src = ''; preview.style.display = 'none';
      document.getElementById('caption-input').value = '';
      alert("Meme berhasil dipost! 🔥");
    });

    // Init
    renderMemes();
  </script>
</body>
</html>