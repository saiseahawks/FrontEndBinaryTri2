---
toc: true
comments: false
layout: post
title: Picture to binary
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
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f8f8f8;
            color: #333;
            margin: 0;
            padding: 20px;
            text-align: center;
        }

        h1 {
            color: #555;
        }

        #uploadForm {
            margin: 20px 0;
        }

        #imageInput {
            padding: 10px;
            margin-right: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            background-color: #fff;
            color: #333;
            cursor: pointer;
        }

        #convertButton {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            background-color: #4caf50;
            color: #fff;
            cursor: pointer;
        }

        #binaryCanvas {
            image-rendering: pixelated;
            display: block;
            margin: auto;
            border: 1px solid #ccc;
            border-radius: 5px;
            margin-top: 20px;
        }

        #binaryText {
            white-space: pre-wrap;
            font-size: 14px;
            margin-top: 20px;
            overflow-x: auto;
        }
    </style>
</head>
<body>
    <h1>Image to Binary Converter</h1>

    <form id="uploadForm" enctype="multipart/form-data">
        <input type="file" id="imageInput" accept="image/*" />
        <button type="button" id="convertButton" onclick="convertImage()">Convert to Binary</button>
    </form>

    <canvas id="binaryCanvas" style="display: none;"></canvas>
    <pre id="binaryText"></pre>

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
                        displayBinaryResult(binaryData, img.width, img.height);
                    };
                };

                reader.readAsDataURL(file);
            }
        }

        function convertToBinary(pixelData) {
            var binaryData = [];

            for (var i = 0; i < pixelData.length; i += 4) {
                var grayscale = (pixelData[i] + pixelData[i + 1] + pixelData[i + 2]) / 3;
                binaryData.push(grayscale < 128 ? '0' : '1');
            }

            return binaryData;
        }

        function displayBinaryResult(binaryData, width, height) {
            var binaryCanvas = document.getElementById('binaryCanvas');
            binaryCanvas.width = width * 2;
            binaryCanvas.height = height * 2;

            var ctx = binaryCanvas.getContext('2d');
            var imageData = ctx.createImageData(width * 2, height * 2);

            for (var i = 0; i < binaryData.length; i++) {
                var pixelValue = parseInt(binaryData[i], 2) * 255;
                var x = (i % width) * 2;
                var y = Math.floor(i / width) * 2;

                for (var dx = 0; dx < 2; dx++) {
                    for (var dy = 0; dy < 2; dy++) {
                        var index = ((y + dy) * width * 2 + (x + dx)) * 4;
                        imageData.data[index] = pixelValue;
                        imageData.data[index + 1] = pixelValue;
                        imageData.data[index + 2] = pixelValue;
                        imageData.data[index + 3] = 255;
                    }
                }
            }

            ctx.putImageData(imageData, 0, 0);
            binaryCanvas.style.display = 'block';

            var binaryText = document.getElementById('binaryText');
            binaryText.textContent = binaryData.join('\n');
        }
    </script>
</body>
</html>

