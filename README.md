<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">

<title>PS4 Browser Engine</title>

<meta name="viewport"
content="width=device-width,initial-scale=1">

<style>

html,body{

margin:0;
overflow:hidden;
background:#101010;

}

canvas{

display:block;
position:absolute;
left:0;
top:0;

}

</style>

</head>

<body>

<canvas id="screen"></canvas>

<script>

//////////////////////////////////////////////////////////////
// ENGINE
//////////////////////////////////////////////////////////////

const canvas=document.getElementById("screen");
const ctx=canvas.getContext("2d");

function resize(){

canvas.width=window.innerWidth;
canvas.height=window.innerHeight;

}

window.addEventListener("resize",resize);

resize();

//////////////////////////////////////////////////////////////
// TIMING
//////////////////////////////////////////////////////////////

let lastTime=performance.now();

let fps=0;
let delta=0;

let frameCounter=0;
let fpsTimer=0;

//////////////////////////////////////////////////////////////
// DEBUG
//////////////////////////////////////////////////////////////

const consoleLines=[];

function log(text){

consoleLines.unshift(text);

while(consoleLines.length>18)
consoleLines.pop();

}

//////////////////////////////////////////////////////////////
// INPUT PLACEHOLDERS
//////////////////////////////////////////////////////////////

const Input={

mouseX:canvas.width/2,
mouseY:canvas.height/2,

targetX:canvas.width/2,
targetY:canvas.height/2,

buttons:{},

keys:{}

};

//////////////////////////////////////////////////////////////
// BROWSER INFO
//////////////////////////////////////////////////////////////

const Browser={

userAgent:navigator.userAgent,

platform:navigator.platform,

vendor:navigator.vendor,

language:navigator.language,

cookie:navigator.cookieEnabled,

touch:"ontouchstart" in window,

pointer:!!window.PointerEvent,

gamepad:"getGamepads" in navigator,

webgl:!!document.createElement("canvas")
.getContext("webgl"),

webgl2:!!document.createElement("canvas")
.getContext("webgl2")

};

log("Browser scanned.");

//////////////////////////////////////////////////////////////
// GRID
//////////////////////////////////////////////////////////////

function drawGrid(){

ctx.strokeStyle="#1d1d1d";
ctx.lineWidth=1;

for(let x=0;x<canvas.width;x+=50){

ctx.beginPath();

ctx.moveTo(x,0);
ctx.lineTo(x,canvas.height);

ctx.stroke();

}

for(let y=0;y<canvas.height;y+=50){

ctx.beginPath();

ctx.moveTo(0,y);
ctx.lineTo(canvas.width,y);

ctx.stroke();

}

}

//////////////////////////////////////////////////////////////
// CROSSHAIR
//////////////////////////////////////////////////////////////

function drawCrosshair(){

ctx.strokeStyle="#ff6600";

ctx.lineWidth=2;

ctx.beginPath();

ctx.moveTo(Input.mouseX-15,Input.mouseY);

ctx.lineTo(Input.mouseX+15,Input.mouseY);

ctx.moveTo(Input.mouseX,Input.mouseY-15);

ctx.lineTo(Input.mouseX,Input.mouseY+15);

ctx.stroke();

ctx.beginPath();

ctx.arc(

Input.mouseX,
Input.mouseY,
5,
0,
Math.PI*2

);

ctx.fillStyle="#ff6600";

ctx.fill();

}

//////////////////////////////////////////////////////////////
// INFO PANEL
//////////////////////////////////////////////////////////////

function drawInfo(){

ctx.fillStyle="rgba(0,0,0,.7)";

ctx.fillRect(10,10,390,320);

ctx.fillStyle="white";

ctx.font="16px monospace";

let y=35;

function line(t){

ctx.fillText(t,20,y);

y+=18;

}

line("PS4 Browser Engine");

line("");

line("FPS : "+fps);

line("Frame : "+delta.toFixed(2)+" ms");

line("");

line("Mouse X : "+Math.round(Input.mouseX));

line("Mouse Y : "+Math.round(Input.mouseY));

line("");

line("Gamepad API : "+Browser.gamepad);

line("Pointer : "+Browser.pointer);

line("Touch : "+Browser.touch);

line("WebGL : "+Browser.webgl);

line("WebGL2 : "+Browser.webgl2);

line("");

line("Platform:");

line(Browser.platform);

line("");

line("Language:");

line(Browser.language);

}

