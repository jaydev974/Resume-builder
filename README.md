# Resume-builder
Online resume builder with template
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Resume Builder</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    :root {
      --bg: #f6f7fb;
      --card: #ffffff;
      --text: #1f2937;
      --muted: #6b7280;
      --accent: #2563eb;
      --border: #e5e7eb;
    }
    * { box-sizing: border-box; }
    body {
      margin: 0; font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
      color: var(--text); background: var(--bg);
    }
    header {
      padding: 16px 24px; background: var(--card); border-bottom: 1px solid var(--border);
      position: sticky; top: 0; z-index: 10; display: flex; gap: 12px; align-items: center;
    }
    header h1 { font-size: 18px; margin: 0; font-weight: 600; }
    header .actions { margin-left: auto; display: flex; gap: 8px; }
    button, .btn {
      background: var(--accent); color: #fff; border: none; padding: 10px 14px; border-radius: 8px;
      cursor: pointer; font-weight: 600; font-size: 14px;
    }
    button.secondary, .btn.secondary {
      background: #0ea5e9;
    }
    button.gray { background: #374151; }
    .container {
      display: grid; grid-template-columns: 1fr 1fr; gap: 20px; padding: 20px;
    }
    .panel {
      background: var(--card); border: 1px solid var(--border); border-radius: 12px; padding: 16px;
    }
    .panel h2 { margin: 0 0 8px; font-size: 18px; }
    .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
    label { font-size: 12px; color: var(--muted); display: block; margin-bottom: 6px; }
    input, textarea, select {
      width: 100%; padding: 10px 12px; border: 1px solid var(--border); border-radius: 8px;
      outline: none; font-size: 14px; background: #fff;
    }
    textarea { min-height: 80px; resize: vertical; }
    .section { border-top: 1px dashed var(--border); margin-top: 16px; padding-top: 16px; }
    .row { display: flex; gap: 8px; align-items: center; }
    .inline { display: flex; gap: 8px; }
    .list { display: flex; flex-direction: column; gap: 12px; }
    .item {
      border: 1px solid var(--border); border-radius: 10px; padding: 12px; background: #fff;
    }
    .item .row > button { padding: 8px 10px; }
    .tag-input { display: flex; gap: 8px; }
    .tags { display: flex; gap: 8px; flex-wrap: wrap; margin-top: 8px; }
    .tag {
      background: #e0e7ff; color: #1e3a8a; padding: 6px 10px; border-radius: 999px; font-size: 12px;
      display: inline-flex; align-items: center; gap: 6px;
    }
    .tag button { background: transparent; color: #1e3a8a; border: none; cursor: pointer; font-size: 12px; }
    /* Resume preview */
    .resume {
      padding: 28px; border: 1px solid var(--border); border-radius: 12px; background: #fff;
      max-width: 900px; margin: 0 auto;
    }
    .name { font-size: 28px; font-weight: 700; }
    .title { color: var(--muted); margin-top: 4px; }
    .meta { font-size: 13px; color: var(--muted); display: flex; gap: 10px; flex-wrap: wrap; }
    .section-title {
      font-size: 16px; font-weight: 700; border-bottom: 2px solid var(--border); padding-bottom: 6px; margin-top: 18px;
    }
    .xp, .edu { margin-top: 12px; }
    .xp-item, .edu-item { margin-bottom: 10px; }
    .xp-item .top, .edu-item .top { display: flex; justify-content: space-between; gap: 10px; }
    .role { font-weight: 600; }
    .company { color: var(--muted); }
    .dates { color: var(--muted); font-size: 13px; }
    .summary { margin-top: 8px; }
    ul.bullets { margin: 6px 0 0 18px; }
    @media print {
      header, .panel.form { display: none; }
      .container { grid-template-columns: 1fr; }
      body { background: #fff; }
      .resume { border: none; }
    }
  </style>
</head>
<body>
  <header>
    <h1>Resume Builder</h1>
    <div class="actions">
      <button class="secondary" id="load">Load saved</button>
      <button class="gray" id="reset">Reset</button>
      <button id="print">Print / Export PDF</button>
    </div>
  </header>

  <div class="container">
    <!-- Form panel -->
    <div class="panel form">
      <h2>Your details</h2>
      <div class="grid-2">
        <div>
          <label>Full name</label>
          <input id="name" placeholder="e.g., Jaydev Patil" />
        </div>
        <div>
          <label>Professional title</label>
          <input id="headline" placeholder="e.g., Software Engineer" />
        </div>
        <div>
          <label>Email</label>
          <input id="email" placeholder="e.g., jaydev@email.com" />
        </div>
        <div>
          <label>Phone</label>
          <input id="phone" placeholder="e.g., +91 98XXXXXXXX" />
        </div>
        <div>
          <label>Location</label>
          <input id="location" placeholder="e.g., Pune, MH, India" />
        </div>
        <div>
          <label>Links (comma-separated)</label>
          <input id="links" placeholder="e.g., LinkedIn, GitHub, Portfolio" />
        </div>
      </div>

      <div class="section">
        <h2>Professional summary</h2>
        <textarea id="summary" placeholder="2–4 lines about your experience, strengths, and goals."></textarea>
      </div>

      <div class="section">
        <h2>Experience</h2>
        <div class="list" id="xpList"></div>
        <button id="addXp">+ Add experience</button>
      </div>

      <div class="section">
        <h2>Education</h2>
        <div class="list" id="eduList"></div>
        <button id="addEdu">+ Add education</button>
      </div>

      <div class="section">
        <h2>Skills</h2>
        <div class="tag-input">
          <input id="skillInput" placeholder="Add a skill and press Enter" />
          <button id="addSkill">Add</button>
        </div>
        <div class="tags" id="skills"></div>
      </div>

      <div class="section">
        <h2>Certifications</h2>
        <div class="list" id="certList"></div>
        <button id="addCert">+ Add certification</button>
      </div>
    </div>

    <!-- Resume preview -->
    <div class="panel">
      <div class="resume" id="resume">
        <div class="name" id="pv_name">Your Name</div>
        <div class="title" id="pv_headline">Your Title</div>
        <div class="meta" id="pv_meta"></div>

        <div class="section-title">Summary</div>
        <div class="summary" id="pv_summary">Brief summary goes here.</div>

        <div class="section-title">Experience</div>
        <div class="xp" id="pv_xp"></div>

        <div class="section-title">Education</div>
        <div class="edu" id="pv_edu"></div>

        <div class="section-title">Skills</div>
        <div class="skills" id="pv_skills"></div>

        <div class="section-title">Certifications</div>
        <div class="certs" id="pv_certs"></div>
      </div>
    </div>
  </div>

  <script>
    // Helper: state and persistence
    const state = {
      name: "", headline: "", email: "", phone: "", location: "", links: [],
      summary: "",
      xp: [],
      edu: [],
      skills: [],
      certs: []
    };
    const $ = (id) => document.getElementById(id);

    // Render preview
    function render() {
      $("pv_name").textContent = state.name || "Your Name";
      $("pv_headline").textContent = state.headline || "Your Title";

      const metaItems = [];
      if (state.email) metaItems.push(state.email);
      if (state.phone) metaItems.push(state.phone);
      if (state.location) metaItems.push(state.location);
      metaItems.push(...state.links.filter(Boolean));
      $("pv_meta").innerHTML = metaItems.map(m => `<span>${escapeHTML(m)}</span>`).join(" • ");

      $("pv_summary").textContent = state.summary || "Brief summary goes here.";

      $("pv_xp").innerHTML = state.xp.map(x => `
        <div class="xp-item">
          <div class="top">
            <div>
              <span class="role">${escapeHTML(x.role || "Role")}</span>
              <span class="company"> — ${escapeHTML(x.company || "Company")}</span>
            </div>
            <div class="dates">${escapeHTML(formatDates(x.start, x.end))}</div>
          </div>
          <div class="summary">${escapeHTML(x.location || "")}</div>
          ${x.bullets && x.bullets.length ? `
            <ul class="bullets">
              ${x.bullets.map(b => `<li>${escapeHTML(b)}</li>`).join("")}
            </ul>` : ""}
        </div>
      `).join("");

      $("pv_edu").innerHTML = state.edu.map(e => `
        <div class="edu-item">
          <div class="top">
            <div>
              <span class="role">${escapeHTML(e.degree || "Degree")}</span>
              <span class="company"> — ${escapeHTML(e.school || "Institution")}</span>
            </div>
            <div class="dates">${escapeHTML(formatDates(e.start, e.end))}</div>
          </div>
          <div class="summary">${escapeHTML(e.location || "")}</div>
          ${e.details ? `<div>${escapeHTML(e.details)}</div>` : ""}
        </div>
      `).join("");

      $("pv_skills").innerHTML = state.skills.length
        ? state.skills.map(s => `<span class="tag">${escapeHTML(s)}</span>`).join(" ")
        : `<span class="tag">Skill 1</span> <span class="tag">Skill 2</span>`;

      $("pv_certs").innerHTML = state.certs.map(c => `
        <div class="edu-item">
          <div class="top">
            <div>
              <span class="role">${escapeHTML(c.name || "Certification")}</span>
              <span class="company"> — ${escapeHTML(c.org || "Issuer")}</span>
            </div>
            <div class="dates">${escapeHTML(c.year || "")}</div>
          </div>
          <div class="summary">${escapeHTML(c.id || "")}</div>
        </div>
      `).join("");

      localStorage.setItem("resumeState", JSON.stringify(state));
    }

    // Escape HTML
    function escapeHTML(str) {
      return (str || "").toString().replace(/[&<>"']/g, s => ({
        "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;", "'": "&#39;"
      }[s]));
    }

    function formatDates(start, end) {
      if (!start && !end) return "";
      if (start && !end) return `${start} — Present`;
      return `${start || ""} — ${end || ""}`;
    }

    // Bind top-level fields
    ["name","headline","email","phone","location","summary"].forEach(id => {
      $(id).addEventListener("input", e => {
        state[id] = e.target.value;
        render();
      });
    });

    // Links
    $("links").addEventListener("input", e => {
      state.links = e.target.value.split(",").map(s => s.trim()).filter(Boolean);
      render();
    });

    // Skills
    $("addSkill").addEventListener("click", () => addSkill());
    $("skillInput").addEventListener("keydown", e => {
      if (e.key === "Enter") { e.preventDefault(); addSkill(); }
    });
    function addSkill() {
      const val = $("skillInput").value.trim();
      if (!val) return;
      state.skills.push(val);
      $("skillInput").value = "";
      drawSkills();
      render();
    }
    function drawSkills() {
      $("skills").innerHTML = state.skills.map((s, i) =>
        `<span class="tag">${escapeHTML(s)} <button onclick="removeSkill(${i})">✕</button></span>`
      ).join("");
    }
    window.removeSkill = (i) => { state.skills.splice(i,1); drawSkills(); render(); };

    // Experience
    $("addXp").addEventListener("click", () => addXpItem());
    function addXpItem(pref = {}) {
      state.xp.push({ role: pref.role || "", company: pref.company || "", start: pref.start || "", end: pref.end || "", location: pref.location || "", bullets: pref.bullets || [] });
      drawXp();
      render();
    }
    function drawXp() {
      $("xpList").innerHTML = state.xp.map((x, i) => `
        <div class="item">
          <div class="grid-2">
            <div><label>Role</label><input value="${escapeAttr(x.role)}" oninput="updXp(${i}, 'role', this.value)" /></div>
            <div><label>Company</label><input value="${escapeAttr(x.company)}" oninput="updXp(${i}, 'company', this.value)" /></div>
            <div><label>Start</label><input placeholder="MM/YYYY" value="${escapeAttr(x.start)}" oninput="updXp(${i}, 'start', this.value)" /></div>
            <div><label>End</label><input placeholder="MM/YYYY or Present" value="${escapeAttr(x.end)}" oninput="updXp(${i}, 'end', this.value)" /></div>
            <div class="grid-2" style="grid-column: 1 / -1;">
              <div><label>Location</label><input value="${escapeAttr(x.location)}" oninput="updXp(${i}, 'location', this.value)" /></div>
              <div class="row" style="align-items:flex-end; justify-content:flex-end;">
                <button class="gray" onclick="delXp(${i})">Delete</button>
              </div>
            </div>
          </div>
          <div class="section">
            <label>Achievements (one per line)</label>
            <textarea oninput="updXpBullets(${i}, this.value)">${escapeHTML(x.bullets.join("\n"))}</textarea>
          </div>
        </div>
      `).join("");
    }
    window.updXp = (i, key, val) => { state.xp[i][key] = val; render(); };
    window.updXpBullets = (i, val) => { state.xp[i].bullets = val.split("\n").map(s => s.trim()).filter(Boolean); render(); };
    window.delXp = (i) => { state.xp.splice(i,1); drawXp(); render(); };

    // Education
    $("addEdu").addEventListener("click", () => addEduItem());
    function addEduItem(pref = {}) {
      state.edu.push({ degree: pref.degree || "", school: pref.school || "", start: pref.start || "", end: pref.end || "", location: pref.location || "", details: pref.details || "" });
      drawEdu();
      render();
    }
    function drawEdu() {
      $("eduList").innerHTML = state.edu.map((e, i) => `
        <div class="item">
          <div class="grid-2">
            <div><label>Degree</label><input value="${escapeAttr(e.degree)}" oninput="updEdu(${i}, 'degree', this.value)" /></div>
            <div><label>Institution</label><input value="${escapeAttr(e.school)}" oninput="updEdu(${i}, 'school', this.value)" /></div>
            <div><label>Start</label><input placeholder="YYYY" value="${escapeAttr(e.start)}" oninput="updEdu(${i}, 'start', this.value)" /></div>
            <div><label>End</label><input placeholder="YYYY" value="${escapeAttr(e.end)}" oninput="updEdu(${i}, 'end', this.value)" /></div>
            <div class="grid-2" style="grid-column: 1 / -1;">
              <div><label>Location</label><input value="${escapeAttr(e.location)}" oninput="updEdu(${i}, 'location', this.value)" /></div>
              <div class="row" style="align-items:flex-end; justify-content:flex-end;">
                <button class="gray" onclick="delEdu(${i})">Delete</button>
              </div>
            </div>
          </div>
          <div class="section">
            <label>Details</label>
            <textarea oninput="updEdu(${i}, 'details', this.value)">${escapeHTML(e.details)}</textarea>
          </div>
        </div>
      `).join("");
    }
    window.updEdu = (i, key, val) => { state.edu[i][key] = val; render(); };
    window.delEdu = (i) => { state.edu.splice(i,1); drawEdu(); render(); };

    // Certifications
    $("addCert").addEventListener("click", () => addCertItem());
    function addCertItem(pref = {}) {
      state.certs.push({ name: pref.name || "", org: pref.org || "", year: pref.year || "", id: pref.id || "" });
      drawCerts();
      render();
    }
    function drawCerts() {
      $("certList").innerHTML = state.certs.map((c, i) => `
        <div class="item">
          <div class="grid-2">
            <div><label>Name</label><input value="${escapeAttr(c.name)}" oninput="updCert(${i}, 'name', this.value)" /></div>
            <div><label>Issuer</label><input value="${escapeAttr(c.org)}" oninput="updCert(${i}, 'org', this.value)" /></div>
            <div><label>Year</label><input value="${escapeAttr(c.year)}" oninput="updCert(${i}, 'year', this.value)" /></div>
            <div><label>ID/URL</label><input value="${escapeAttr(c.id)}" oninput="updCert(${i}, 'id', this.value)" /></div>
          </div>
          <div class="row" style="justify-content:flex-end; margin-top:8px;">
            <button class="gray" onclick="delCert(${i})">Delete</button>
          </div>
        </div>
      `).join("");
    }
    window.updCert = (i, key, val) => { state.certs[i][key] = val; render(); };
    window.delCert = (i) => { state.certs.splice(i,1); drawCerts(); render(); };

    // Attr escape for value=""
    function escapeAttr(str) {
      return (str || "").toString().replace(/"/g, "&quot;");
    }

    // Controls
    $("print").addEventListener("click", () => window.print());
    $("reset").addEventListener("click", () => {
      if (!confirm("Clear all data?")) return;
      localStorage.removeItem("resumeState");
      Object.assign(state, { name:"", headline:"", email:"", phone:"", location:"", links:[], summary:"", xp:[], edu:[], skills:[], certs:[] });
      ["name","headline","email","phone","location","summary","links","skillInput"].forEach(id => $(id).value = "");
      drawXp(); drawEdu(); drawCerts(); drawSkills(); render();
    });
    $("load").addEventListener("click", () => {
      const saved = localStorage.getItem("resumeState");
      if (!saved) { alert("No saved resume found."); return; }
      Object.assign(state, JSON.parse(saved || "{}"));
      // populate inputs
      ["name","headline","email","phone","location","summary"].forEach(id => $(id).value = state[id] || "");
      $("links").value = (state.links || []).join(", ");
      drawXp(); drawEdu(); drawCerts(); drawSkills(); render();
    });

    // Seed example
    function seedExample() {
      Object.assign(state, {
        name: "Jaydev Patil",
        headline: "Software Engineer",
        email: "jaydev@example.com",
        phone: "+91 98XXXXXXXX",
        location: "Haveli, Pune, MH, India",
        links: ["linkedin.com/in/jaydev", "github.com/jaydev"],
        summary: "Full-stack developer with 4+ years building scalable web apps. Passionate about clean architecture, performance, and delightful UX.",
        skills: ["JavaScript", "TypeScript", "React", "Node.js", "SQL", "REST APIs", "Git", "Docker"],
        xp: [
          { role: "Senior Frontend Engineer", company: "TechNest", start: "03/2023", end: "Present", location: "Pune, India", bullets: [
            "Led migration to React 18 and Vite, cutting build times by 40%.",
            "Implemented design system improving feature delivery speed by 30%.",
            "Collaborated with backend to optimize API payloads, reducing TTI by 25%."
          ]},
          { role: "Software Engineer", company: "Skyware", start: "06/2021", end: "02/2023", location: "Remote", bullets: [
            "Built role-based access and audit logging for B2B SaaS.",
            "Introduced Cypress e2e tests, decreasing production bugs by 20%."
          ]}
        ],
        edu: [
          { degree: "B.E. Computer Engineering", school: "Savitribai Phule Pune University", start: "2017", end: "2021", location: "Pune", details: "Graduated with First Class. Coursework: DSA, DBMS, Web Technologies." }
        ],
        certs: [
          { name: "AWS Certified Cloud Practitioner", org: "Amazon Web Services", year: "2024", id: "Credential URL" }
        ]
      });
      // populate inputs
      ["name","headline","email","phone","location","summary"].forEach(id => $(id).value = state[id] || "");
      $("links").value = (state.links || []).join(", ");
      drawXp(); drawEdu(); drawCerts(); drawSkills(); render();
    }

    // Init
    const saved = localStorage.getItem("resumeState");
    if (saved) {
      Object.assign(state, JSON.parse(saved));
      ["name","headline","email","phone","location","summary"].forEach(id => $(id).value = state[id] || "");
      $("links").value = (state.links || []).join(", ");
      drawXp(); drawEdu(); drawCerts(); drawSkills(); render();
    } else {
      seedExample();
    }
  </script>
</body>
</html>
