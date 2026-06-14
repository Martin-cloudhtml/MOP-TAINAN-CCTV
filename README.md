<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">

<title>監控牆 v6.3（iOS Swipe版）</title>

<style>

html, body{
    margin:0;
    padding:0;
    width:100%;
    height:100%;
    overflow:hidden;

    /* 🔥 iOS 關鍵 */
    position:fixed;
    inset:0;

    overscroll-behavior:none;
    background:#0b0f14;
    touch-action:none;
    -webkit-user-select:none;
    user-select:none;
}

/* ===== DESKTOP GRID ===== */
.grid{
    display:grid;
    width:100vw;
    height:100vh;
    gap:4px;

    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(2, 1fr);
}

/* ===== CAMERA ===== */
.camera{
    position:relative;
    overflow:hidden;
    background:#000;
    cursor:pointer;
}

/* iframe（讓它不要吃手勢） */
iframe{
    position:absolute;
    inset:0;
    width:100%;
    height:100%;
    border:0;

    pointer-events:auto;
}

/* title */
.title{
    position:absolute;
    top:6px;
    left:6px;
    z-index:10;

    background:rgba(0,0,0,0.7);
    color:#00ff7f;

    padding:4px 8px;
    border-radius:4px;
    font-size:12px;
}

/* ===== DESKTOP FOCUS ===== */
.focusOverlay{
    position:fixed;
    inset:0;
    background:rgba(0,0,0,0.92);

    display:none;
    align-items:center;
    justify-content:center;

    z-index:9999;
}

.focusOverlay.active{
    display:flex;
}

.focusCam{
    width:100vw;
    height:100vh;
}

/* ===== MOBILE SWIPE VIEW ===== */
.swipeView{
    display:none;
    width:100vw;
    height:100vh;
    position:relative;
    overflow:hidden;
}

/* 單一畫面 */
.swipeCam{
    width:100%;
    height:100%;
    position:absolute;
    inset:0;
}

/* ===== RWD ===== */
@media (max-width:768px){

    .grid{
        display:none;
    }

    .swipeView{
        display:block;
    }
}

</style>
</head>

<body>

<!-- ===== DESKTOP GRID ===== -->
<div class="grid" id="grid">

    <div class="camera" onclick="focusCam(this)">
        <div class="title">CAM-01</div>
        <iframe src="https://trafficvideo.tainan.gov.tw/1914bcd5"></iframe>
    </div>

    <div class="camera" onclick="focusCam(this)">
        <div class="title">CAM-02</div>
        <iframe src="https://trafficvideo.tainan.gov.tw/3e6342c7"></iframe>
    </div>

    <div class="camera" onclick="focusCam(this)">
        <div class="title">CAM-03</div>
        <iframe src="https://trafficvideo2.tainan.gov.tw/26f189b2"></iframe>
    </div>

    <div class="camera" onclick="focusCam(this)">
        <div class="title">CAM-04</div>
        <iframe src="https://trafficvideo2.tainan.gov.tw/6d3a22cf"></iframe>
    </div>

    <div class="camera" onclick="focusCam(this)">
        <div class="title">CAM-05</div>
        <iframe src="https://trafficvideo2.tainan.gov.tw/b15b446c"></iframe>
    </div>

    <div class="camera" onclick="focusCam(this)">
        <div class="title">CAM-06</div>
        <iframe src="https://trafficvideo.tainan.gov.tw/48e70b5d"></iframe>
    </div>

</div>

<!-- ===== MOBILE SWIPE ===== -->
<div class="swipeView" id="swipeView"></div>

<!-- ===== DESKTOP FOCUS ===== -->
<div class="focusOverlay" id="overlay" onclick="closeFocus()"></div>

<script>

let cams = document.querySelectorAll(".camera");
let overlay = document.getElementById("overlay");

let currentIndex = null;

/* ===== DESKTOP FOCUS ===== */
function focusCam(el){

    const index = Array.from(cams).indexOf(el);

    if(overlay.classList.contains("active") && currentIndex === index){
        closeFocus();
        return;
    }

    currentIndex = index;

    const clone = el.cloneNode(true);
    clone.classList.add("focusCam");

    clone.onclick = closeFocus;

    overlay.innerHTML = "";
    overlay.appendChild(clone);

    overlay.classList.add("active");
}

/* ===== CLOSE ===== */
function closeFocus(){
    overlay.classList.remove("active");
    overlay.innerHTML = "";
    currentIndex = null;
}

/* ===== KEYBOARD ===== */
document.addEventListener("keydown", (e)=>{

    const key = parseInt(e.key);

    if(key >= 1 && key <= 9){
        const target = cams[key - 1];
        if(target) focusCam(target);
    }

    if(e.key === "Escape"){
        closeFocus();
    }
});

/* ===== MOBILE SWIPE ENGINE ===== */
let swipeIndex = 0;
let startX = 0;

function initSwipe(){

    const swipe = document.getElementById("swipeView");

    cams.forEach((cam, i)=>{

        const clone = cam.cloneNode(true);
        clone.classList.add("swipeCam");

        clone.style.display = (i === 0) ? "block" : "none";

        swipe.appendChild(clone);
    });

    document.addEventListener("touchstart", (e)=>{
        startX = e.touches[0].clientX;
    }, {passive:true});

    document.addEventListener("touchend", (e)=>{

        const endX = e.changedTouches[0].clientX;
        const diff = endX - startX;

        if(Math.abs(diff) > 50){

            const swipe = document.getElementById("swipeView");
            const items = swipe.querySelectorAll(".swipeCam");

            items[swipeIndex].style.display = "none";

            if(diff < 0){
                swipeIndex = (swipeIndex + 1) % items.length;
            }else{
                swipeIndex = (swipeIndex - 1 + items.length) % items.length;
            }

            items[swipeIndex].style.display = "block";
        }

    }, {passive:true});
}

/* ===== iOS 防彈滑動（重要） ===== */
document.addEventListener('touchmove', function(e){
    e.preventDefault();
}, { passive:false });

initSwipe();

</script>

</body>
</html>
