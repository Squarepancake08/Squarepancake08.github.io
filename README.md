<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>PS4 Keyboard Event Test</title>

<style>
body{
    margin:0;
    background:#111;
    color:#fff;
    font-family:monospace;
    padding:20px;
}

#log{
    white-space:pre-wrap;
    border:2px solid #555;
    padding:15px;
    margin-top:20px;
    min-height:300px;
    overflow:auto;
}

button{
    font-size:24px;
    padding:15px 30px;
}
</style>
</head>
<body>

<h1>PS4 Input Test</h1>

<p>Click the button once with ✕, then press every button on the controller.</p>

<button autofocus>Activate Me</button>

<div id="log">Waiting for input...</div>

<script>

const log=document.getElementById("log");

function add(type,e){

log.textContent=
type+
"\n\n"+
"key: "+e.key+
"\ncode: "+e.code+
"\nkeyCode: "+e.keyCode+
"\nwhich: "+e.which+
"\nlocation: "+e.location+
"\nrepeat: "+e.repeat;

}

["keydown","keyup","keypress"].forEach(type=>{
document.addEventListener(type,e=>add(type,e));
});

["mousedown","mouseup","click"].forEach(type=>{
document.addEventListener(type,e=>{
log.textContent=
type+
"\nbutton: "+e.button+
"\nbuttons: "+e.buttons;
});
});

document.addEventListener("focusin",e=>{
log.textContent="Focus moved to: "+e.target.tagName;
});

</script>

</body>
</html>
