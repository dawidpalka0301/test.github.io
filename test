<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Generator Dyplomów</title>
</head>
<body>
  <h1>Generator Dyplomów</h1>
  
  <div>
    <label for="fileInput">Wybierz wzór dyplomu (plik Word):</label>
    <input type="file" id="fileInput" accept=".docx">
  </div>

  <div>
    <label for="dateInput">Data:</label>
    <input type="date" id="dateInput">
  </div>

  <div>
    <label for="peopleList">Lista osób (imię, nazwisko):</label><br>
    <textarea id="peopleList" rows="4" cols="50"></textarea>
  </div>

  <button id="generateDiplomasBtn">Generuj Dyplomy</button>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.5.0/jszip.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.0/FileSaver.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/Promise.min.js/8.0.3/promise.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf-lib/1.16.0/pdf-lib.js"></script>
  <script>
    const generateDiplomasBtn = document.getElementById('generateDiplomasBtn');
    const fileInput = document.getElementById('fileInput');
    const dateInput = document.getElementById('dateInput');
    const peopleListInput = document.getElementById('peopleList');

    generateDiplomasBtn.addEventListener('click', async function() {
      const selectedDate = dateInput.value;
      const peopleList = peopleListInput.value.trim().split('\n').map(line => line.split(',').map(item => item.trim()));

      if (!fileInput.files.length) {
        alert("Wybierz plik wzoru dyplomu!");
        return;
      }

      const file = fileInput.files[0];
      const fileReader = new FileReader();

      fileReader.onload = async function(event) {
        const templateBuffer = event.target.result;

        const zip = new JSZip();
        const doc = new window.docxtemplater();

        zip.loadAsync(templateBuffer).then(function() {
          const docx = zip.file("word/document.xml").asText();
          doc.loadZip(zip);

          const pdfDoc = PDFLib.PDFDocument.create();

          peopleList.forEach((person, index) => {
            const [firstName, lastName] = person;
            const newPdfPage = pdfDoc.addPage();

            const text = `Imię i Nazwisko: ${firstName} ${lastName}\nData: ${selectedDate}\nNumeracja: ${index + 1}`;
            newPdfPage.drawText(text, {
              x: 50,
              y: newPdfPage.getHeight() - 100,
              size: 24,
            });
          });

          pdfDoc.setTitle('Dyplomy');

          const pdfBytes = await pdfDoc.save();
          const blob = new Blob([pdfBytes], { type: 'application/pdf' });
          saveAs(blob, 'Dyplomy.pdf');
        });
      };

      fileReader.onerror = function() {
        alert("Wystąpił błąd podczas wczytywania pliku.");
      };

      fileReader.readAsArrayBuffer(file);
    });
  </script>
</body>
</html>
