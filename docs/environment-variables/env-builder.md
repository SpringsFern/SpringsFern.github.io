---
hide:
  - toc
---


# Interactive `.env` Builder

Use this tool to generate your `.env` file for tgfilestream.

<style>
.env-section input,
.env-section select,
.env-section textarea {
  width: 100%;
  padding: 8px 10px;
  margin: 6px 0 16px 0;
  border-radius: 6px;
  border: 1px solid var(--md-default-fg-color--lightest);
  background-color: rgba(0,0,0,0.04);
  color: inherit;
  font: inherit;
}

[data-md-color-scheme="slate"] .env-section input,
[data-md-color-scheme="slate"] .env-section select,
[data-md-color-scheme="slate"] .env-section textarea {
  background-color: rgba(255,255,255,0.05);
}

.env-section input:focus,
.env-section select:focus,
.env-section textarea:focus {
  outline: none;
  border-color: var(--md-primary-fg-color);
}

.checkbox-row {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 16px;
}

.checkbox-row input {
  width: auto;
  margin: 0;
}

.tag {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 4px 10px;
  border-radius: 20px;
  border: 1px solid var(--md-default-fg-color--lightest);
  background: rgba(0,0,0,0.05);
  margin: 4px 6px 4px 0;
}

[data-md-color-scheme="slate"] .tag {
  background: rgba(255,255,255,0.06);
}

.token-block {
  border: 1px solid var(--md-default-fg-color--lightest);
  padding: 12px;
  border-radius: 6px;
  margin-bottom: 12px;
  background: rgba(0,0,0,0.03);
}

[data-md-color-scheme="slate"] .token-block {
  background: rgba(255,255,255,0.05);
}

.env-section button {
  border: 1px solid var(--md-default-fg-color--lightest);
  background: transparent;
  padding: 6px 12px;
  border-radius: 6px;
  cursor: pointer;
  font: inherit;
}

.env-section button:hover {
  border-color: var(--md-primary-fg-color);
}
</style>

<div id="ENV_BUILDER"></div>

---

### Generated `.env`

<div class="env-section">
<textarea id="ENV_OUTPUT" rows="16" readonly></textarea>
<br>
<button onclick="copyEnv()">Copy to Clipboard</button>
</div>

---

### Credits
- Our beloved ChatGPT
- Deekshith SH (project author)

---

<script>

/* =============================
   SCHEMA
============================= */

const SCHEMA = [
  {
    group: "Telegram Configuration",
    fields: [
      { key: "API_ID", type: "number", required: true },
      { key: "API_HASH", type: "text", required: true },
      { key: "BOT_TOKEN", type: "text", required: true },
      { key: "BIN_CHANNEL", type: "number", required: true }
    ]
  },
  {
    group: "Server Configuration",
    fields: [
      { key: "HOST", type: "text", default: "0.0.0.0" },
      { key: "PORT", type: "number", default: 8080 },
      { key: "PUBLIC_URL", type: "text" },
      { key: "DEBUG", type: "boolean", default: false }
    ]
  },
  {
    group: "Performance Settings",
    fields: [
      { key: "CONNECTION_LIMIT", type: "number" },
      { key: "DOWNLOAD_PART_SIZE", type: "number" }
    ]
  },
  {
    group: "Bot Behavior Settings",
    fields: [
      { key: "NO_UPDATE", type: "boolean"},
      { key: "NO_MAIN", type: "boolean"},
      { key: "SEQUENTIAL_UPDATES", type: "boolean"},
      { key: "FILE_INDEX_LIMIT", type: "number"},
      { key: "MAX_WARNS", type: "number"},
    ]
  },
  {
    group: "Access Control",
    fields: [
      { key: "ADMIN_IDS", type: "tags" },
      { key: "ALLOWED_IDS", type: "tags" }
    ]
  },
  {
    group: "Multi Tokens",
    fields: [
      { key: "MULTI_TOKEN", type: "multiToken" }
    ]
  },
  {
    group: "Database Backend",
    fields: [
      {
        key: "DB_BACKEND",
        type: "choice",
        options: ["mongodb"],
        default: "mongodb"
      },
      { key: "MONGODB_URI", type: "text", required: true, showIf: { key: "DB_BACKEND", value: "mongodb" } },
      { key: "MONGODB_DBNAME", type: "text", default: "TGFS", showIf: { key: "DB_BACKEND", value: "mongodb" } }
    ]
  }
];


/* =============================
   STATE
============================= */

const state = {};
const tagState = {};
let multiTokens = [];


/* =============================
   RENDER
============================= */

function renderBuilder() {
  const container = document.getElementById("ENV_BUILDER");
  container.innerHTML = "";

  SCHEMA.forEach(section => {

    const title = document.createElement("h2");
    title.textContent = section.group;
    container.appendChild(title);

    const wrapper = document.createElement("div");
    wrapper.className = "env-section";
    container.appendChild(wrapper);

    section.fields.forEach(field => {
      renderField(wrapper, field);
    });
  });

  updateVisibility();
  updateEnv();
}

