<!-- <!DOCTYPE html> -->
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Colorful Binary, Hexadecimal, and Analog Clock</title>
    <style>
        /* Styling for the page */
        body {
            font-family: 'Arial', sans-serif;
            background-color: #282c35;
            color: #fff;
            text-align: center;
            margin: 50px;
        }

        /* Styling for the main clock display */
        #clock {
            font-size: 2em; /* Reduced font size */
            letter-spacing: 10px; /* Adjusted letter spacing */
            margin-bottom: 20px;
            color: #FFD700; /* Gold */
        }

        /* Styling for additional time representations */
        #integerTime,
        #hexTime {
            font-size: 1.2em; /* Adjusted font size */
            margin-bottom: 20px;
        }

        /* Styling for binary lights and information */
        #binaryLights,
        #binaryInfo {
            font-size: 1.2em;
            margin-bottom: 20px;
        }

        /* Styling for individual binary lights */
        .light-bulb {
            display: inline-block;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            margin: 0 3px; /* Adjust the margin to add a small gap between bulbs */
        }

        /* Styling for lit and unlit binary lights */
        .light-on {
            background-color: #55a1ff; /* Sky Blue */
        }

        .light-off {
            background-color: #333;
        }

        /* Adjust margin to add a small gap between hours, minutes, seconds, and milliseconds */
        .space {
            margin: 0 5px;
        }

        /* Styling for paragraphs in the binary information section */
        #binaryInfo p {
            margin: 10px;
        }
    </style>
</head>

<body>
    <!-- Main clock display elements -->
    <div id="clock"></div>
    <div id="integerTime"></div>
    <div id="hexTime"></div>

    <!-- Binary-related elements -->
    <div id="binaryLights"></div>
    <div id="binaryInfo">
        <!-- Information about binary -->
        <p>Binary is a base-2 numeral system that uses only two digits: 0 and 1.</p>
        <p>This clock represents the current time in binary format, providing a unique and visually interesting display.</p>
        <p>Explore the binary lights and their pattern to understand the binary representation of hours, minutes, and seconds.</p>
    </div>

    <!-- JavaScript code for clock functionality -->
    <script>
        // Function to add leading zeros to numbers less than 10
        function addLeadingZero(number) {
            return number < 10 ? "0" + number : number;
        }

        // Function to update the binary clock display
        function updateBinaryClock() {
            // Get the current date and time
            const now = new Date();

            // Convert hours, minutes, seconds, and milliseconds to binary
            const hoursBinary = addLeadingZero(now.getHours()).toString(2).padStart(4, '0');
            const minutesBinary = addLeadingZero(now.getMinutes()).toString(2).padStart(6, '0');
            const secondsBinary = addLeadingZero(now.getSeconds()).toString(2).padStart(6, '0');
            const millisecondsBinary = now.getMilliseconds().toString(2).padStart(10, '0');
            const binaryTime = `${hoursBinary}:${minutesBinary}:${secondsBinary}`;

            // Display binary lights
            const binaryLights = binaryTime.split('').map(bit => `<span class="light-bulb ${bit === '1' ? 'light-on' : 'light-off'}"></span>`).join('');

            // Convert date, month, and year to binary
            const dateBinary = addLeadingZero(now.getDate()).toString(2).padStart(5, '0');
            const monthBinary = addLeadingZero(now.getMonth() + 1).toString(2).padStart(4, '0'); // Month is zero-based
            const yearBinary = now.getFullYear().toString(2).padStart(12, '0');

            // Display various time representations
            const binaryClock = `${hoursBinary}:${minutesBinary}:${secondsBinary}.${millisecondsBinary} <br> ${dateBinary}:${monthBinary}:${yearBinary}`;
            const integerClock = `${addLeadingZero(now.getHours())}:${addLeadingZero(now.getMinutes())}:${addLeadingZero(now.getSeconds())}.${addLeadingZero(now.getMilliseconds())} <br> ${addLeadingZero(now.getDate())}/${addLeadingZero(now.getMonth() + 1)}/${now.getFullYear()}`;
            const hexClock = `${now.getHours().toString(16)}:${addLeadingZero(now.getMinutes().toString(16))}:${addLeadingZero(now.getSeconds().toString(16))}.${now.getMilliseconds().toString(16)} <br> ${addLeadingZero(now.getDate().toString(16))}/${addLeadingZero((now.getMonth() + 1).toString(16))}/${now.getFullYear().toString(16)}`;

            // Update the HTML elements with the new time information
            document.getElementById('clock').innerHTML = binaryClock;
            document.getElementById('integerTime').innerHTML = `Integer Time: ${integerClock}`;
            document.getElementById('hexTime').innerHTML = `Hexadecimal Time: ${hexClock}`;
            document.getElementById('binaryLights').innerHTML = `Binary Lights: ${binaryLights}`;
        }

        // Update the clock every second
        setInterval(updateBinaryClock, 100);

        // Initial update
        updateBinaryClock();
    </script>
</body>

</html>
