# Netto-Rechner Italien

> Brutto-Netto in Italien 2026: Arbeitnehmerbeiträge, Einkommensteuer (IRPEF) mit Abzügen, Regional- und Gemeindezuschläge für Mailand, Rom, Turin, Bologna und Neapel.


<section> <span class="eyebrow">Netto-Rechner</span> <h1 id="was-ihr-mitarbeiter-in-italien-netto-erhaelt">Was Ihr Mitarbeiter in Italien netto erhält</h1> <p class="lede">Geben Sie das Bruttojahresgehalt und die Stadt ein. Die Schätzung wendet die Regeln für 2026 an: Arbeitnehmerbeiträge, Einkommensteuer mit Abzügen für Arbeitnehmer und die lokalen Zuschläge.</p> </section> <section> <div class="grid"> <form class="panel" id="nf" style="display:grid;gap:16px"> <label for="g">Bruttojahresgehalt, RAL (EUR)<input id="g" type="number" min="10000" step="1000" value="50000"></label> <label for="city">Stadt<select id="city"><option value="Milan">Mailand</option><option value="Rome">Rom</option><option value="Turin">Turin</option><option value="Bologna">Bologna</option><option value="Naples">Neapel</option></select></label> <label for="inst">Gezahlt in<select id="inst"><option value="13">13 Raten</option><option value="14">14 Raten</option><option value="12">12 (13./14. anteilig)</option></select></label> </form> <div class="panel" style="min-width:0"><div class="tbl"><table id="nt"><thead><tr><th>Posten</th><th class="num">Pro Jahr</th></tr></thead><tbody></tbody></table></div> <p id="nm" style="margin-bottom:0;font-weight:600"></p></div> </div>  </section> <section class="prose"> <h2 id="so-funktioniert-die-berechnung">So funktioniert die Berechnung</h2> <ol> <li>Der Mitarbeiter zahlt 9,19 % Sozialversicherung auf das Brutto, plus 1 % auf den Teil über 56.224 € pro Jahr.</li> <li>Auf den Rest fällt Einkommensteuer (IRPEF) an: 23 % bis 28.000 €, 33 % von 28.000 bis 50.000 €, 43 % darüber.</li> <li>Steuerabzüge für Arbeitnehmer senken die Steuer. Sie sind bei niedrigen Gehältern am höchsten und erreichen bei 50.000 € null. Zwischen 20.000 und 40.000 € gilt ein zusätzlicher Abzug von bis zu 1.000 €.</li> <li>Regional- und Gemeindezuschlag kommen hinzu, außer das Einkommen liegt unter der Freigrenze der Stadt.</li> </ol> <p>Anders als in Deutschland gibt es keine Steuerklassen und keine Kirchensteuer; Familienverhältnisse wirken über Abzüge. Nichts davon ändert Ihre Kosten als Arbeitgeber: siehe <a href="kosten-rechner.html">Kostenrechner</a>.</p> </section> 
<script>(function(){const N = {"status":"secondary","source":"data/params/2026.yaml (irpef, inps, addizionali); TUIR art. 11 and 13; L. 207/2024; L. 199/2025","employee_inps":0.0919,"extra_1pct_above":56224,"irpef":[[28000,0.23],[50000,0.33],[null,0.43]],"cities":{"Milan":{"region":"Lombardei","regional":[[null,0.0123]],"municipal":0.008,"exempt":23000},"Rome":{"region":"Lazio","regional":[[28000,0.0173],[null,0.0333]],"municipal":0.009,"exempt":14000},"Turin":{"region":"Piemont","regional":[[null,0.0162]],"municipal":0.008,"municipal_note":"0.8% to 1.2% by band; 0.8% used","exempt":11790},"Bologna":{"region":"Emilia-Romagna","regional":[[null,0.0133]],"municipal":0.008,"exempt":15000},"Naples":{"region":"Kampanien","regional":[[null,0.0173]],"municipal":0.01,"exempt":12000}}};

const eur = n => (n < 0 ? '−' : '') + Math.round(Math.abs(n)).toLocaleString('de-DE') + ' €';
function bands(x, b){ let t = 0, lo = 0; for (const [hi, r] of b){ const top = hi ?? Infinity; if (x > lo) t += (Math.min(x, top) - lo) * r; lo = top; } return t; }
function credit(r){ if (r <= 15000) return 1955; if (r <= 28000) return 1910 + 1190 * (28000 - r) / 13000; if (r <= 50000) return 1910 * (50000 - r) / 22000 + (r > 25000 && r <= 35000 ? 65 : 0); return 0; }
function extra(r){ if (r > 20000 && r <= 32000) return 1000; if (r > 32000 && r < 40000) return 1000 * (40000 - r) / 8000; return 0; }
function calc(){
  const g = +document.getElementById('g').value || 0, c = N.cities[document.getElementById('city').value], n = +document.getElementById('inst').value;
  const inps = g * N.employee_inps + Math.max(0, g - N.extra_1pct_above) * 0.01;
  const tx = g - inps;
  const irpef = Math.max(0, bands(tx, N.irpef) - credit(tx) - extra(tx));
  const reg = bands(tx, c.regional), mun = tx > c.exempt ? tx * c.municipal : 0;
  const net = g - inps - irpef - reg - mun;
  const rows = [['Bruttogehalt', g], ['Sozialversicherung Arbeitnehmer', -inps], ['Einkommensteuer (IRPEF) nach Abzügen', -irpef], [`Regionalzuschlag (${c.region})`, -reg], ['Gemeindezuschlag', -mun]];
  document.querySelector('#nt tbody').innerHTML = rows.map(([k, v]) => `<tr><td>${k}</td><td class="num">${eur(v)}</td></tr>`).join('') + `<tr><td><b>Nettogehalt</b></td><td class="num"><b>${eur(net)}</b></td></tr>`;
  document.getElementById('nm').textContent = `Etwa ${eur(net / n)} netto in jedem der ${n} Lohnzettel (${(net / g * 100).toFixed(0)} % vom Brutto).`;
}
document.getElementById('nf').addEventListener('input', calc); document.getElementById('nf').addEventListener('submit', e => e.preventDefault()); calc();
})();</script>


---
Source: https://mitarbeiteritalien.com/netto-rechner.html