//////////////////////////////////////////////////////////////
// CONSOLE
//////////////////////////////////////////////////////////////

function drawConsole(){

ctx.fillStyle="rgba(0,0,0,.7)";

ctx.fillRect(

10,
canvas.height-260,
520,
250

);

ctx.fillStyle="#00ff55";

ctx.font="15px monospace";

let y=canvas.height-235;

ctx.fillText("EVENT CONSOLE",20,y);

y+=24;

for(const text of consoleLines){

ctx.fillText(text,20,y);

y+=16;

}

}

//////////////////////////////////////////////////////////////
// UPDATE
//////////////////////////////////////////////////////////////

function update(){

Input.mouseX+=(Input.targetX-Input.mouseX)*0.18;
Input.mouseY+=(Input.targetY-Input.mouseY)*0.18;

}

//////////////////////////////////////////////////////////////
// DRAW
//////////////////////////////////////////////////////////////

function draw(){

ctx.clearRect(

0,
0,
canvas.width,
canvas.height

);

drawGrid();

drawCrosshair();

drawInfo();

drawConsole();

}

//////////////////////////////////////////////////////////////
// LOOP
//////////////////////////////////////////////////////////////

function loop(time){

delta=time-lastTime;

lastTime=time;

frameCounter++;

fpsTimer+=delta;

if(fpsTimer>=1000){

fps=frameCounter;

frameCounter=0;

fpsTimer=0;

}

update();

draw();

requestAnimationFrame(loop);

}

requestAnimationFrame(loop);

//////////////////////////////////////////////////////////////
// STARTUP
//////////////////////////////////////////////////////////////

log("Engine Started.");

log("Canvas Ready.");

log("Waiting For Input...");
//////////////////////////////////////////////////////////////
// INPUT
//////////////////////////////////////////////////////////////

let mouseEvents = 0;
let mouseHz = 0;
let mouseTimer = 0;

let velocityX = 0;
let velocityY = 0;

const trail = [];

function pushTrail(x, y) {
    trail.push({
        x: x,
        y: y,
        life: 1
    });

    if (trail.length > 60)
        trail.shift();
}

//////////////////////////////////////////////////////////////
// MOUSE
//////////////////////////////////////////////////////////////

document.addEventListener("mousemove", e => {

    Input.targetX = e.clientX;
    Input.targetY = e.clientY;

    velocityX = e.movementX;
    velocityY = e.movementY;

    mouseEvents++;

    pushTrail(e.clientX, e.clientY);

    log(
        "Mouse "
        + e.clientX
        + ","
        + e.clientY
    );

});

//////////////////////////////////////////////////////////////
// POINTER
//////////////////////////////////////////////////////////////

document.addEventListener("pointermove", e => {

    log("Pointer " + e.pointerType);

});

//////////////////////////////////////////////////////////////
// KEYBOARD
//////////////////////////////////////////////////////////////

document.addEventListener("keydown", e => {

    if (!Input.keys[e.code]) {

        log("Key Down: " + e.code);

    }

    Input.keys[e.code] = true;

});

document.addEventListener("keyup", e => {

    delete Input.keys[e.code];

    log("Key Up: " + e.code);

});

//////////////////////////////////////////////////////////////
// WINDOW EVENTS
//////////////////////////////////////////////////////////////

window.addEventListener("blur", () => {

    log("WINDOW LOST FOCUS");

});

window.addEventListener("focus", () => {

    log("WINDOW FOCUSED");

});

//////////////////////////////////////////////////////////////
// TRAIL
//////////////////////////////////////////////////////////////

function drawTrail() {

    for (const dot of trail) {

        ctx.globalAlpha = dot.life;

        ctx.beginPath();

        ctx.arc(
            dot.x,
            dot.y,
            4,
            0,
            Math.PI * 2
        );

        ctx.fillStyle = "#00ffff";

        ctx.fill();

        dot.life -= 0.03;

    }

    ctx.globalAlpha = 1;

    while (
        trail.length &&
        trail[0].life <= 0
    )
        trail.shift();

}

//////////////////////////////////////////////////////////////
// DRAW EXTRA INFO
//////////////////////////////////////////////////////////////

const oldDraw = draw;

