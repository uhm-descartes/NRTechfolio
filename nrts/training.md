---
layout: nrt
type: nrt
title: "Training Plan"
# All dates must be YYYY-MM-DD format!
date: 2016-02-06
published: true
labels:
  - Training
---
<img width="200px" class="rounded float-start pe-4" src="../img/difficulty/degree_difficulty.jpg">

# NRT DESCARTES training plan
<span id="primary_mentor_span" class="badge bg-primary">Primary Mentor: </span>
<span id="secondary_mentor_span" class="badge bg-secondary">Secondary Mentor: </span>
<hline>

<script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
<script type="text/javascript">

google.charts.load('current', {'packages':['corechart']});
google.charts.setOnLoadCallback(querySheet);

function querySheet() {
    var queryString = encodeURIComponent(`SELECT * WHERE C = "{{ site.data.bio.basics.email }}"`);
    var query = new google.visualization.Query(
        `https://docs.google.com/spreadsheets/d/1cYoC5aqpM6r2DceIvGN8y0H5AK1b-n1CC-yX-NmWUtI/gviz/tq?sheet=Training&tq=${queryString}`
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

  let tabledata = jsonData.rows.map(row => row.c.map(cell => cell.v))[0];
  let headers = jsonData.cols.map(col => col.label);
    const primaryMentorIndex = headers.indexOf("Primary Mentor");
    const secondaryMentorIndex = headers.indexOf("Secondary Mentor");
    primary_mentor_span.innerHTML += tabledata[primaryMentorIndex];
    secondary_mentor_span.innerHTML += tabledata[secondaryMentorIndex];
}
</script>