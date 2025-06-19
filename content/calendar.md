+++
title = "Kalender"
description = "Terminübersicht"
date = "2019-02-28"
aliases = ["kalender"]
author = "Max"
+++

<div id="calendar"></div>
<form id="booking-form">
  <label for="name">Name:</label>
  <input type="text" id="name" name="name" required>

  <label for="termin">Terminwunsch:</label>
  <input type="datetime-local" id="termin" name="termin" required>

  <label for="service">Service:</label>
  <select id="service" name="service" multiple required>
    <option value="Haare">Haare</option>
    <option value="Bart">Bart</option>
    <option value="Augenbrauen">Augenbrauen</option>
  </select>

  <label for="email">E-Mail-Adresse:</label>
  <input type="email" id="email" name="email" required>

  <label for="whatsapp">WhatsApp (optional):</label>
  <input type="tel" id="whatsapp" name="whatsapp" placeholder="+491701234567">

  <label for="whatsapp">Kommentar:</label>
  <input type="text" id="kommentar" name="kommentar" placeholder="Dein Kommentar">

  <button type="submit">Termin anfragen</button>
</form>

<script>
  document.getElementById('booking-form').addEventListener('submit', async (e) => {
    e.preventDefault();

    const form = e.target;
    const formData = new FormData(form);

    // Multiple Choice Service zu Array verarbeiten
    const services = Array.from(form.querySelector('#service').selectedOptions).map(opt => opt.value);

    const data = {
      name: formData.get('name'),
      termin: formData.get('termin'),
      service: services,
      email: formData.get('email'),
      kommentar: formData.get('kommentar'),
      whatsapp: formData.get('whatsapp') || null
    };

    const response = await fetch('https://oettigmann.app.n8n.cloud/webhook/63c1623a-2297-457b-bf3e-562052abf738', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });

    const result = await response.json();
    alert(result.message || 'Danke! Wir melden uns bald mit der Bestätigung.');
    form.reset();
  });
</script>