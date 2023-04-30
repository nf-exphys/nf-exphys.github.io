---
layout: page
permalink: /apps/
title: App
description: A test to see if I can host HTML on my website. 
nav: true
nav_order: 1
---

<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Calculate VO2max</title>
  </head>
  <body>
    <h1>Calculate VO2max</h1>
    <form id="vo2max-form">
      <label for="weight">Weight (in kg):</label>
      <input type="number" id="weight" name="weight" required><br><br>
      <label for="vo2max">VO2max:</label>
      <input type="number" id="vo2max" name="vo2max" step="0.01" required><br><br>
      <label for="file">Upload Excel spreadsheet:</label>
      <input type="file" id="file" name="file"><br><br>
      <input type="button" value="Go" onclick="calculateVO2max()">
    </form>
    <div id="output"></div>
    
    <script>
      function calculateVO2max() {
        // Get input values
        var weight = document.getElementById("weight").value;
        var vo2max = document.getElementById("vo2max").value;
        var file = document.getElementById("file").files[0];
        
        // Calculate VO2max
        var result = (vo2max * weight) / 1000;
        
        // Display result
        var output = document.getElementById("output");
        output.innerHTML = "<p>Result: " + result.toFixed(2) + "</p>";
        
        // Display column names of the uploaded spreadsheet
        if (file) {
          var reader = new FileReader();
          reader.onload = function(e) {
            var data = e.target.result;
            var workbook = XLSX.read(data, {type: 'binary'});
            var sheetName = workbook.SheetNames[0];
            var sheet = workbook.Sheets[sheetName];
            var range = XLSX.utils.decode_range(sheet['!ref']);
            var columnNames = [];
            for (var i = range.s.c; i <= range.e.c; i++) {
              var cellAddress = XLSX.utils.encode_cell({r: range.s.r, c: i});
              columnNames.push(sheet[cellAddress].v);
            }
            output.innerHTML += "<p>Column names: " + columnNames.join(", ") + "</p>";
          };
          reader.readAsBinaryString(file);
        }
      }
    </script>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.16.9/xlsx.full.min.js"></script>
  </body>
</html>
