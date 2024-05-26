---
layout: nrt
type: nrt
title: "Modules Completed"
# All dates must be YYYY-MM-DD format!
date:
published: true
labels:
  - Modules
---
<div id="table-container" class="container">
<div class="row"><div class="col-md-6 fw-bold">Module</div><div class="col-md-6 fw-bold">Completed Date</div></div>
</div>

<script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
<script type="text/javascript">

google.charts.load('current', {'packages':['corechart']});
google.charts.setOnLoadCallback(querySheet);

function querySheet() {
    var queryString = encodeURIComponent(`SELECT B, C WHERE A = "{{ site.data.bio.basics.email }}"`);
    var query = new google.visualization.Query(
        `https://docs.google.com/spreadsheets/d/1cYoC5aqpM6r2DceIvGN8y0H5AK1b-n1CC-yX-NmWUtI/gviz/tq?sheet=Modules&tq=${queryString}`
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
            
        (async () => {
        for (var i = 0; i < numRows; i++) {
            var module = data.getValue(i, 0);
            var completedDate = data.getValue(i, 1);
            const title = await fetchTitle(module);
                document.getElementById('table-container').innerHTML += 
                `<div class="row"><div class="col-md-6"><a class="badge bg-primary" href="${module}">${title}</a></div><div class="col-md-6">${completedDate.toLocaleDateString()}</div></div>`;
        }
        })();
}

    async function fetchTitle(url) {
        try {

            // Fetch the HTML content through the proxy
            const response = await fetch(url);
            const text = await response.text();
            
            // Create a DOM parser
            const parser = new DOMParser();
            const doc = parser.parseFromString(text, 'text/html');
            
            // Get the title element
            const titleElement = doc.querySelector('title');
            
            // Update the page with the fetched title
            return titleElement ? titleElement.innerHTML : 'No title found';
        } catch (error) {
            console.error('Error fetching title:', error);
            document.getElementById('page-title').innerText = 'Error fetching title';
        }
    }

</script>

