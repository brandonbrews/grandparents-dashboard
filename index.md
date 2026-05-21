
---
layout: default
title: Home
---

<style>
    #photo-bg-1, #photo-bg-2 {
        position: fixed; top: 0; left: 0; width: 100%; height: 100%;
        background-size: cover; background-position: center;
        transition: opacity 2s ease-in-out; z-index: 1; background-color: #000;
    }
    #photo-bg-2 { opacity: 0; }

    .overlay-vignette {
        position: fixed; top: 0; left: 0; width: 100%; height: 100%;
        background: radial-gradient(circle, transparent 20%, rgba(0,0,0,0.75) 100%);
        z-index: 3; pointer-events: none;
    }

    /* ── NATURE CAPTION ── */
    #nature-caption {
        position: fixed; top: 28px; right: 28px;
        z-index: 20; text-align: right;
        opacity: 0; transition: opacity 0.8s ease; pointer-events: none;
    }
    #nature-caption.visible { opacity: 1; pointer-events: auto; }
    .caption-location {
        font-size: 1.5rem; font-weight: 700; color: #fff;
        text-transform: uppercase; letter-spacing: 3px;
        text-shadow: 0 2px 12px rgba(0,0,0,0.7); line-height: 1.2;
    }
    .caption-desc {
        font-size: 1rem; color: rgba(255,255,255,0.75);
        text-shadow: 0 2px 8px rgba(0,0,0,0.7);
        margin-top: 4px; max-width: 320px; margin-left: auto;
    }
    .caption-credit {
        font-size: 0.8rem; color: rgba(255,255,255,0.45);
        margin-top: 6px; text-shadow: 0 1px 6px rgba(0,0,0,0.6);
    }
    .caption-credit a { color: rgba(255,255,255,0.55); text-decoration: underline; }

    /* ── CLOCK / WEATHER ── */
    .bottom-ui {
        position: fixed; bottom: 28px; left: 0; width: 100%;
        display: flex; justify-content: space-between; align-items: flex-end;
        padding: 0 70px; z-index: 10;
    }
    .weather-box {
        width: 380px; background: rgba(0,0,0,0.4); backdrop-filter: blur(15px);
        padding: 20px; border-radius: 20px; border: 1px solid rgba(255,255,255,0.1);
    }
    .clock-box { text-align: right; text-shadow: 0 4px 20px rgba(0,0,0,0.5); }
    #time-display {
        font-size: 10rem; font-weight: 900; line-height: 0.8;
        margin: 0; letter-spacing: -6px; color: #fff;
    }
    #date-display {
        font-size: 2.4rem; font-weight: 300; text-transform: uppercase;
        letter-spacing: 6px; opacity: 0.8; color: #fff;
    }

    /* ── BG SELECTOR ── */
    .bg-selector {
        position: fixed; bottom: 96px; left: 50%;
        transform: translateX(-50%);
        z-index: 1001; display: flex; gap: 6px;
        background: rgba(0,0,0,0.6); backdrop-filter: blur(10px);
        -webkit-backdrop-filter: blur(10px);
        padding: 6px 10px; border-radius: 50px;
        border: 1px solid rgba(255,255,255,0.1);
        opacity: 0.25; transition: opacity 0.4s;
    }
    .bg-selector:hover, .bg-selector:focus-within { opacity: 1; }
    .btn-mode {
        background: transparent; color: rgba(255,255,255,0.7);
        border: none; padding: 6px 18px; border-radius: 50px;
        cursor: pointer; font-size: 1.2rem; font-weight: 700;
        text-transform: uppercase; letter-spacing: 1px;
        transition: color 0.3s, background 0.3s; white-space: nowrap;
        -webkit-tap-highlight-color: transparent; touch-action: manipulation;
    }
    .btn-mode:hover { color: #fff; }
    .btn-mode.active { background: rgba(255,255,255,0.15); color: #fff; }

    /* ── PHOTO NAV ARROWS ── */
    .photo-nav-arrow {
        position: fixed; top: 50%; transform: translateY(-50%);
        z-index: 100;
        background: rgba(255,255,255,0.08); backdrop-filter: blur(10px);
        border: 1px solid rgba(255,255,255,0.18);
        color: white; width: 64px; height: 64px; border-radius: 50%;
        cursor: pointer; font-size: 1.5rem;
        display: flex; align-items: center; justify-content: center;
        opacity: 0; transition: opacity 0.4s, background 0.3s;
        -webkit-tap-highlight-color: transparent; touch-action: manipulation;
    }
    body:hover .photo-nav-arrow { opacity: 1; }
    .photo-nav-arrow:hover { background: rgba(255,255,255,0.28); opacity: 1; }
    @media (hover: none) {
        .photo-nav-arrow { opacity: 0.5; }
        .bg-selector { opacity: 1; }
    }
    #arrow-prev { left: 24px; }
    #arrow-next { right: 24px; }

    /* ── UPLOAD BUTTON — subtle camera icon, top-left ── */
    #upload-btn {
        position: fixed; top: 20px; left: 24px;
        z-index: 500;
        background: rgba(0,0,0,0.35);
        backdrop-filter: blur(10px); -webkit-backdrop-filter: blur(10px);
        border: 1px solid rgba(255,255,255,0.15);
        border-radius: 50%;
        width: 44px; height: 44px;
        display: flex; align-items: center; justify-content: center;
        font-size: 1.2rem; cursor: pointer;
        opacity: 0; transition: opacity 0.4s;
        -webkit-tap-highlight-color: transparent; touch-action: manipulation;
        text-decoration: none;
    }
    body:hover #upload-btn { opacity: 1; }
    #upload-btn:hover { background: rgba(0,0,0,0.6); opacity: 1; }
    @media (hover: none) { #upload-btn { opacity: 0.6; } }
</style>

<div id="photo-bg-1"></div>
<div id="photo-bg-2"></div>
<div class="overlay-vignette"></div>

<div id="nature-caption">
    <div class="caption-location" id="caption-location"></div>
    <div class="caption-desc"     id="caption-desc"></div>
    <div class="caption-credit"   id="caption-credit"></div>
</div>

<button id="arrow-prev" class="photo-nav-arrow" onclick="prevPhoto()" aria-label="Previous photo">&#10094;</button>
<button id="arrow-next" class="photo-nav-arrow" onclick="nextPhoto()" aria-label="Next photo">&#10095;</button>

<!-- Upload button: visible on hover/touch, top-left corner -->
<a id="upload-btn" href="{{ site.baseurl }}/upload" title="Upload photos" aria-label="Upload photos">📷</a>

<div class="bottom-ui">
    <div class="weather-box">
        <a class="weatherwidget-io"
           href="https://forecast7.com/en/47d17n122d52/lakewood/?unit=us"
           data-label_1="LAKEWOOD" data-label_2="WASHINGTON"
           data-icons="Climacons Animated" data-theme="pure"
           data-basecolor="transparent" data-textcolor="#ffffff">LAKEWOOD WA</a>
        </a>
    </div>
    <div class="clock-box">
        <div id="time-display">00:00</div>
        <div id="date-display">LOADING</div>
    </div>
</div>

<!-- BG mode buttons — Jekyll renders only the modes listed in _config.yml -->
<div class="bg-selector">
    {% for mode in site.bg_modes %}
        {% if mode == 'family' %}
        <button class="btn-mode" id="btn-family" onclick="setMode('family')">Family Photos</button>
        {% endif %}
        {% if mode == 'bonus' %}
        <button class="btn-mode" id="btn-bonus" onclick="setMode('bonus')">Bonus Photos</button>
        {% endif %}
        {% if mode == 'nature' %}
        <button class="btn-mode" id="btn-nature" onclick="setMode('nature')">Daily Nature</button>
        {% endif %}
        {% if mode == 'crest' %}
        <button class="btn-mode" id="btn-crest"  onclick="setMode('crest')">Family Crest</button>
        {% endif %}
    {% endfor %}
</div>

<script>
    /* ── All config comes from Jekyll / _config.yml ──────────────── */
    var CLOUD        = '{{ site.cloudinary_cloud }}';
    var FOLDER       = '{{ site.cloudinary_folder }}';
    var FOLDER_BONUS = '{{ site.cloudinary_folder_bonus }}';
    var TAG          = '{{ site.cloudinary_tag }}';
    var CREST_PATH   = '{{ site.baseurl }}{{ site.crest_image }}';
    var UNSPLASH_KEY = '{{ site.unsplash_key }}';
    var BG_MODES     = {{ site.bg_modes | jsonify }};
    var BASE_URL     = '{{ site.baseurl }}';
    /* ─────────────────────────────────────────────────────────────── */

    var photoUrls         = [];
    var activeBg          = 1;
    var currentIndex      = 0;
    var slideshowInterval = null;

    /* ── Clock ── */
    function updateClock() {
        var now = new Date();
        var h   = now.getHours() % 12 || 12;
        var m   = now.getMinutes().toString().padStart(2,'0');
        document.getElementById('time-display').textContent = h + ':' + m;
        document.getElementById('date-display').textContent =
            now.toLocaleDateString('en-US',{weekday:'long',month:'short',day:'numeric'});
    }
    updateClock();
    setInterval(updateClock, 1000);

    /* ── Cross-fade ── */
    function applyBackground(url) {
        var nextBg  = activeBg === 1 ? 2 : 1;
        var current = document.getElementById('photo-bg-' + activeBg);
        var next    = document.getElementById('photo-bg-' + nextBg);
        var img     = new Image();
        img.onload  = function() {
            next.style.backgroundImage = "url('" + url + "')";
            next.style.opacity = 1;
            current.style.opacity = 0;
            activeBg = nextBg;
        };
        img.src = url;
    }

    /* ── Cloudinary: fetch by tag (works reliably across folders) ── */
    async function fetchByTag(tag) {
        var url = 'https://res.cloudinary.com/' + CLOUD
                + '/image/list/' + tag + '.json?cb=' + Date.now();
        var resp = await fetch(url);
        if (!resp.ok) throw new Error('Cloudinary ' + resp.status);
        var data = await resp.json();
        return shuffle(data.resources || []).map(function(r) {
            return 'https://res.cloudinary.com/' + CLOUD
                 + '/image/upload/q_auto,f_auto,w_2560,c_limit/'
                 + r.public_id + '.' + r.format;
        });
    }

    /* ── Cloudinary: fetch by folder prefix ── */
    async function fetchByFolder(folder) {
        // Cloudinary folder listing via resource search tag named after the folder
        // Falls back to tag if folder list isn't exposed
        var url = 'https://res.cloudinary.com/' + CLOUD
                + '/image/list/' + folder + '.json?cb=' + Date.now();
        var resp = await fetch(url);
        if (!resp.ok) throw new Error('Cloudinary ' + resp.status);
        var data = await resp.json();
        return shuffle(data.resources || []).map(function(r) {
            return 'https://res.cloudinary.com/' + CLOUD
                 + '/image/upload/q_auto,f_auto,w_2560,c_limit/'
                 + r.public_id + '.' + r.format;
        });
    }

    function shuffle(arr) {
        return arr.map(function(r){ return {r:r, s:Math.random()}; })
                  .sort(function(a,b){ return a.s - b.s; })
                  .map(function(o){ return o.r; });
    }

    async function loadPhotos(folder, tag) {
        try {
            // Try folder first; fall back to tag
            var urls = [];
            try { urls = await fetchByFolder(folder); } catch(e) {}
            if (urls.length === 0 && tag) { urls = await fetchByTag(tag); }
            if (urls.length === 0) throw new Error('No photos found');
            photoUrls    = urls;
            currentIndex = 0;
            applyBackground(photoUrls[0]);
            startSlideshow();
        } catch(e) {
            console.warn('Photo load failed:', e);
            applyBackground('https://images.unsplash.com/photo-1464822759023-fed622ff2c3b?q=80&w=2560');
        }
    }

    function startSlideshow() {
        if (slideshowInterval) clearInterval(slideshowInterval);
        slideshowInterval = setInterval(function(){
            currentIndex = (currentIndex + 1) % photoUrls.length;
            applyBackground(photoUrls[currentIndex]);
        }, 30000);
    }
    function resetSlideshow() {
        if (slideshowInterval) { clearInterval(slideshowInterval); startSlideshow(); }
    }

    function nextPhoto() {
        if (!photoUrls.length) return;
        currentIndex = (currentIndex + 1) % photoUrls.length;
        applyBackground(photoUrls[currentIndex]);
        resetSlideshow();
    }
    function prevPhoto() {
        if (!photoUrls.length) return;
        currentIndex = (currentIndex - 1 + photoUrls.length) % photoUrls.length;
        applyBackground(photoUrls[currentIndex]);
        resetSlideshow();
    }

    /* ── Nature caption ── */
    function showCaption(loc, desc, name, userUrl, photoUrl) {
        document.getElementById('caption-location').textContent = loc || 'Daily Nature';
        document.getElementById('caption-desc').textContent     = desc || '';
        var el = document.getElementById('caption-credit');
        el.innerHTML = name
            ? 'Photo by <a href="'+userUrl+'" target="_blank" rel="noopener">'+name+'</a>'
            + ' on <a href="'+photoUrl+'" target="_blank" rel="noopener">Unsplash</a>'
            : '';
        document.getElementById('nature-caption').classList.add('visible');
    }
    function hideCaption() {
        document.getElementById('nature-caption').classList.remove('visible');
    }

    /* ── Unsplash daily nature ── */
    async function fetchNature() {
        var today    = new Date();
        var cacheKey = 'nature-' + today.getFullYear() + '-' + (today.getMonth()+1) + '-' + today.getDate();
        var cached   = sessionStorage.getItem(cacheKey);
        if (cached) { var d = JSON.parse(cached); applyBackground(d.url); showCaption(d.loc,d.desc,d.name,d.userUrl,d.photoUrl); return; }

        if (!UNSPLASH_KEY || UNSPLASH_KEY === 'YOUR_UNSPLASH_ACCESS_KEY') {
            applyBackground('https://images.unsplash.com/photo-1470770841072-f978cf4d019e?q=80&w=2560&auto=format&fit=crop');
            showCaption('Daily Nature','Add unsplash_key to _config.yml','','','');
            return;
        }
        try {
            var resp = await fetch('https://api.unsplash.com/photos/random'
                +'?query=nature+landscape+scenic&orientation=landscape'
                +'&content_filter=high&client_id='+UNSPLASH_KEY);
            if (!resp.ok) throw new Error(resp.status);
            var p = await resp.json();
            var d = {
                url:     p.urls.raw+'&w=2560&q=85&fit=crop&auto=format',
                loc:     (p.location&&(p.location.name||p.location.city||p.location.country))||'',
                desc:    (p.description||p.alt_description||'').substring(0,80),
                name:    p.user.name,
                userUrl: p.user.links.html+'?utm_source=family_dashboard&utm_medium=referral',
                photoUrl:p.links.html+'?utm_source=family_dashboard&utm_medium=referral'
            };
            sessionStorage.setItem(cacheKey, JSON.stringify(d));
            applyBackground(d.url);
            showCaption(d.loc, d.desc, d.name, d.userUrl, d.photoUrl);
        } catch(e) {
            console.warn('Unsplash error:', e);
            applyBackground('https://images.unsplash.com/photo-1470770841072-f978cf4d019e?q=80&w=2560&auto=format&fit=crop');
        }
    }

    /* ── Mode selector ── */
    function setMode(mode) {
        if (BG_MODES.indexOf(mode) === -1) mode = BG_MODES[0];
        localStorage.setItem(BASE_URL + '-bg-mode', mode);

        document.querySelectorAll('.btn-mode').forEach(function(b){ b.classList.remove('active'); });
        var btn = document.getElementById('btn-' + mode);
        if (btn) btn.classList.add('active');

        var showArrows = mode === 'family' || mode === 'bonus';
        document.getElementById('arrow-prev').style.display = showArrows ? '' : 'none';
        document.getElementById('arrow-next').style.display = showArrows ? '' : 'none';

        if (slideshowInterval) { clearInterval(slideshowInterval); slideshowInterval = null; }
        photoUrls = [];
        hideCaption();

        if      (mode === 'family') { loadPhotos(FOLDER, TAG); }
        else if (mode === 'bonus')  { loadPhotos(FOLDER_BONUS, TAG + '-bonus'); }
        else if (mode === 'nature') { fetchNature(); }
        else if (mode === 'crest')  { applyBackground(CREST_PATH); }
    }

    var saved = localStorage.getItem(BASE_URL + '-bg-mode') || BG_MODES[0];
    setMode(saved);

    !function(d,s,id){
        var js,fjs=d.getElementsByTagName(s)[0];
        if(!d.getElementById(id)){js=d.createElement(s);js.id=id;
        js.src='https://weatherwidget.io/js/widget.min.js';
        fjs.parentNode.insertBefore(js,fjs);}
    }(document,'script','weatherwidget-io-js');
</script>