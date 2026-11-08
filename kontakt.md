# Angebot anfordern

> Fordern Sie ein Angebot für die Anstellung eines Mitarbeiters in Italien an. Drei kurze Schritte, Antwort innerhalb eines Werktags.


<section> <span class="eyebrow">Angebot anfordern</span> <h1 id="wen-moechten-sie-in-italien-einstellen">Wen möchten Sie in Italien einstellen?</h1> <p class="lede">Drei kurze Schritte. Innerhalb eines Werktags erhalten Sie die vollständigen Kosten und den Vertragsentwurf in Grundzügen.</p> </section> <section> <form id="lead" class="panel" style="display:grid;gap:18px;max-width:640px" novalidate> <p id="stepinfo" class="tag" style="justify-self:start">Schritt 1 von 3</p> <fieldset data-step="1" style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="email">Geschäftliche E-Mail<input id="email" type="email" autocomplete="email" required></label> <label for="company">Unternehmen<input id="company" autocomplete="organization"></label> </fieldset> <fieldset data-step="2" hidden style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="service">Was benötigen Sie?<select id="service"><option value="Employer of Record">Employer of Record</option><option value="Payroll (we have an Italian entity)">Lohnabrechnung (wir haben eine italienische Gesellschaft)</option><option value="Work visa / Blue Card">Arbeitsvisum / Blaue Karte EU</option><option value="Recruitment">Rekrutierung</option></select></label> <label for="count">Wie viele Personen in Italien?<input id="count" type="number" min="1" value="1"></label> <label for="city">Stadt oder Region<input id="city" placeholder="Mailand, Bozen, remote…"></label> <label for="salary">Geplantes Bruttojahresgehalt (EUR)<input id="salary" type="number" step="1000"></label> </fieldset> <fieldset data-step="3" hidden style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="name">Ihr Name<input id="name" autocomplete="name"></label> <label for="start">Geplanter Eintritt<input id="start" type="date"></label> <label for="notes">Sonstiges<textarea id="notes" rows="4"></textarea></label> <label style="display:flex;gap:10px;align-items:flex-start;font-weight:400"><input id="consent" type="checkbox" style="width:auto;margin-top:5px">Ich bin einverstanden, dass MitarbeiterItalien meine Daten zur Beantwortung dieser Anfrage verarbeitet (Datenschutzerklärung).</label> </fieldset> <p id="msg" role="status" style="margin:0;color:var(--accent)"></p> <div class="cta"><button type="button" class="btn ghost" id="back" hidden>Zurück</button><button type="submit" class="btn" id="next">Weiter</button></div> </form> </section> 
<script>
// Il passo 1 salva subito l'email (lead abbandonato recuperabile dal nurturing). Endpoint del worker: /api/lead.
const f = document.getElementById('lead'); let step = 1;
// Oggetto dalla pagina partner: ?subject=Partner precompila le note.
{ const sj = new URLSearchParams(location.search).get('subject'); if (sj) document.getElementById('notes').value = '[' + sj.slice(0, 40) + '] '; }
const sets = [...f.querySelectorAll('fieldset')];
const data = () => Object.fromEntries([...f.querySelectorAll('input,select,textarea')].map(e => [e.id, e.type === 'checkbox' ? e.checked : e.value]));
async function save(final){ try { await fetch('/api/lead', {method:'POST', headers:{'Content-Type':'application/json'}, body: JSON.stringify({...data(), step, final, brand:'mitarbeiteritalien'})}); } catch(e){} }
function show(){ sets.forEach(s => s.hidden = +s.dataset.step !== step);
  document.getElementById('stepinfo').textContent = `Schritt ${step} von 3`;
  document.getElementById('back').hidden = step === 1;
  document.getElementById('next').textContent = step === 3 ? 'Anfrage senden' : 'Weiter'; }
// Ripresa dal link nelle email: ?resume=<token> precompila i campi e salta al passo successivo.
(async () => { const tok = new URLSearchParams(location.search).get('resume'); if (!tok) return;
  try { const r = await fetch('/api/lead/' + encodeURIComponent(tok)); if (!r.ok) return; const d = await r.json();
    for (const [k, v] of Object.entries(d)) { const el = document.getElementById(k); if (el && v != null) el.value = v; }
    step = d.step || 2; show(); } catch(e){} })();
document.getElementById('back').onclick = () => { step--; show(); };
f.addEventListener('submit', e => { e.preventDefault(); const m = document.getElementById('msg'); m.textContent = '';
  if (step === 1 && !document.getElementById('email').checkValidity()) { m.textContent = 'Bitte geben Sie eine gültige geschäftliche E-Mail-Adresse ein.'; return; }
  if (step === 3 && !document.getElementById('consent').checked) { m.textContent = 'Bitte bestätigen Sie die Einwilligung, damit wir antworten können.'; return; }
  save(step === 3);
  if (step < 3) { step++; show(); } else { f.querySelectorAll('fieldset,.cta').forEach(x => x.hidden = true); m.textContent = 'Vielen Dank. Wir antworten innerhalb eines Werktags.'; } });
</script>


---
Source: https://mitarbeiteritalien.com/kontakt.html
