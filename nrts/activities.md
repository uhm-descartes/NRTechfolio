---
layout: nrt
type: nrt
title: "Activities"
# All dates must be YYYY-MM-DD format!
date:
published: true
labels:
  - Activities
  - Learning
---
<div id="table-container" class="container"></div>

<script type="text/javascript">
/*
      const query = 'SELECT * WHERE C = "{{ site.data.bio.basics.email }}"';
      fetch(`https://docs.google.com/spreadsheets/d/1cYoC5aqpM6r2DceIvGN8y0H5AK1b-n1CC-yX-NmWUtI/gviz/tq?sheet=Activities&tq=${query}`)
        .then(response => {
          if (!response.ok) {
            throw new Error('Network response was not ok');
          }
          return response.text();
        })
        .then(responseText => {
console.log(responseText);
          let jsonString = responseText.match(/\{.*\}/)[0];
          let jsonData = JSON.parse(jsonString);
          let data = jsonData.table.rows.map(row => row.c.map(cell => cell.f))[0];
          let headers = jsonData.table.cols.map(col => col.label);
            tableHtml = '<table class="table table-bordered"><tbody><thead><tr><th>Activity</th><th>Completed Date</th></tr></thead>';
            for(let i = 3; i < headers.length; i++) {
                if(data[i] === undefined) {
                    continue;
                }
              tableHtml += `<tr><td>${headers[i]}</td><td>${data[i]}</td></tr>`;
            }
            tableHtml += '<tbody></table>';
            document.getElementById('table-container').innerHTML = tableHtml;
        })
        .catch(error => {
          console.error('There was a problem fetching the data:', error);
        });
*/
    </script>

<script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
<script type="text/javascript">

google.charts.load('current', {'packages':['corechart']});
google.charts.setOnLoadCallback(drawTable);

function drawTable() {
    var queryString = encodeURIComponent(`SELECT * WHERE C = "{{ site.data.bio.basics.email }}"`);
    var query = new google.visualization.Query(
        `https://docs.google.com/spreadsheets/d/1cYoC5aqpM6r2DceIvGN8y0H5AK1b-n1CC-yX-NmWUtI/gviz/tq?sheet=Activities&tq=${queryString}`
        );
    
    query.send(handleQueryResponse);
}

function handleQueryResponse(response) {
    if (response.isError()) {
        console.log('Error in query: ' + response.getMessage() + ' ' + response.getDetailedMessage());
        return;
    }

    var data = response.getDataTable();
    var numRows = data.getNumberOfRows();

    tableHtml = '<table class="table table-bordered"><tbody><thead><tr><th>Year</th><th>Activity</th><th>Completed Date</th></tr></thead>';
    for(let row = 0; row < numRows; row++) {
        let activity_year = data.getValue(row,0);
        for(let i = 3; i < data.getNumberOfColumns(); i++) {
            let activity = data.getColumnLabel(i);
            let completedDate = data.getValue(row,i);
            if(completedDate === undefined || completedDate === '') {
                continue;
            }
          tableHtml += `<tr><td>${activity_year}</td><td>${activity}</td><td>${completedDate.toLocaleDateString()}</td></tr>`;
        }
    }
    tableHtml += '<tbody></table>';
    document.getElementById('table-container').innerHTML = tableHtml;
}
</script>