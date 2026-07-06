<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>PS4 Input Detective</title>

<style>
body{
    margin:0;
    background:#111;
    color:#fff;
    font-family:monospace;
    overflow:hidden;
}

#info{
    position:fixed;
    left:10px;
    top:10px;
    background:#222;
    padding:15px;
    border-radius:10px;
    white-space:pre-wrap;
}

#cursor{
    position:absolute;
    width:20px;
    height:20px;
    border-radius:50%;
    background:#ff6600;
    left:50%;
    top:50%;
    transform:translate(-50%,-50%);
}
</style>
</head>
<body>

<div id="cursor"></div>

<div id="info">
Waiting...
</div>

<script>

const info=document.getElementById("info");
const cursor=document.getElementById("cursor");

let x=window.innerWidth/2;
let y=window.innerHeight/2;

function log(text){
    info.textContent=text;
}

document.addEventListener("mousemove",e=>{
    x=e.clientX;
    y=e.clientY;
    cursor.style.left=x+"px";
    cursor.style.top=y+"px";

    log(
`EVENT: mousemove

x: ${e.clientX}
y: ${e.clientY}

movementX: ${e.movementX}
movementY: ${e.movementY}`);
});

document.addEventListener("pointermove",e=>{
    log(
`EVENT: pointermove

x:${e.clientX}
y:${e.clientY}

pointerType:${e.pointerType}`);
});

document.addEventListener("wheel",e=>{
    log(
`EVENT: wheel

deltaX:${e.deltaX}
deltaY:${e.deltaY}`);
});

document.addEventListener("keydown",e=>{
    log(
`EVENT: keydown

key:${e.key}
code:${e.code}
keyCode:${e.keyCode}`);
});

document.addEventListener("keyup",e=>{
    log(
`EVENT: keyup

key:${e.key}`);
});

document.addEventListener("mousedown",e=>{
    log(
`EVENT: mousedown

button:${e.button}`);
});

document.addEventListener("mouseup",e=>{
    log(
`EVENT: mouseup

button:${e.button}`);
});

document.addEventListener("touchstart",e=>{
    log("EVENT: touchstart");
});

document.addEventListener("touchmove",e=>{
    log("EVENT: touchmove");
});

document.addEventListener("touchend",e=>{
    log("EVENT: touchend");
});

window.addEventListener("blur",()=>{
    log("WINDOW LOST FOCUS");
});

window.addEventListener("focus",()=>{
    log("WINDOW FOCUSED");
});

</script>

</body>
</html><!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>PS4 Input Detective</title>

<style>
body{
    margin:0;
    background:#111;
    color:#fff;
    font-family:monospace;
    overflow:hidden;
}

#info{
    position:fixed;
    left:10px;
    top:10px;
    background:#222;
    padding:15px;
    border-radius:10px;
    white-space:pre-wrap;
}

#cursor{
    position:absolute;
    width:20px;
    height:20px;
    border-radius:50%;
    background:#ff6600;
    left:50%;
    top:50%;
    transform:translate(-50%,-50%);
}
</style>
</head>
<body>

<div id="cursor"></div>

<div id="info">
Waiting...
</div>

<script>

const info=document.getElementById("info");
const cursor=document.getElementById("cursor");

let x=window.innerWidth/2;
let y=window.innerHeight/2;

function log(text){
    info.textContent=text;
}

document.addEventListener("mousemove",e=>{
    x=e.clientX;
    y=e.clientY;
    cursor.style.left=x+"px";
    cursor.style.top=y+"px";

    log(
`EVENT: mousemove

x: ${e.clientX}
y: ${e.clientY}

movementX: ${e.movementX}
movementY: ${e.movementY}`);
});

document.addEventListener("pointermove",e=>{
    log(
`EVENT: pointermove

x:${e.clientX}
y:${e.clientY}

pointerType:${e.pointerType}`);
});

document.addEventListener("wheel",e=>{
    log(
`EVENT: wheel

deltaX:${e.deltaX}
deltaY:${e.deltaY}`);
});

document.addEventListener("keydown",e=>{
    log(
`EVENT: keydown

key:${e.key}
code:${e.code}
keyCode:${e.keyCode}`);
});

document.addEventListener("keyup",e=>{
    log(
`EVENT: keyup

key:${e.key}`);
});

document.addEventListener("mousedown",e=>{
    log(
`EVENT: mousedown

button:${e.button}`);
});

document.addEventListener("mouseup",e=>{
    log(
`EVENT: mouseup

button:${e.button}`);
});

document.addEventListener("touchstart",e=>{
    log("EVENT: touchstart");
});

document.addEventListener("touchmove",e=>{
    log("EVENT: touchmove");
});

document.addEventListener("touchend",e=>{
    log("EVENT: touchend");
});

window.addEventListener("blur",()=>{
    log("WINDOW LOST FOCUS");
});

window.addEventListener("focus",()=>{
    log("WINDOW FOCUSED");
});

</script>

</body>
</html>
