---
layout: nrt
type: nrt
title: "Training Plan"
# All dates must be YYYY-MM-DD format!
date:
published: true
labels:
  - Training
---

<div class="container">
    <div class="row">
        <div class="col-md-6">
            <img width="200px" class="rounded float-start pe-4" src="../img/difficulty/degree_difficulty.jpg">
        </div>
        <div class="col-md-6">
            <span id="primary_advisor_span" class="badge bg-primary">Primary Advisor: </span>
            <span id="secondary_advisor_span" class="badge bg-secondary">Secondary Advisor: </span>
        </div>
    </div>
    <div class="row" id="plan_div">
    </div>
</div>


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
    var numRows = data.getNumberOfRows();
    var jsonData = JSON.parse(data.toJSON());
    let primaryAdvisorIndex = data.getColumnIndex("Primary Advisor");
    let secondaryAdvisorIndex = data.getColumnIndex("Secondary Advisor");

    primary_advisor_span.innerHTML += data.getValue(0, primaryAdvisorIndex);
    secondary_advisor_span.innerHTML += data.getValue(0, secondaryAdvisorIndex);

    // get the plan data after Secondary Advisor column
    for(let i = secondaryAdvisorIndex + 1; i < data.getNumberOfColumns(); i++) {
        let plan_item = data.getColumnLabel(i);
        let plan_item_data = data.getValue(0, i);
        if(plan_item_data === null || plan_item_data === "") {
            continue;
        }
console.log(plan_item_data);
        plan_div.innerHTML += `<div class="card">${plan_item_data}</div>`;
}

}
</script>