draw = function () {

    oldDraw();

    drawTrail();

    ctx.fillStyle = "white";
    ctx.font = "16px monospace";

    ctx.fillText(
        "Velocity X: "
        + velocityX,
        430,
        30
    );

    ctx.fillText(
        "Velocity Y: "
        + velocityY,
        430,
        50
    );

    ctx.fillText(
        "Mouse Hz: "
        + mouseHz,
        430,
        70
    );

    let y = 110;

    ctx.fillText(
        "Held Keys:",
        430,
        y
    );

    y += 20;

    for (const key in Input.keys) {

        ctx.fillText(
            key,
            430,
            y
        );

        y += 18;

    }

};

//////////////////////////////////////////////////////////////
// UPDATE EXTENSION
//////////////////////////////////////////////////////////////

const oldUpdate = update;

update = function () {

    oldUpdate();

    mouseTimer += delta;

    if (mouseTimer >= 1000) {

        mouseHz = mouseEvents;

        mouseEvents = 0;

        mouseTimer = 0;

    }

};

//////////////////////////////////////////////////////////////
// PS4 BROWSER INSPECTOR
//////////////////////////////////////////////////////////////

const BrowserInspector = {

    globals: [],
    sony: [],
    eventProps: [],
    navigatorProps: [],
    windowProps: []

};

function inspectBrowser(){

    log("Scanning Browser...");

    //////////////////////////////////////////////////////
    // WINDOW
    //////////////////////////////////////////////////////

    BrowserInspector.windowProps =
        Object.getOwnPropertyNames(window).sort();

    //////////////////////////////////////////////////////
    // NAVIGATOR
    //////////////////////////////////////////////////////

    BrowserInspector.navigatorProps =
        Object.getOwnPropertyNames(navigator).sort();

    //////////////////////////////////////////////////////
    // LOOK FOR SONY / WEBKIT OBJECTS
    //////////////////////////////////////////////////////

    const keywords=[
        "sce",
        "sony",
        "playstation",
        "orbis",
        "webkit",
        "ps",
        "dualshock",
        "controller",
        "gamepad"
    ];

    function search(obj,name){

        Object.getOwnPropertyNames(obj).forEach(key=>{

            const lower=key.toLowerCase();

            keywords.forEach(word=>{

                if(lower.includes(word)){

                    BrowserInspector.sony.push(
                        name+"."+key
                    );

                }

            });

        });

    }

    search(window,"window");
    search(navigator,"navigator");
    search(document,"document");

    log("Browser Scan Finished");

}

inspectBrowser();

//////////////////////////////////////////////////////////////
// EVENT PROPERTY LOGGER
//////////////////////////////////////////////////////////////

function dumpEvent(event){

    BrowserInspector.eventProps=[];

    for(const key in event){

        try{

            BrowserInspector.eventProps.push(

                key+" : "+event[key]

            );

        }

        catch(e){}

    }

}

document.addEventListener("mousemove",dumpEvent);

document.addEventListener("keydown",dumpEvent);

//////////////////////////////////////////////////////////////
// EXTRA DEBUG PANEL
//////////////////////////////////////////////////////////////

const oldDraw2=draw;

draw=function(){

    oldDraw2();

    let x=canvas.width-420;
    let y=15;

    ctx.fillStyle="rgba(0,0,0,.75)";
    ctx.fillRect(
        x,
        y,
        400,
        canvas.height-30
    );

    ctx.fillStyle="#00ff88";

    ctx.font="15px monospace";

    let yy=y+20;

    ctx.fillText(
        "PS4 INSPECTOR",
        x+10,
        yy
    );

    yy+=25;

    ctx.fillStyle="white";

    ctx.fillText(
        "Sony Objects:",
        x+10,
        yy
    );

    yy+=20;

    if(BrowserInspector.sony.length){

        BrowserInspector.sony.forEach(obj=>{

            if(yy<canvas.height-40){

                ctx.fillText(
                    obj,
                    x+10,
                    yy
                );

                yy+=16;

            }

        });

    }else{

        ctx.fillText(
            "None Found",
            x+10,
            yy
        );

        yy+=20;

    }

    yy+=10;

    ctx.fillStyle="#00ffff";

    ctx.fillText(
        "Last Event:",
        x+10,
        yy
    );

    yy+=20;

    ctx.fillStyle="white";

    BrowserInspector.eventProps
    .slice(0,20)
    .forEach(line=>{

        if(yy<canvas.height-20){

            ctx.fillText(
                line,
                x+10,
                yy
            );

            yy+=15;

        }

    });

};
</script>

</body>
</html>
