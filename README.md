<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <title>Excelga o'xshash jadval</title>

  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/handsontable/dist/handsontable.full.min.css">
  <script src="https://cdn.jsdelivr.net/npm/handsontable/dist/handsontable.full.min.js"></script>

  <style>
    #toolbar {
      margin-bottom: 10px;
      background: #f0f0f0;
      padding: 10px;
      border-radius: 6px;
    }
    button {
      padding: 8px 15px;
      background: #0080ff;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 15px;
    }
    button:hover {
      background: #0066cc;
    }
  </style>
</head>

<body>

<div id="toolbar">
  <button id="calcBtn">Foiz Hisoblash</button>
</div>

<div id="example" style="width:100%; height:600px;"></div>

<script>
  const container = document.getElementById('example');
  const hot = new Handsontable(container, {
    data: Handsontable.helper.createSpreadsheetData(1000, 50),
    rowHeaders: true,
    colHeaders: true,
    width: '100%',
    height: 600,
    licenseKey: 'non-commercial-and-evaluation'
  });

  let step = 0;
  let oktCell = null;
  let senCell = null;

  document.getElementById('calcBtn').onclick = () => {
    step = 1;
    alert("1-qadam: OKTYABR bahosini tanlang (masalan: 4).");
  };

  // katak tanlanganda ishlaydi
  hot.addHook('afterSelectionEnd', function(r, c) {

    // 1-qadam: oktyabr katagi
    if (step === 1) {
      oktCell = { row: r, col: c };
      step = 2;
      alert("2-qadam: Sentyabr bahosini tanlang (masalan: 5).");
    }

    // 2-qadam: sentyabr katagi
    else if (step === 2) {
      senCell = { row: r, col: c };
      step = 3;

      calculatePercent();
      step = 0; // qayta boshlash
    }
  });

  function calculatePercent() {
    const startRow = oktCell.row;
    const oktCol = oktCell.col;
    const senCol = senCell.col;

    for (let r = startRow; r < hot.countRows(); r++) {

      let okt = parseFloat(hot.getDataAtCell(r, oktCol));
      let sen = parseFloat(hot.getDataAtCell(r, senCol));

      if (!isNaN(okt) && !isNaN(sen) && sen !== 0) {
        let diff = okt - sen;
        let perc = Math.round((diff / sen) * 100);

        let arrow = "";
        if (perc > 0) arrow = " /\\";
        if (perc < 0) arrow = " \\/";

        let text = okt + "   " + Math.abs(perc) + "%" + arrow;

        hot.setDataAtCell(r, oktCol, text);
      }
    }

    alert("Foizlar hisoblandi!");
  }
</script>

</body>
</html>
