<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Professional Speed Test</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #1d1e22;
            color: #fff;
            margin: 0;
            padding: 0;
        }
        .container {
            max-width: 700px;
            margin: 100px auto;
            padding: 30px;
            background: #2c2f33;
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
            border-radius: 15px;
        }
        h1 {
            color: #fff;
            margin-bottom: 20px;
        }
        .meter {
            position: relative;
            width: 250px;
            height: 250px;
            border-radius: 50%;
            background: conic-gradient(
                #4caf50 0%, 
                #ffeb3b 50%, 
                #f44336 100%
            );
            margin: 30px auto;
        }
        .meter-inner {
            position: absolute;
            width: 220px;
            height: 220px;
            border-radius: 50%;
            background: #1d1e22;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 32px;
            font-weight: bold;
            color: #fff;
        }
        button {
            padding: 15px 30px;
            font-size: 18px;
            color: #fff;
            background: #7289da;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background 0.3s ease;
        }
        button:hover {
            background: #5a6dbd;
        }
        #rating {
            font-size: 20px;
            margin-top: 20px;
        }
        #thankYou {
            font-size: 18px;
            margin-top: 30px;
            color: #4caf50;
            display: none;
        }
        footer {
            margin-top: 50px;
            font-size: 14px;
            color: #888;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Speed Test</h1>
        <div class="meter">
            <div class="meter-inner" id="speedValue">0 Mbps</div>
        </div>
        <button onclick="testSpeed()">Start Test</button>
        <p id="rating"></p>
        <p id="ping">Ping: - ms</p>
        <p id="thankYou">Thank you for visiting this website!</p>
    </div>
    <footer>
        Powered by Professional Speed Test | Made by Mr. Maruf</footer>
    <script>
        async function testSpeed() {
            const speedValue = document.getElementById('speedValue');
            const rating = document.getElementById('rating');
            const pingElement = document.getElementById('ping');
            const thankYou = document.getElementById('thankYou');

            try {
                // Measure ping
                const pingStartTime = performance.now();
                await fetch('https://speed.hetzner.de/1MB.bin', { method: 'HEAD' }); // Ping test with a lightweight request
                const pingEndTime = performance.now();
                const ping = pingEndTime - pingStartTime;
                pingElement.textContent = `Ping: ${ping.toFixed(2)} ms`;

                // Start the download test
                const startTime = performance.now();
                const response = await fetch('https://speed.hetzner.de/10MB.bin'); // Test file (10MB)
                const endTime = performance.now();

                // Calculate download speed
                const fileSizeInBits = 10 * 1024 * 1024 * 8; // 10 MB in bits
                const durationInSeconds = (endTime - startTime) / 1000;
                const downloadSpeed = fileSizeInBits / (durationInSeconds * 1024 * 1024); // Mbps

                speedValue.textContent = downloadSpeed.toFixed(2) + ' Mbps';

                // Provide a rating based on the speed
                if (downloadSpeed < 20) {
                    rating.textContent = 'Rating: Poor';
                    rating.style.color = '#f44336';
                } else if (downloadSpeed < 100) {
                    rating.textContent = 'Rating: Average';
                    rating.style.color = '#ffeb3b';
                } else {
                    rating.textContent = 'Rating: Excellent';
                    rating.style.color = '#4caf50';
                }

                // Display thank you message
                thankYou.style.display = 'block';
            } catch (error) {
                speedValue.textContent = 'Error';
                rating.textContent = 'Unable to test speed. Please try again later.';
                rating.style.color = '#f44336';
                pingElement.textContent = 'Ping: Error';
                console.error('Speed test failed:', error);
            }
        }
    </script>
</body>
</html>
