# stjepanvilus19-bit.github.io

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Meteorološka Stanica</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
body {
    background: #0f172a;
    color: white;
    font-family: Arial;
    text-align: center;
}
.container {
    width: 90%;
    margin: auto;
}
canvas {
    background: #1e293b;
    border-radius: 10px;
    margin: 20px 0;
}
h1 {
    margin-top: 30px;
}
</style>
</head>
<body>

<h1>IoT Meteorološka Stanica</h1>

<div class="container">
    <canvas id="tempChart"></canvas>
    <canvas id="humChart"></canvas>
</div>

<script>
const channelID = "YOUR_CHANNEL_ID";

async function fetchData() {
    const response = await fetch(`https://api.thingspeak.com/channels/${channelID}/feeds.json?results=20`);
    const data = await response.json();
    return data.feeds;
}

async function drawCharts() {
    const feeds = await fetchData();

    const labels = feeds.map(f => new Date(f.created_at).toLocaleTimeString());
    const temp = feeds.map(f => f.field1);
    const hum = feeds.map(f => f.field2);

    new Chart(document.getElementById("tempChart"), {
        type: "line",
        data: {
            labels: labels,
            datasets: [{
                label: "Temperatura (°C)",
                data: temp,
                borderColor: "red"
            }]
        }
    });

    new Chart(document.getElementById("humChart"), {
        type: "line",
        data: {
            labels: labels,
            datasets: [{
                label: "Vlaga (%)",
                data: hum,
                borderColor: "cyan"
            }]
        }
    });
}

drawCharts();
setInterval(drawCharts, 20000);
</script>

</body>
</html>


