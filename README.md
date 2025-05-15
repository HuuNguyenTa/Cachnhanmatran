
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Máy tính nhân ma trận</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <style>
    input[type="number"] {
      width: 60px;
      text-align: center;
    }
    table td {
      padding: 4px;
    }
  </style>
</head>
<body class="bg-light">

<div class="container py-5">
  <h2 class="text-center mb-4">🧮 Ứng dụng nhân hai ma trận hình chữ nhật</h2>

  <div class="row g-3">
    <div class="col-md-6">
      <h5>Ma trận A</h5>
      <label>Hàng:</label>
      <input type="number" id="rowsA" class="form-control mb-2" value="2" min="1">
      <label>Cột:</label>
      <input type="number" id="colsA" class="form-control mb-2" value="3" min="1">
    </div>
    <div class="col-md-6">
      <h5>Ma trận B</h5>
      <label>Hàng:</label>
      <input type="number" id="rowsB" class="form-control mb-2" value="3" min="1">
      <label>Cột:</label>
      <input type="number" id="colsB" class="form-control mb-2" value="2" min="1">
    </div>
  </div>

  <div class="text-center my-3">
    <button class="btn btn-primary" onclick="generateInputs()">➕ Tạo ma trận</button>
  </div>

  <div id="matrices" class="row g-4"></div>

  <div class="text-center mt-4">
    <button class="btn btn-success" onclick="multiplyMatrices()">🟰 Nhân ma trận</button>
  </div>

  <div class="mt-4">
    <h5>Kết quả:</h5>
    <div id="result"></div>
  </div>
</div>

<script>
  function generateMatrix(id, rows, cols) {
    let html = `<h6 class="text-primary">Ma trận ${id}</h6><table class="table table-bordered table-sm"><tbody>`;
    for (let i = 0; i < rows; i++) {
      html += '<tr>';
      for (let j = 0; j < cols; j++) {
        html += `<td><input type="number" class="form-control form-control-sm" id="${id}_${i}_${j}" value="0"></td>`;
      }
      html += '</tr>';
    }
    html += '</tbody></table>';
    return html;
  }

  function generateInputs() {
    const rowsA = parseInt(document.getElementById('rowsA').value);
    const colsA = parseInt(document.getElementById('colsA').value);
    const rowsB = parseInt(document.getElementById('rowsB').value);
    const colsB = parseInt(document.getElementById('colsB').value);

    const container = document.getElementById('matrices');
    container.innerHTML = '';

    if (colsA !== rowsB) {
      alert('⚠️ Số cột của ma trận A phải bằng số hàng của ma trận B để nhân!');
      return;
    }

    const matrixAHTML = generateMatrix('A', rowsA, colsA);
    const matrixBHTML = generateMatrix('B', rowsB, colsB);

    container.innerHTML = `
      <div class="col-md-6">${matrixAHTML}</div>
      <div class="col-md-6">${matrixBHTML}</div>
    `;

    document.getElementById('result').innerHTML = '';
  }

  function getMatrix(id, rows, cols) {
    const matrix = [];
    for (let i = 0; i < rows; i++) {
      matrix[i] = [];
      for (let j = 0; j < cols; j++) {
        const val = parseFloat(document.getElementById(`${id}_${i}_${j}`).value);
        matrix[i][j] = isNaN(val) ? 0 : val;
      }
    }
    return matrix;
  }

  function multiplyMatrices() {
    const rowsA = parseInt(document.getElementById('rowsA').value);
    const colsA = parseInt(document.getElementById('colsA').value);
    const rowsB = parseInt(document.getElementById('rowsB').value);
    const colsB = parseInt(document.getElementById('colsB').value);

    if (colsA !== rowsB) {
      alert('❌ Không thể nhân hai ma trận này!');
      return;
    }

    const A = getMatrix('A', rowsA, colsA);
    const B = getMatrix('B', rowsB, colsB);

    const result = [];
    for (let i = 0; i < rowsA; i++) {
      result[i] = [];
      for (let j = 0; j < colsB; j++) {
        result[i][j] = 0;
        for (let k = 0; k < colsA; k++) {
          result[i][j] += A[i][k] * B[k][j];
        }
      }
    }

    // Hiển thị kết quả
    let html = '<table class="table table-bordered table-sm w-auto"><tbody>';
    for (let i = 0; i < result.length; i++) {
      html += '<tr>';
      for (let j = 0; j < result[0].length; j++) {
        html += `<td><strong>${result[i][j]}</strong></td>`;
      }
      html += '</tr>';
    }
    html += '</tbody></table>';

    document.getElementById('result').innerHTML = html;
  }
</script>

</body>
</html>
