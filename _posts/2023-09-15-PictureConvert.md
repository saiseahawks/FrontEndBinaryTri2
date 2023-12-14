---
toc: true
comments: false
layout: post
title: Jake is the Opposition
description: Example Blog!!!  This shows planning and notes from hacks.
type: plans
courses: { compsci: {week: 13} }
---
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Image to Binary Converter</title>
    <style>
        #container {
            display: flex;
            justify-content: space-around;
        }
        #originalCanvas, #binaryCanvas {
            image-rendering: pixelated;
            border: 1px solid black; /* Add a border for better visibility */
        }
    </style>
</head>
<body>
    <h1>Image to Binary Converter</h1>
    
    <form id="uploadForm" enctype="multipart/form-data">
        <input type="file" id="imageInput" accept="image/*" />
        <button type="button" onclick="convertImage()">Convert to Binary</button>
    </form>

    <div id="container">
        <canvas id="originalCanvas" style="display: none;"></canvas>
        <canvas id="binaryCanvas" style="display: none;"></canvas>
        <pre id="binaryText"></pre>
    </div>

    <script>
        function convertImage() {
            var input = document.getElementById('imageInput');
            var file = input.files[0];

            if (file) {
                var reader = new FileReader();

                reader.onload = function(e) {
                    var img = new Image();
                    img.src = e.target.result;

                    img.onload = function() {
                        var canvas = document.createElement('canvas');
                        var ctx = canvas.getContext('2d');
                        canvas.width = img.width;
                        canvas.height = img.height;
                        ctx.drawImage(img, 0, 0, img.width, img.height);

                        var binaryData = convertToBinary(ctx.getImageData(0, 0, img.width, img.height).data);

                        displayOriginalImage(canvas, img.width, img.height);
                        displayBinaryResult(binaryData, img.width, img.height);
                    };
                };

                reader.readAsDataURL(file);
            }
        }

        function convertToBinary(pixelData) {
            var binaryData = [];

            for (var i = 0; i < pixelData.length; i += 4) {
                // If the grayscale value is closer to 0, set the binary value to '0'; otherwise, set it to '1'
                binaryData.push(pixelData[i] < 128 ? '0' : '1');
            }

            return binaryData;
        }

        function displayOriginalImage(originalCanvas, width, height) {
            var container = document.getElementById('container');
            var originalCanvasElement = document.getElementById('originalCanvas');

            originalCanvasElement.width = width;
            originalCanvasElement.height = height;
            originalCanvasElement.getContext('2d').drawImage(originalCanvas, 0, 0);

            originalCanvasElement.style.display = 'block';
        }

        function displayBinaryResult(binaryData, width, height) {
            var binaryCanvas = document.getElementById('binaryCanvas');
            binaryCanvas.width = width;
            binaryCanvas.height = height;

            var ctx = binaryCanvas.getContext('2d');
            var imageData = ctx.createImageData(width, height);

            for (var i = 0; i < binaryData.length; i++) {
                var pixelValue = parseInt(binaryData[i], 2) * 255; // Convert binary to 0 or 255
                imageData.data[i * 4] = pixelValue;
                imageData.data[i * 4 + 1] = pixelValue;
                imageData.data[i * 4 + 2] = pixelValue;
                imageData.data[i * 4 + 3] = 255; // Alpha channel
            }

            ctx.putImageData(imageData, 0, 0);
            binaryCanvas.style.display = 'block';

            var binaryText = document.getElementById('binaryText');
            binaryText.textContent = binaryData.join(' ');
        }
    </script>
</body>
</html>