function renderField(wrapper, field) {

  if (field.type === "multiToken") {
    renderMultiToken(wrapper);
    return;
  }

  const fieldWrapper = document.createElement("div");
  fieldWrapper.dataset.key = field.key;
  wrapper.appendChild(fieldWrapper);

  const label = document.createElement("label");
  label.textContent = field.key + (field.required ? " *" : "");
  fieldWrapper.appendChild(label);

  let input;

  if (field.type === "choice") {
    input = document.createElement("select");
    field.options.forEach(opt => {
      const o = document.createElement("option");
      o.value = opt;
      o.textContent = opt;
      input.appendChild(o);
    });

    input.value = field.default || field.options[0];
    state[field.key] = input.value;

    input.addEventListener("change", () => {
      state[field.key] = input.value;
      updateVisibility();
      updateEnv();
    });

    fieldWrapper.appendChild(input);
    return;
  }

  if (field.type === "boolean") {
    const row = document.createElement("div");
    row.className = "checkbox-row";

    input = document.createElement("input");
    input.type = "checkbox";
    input.checked = field.default || false;
    state[field.key] = input.checked;

    input.addEventListener("change", () => {
      state[field.key] = input.checked;
      updateEnv();
    });

    row.appendChild(input);
    row.appendChild(document.createTextNode("Enable"));
    fieldWrapper.appendChild(row);
    return;
  }

  if (field.type === "tags") {
    renderTags(fieldWrapper, field.key);
    return;
  }

  input = document.createElement("input");
  input.type = field.type;
  input.value = field.default || "";
  state[field.key] = input.value;

  input.addEventListener("input", () => {
    state[field.key] = input.value;
    updateEnv();
  });

  fieldWrapper.appendChild(input);
}


/* =============================
   VISIBILITY (FIXED)
============================= */

function updateVisibility() {
  SCHEMA.forEach(section => {
    section.fields.forEach(field => {
      if (!field.showIf) return;

      const wrapper = document.querySelector(`[data-key="${field.key}"]`);
      if (!wrapper) return;

      const shouldShow = state[field.showIf.key] === field.showIf.value;
      wrapper.style.display = shouldShow ? "block" : "none";
    });
  });
}


/* =============================
   TAG FIELD
============================= */

function renderTags(wrapper, key) {
  tagState[key] = [];

  const input = document.createElement("input");
  input.placeholder = "Type value and press Enter";

  const container = document.createElement("div");

  function commitValue() {
    const value = input.value.trim();
    if (!value) return;

    const parts = value.split(/[,\s]+/).filter(Boolean);

    parts.forEach(v => {
      if (!tagState[key].includes(v) && !isNaN(v)) {
        tagState[key].push(v);
      }
    });

    input.value = "";
    renderTagItems(container, key);
    updateEnv();
  }

  // Handle space and comma via input
  input.addEventListener("input", () => {
    if (/[,\s]/.test(input.value)) {
      commitValue();
    }
  });

  // Handle Enter (desktop + mobile)
  input.addEventListener("keydown", e => {
    if (e.key === "Enter") {
      e.preventDefault();
      commitValue();
    }
  });

  // Fallback when user taps away
  input.addEventListener("blur", commitValue);

  wrapper.appendChild(input);
  wrapper.appendChild(container);
}

function renderTagItems(container, key) {
  container.innerHTML = "";
  tagState[key].forEach((value, i) => {
    const tag = document.createElement("span");
    tag.className = "tag";
    tag.innerHTML = value + " <b style='cursor:pointer;'>×</b>";
    tag.querySelector("b").onclick = () => {
      tagState[key].splice(i,1);
      renderTagItems(container, key);
      updateEnv();
    };
    container.appendChild(tag);
  });
}


/* =============================
   MULTI TOKEN
============================= */

function renderMultiToken(wrapper) {
  const container = document.createElement("div");

  const button = document.createElement("button");
  button.textContent = "Add Token";
  button.onclick = () => {
    multiTokens.push("");
    renderMultiTokens(container);
    updateEnv();
  };

  wrapper.appendChild(container);
  wrapper.appendChild(button);
}

function renderMultiTokens(container) {
  container.innerHTML = "";

  multiTokens.forEach((value, i) => {

    const block = document.createElement("div");
    block.className = "token-block";

    const label = document.createElement("label");
    label.textContent = `MULTI_TOKEN${i+1}`;
    block.appendChild(label);

    const input = document.createElement("input");
    input.value = value;

    input.addEventListener("input", () => {
      multiTokens[i] = input.value;
      updateEnv();
    });

    const remove = document.createElement("button");
    remove.textContent = "Remove";
    remove.onclick = () => {
      multiTokens.splice(i,1);
      renderMultiTokens(container);
      updateEnv();
    };

    block.appendChild(input);
    block.appendChild(remove);
    container.appendChild(block);
  });
}


/* =============================
   ENV OUTPUT
============================= */

function updateEnv() {
  let lines = [];

  SCHEMA.forEach(section => {

    let groupLines = [];

    section.fields.forEach(field => {

      // MULTI_TOKEN handled separately
      if (field.type === "multiToken") {
        multiTokens.forEach((token, i) => {
          if (token) {
            groupLines.push(`MULTI_TOKEN${i+1}=${token}`);
          }
        });
        return;
      }

      // TAG fields
      if (field.type === "tags") {
        if (tagState[field.key] && tagState[field.key].length) {
          groupLines.push(
            `${field.key}=${tagState[field.key].join(",")}`
          );
        }
        return;
      }

      // Conditional fields (showIf)
      if (field.showIf) {
        const conditionMet =
          state[field.showIf.key] === field.showIf.value;
        if (!conditionMet) return;
      }

      const value = state[field.key];

      if (field.type === "boolean") {
        if (value === true) {
          groupLines.push(`${field.key}=true`);
        }
        return;
      }

      if (value !== undefined && value !== "") {
        groupLines.push(`${field.key}=${value}`);
      }
    });

    // Only print group if it has values
    if (groupLines.length > 0) {
      lines.push(`# ${section.group}`);
      lines.push(...groupLines);
      lines.push(""); // empty line between groups
    }

  });

  document.getElementById("ENV_OUTPUT").value = lines.join("\n").trim();
}

function copyEnv() {
  navigator.clipboard.writeText(
    document.getElementById("ENV_OUTPUT").value
  );
}

renderBuilder();

</script>