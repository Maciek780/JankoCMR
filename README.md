<!DOCTYPE html>
<html lang="pl">
<head>
  <meta charset="UTF-8">
  <title>Formularz CMR</title>
  <style>
    body {
      font-family: Arial;
      padding: 20px;
    }
    label, input, textarea {
      display: block;
      margin: 10px 0;
      width: 100%;
    }
    input[type="checkbox"] {
      width: auto;
      margin-right: 5px;
    }
    button {
      padding: 10px 20px;
      font-size: 16px;
      margin-top: 15px;
    }
  </style>
</head>
<body>
  <h2>Formularz CMR</h2>

  <form id="cmrForm">
    <label>Kto wypełnia (nick z Discorda):</label>
    <input name="autor" required>

    <label>Nadawca (Firma, Adres, Kraj):</label>
    <textarea name="nadawca" required></textarea>

    <label>Odbiorca (Firma, Adres, Kraj):</label>
    <textarea name="odbiorca" required></textarea>

    <label>Miejsce przeznaczenia (Miasto, Kraj):</label>
    <input name="miejsce" required>

    <label>Data i miejsce załadunku:</label>
    <input name="zaladunek" required>

    <label>Rodzaj towaru:</label>
    <input name="towar" required>

    <label>Waga w kg:</label>
    <input name="waga" type="number" required>

    <label>Numer rejestracyjny pojazdu:</label>
    <input name="rejestracja" required>

    <label>
      <input type="checkbox" name="adr">
      Czy wieziesz ADR?
    </label>

    <button type="submit">Wyślij na Discorda</button>
  </form>

  <p id="info"></p>

  <script>
    const webhookURL = "https://discord.com/api/webhooks/1375560830640328735/risw0FVGB48nElVgvAXuOoKAk7RIJQJbu4sr3uwhKjUYGsRgTeDMbwSl-gjf5vPKZ3K1";

    document.getElementById("cmrForm").addEventListener("submit", function(e) {
      e.preventDefault();

      const form = e.target;
      const data = {
        autor: form.autor.value.trim(),
        nadawca: form.nadawca.value.trim(),
        odbiorca: form.odbiorca.value.trim(),
        miejsce: form.miejsce.value.trim(),
        zaladunek: form.zaladunek.value.trim(),
        towar: form.towar.value.trim(),
        waga: form.waga.value.trim(),
        rejestracja: form.rejestracja.value.trim(),
        adr: form.adr.checked ? "✅ Tak" : "❌ Nie"
      };

      const message = `
📦 **Nowy dokument CMR** 📦

** Wypełnił:** ${data.autor}
**Nadawca:** ${data.nadawca}
**Odbiorca:** ${data.odbiorca}
**Miejsce przeznaczenia:** ${data.miejsce}
**Załadunek:** ${data.zaladunek}
**Towar:** ${data.towar}
**Waga:** ${data.waga} kg
**Numer rejestracyjny:** ${data.rejestracja}
**ADR:** ${data.adr}
`;

      fetch(webhookURL, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ content: message })
      }).then(res => {
        if (res.status === 204) {
          document.getElementById("info").innerText = "✅ Wysłano pomyślnie!";
          form.reset();
        } else {
          document.getElementById("info").innerText = "❌ Błąd przy wysyłaniu!";
        }
      }).catch(err => {
        console.error("Błąd:", err);
        document.getElementById("info").innerText = "❌ Nie udało się połączyć z Discordem.";
      });
    });
  </script>
</body>
</html>
