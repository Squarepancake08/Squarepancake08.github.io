<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>PS4 Navigation Test</title>

<style>
body{
    background:#111;
    color:white;
    font-family:Arial;
    text-align:center;
    margin-top:60px;
}

button{
    display:block;
    width:300px;
    margin:20px auto;
    padding:20px;
    font-size:24px;
    border:none;
    border-radius:12px;
    background:#333;
    color:white;
}

button:focus{
    background:#ff6600;
    color:black;
    outline:6px solid yellow;
}
</style>
</head>

<body>

<h1>PS4 Browser Navigation Test</h1>

<p>Try using the D-Pad, left stick, and ✕.</p>

<button autofocus onclick="alert('Button 1')">
Button 1
</button>

<button onclick="alert('Button 2')">
Button 2
</button>

<button onclick="alert('Button 3')">
Button 3
</button>

<button onclick="alert('Button 4')">
Button 4
</button>

</body>
</html>
