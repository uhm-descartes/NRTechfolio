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
    var queryString = encodeURIComponent(`SELECT C,D,E WHERE B = "{{ site.data.bio.basics.email }}"`);
    var query = new google.visualization.Query(
        `https://docs.google.com/spreadsheets/d/1cYoC5aqpM6r2DceIvGN8y0H5AK1b-n1CC-yX-NmWUtI/gviz/tq?sheet=Achievements&tq=${queryString}`
        );
    
    query.send(handleQueryResponse);
}

function handleQueryResponse(response) {
    if (response.isError()) {
        console.log('Error in query: ' + response.getMessage() + ' ' + response.getDetailedMessage());
        return;
    }

    let tableHtml ='<div class="row">';

      var data = response.getDataTable();
      var numRows = data.getNumberOfRows();

      for (var i = 0; i < numRows; i++) {
        var badge = data.getValue(i, 0);
        var date = data.getValue(i, 1);
        var URL = data.getValue(i, 2);
          tableHtml += ` <div class="col-md-6"><a href="${URL}"><img src="https://img.shields.io/badge/${badge}-Success-brightgreen" alt="${badge} Badge" /></a><br>${date.toLocaleDateString()}</br></div>`;
    
        }
    tableHtml +='</div>';
    document.getElementById('table-container').innerHTML = tableHtml;

}
</script>