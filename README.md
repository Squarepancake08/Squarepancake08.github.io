<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>PS4 Controller Test</title>

<style>
body{
    margin:0;
    background:#111;
    color:white;
    font-family:Arial;
    overflow:hidden;
}

#panel{
    position:absolute;
    left:20px;
    top:20px;
    background:#222;
    padding:20px;
    border-radius:12px;
    width:340px;
}

.line{
    margin:6px 0;
}

.big{
    font-size:22px;
}

#cursor{
    position:absolute;
    width:24px;
    height:24px;
    background:#ff6600;
    border-radius:50%;
    left:50%;
    top:50%;
    transform:translate(-50%,-50%);
}
</style>
</head>
<body>

<div id="cursor"></div>

<div id="panel">

<div class="big">PS4 Controller</div>

<div id="status" class="line">Waiting...</div>

<hr>

<div id="sticks"></div>

<hr>

<div id="buttons"></div>

</div>

<script>

let cursor=document.getElementById("cursor");

let x=window.innerWidth/2;
let y=window.innerHeight/2;

function update(){

let pads=navigator.getGamepads();

if(!pads)return;

let pad=null;

for(let p of pads){

if(p){
pad=p;
break;
}

}

if(!pad){

requestAnimationFrame(update);
return;

}

document.getElementById("status").innerHTML=
"Connected<br>"+pad.id;

let lx=pad.axes[0];
let ly=pad.axes[1];
let rx=pad.axes[2];
let ry=pad.axes[3];

document.getElementById("sticks").innerHTML=
`
Left X : ${lx.toFixed(2)}<br>
Left Y : ${ly.toFixed(2)}<br>
Right X : ${rx.toFixed(2)}<br>
Right Y : ${ry.toFixed(2)}
`;

x+=lx*10;
y+=ly*10;

x=Math.max(0,Math.min(window.innerWidth-24,x));
y=Math.max(0,Math.min(window.innerHeight-24,y));

cursor.style.left=x+"px";
cursor.style.top=y+"px";

let txt="";

const names=[
"Cross",
"Circle",
"Square",
"Triangle",
"L1",
"R1",
"L2",
"R2",
"Share",
"Options",
"L3",
"R3",
"Up",
"Down",
"Left",
"Right",
"PS",
"Touchpad"
];

for(let i=0;i<pad.buttons.length;i++){

if(pad.buttons[i].pressed){

txt+=names[i]||("Button "+i);
txt+="<br>";

}

}

document.getElementById("buttons").innerHTML=txt||"No buttons pressed";

requestAnimationFrame(update);

}

window.addEventListener("gamepadconnected",e=>{

document.getElementById("status").innerHTML=
"Controller Connected";

update();

});

window.addEventListener("gamepaddisconnected",e=>{

document.getElementById("status").innerHTML=
"Controller Disconnected";

});

update();

</script>

</body>
</html>
