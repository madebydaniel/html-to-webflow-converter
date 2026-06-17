# Code to Webflow Converter

The Code to Webflow Converter is a standalone utility designed to bridge the gap between AI-generated (or raw) web code and Webflow's proprietary visual canvas. It allows you to skip the tedious process of manually recreating nodes and styles by translating standard web code directly into Webflow's clipboard format.

---

### What It Can Do

- **Puts Code Right into the Designer:** You can paste in raw HTML, and the tool will instantly translate it so you can paste it directly onto the Webflow Designer canvas. It pulls standard styles straight into the Webflow GUI, allowing you to click and visually edit them immediately.
- **Handles Complex CSS:** Whatever Webflow's visual GUI can't natively accept on-paste (like complex nested selectors, hover states, or pseudo-classes like `:nth-child`), the tool smartly parses out into a clean, separate CSS block. You can simply copy and drop this right into your Webflow Page Settings.
- **Preserves Your Structure:** It ensures your `id`s and custom `data-attributes` are perfectly moved over to the Webflow canvas, ensuring your scripts, interactions, or third-party integrations (like Relume or Finsweet) continue to work seamlessly.
- **Smart Class Handling:** Built for component-based workflows, it uses deterministic class hashing. This means that if you paste the same generated component across 10 different pages over the course of a week, Webflow won't clutter your project by auto-creating duplicate classes like `.faq-wrap-2` or `.faq-wrap-3`.
- **Isolates JavaScript:** It automatically strips out any `<script>` tags from your raw HTML and packages them up nicely so you can quickly paste them into Webflow's 'Before `</body>` tag' section.

---

### How to Use It

1. **Input:** Paste your raw HTML structure into the **Paste Raw HTML** box.
2. **Convert:** Click **Convert to Webflow**. The tool will instantly parse the DOM and generate the Webflow payload.
3. **Copy & Paste Structure:** Click **Copy Converted HTML**, then navigate directly to your Webflow designer canvas, select a container, and press `Cmd+V` / `Ctrl+V`. The structure will appear instantly.
4. **Copy & Paste CSS/JS:** If your input contained complex styles or scripts, copy them using the respective output buttons and paste them into Webflow's Page Settings (inside the `<head>` tag for CSS, and before the `</body>` tag for JS).

---

### Diagnostic Tools

Because Webflow's clipboard format is proprietary and constantly evolving, this tool includes a suite of diagnostics to help you troubleshoot when things don't paste as expected.

#### The "Test Native Snippet" Button
**What it is:** This button instantly copies a tiny, statically-typed, perfectly formatted piece of native Webflow clipboard JSON to your clipboard.
**When to use it:** Use this if you hit a wall where nothing happens when you press `Cmd+V` in Webflow. 
- If the native snippet pastes into Webflow but your converted HTML does not, the issue lies within our parsing logic. 
- If the native snippet also fails to paste, the issue is external (e.g., your browser is blocking clipboard writes, or Webflow is experiencing a platform bug).

#### The "Bug Report Generator"
**What it is:** A streamlined diagnostic packaging tool. When you fill out its fields and click generate, it builds a massive text document containing your raw input, our generated JSON, a "control" JSON object you copied natively from Webflow, and the exact console errors.
**When to use it:** Use this whenever a paste fails or crashes Webflow. 
1. **Paste Webflow's Version:** Go into Webflow, build a tiny, working version of the element that failed, copy it (`Cmd+C`), and paste it into this box. The tool intercepts the clipboard and grabs Webflow's raw JSON so we can see *exactly* how Webflow expects it to look.
2. **Paste Console Errors:** Open your browser's inspect tool/developer console inside the Webflow designer tab. Find the red error stack trace that fired when your paste failed, and paste it into this box.
3. **Generate:** Click **Generate & Copy Bug Report**. You can then provide this exact block to an AI Agent or developer to instantly patch the parser logic.
