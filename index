<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">

<title>監控牆 v6.4（置中穩定版）</title>

<style>

/* ===== 基礎鎖死 ===== */
html, body{
    margin:0;
    padding:0;
    width:100%;
    height:100%;

    overflow:hidden;

    /* 🔥 真正置中核心 */
    display:flex;
    justify-content:center;
    align-items:center;

    background:#0b0f14;
}

/* ===== 外層容器（防 GitHub Pages 誤差） ===== */
.wrapper{
    width:100vw;
    height:100dvh;   /* 🔥 比 100vh 穩 */

    display:flex;
    justify-content:center;
    align-items:center;
}

/* ===== GRID ===== */
.grid{
    width:100%;
    height:100%;

    display:grid;

    gap:4px;

    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(2, 1fr);

    /* 🔥 防止超出 */
    box-sizing:border-box;
}

/* ===== CAMERA ===== */
.camera{
    position:relative;
    overflow:hidden;
    background:#000;
    cursor:pointer;
}

/* iframe */
iframe{
    position:absolute;
    inset:0;
    width:100%;
    height:100%;
    border:0;
    display:block;
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

/* ===== FOCUS ===== */
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
    height:100dvh;  /* 🔥 mobile 修正 */
}

/* ===== MOBILE ===== */
@media (max-width:768px){

    .grid{
        grid-template-columns: 1fr;
        grid-template-rows: auto;
        height:auto;
    }

}

</style>
</head>

<body>

<div class="wrapper">

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

</div>

<!-- focus -->
<div class="focusOverlay" id="overlay" onclick="closeFocus()"></div>

<script>

let cams = document.querySelectorAll(".camera");
let overlay = document.getElementById("overlay");
let currentIndex = null;

/* ===== focus ===== */
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

/* ===== close ===== */
function closeFocus(){
    overlay.classList.remove("active");
    overlay.innerHTML = "";
    currentIndex = null;
}

/* ===== keyboard ===== */
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

</script>

</body>
</html>
