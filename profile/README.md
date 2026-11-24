# 🧩 Web Widgets Community

**Web Widgets Community** is an open-source, community-driven collection of widgets for the **Kurug** desktop widget engine. Our goal is to make it easy to build, share, and discover useful, creative, lightweight widgets that personalize your desktop.

Widgets use familiar web technologies: HTML, CSS, and JavaScript. A widget can be a self-contained local project, a Git repository, or a web page added through Kurug.

## 🌟 What You Can Do Here

- Find ready-to-use widgets that work with Kurug.
- Explore widget source code and learn how it is built.
- Publish your own widget and share it with the community.
- Improve existing widgets by opening issues or pull requests.

## 🚀 Install a Widget

1. Choose a widget repository from this community.
2. In Kurug, select **Add Widget** and provide the repository’s Git URL, a local folder, or a website URL.
3. Enable the widget from your Kurug widget list.
4. Open that widget’s settings to adjust its size, position, opacity, behavior, and configuration values.

For a local widget, Kurug reads `index.html` from the selected folder. Git widgets are cloned into Kurug’s widget directory. Each widget gets its own `config.json`; for local and Git widgets it is kept in the widget’s folder. URL widgets keep their config in Kurug’s own `widgets/configs` directory.

## 🛠️ Create a Widget

The simplest widget is a folder containing an `index.html` file. Add a `config.json` file beside it when you want to provide default CSS, JavaScript, environment, or widget values.

```text
my-clock/
├── index.html
└── config.json
```

### 1. Create `index.html`

This example displays a live clock. The page owns its layout and behavior, so it works as a regular local HTML page as well as a Kurug widget.

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>My Clock</title>
    <style>
      * { box-sizing: border-box; }
      html, body { margin: 0; min-height: 100%; }
      body {
        display: grid;
        place-items: center;
        min-height: 100vh;
        padding: 16px;
        color: #f8fafc;
        background: transparent;
        font-family: system-ui, sans-serif;
      }
      #widget { text-align: center; }
      #clock { font-size: 42px; font-weight: 700; font-variant-numeric: tabular-nums; }
      #label { margin-top: 6px; color: #a5b4fc; font-size: 13px; }
    </style>
  </head>
  <body>
    <main id="widget">
      <div id="clock" aria-live="off">--:--:--</div>
      <div id="label">Local time</div>
    </main>
    <script>
      const clock = document.getElementById('clock');
      const updateClock = () => {
        clock.textContent = new Intl.DateTimeFormat(undefined, {
          hour: '2-digit', minute: '2-digit', second: '2-digit'
        }).format(new Date());
      };
      updateClock();
      setInterval(updateClock, 1000);
    </script>
  </body>
</html>
```

### 2. Add `config.json`

Configuration is a JSON object with four optional sections. Each section is a set of **key/value pairs**, which can be edited in the widget’s Kurug settings page. Empty objects are valid defaults.

```json
{
  "css": {
    "color": "#f8fafc",
    "fontSize": "42px",
    "controlBackground": "rgba(15, 23, 42, 0.35)"
  },
  "js": {
    "clockFormat": "24-hour"
  },
  "env": {
    "API_BASE_URL": "https://example.com/api"
  },
  "widget": {
    "title": "My Clock",
    "description": "A simple desktop clock"
  }
}
```

#### Configuration sections

- **`css`**: CSS property/value overrides for the widget page. Standard style properties such as `color` and `fontSize` apply to the widget root (`#widget`, or the page body if there is no `#widget`). The supported theme keys `controlBackground`, `selectBackground`, `dateBackground`, and `controlBorderColor` set the matching CSS variables `--control-background`, `--select-background`, `--date-background`, and `--control-border`. A selector can also be used as a key with a CSS declaration string, for example `"#clock": "letter-spacing: 0.04em;"`.
- **`js`**: Makes each configured value available as a property on `window`. For example, `"clockFormat": "24-hour"` is available as `window.clockFormat`.
- **`env`**: Makes values available on `window.widgetEnv`. For example, `"API_BASE_URL": "https://example.com/api"` is available as `window.widgetEnv.API_BASE_URL`.
- **`widget`**: Updates a matching element’s text using its ID. For example, `"clock": "Ready"` sets the text of `<div id="clock">`. The special `title` key updates the page title.

Configuration values are strings. Parse values such as numbers or booleans in your widget code when needed. For example: `const refreshSeconds = Number(window.widgetEnv.REFRESH_SECONDS || 60);`.

> **Keep secrets out of widget configs.** `config.json` values are stored as readable text and may be visible to widget code. Do not put passwords, private tokens, or other secrets in `env` or any other section. Use public settings only.

### 3. Preview and share

Open the folder as a local widget in Kurug and enable it. Check the widget at its intended size and verify that its controls, scrolling, and layout work. Settings are saved automatically, and Kurug restarts an active widget so the changes take effect. Include both `index.html` and `config.json` in your repository, along with a README describing the widget, setup, and any network access or dependencies. Screenshots and a license are welcome.

## 📐 Widget Tips

- Keep the page self-contained when possible; avoid unnecessary frameworks and large dependencies.
- Use a transparent page background if you want the desktop wallpaper to show through.
- Set page-level scrolling in your own HTML/CSS. For example, `body { overflow-y: auto; }` enables vertical page scrolling. Kurug’s window-level scroll option also needs to be enabled for scroll input to pass through.
- Style form controls in the widget’s own stylesheet. A URL widget is embedded from another origin, so browser security prevents Kurug from styling the remote page’s internal controls.
- Use relative paths for local images, stylesheets, and scripts. Kurug preserves the widget folder as the base path for local and Git widgets.
- Keep the widget usable at different window sizes and avoid relying on a particular screen resolution.

## 📦 Recommended Repository Layout

Use one repository per widget and name it `widget-<name>` when practical, such as `widget-weather` or `widget-system-monitor`.

```text
widget-example/
├── README.md       # What it does, setup, configuration, screenshots
├── index.html      # Widget entry page
├── config.json     # Optional default key/value settings
├── assets/         # Optional images, fonts, and other local assets
└── LICENSE         # Recommended open-source license
```

Keep configuration valid JSON: use double quotes around keys and string values, and do not add comments or trailing commas.

## 🤝 Contribute a Widget

1. Create a widget repository in your personal GitHub account.
2. Add a clear `README.md` with its purpose, installation steps, configuration keys, dependencies, and any permissions or network use.
3. Include the widget’s source files and a valid `config.json` if it needs default settings. Add screenshots if available.
4. Open an issue in the [Kurug repository](https://github.com/osmn-byhn/kurug/issues) to ask about contributing it to this community, or contact the maintainers.
5. Once the repository is part of the community, submit improvements as pull requests and respond to feedback.

## 🆘 Help and Support

- Ask questions or suggest community-wide improvements in the organization’s `community` repository.
- Report widget-specific bugs in that widget’s repository.
- Open a [Kurug issue](https://github.com/osmn-byhn/kurug/issues) for problems with the desktop engine.
- Pull requests that improve widgets or documentation are welcome.

## 📄 License

Each widget repository should include its own license so people know how they may use and modify that widget. Unless a repository says otherwise, follow the license included with that repository.

---

*Keep coding, reinvent your desktop!* 💻✨
