<!DOCTYPE html>
<html>
<body style="background:black;color:white;font-family:Arial">

<h2 id="out"></h2>

<script>
document.getElementById("out").textContent =
"Gamepad API: " + ("getGamepads" in navigator);
</script>

</body>
</html>
