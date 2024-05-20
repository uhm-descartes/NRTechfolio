---
layout: nrt
type: nrt
title: "NRT Achievements"
# All dates must be YYYY-MM-DD format!
date: 
published: true
labels:
  - Badges
  - Micro Credentials
---
<div id="table-container" class="container"></div>


<script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
<script type="text/javascript">

google.charts.load('current', {'packages':['corechart']});
google.charts.setOnLoadCallback(drawTable);

function drawTable() {
    var queryString = encodeURIComponent(`SELECT C,D WHERE B = "{{ site.data.bio.basics.email }}"`);
    var query = new google.visualization.Query(
        `https://docs.google.com/spreadsheets/d/1cYoC5aqpM6r2DceIvGN8y0H5AK1b-n1CC-yX-NmWUtI/gviz/tq?sheet=Badges&tq=${queryString}`
        );
    
    query.send(handleQueryResponse);
}

function handleQueryResponse(response) {
    if (response.isError()) {
        console.log('Error in query: ' + response.getMessage() + ' ' + response.getDetailedMessage());
        return;
    }

    var data = response.getDataTable();
    var jsonData = JSON.parse(data.toJSON());
console.log(jsonData);

 let headers = jsonData.cols.map(col => col.label);

    tableHtml = '<table class="table table-bordered"><tbody><thead><tr><th>Badge</th><th>Awarded</th></tr></thead>';

for(let i in jsonData.rows) {
    let row = jsonData.rows[i];
    let badge = row.c[0].v;
    let date = row.c[1].f;
      tableHtml += `<tr><td><img src="https://img.shields.io/badge/${badge}-Success-brightgreen" alt="${badge} Badge" /></td><td>${date}</td></tr>`;

}
   


    tableHtml += '<tbody></table>';
    document.getElementById('table-container').innerHTML = tableHtml;

}
</script>