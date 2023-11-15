---
toc: true
comments: false
layout: post
title: Experiment
description: Example Blog!!!  This shows planning and notes from hacks.
type: plans
courses: { compsci: {week: 13} }
---


<canvas id = "canvas" width = "400px" height = "400px"> </canvas>
<style>
    #canvas{
        border: 2px solid green;
    }
</style>
<!-- <button>Invert</button> -->
<script>
    var player = {
        "name" : "player",
        "item" : "y",
        "x": 5,
        "y" : 5,
        "rotation" : 0
    }
    var dummyObject = {
        "name" : "Simple Object",
        "item" : "y",
        "vertInfo":{
            "0":{
                "item":"k",
                "x": 2,
                "y": 6,
                "color": "#FFA500"
            },
            "1":{
                "item":"k",
                "x": 4,
                "y": 4,
                "color": "#FFA500"
            },
            "2":{
                "item":"k",
                "x": 6,
                "y": 4,
                "color": "#FFA500"
            },
            "3":{
                "item":"k",
                "x": 8,
                "y": 6,
                "color": "#FFA500"
            },
            "4":{
                "item":"k",
                "x": 6,
                "y": 8,
                "color": "#FFA500"
            },
            "5":{
                "item":"k",
                "x": 4,
                "y": 8,
                "color": "#FFA500"
            },
        },
        "faceInfo":{},
        "x" : 13,
        "y" : 2,
    };
    var impeadment = {
        "name" : "Second Simple Object",
        "item" : "z",
        "vertInfo":{
            "0":{
                "item" : "b",
                "x": 2,
                "y": 2,
            },
            "1":{
                "item" : "b",
                "x": 4,
                "y": 2,
            },
            "2":{
                "item" : "b",
                "x": 6,
                "y": 2,
            },
            "3":{
                "item" : "b",
                "x": 8,
                "y": 2,
            },
        },
        "faceInfo":{},
        "x" : 14,
        "y" : 4,
    };
    var floor = {
        "name" : "Floor",
        "vertInfo":{
            "0":{
                "x" : 0,
                "y" : 0
            },
            "1":{
                "x" : 0,
                "y" : 5
            },
            "2":{
                "x" : 400,
                "y" : 5
            },
            "3":{
                "x" : 400,
                "y" : 0
            }
        },
        "x": 10,
        "y": 3
    }
    var objNames = [dummyObject, impeadment, floor]
    var rows = 20
    var cols = 20
    var result = ""
    addEventListener("keydown", function(event){
    if(event.defaultPrevented){
        return;
    }
    switch (event.key) {
        case "w":
            result = movement("w")
            break;
        case "a":
            result = movement("a")
            break;
        case "s":
            result = movement("s")
            break;
        case "d":
            result = movement("d")
            break;
        default:
            break;
    }
    drawPath(result, 25, 25)
    })
    function overLay(canvasWidth, canvasHeight){
        var canvas = document.getElementById("canvas");
        var ctx = canvas.getContext("2d")
        ctx.fillRect(canvas.width / 2, (0.75 * canvas.height), canvasWidth, canvas.height / canvasHeight)

    }

    function drawPath(xPlain, horizontalDistance, heightDistance){
        console.log("Data for Plain is", xPlain, xPlain[0])
        var canvas = document.getElementById("canvas");
        var ctx = canvas.getContext("2d")
        horizontalDistance = canvas.width / horizontalDistance
        // console.log(horizontalDistance)
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        for(var objects in xPlain){
            console.log("Object is", objNames[objects])
            console.log("Length of Verts are", Object.keys(objNames[objects]["vertInfo"]).length)
            ctx.fillStyle = "#00FF80"
            console.log("Center is", horizontalDistance * xPlain[objects]["x"], (canvas.height / heightDistance) * (heightDistance - xPlain[objects]["y"]))
            ctx.fillRect(horizontalDistance * xPlain[objects]["x"], (canvas.height / heightDistance) * (heightDistance - xPlain[objects]["y"]) - (canvas.height / heightDistance), horizontalDistance, canvas.height / heightDistance)
            for(var verts = 0; verts < Object.keys(objNames[objects]["vertInfo"]).length; verts++){
                console.log("Verts are", objNames[objects]["vertInfo"][verts])
                if(verts == 0){
                    ctx.beginPath()
                    ctx.lineWidth = 10;
                    ctx.lineJoin = "round";
                    // console.log("Start from,", (xPlain[objects]["vertInfo"][verts]["y"]) * horizontalDistance + (horizontalDistance / 2), (heightDistance -xPlain[objects]["vertInfo"][verts]["y"]) * (canvas.height / heightDistance) + (canvas.height / heightDistance / 2))
                    ctx.moveTo(xPlain[objects]["vertInfo"][verts]["x"] * horizontalDistance + (horizontalDistance / 2), (heightDistance - xPlain[objects]["vertInfo"][verts]["y"]) * (canvas.height / heightDistance) + (canvas.height / heightDistance / 2))
                    if(xPlain[objects]["vertInfo"][verts]["color"] != undefined){
                        ctx.fillStyle = String(xPlain[objects]["vertInfo"][verts]["color"])
                    } else{
                        ctx.fillStyle = "#FF0000"
                    }
                    ctx.fillRect(horizontalDistance * xPlain[objects]["vertInfo"][verts]["x"], (heightDistance - xPlain[objects]["vertInfo"][verts]["y"]) * (canvas.height / heightDistance), horizontalDistance, canvas.height / heightDistance);
                }
                var currentPlain = xPlain[objects]["vertInfo"][verts]
                console.log("Current Element would be", currentPlain)
                if(currentPlain["color"] != undefined){
                    ctx.fillStyle = currentPlain["color"]
                }
                else{
                    ctx.fillStyle = "#FF0000"
                }
                
                var yOffset = (heightDistance - currentPlain["y"]) * (canvas.height / heightDistance)
                var yMid = (canvas.height / heightDistance) / 2
                var xMid = (canvas.width / heightDistance) / 2
                ctx.lineTo(horizontalDistance * currentPlain["x"] + xMid, yOffset + yMid)
                console.log("Add-ons are", horizontalDistance, yOffset)
                console.log("Selected point is", horizontalDistance * currentPlain["x"] + xMid, yOffset + yMid)
                ctx.fillRect(horizontalDistance * currentPlain["x"], yOffset, horizontalDistance, 2*yMid);
                // console.log(horizontalDistance * currentPlain)
            }
            ctx.closePath()
            ctx.stroke()
        }
        overLay(horizontalDistance, heightDistance)
    }
    function movement(moves) {
        var vertMoveCt = 0;
        var movedFrom = ""
        var screenArray = []
        switch(moves){
            case "w":
                screenArray = compileObjs(objNames);
                for(var movedObject in objNames){
                    console.log("Moved Object is", screenArray[movedObject])
                    objNames[movedObject]["y"] -= 1;
                    for(var verts in Object.keys(objNames[movedObject]["vertInfo"])){
                        objNames[movedObject]["vertInfo"][verts]["y"] -= 1
                    }
                }
                break;
            case "a":
                screenArray = compileObjs(objNames);
                for(var movedObject in objNames){
                    console.log("Moved Object is", screenArray[movedObject])
                    objNames[movedObject]["x"] += 1;
                    for(var verts in Object.keys(objNames[movedObject]["vertInfo"])){
                        objNames[movedObject]["vertInfo"][verts]["x"] += 1
                    }
                }
                break;
            case "s":
                screenArray = compileObjs(objNames);
                for(var movedObject in objNames){
                    console.log("Moved Object is", screenArray[movedObject])
                    objNames[movedObject]["y"] += 1;
                    for(var verts in Object.keys(objNames[movedObject]["vertInfo"])){
                        objNames[movedObject]["vertInfo"][verts]["y"] += 1
                    }
                }
                break;
            case "d":
                screenArray = compileObjs(objNames);
                for(var movedObject in objNames){
                    console.log("Moved Object is", screenArray[movedObject])
                    objNames[movedObject]["x"] -= 1;
                    for(var verts in Object.keys(objNames[movedObject]["vertInfo"])){
                        objNames[movedObject]["vertInfo"][verts]["x"] -= 1
                    }
                }
                break;
        }
        console.log(player)
        return objNames
        // movedFrom = "Move because of input " + moves;
        // return screenArray;
    }
    function compileObjs(theObjs){
        var objCt = 0;
        var trueData = [];
        var vertData = [];
        var vertCt = 0;
        var vertTransfer = 0;
        var sortData = function(a, b) {
            return a + (b * rows);
        };
        for (objCt = 0; objCt < theObjs.length; objCt++) {
            var currentObj = theObjs[objCt];
            var XandY = sortData(currentObj["x"], currentObj["y"]);
            trueData.push([XandY, currentObj]);
            if(Object.hasOwn(currentObj, "vertInfo")){
                for(vertCt = 0; vertCt < Object.keys(currentObj["vertInfo"]).length; vertCt++){
                        var vert = currentObj["vertInfo"][String(vertCt)];
                        // console.log(currentObj["name"], vertCt, "is", vert["x"], vert["y"])
                        var vertX = vert["x"] + currentObj["x"]
                        var vertY =  vert["y"] + currentObj["y"]
                        var vertXAndY = sortData(vertX, vertY);
                        if(Object.hasOwn(vert, "color")){
                            vertData["color"] = vert["color"]
                        }
                        strVertCt = String(vertCt)
                        var vertPlacement = {
                            "index" : vertCt,
                            "name" : objCt,
                            "item" : vert["item"],
                            "x": vertX,
                            "y": vertY,
                            "color": vert["color"]
                        }
                        if(vertX >= 0 && vertX <= rows && vertY >= 0 && vertY <= cols){
                            vertData.push([vertXAndY, vertPlacement]);
                        } else{
                            console.log("Rejected", currentObj["name"], vertX, ",", vertY)
                        }
                        // console.log("Available Data is", vert)
                }
            }
        }
        for(vertTransfer = 0; vertTransfer < vertData.length; vertTransfer++){
            trueData.push([vertData[vertTransfer][0], vertData[vertTransfer][1]]);
        }
    
        // Need to determin weather X is > 0 or Y is > 0 for the variables
    
        trueData.sort((a, b) => {
            if(a[0] > b[0]){
                return 1;
            }
            if(a[0] < b[0]){
                return -1;
            }
            return 0;
        }
        )
        for (var sortCt = 0; sortCt < trueData.length; sortCt++) {
            trueData[sortCt] = {
                "index" : trueData[sortCt][1]["index"],
                "name" : trueData[sortCt][1]["name"],
                "item" : trueData[sortCt][1]["item"],
                "x" : trueData[sortCt][1]["x"],
                "y" : trueData[sortCt][1]["y"],
                "color": trueData[sortCt][1]["color"]
            };
        }
        console.log("TrueData is", trueData)
        return trueData;
    }
</script>