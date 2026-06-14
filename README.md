<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>監控牆 v6.2（跨裝置版）</title>

<style>

html, body{
    margin:0;
    padding:0;
    width:100%;
    height:100%;
    overflow:hidden;
    background:#0b0f14;
    font-family:Arial;
}

/* ===== GRID（桌面） ===== */
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
    min-height:0;

    border:2px solid #1f2a33;
}

iframe{
    position:absolute;
    inset:0;
    width:100%;
    height:100%;
    border:0;
}

/* 標題 */
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

/* ===== FOCUS（桌面） ===== */
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
    animation:zoomIn 0.2s ease;
}

@keyframes zoomIn{
    from{transform:scale(0.85); opacity:0;}
    to{transform:scale(1); opacity:1;}
}

/* ===== MOBILE MODE ===== */
@media (max-width: 768px){

    html, body{
        overflow:auto;
    }

    .grid{
        grid-template-columns: 1fr;
        grid-template-rows: auto;
        height:auto;
    }

    .camera{
        aspect-ratio:16/9;
    }

}

/* ===== SWIPE VIEW（手機單畫面） ===== */
.swipeView{
    display:none;
    width:100vw;
    height:100vh;
    overflow:hidden;
    position:relative;
}

.swipeCam{
    width:100%;
    height:100%;
    position:absolute;
    top:0;
    left:0;
}

/* 手機才啟用 swipe */
@media (max-width:768px){
    .swipeView{ display:block; }
    .grid{ display:none; }
}

</style>
</head>

<body>

<!-- ===== 桌面監控牆 ===== -->
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

<!-- ===== mobile swipe view ===== -->
<div class="swipeView" id="swipeView"></div>

<!-- focus -->
<div class="focusOverlay" id="overlay" onclick="overlayClick(event)"></div>

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

/* ===== overlay click ===== */
function overlayClick(e){
    if(e.target.id === "overlay"){
        closeFocus();
    }
}

/* ===== KEYBOARD (desktop) ===== */
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

/* ===== MOBILE SWIPE MODE ===== */
let swipeIndex = 0;

function initSwipe(){

    const swipe = document.getElementById("swipeView");

    cams.forEach((cam, i)=>{
        const clone = cam.cloneNode(true);
        clone.classList.add("swipeCam");

        clone.style.display = (i === 0) ? "block" : "none";

        swipe.appendChild(clone);
    });

    swipe.addEventListener("touchstart", handleTouch);
}

let startX = 0;

function handleTouch(e){
    startX = e.touches[0].clientX;
}

document.addEventListener("touchend", (e)=>{

    const endX = e.changedTouches[0].clientX;

    if(Math.abs(endX - startX) > 50){

        const swipe = document.getElementById("swipeView");

        const items = swipe.querySelectorAll(".swipeCam");

        items[swipeIndex].style.display = "none";

        if(endX < startX){
            swipeIndex = (swipeIndex + 1) % items.length;
        }else{
            swipeIndex = (swipeIndex - 1 + items.length) % items.length;
        }

        items[swipeIndex].style.display = "block";
    }

});

/* init mobile */
initSwipe();

</script>

</body>
</html>
