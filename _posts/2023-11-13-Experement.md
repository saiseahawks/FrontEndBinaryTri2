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
        border: 2px solid white;
    }
</style>

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
        "x" : 5,
        "y" : 6,
    };
    var impeadment = {
        "name" : "Second Simple Object",
        "item" : "z",
        "vertInfo":{
            "0":{
                "item" : "b",
                "x": 2,
                "y": 12,
            },
            "1":{
                "item" : "b",
                "x": 4,
                "y": 12,
            },
            "2":{
                "item" : "b",
                "x": 6,
                "y": 12,
            },
            "3":{
                "item" : "b",
                "x": 8,
                "y": 12,
            },
        },
        "faceInfo":{},
        "x" : 2,
        "y" : 12,
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
                "x" : 50,
                "y" : 5
            },
            "3":{
                "x" : 50,
                "y" : 0
            }
        },
        "x": 0,
        "y": 0,
    }
    var floorBarrier = {
        "relatedTo" : 2,
        "x" : 0,
        "y" : 0,
        "width" : 50,
        "height" : 5
    }
    var objNames = [dummyObject, impeadment, floor]
    var barrierNames = [floorBarrier]
    var rows = 25
    var cols = 25
    var result = ""
    addEventListener("keydown", function(event){
    if(event.defaultPrevented){
        return;
    }
    switch (event.key) {
        case "w":
            if(detectIntersect(0, -1)){
                result = movement("w")
            }
            break;
        case "a":
            if(detectIntersect(1, 0)){
                result = movement("a")
            }
            break;
        case "s":
            if(detectIntersect(0, -1)){
                result = movement("s")
            }
            break;
        case "d":
            if(detectIntersect(1, 0)){
                result = movement("d")
            }
            break;
        case "q":
            rotation(moves);
        default:
            break;
    }
    drawPath(result, 25, 25)
    })
    function rotation(moves){
        var positions = [[0, 1], [1, 1], [1, 0], [1, -1], [0, -1], [-1, -1], [-1, 0], [-1, 1]]
    }
    

    function detectIntersect(playerX, playerY){
        var barrier = barrierNames[0] 
        var relatedShape = objNames[barrier["relatedTo"]]
        var isAir = true
        console.log("Related Shape is", relatedShape)

        console.log("X check player is", parseInt(Math.floor(0.5 * canvas.width) / 25))
        console.log("X check 1 is", relatedShape["x"] - 5 > parseInt(Math.floor(0.5 * canvas.width / 25)) || parseInt(Math.floor(0.5 * canvas.width / 25)) >= relatedShape["x"] + barrier["width"] - 5)
        console.log("X check 1 nums are", barrier["width"] + relatedShape["x"], relatedShape["x"])

        console.log("Y check player is", parseInt(Math.floor(0.6 * canvas.height) / 25))
        console.log("Y check 1 is", relatedShape["y"] > parseInt(Math.floor(0.6 * canvas.height / 25)) || parseInt(Math.floor(0.6 * canvas.height / 25)) >= relatedShape["y"] + barrier["height"])
        console.log("Y check 1 nums are", barrier["height"] + relatedShape["y"], relatedShape["y"])

        if(relatedShape["y"] + playerY - 1 > parseInt(Math.floor(0.6 * canvas.height / 25)) || parseInt(Math.floor(0.6 * canvas.height / 25)) > relatedShape["y"] + barrier["height"] - playerY - 1){
        }
        else if(relatedShape["x"] - 5 - playerX > parseInt(Math.floor(0.5 * canvas.width / 25)) || parseInt(Math.floor(0.5 * canvas.width / 25)) > relatedShape["x"] + barrier["width"] - 5 + playerX){
        }
        else{
            isAir = false    
        }
        return isAir
    }

    function overLay(canvasWidth, canvasHeight, xMid, yMid){
        var canvas = document.getElementById("canvas");
        var ctx = canvas.getContext("2d")
        ctx.fillStyle = "#0080FF"
        ctx.fillRect((canvas.width * 0.5) + xMid, (0.6 * canvas.height), canvasWidth, canvasHeight)

    }

    function drawPath(xPlain, horizontalDistance, heightDistance){
        console.log("Data for Plain is", xPlain, xPlain[0])
        var canvas = document.getElementById("canvas");

        var ctx = canvas.getContext("2d")
        horizontalDistance = canvas.width / horizontalDistance
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        for(var objects in xPlain){
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
                    ctx.fillStyle = String(currentPlain["color"])
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
            ctx.fill()
            ctx.stroke()
            console.log("Object is", objNames[objects])
            console.log("Length of Verts are", Object.keys(objNames[objects]["vertInfo"]).length)
            ctx.fillStyle = "#00FF80"
            console.log("Center is", horizontalDistance * xPlain[objects]["x"], (canvas.height / heightDistance) * (heightDistance - xPlain[objects]["y"]))
            ctx.fillRect(horizontalDistance * xPlain[objects]["x"], (canvas.height / heightDistance) * (heightDistance - xPlain[objects]["y"] + 1) - (canvas.height / heightDistance), horizontalDistance, canvas.height / heightDistance)
        }
        overLay(horizontalDistance, canvas.height / heightDistance, xMid, yMid)
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
        return trueData;
    }
</script>
