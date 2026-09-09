<!DOCTYPE html>
<html lang="zh-Hant">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>一對一發紅包功能 — 產品規格書與原型</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mermaid/10.9.1/mermaid.min.js"></script>
  <style>
    :root {
      --brand-red: #c42e2e;
      --brand-red-dark: #991b1b;
      --brand-red-light: #fef2f2;
      --brand-red-border: #fecaca;
      --brand-gold: #d97706;
      --brand-gold-light: #fffbeb;
      --brand-gold-border: #fde68a;
      --brand-gold-dark: #92400e;

      --ink-heading: #0f172a;
      --ink-body: #334155;
      --ink-muted: #64748b;
      --ink-subtle: #94a3b8;

      --bg-canvas: #f8fafc;
      --bg-card: #ffffff;
      --line: #e2e8f0;
      --line-subtle: #f1f5f9;
      --code-bg: #f8fafc;

      --accent-blue: #2563eb;
      --accent-blue-light: #eff6ff;
      --accent-blue-border: #bfdbfe;
      --accent-emerald: #059669;
      --accent-emerald-light: #ecfdf5;
      --accent-emerald-border: #a7f3d0;

      --sidebar-width: 280px;
      --topbar-height: 60px;
      --radius-sm: 6px;
      --radius-md: 10px;
      --radius-lg: 16px;

      --shadow-xs: 0 1px 2px rgba(0, 0, 0, 0.04);
      --shadow-sm: 0 2px 6px rgba(0, 0, 0, 0.04), 0 1px 2px rgba(0, 0, 0, 0.02);
      --shadow-md: 0 6px 20px -4px rgba(15, 23, 42, 0.06), 0 2px 6px -1px rgba(15, 23, 42, 0.03);
      --shadow-lg: 0 16px 36px -8px rgba(15, 23, 42, 0.08), 0 4px 12px -2px rgba(15, 23, 42, 0.03);
    }

    * {
      box-sizing: border-box;
    }

    html {
      scroll-behavior: smooth;
      font-size: 15px;
    }

    body {
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "PingFang TC", "Microsoft JhengHei", "Noto Sans TC", sans-serif;
      background: var(--bg-canvas);
      color: var(--ink-body);
      line-height: 1.8;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
    }

    /* Top Sticky Bar & Progress */
    .topbar {
      position: sticky;
      top: 0;
      z-index: 1000;
      height: var(--topbar-height);
      background: rgba(255, 255, 255, 0.94);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      border-bottom: 1px solid var(--line);
      box-shadow: var(--shadow-xs);
    }

    .topbar-content {
      max-width: 1440px;
      height: 100%;
      margin: 0 auto;
      padding: 0 28px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 16px;
    }

    .topbar-left {
      display: flex;
      align-items: center;
      gap: 12px;
      min-width: 0;
    }

    .doc-icon {
      width: 32px;
      height: 32px;
      border-radius: 8px;
      background: linear-gradient(135deg, #e02424, #991b1b);
      color: #fff;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 16px;
      box-shadow: 0 2px 8px rgba(196, 46, 46, 0.3);
      flex-shrink: 0;
    }

    .topbar-title {
      font-size: 15px;
      font-weight: 700;
      color: var(--ink-heading);
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      margin: 0;
    }

    .topbar-tags {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .topbar-right {
      display: flex;
      align-items: center;
      gap: 10px;
      flex-shrink: 0;
    }

    .nav-pill {
      display: inline-flex;
      align-items: center;
      gap: 5px;
      padding: 5px 12px;
      border-radius: 999px;
      font-size: 12.5px;
      font-weight: 500;
      text-decoration: none;
      color: var(--ink-body);
      background: #f1f5f9;
      border: 1px solid var(--line);
      transition: all 0.15s ease;
    }

    .nav-pill:hover {
      background: #e2e8f0;
      color: var(--ink-heading);
    }

    .nav-pill.primary {
      background: var(--brand-red-light);
      color: var(--brand-red);
      border-color: var(--brand-red-border);
      font-weight: 600;
    }

    .nav-pill.primary:hover {
      background: #fee2e2;
    }

    .scroll-progress-container {
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 3px;
      background: rgba(226, 232, 240, 0.6);
    }

    .scroll-progress-bar {
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, var(--brand-red), #f59e0b);
      transition: width 0.08s ease-out;
    }

    /* Main Layout Framework */
    .doc-layout {
      max-width: 1440px;
      margin: 0 auto;
      display: flex;
      gap: 36px;
      padding: 28px 28px 100px;
    }

    /* Sidebar Navigation (Sticky) */
    .doc-sidebar {
      width: var(--sidebar-width);
      flex-shrink: 0;
    }

    .sidebar-inner {
      position: sticky;
      top: calc(var(--topbar-height) + 24px);
      max-height: calc(100vh - var(--topbar-height) - 48px);
      overflow-y: auto;
      padding: 16px 14px;
      background: var(--bg-card);
      border-radius: var(--radius-md);
      border: 1px solid var(--line);
      box-shadow: var(--shadow-sm);
    }

    .sidebar-inner::-webkit-scrollbar {
      width: 5px;
    }

    .sidebar-inner::-webkit-scrollbar-thumb {
      background: #cbd5e1;
      border-radius: 4px;
    }

    .sidebar-header {
      font-size: 11.5px;
      font-weight: 700;
      letter-spacing: 0.08em;
      text-transform: uppercase;
      color: var(--ink-muted);
      padding: 4px 8px 10px;
      border-bottom: 1px solid var(--line);
      margin-bottom: 8px;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    .toc-menu {
      list-style: none;
      padding: 0;
      margin: 0;
    }

    .toc-item {
      margin: 2px 0;
    }

    .toc-link {
      display: flex;
      align-items: center;
      gap: 8px;
      padding: 7px 10px;
      border-radius: var(--radius-sm);
      color: var(--ink-body);
      text-decoration: none;
      font-size: 13px;
      font-weight: 500;
      transition: all 0.15s ease;
      line-height: 1.4;
    }

    .toc-link:hover {
      background: #f1f5f9;
      color: var(--ink-heading);
    }

    .toc-link.active {
      background: var(--brand-red-light);
      color: var(--brand-red);
      font-weight: 600;
    }

    .toc-num {
      font-size: 11px;
      font-weight: 700;
      color: var(--ink-muted);
      min-width: 18px;
    }

    .toc-link.active .toc-num {
      color: var(--brand-red);
    }

    .toc-sub {
      list-style: none;
      padding-left: 20px;
      margin: 2px 0 4px;
      border-left: 1.5px solid #f1f5f9;
      margin-left: 14px;
    }

    .toc-sub .toc-link {
      padding: 4px 8px;
      font-size: 12.5px;
      color: var(--ink-muted);
      font-weight: 400;
    }

    .toc-sub .toc-link:hover {
      color: var(--ink-heading);
    }

    .toc-sub .toc-link.active {
      color: var(--brand-red);
      font-weight: 600;
      background: transparent;
    }

    .sidebar-actions {
      margin-top: 14px;
      padding-top: 12px;
      border-top: 1px solid var(--line);
      display: flex;
      flex-direction: column;
      gap: 6px;
    }

    .sidebar-btn {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 6px;
      width: 100%;
      padding: 6px 12px;
      background: #f8fafc;
      border: 1px solid var(--line);
      border-radius: var(--radius-sm);
      font-size: 12px;
      color: var(--ink-body);
      text-decoration: none;
      font-weight: 500;
      cursor: pointer;
      transition: all 0.15s ease;
    }

    .sidebar-btn:hover {
      background: #f1f5f9;
      color: var(--ink-heading);
      border-color: #cbd5e1;
    }

    /* Main Content Area */
    .doc-main {
      flex: 1;
      min-width: 0;
      max-width: 980px;
    }

    .content-canvas {
      background: var(--bg-card);
      border-radius: var(--radius-lg);
      border: 1px solid var(--line);
      box-shadow: var(--shadow-md);
      padding: 48px 56px 90px;
    }

    @media (max-width: 1200px) {
      .doc-layout {
        padding: 20px 20px 80px;
        gap: 24px;
      }

      .content-canvas {
        padding: 36px 40px 70px;
      }
    }

    @media (max-width: 992px) {
      .doc-sidebar {
        display: none;
      }

      .doc-layout {
        padding: 16px 12px 60px;
      }

      .content-canvas {
        padding: 28px 20px 60px;
        border-radius: var(--radius-md);
      }

      .topbar-right .nav-pill:not(.primary) {
        display: none;
      }
    }

    /* Typography & Document Headers */
    .doc-header {
      margin-bottom: 28px;
      padding-bottom: 22px;
      border-bottom: 1px solid var(--line);
    }

    h1.doc-main-title {
      font-size: 28px;
      font-weight: 800;
      color: var(--ink-heading);
      letter-spacing: -0.03em;
      margin: 0 0 16px;
      line-height: 1.35;
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .doc-meta-row {
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      gap: 10px;
    }

    .meta-tag {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      background: #f8fafc;
      color: var(--ink-body);
      padding: 4px 12px;
      border-radius: 999px;
      font-size: 12.5px;
      font-weight: 500;
      border: 1px solid var(--line);
    }

    .meta-tag .label {
      color: var(--ink-subtle);
      font-weight: 400;
    }

    .meta-tag.status-badge {
      background: var(--brand-gold-light);
      color: var(--brand-gold-dark);
      border-color: var(--brand-gold-border);
      font-weight: 600;
    }

    .meta-tag.status-badge::before {
      content: "";
      display: inline-block;
      width: 7px;
      height: 7px;
      border-radius: 50%;
      background: #f59e0b;
    }

    /* Section Headings */
    h2 {
      font-size: 22px;
      font-weight: 700;
      color: var(--ink-heading);
      margin-top: 52px;
      margin-bottom: 18px;
      padding-top: 24px;
      padding-bottom: 10px;
      border-bottom: 2px solid var(--line-subtle);
      display: flex;
      align-items: center;
      gap: 12px;
      scroll-margin-top: 80px;
    }

    h2:first-of-type {
      margin-top: 20px;
      padding-top: 0;
    }

    .chapter-badge {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      min-width: 28px;
      height: 28px;
      border-radius: 7px;
      background: var(--brand-red-light);
      color: var(--brand-red);
      font-size: 13px;
      font-weight: 700;
      border: 1px solid var(--brand-red-border);
      letter-spacing: -0.02em;
    }

    h3 {
      font-size: 17px;
      font-weight: 700;
      margin-top: 36px;
      margin-bottom: 14px;
      color: var(--ink-heading);
      display: flex;
      align-items: center;
      gap: 10px;
      scroll-margin-top: 80px;
    }

    .sub-badge {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      padding: 2px 8px;
      border-radius: 6px;
      background: #eff6ff;
      color: var(--accent-blue);
      font-size: 12px;
      font-weight: 700;
      border: 1px solid var(--accent-blue-border);
    }

    h4 {
      font-size: 15px;
      font-weight: 600;
      margin-top: 28px;
      margin-bottom: 12px;
      color: var(--ink-heading);
      scroll-margin-top: 80px;
    }

    p {
      margin: 14px 0;
      font-size: 15px;
      color: var(--ink-body);
      line-height: 1.85;
    }

    strong {
      color: var(--ink-heading);
      font-weight: 600;
    }

    ul,
    ol {
      font-size: 14.5px;
      padding-left: 24px;
      margin: 12px 0 18px;
      color: var(--ink-body);
    }

    li {
      margin: 8px 0;
      line-height: 1.75;
    }

    ul li::marker {
      color: var(--brand-red);
    }

    ol li::marker {
      font-weight: 700;
      color: var(--ink-muted);
    }

    /* Scope & Alert Callout Boxes */
    .callout {
      margin: 22px 0 28px;
      padding: 18px 22px;
      border-radius: var(--radius-md);
      font-size: 14px;
      line-height: 1.8;
      border: 1px solid transparent;
    }

    .callout-title {
      font-size: 14.5px;
      font-weight: 700;
      margin-bottom: 8px;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .callout-scope {
      background: #fff8f8;
      border-color: #fee2e2;
      border-left: 5px solid var(--brand-red);
      color: #7f1d1d;
    }

    .callout-scope .callout-title {
      color: var(--brand-red-dark);
    }

    .callout-scope p {
      color: #991b1b;
      margin: 0;
    }

    .callout-info {
      background: var(--accent-blue-light);
      border-color: var(--accent-blue-border);
      border-left: 5px solid var(--accent-blue);
      color: #1e3a8a;
    }

    .callout-info .callout-title {
      color: #1d4ed8;
    }

    .callout-info p {
      color: #1e40af;
      margin: 0;
    }

    .callout-tech {
      background: #f8fafc;
      border-color: var(--line);
      border-left: 5px solid #475569;
      color: #334155;
    }

    .callout-tech .callout-title {
      color: #0f172a;
    }

    .callout-tech p {
      margin: 4px 0 0;
      color: #334155;
    }

    blockquote {
      margin: 18px 0 22px;
      padding: 14px 20px;
      background: #f8fafc;
      border-left: 4px solid #94a3b8;
      border-radius: 0 var(--radius-sm) var(--radius-sm) 0;
      color: var(--ink-body);
      font-size: 14px;
      line-height: 1.75;
      border-top: 1px solid var(--line-subtle);
      border-right: 1px solid var(--line-subtle);
      border-bottom: 1px solid var(--line-subtle);
    }

    blockquote p {
      margin: 4px 0;
      font-size: 14px;
    }

    /* Code Snippets */
    code {
      background: var(--code-bg);
      padding: 2.5px 7px;
      border-radius: 5px;
      font-size: 13px;
      color: #0f172a;
      font-family: "SFMono-Regular", Consolas, "Liberation Mono", Menlo, Courier, monospace;
      border: 1px solid var(--line);
    }

    /* Clean Modern Tables */
    .table-responsive {
      overflow-x: auto;
      margin: 18px 0 28px;
      border-radius: var(--radius-md);
      border: 1px solid var(--line);
      background: #fff;
      box-shadow: var(--shadow-xs);
    }

    table {
      width: 100%;
      border-collapse: collapse;
      font-size: 14px;
      line-height: 1.65;
    }

    th,
    td {
      padding: 12px 18px;
      text-align: left;
      vertical-align: top;
      border-bottom: 1px solid var(--line-subtle);
      border-right: 1px solid var(--line-subtle);
    }

    th:last-child,
    td:last-child {
      border-right: none;
    }

    tr:last-child td {
      border-bottom: none;
    }

    th {
      background: #f8fafc;
      color: var(--ink-heading);
      font-weight: 600;
      font-size: 13px;
      letter-spacing: 0.02em;
      border-bottom: 1px solid var(--line);
      white-space: nowrap;
    }

    tr:nth-child(even) td {
      background: #fafbfd;
    }

    tr:hover td {
      background: #f1f5f9;
    }

    td:first-child {
      font-weight: 600;
      color: var(--ink-heading);
    }

    .badge-type {
      display: inline-block;
      padding: 2px 7px;
      border-radius: 4px;
      font-size: 11.5px;
      font-family: monospace;
      background: #f1f5f9;
      color: #475569;
      border: 1px solid #e2e8f0;
    }

    /* Screen Specification Cards */
    .screen-card {
      background: #fff;
      border-radius: var(--radius-md);
      border: 1px solid var(--line);
      box-shadow: var(--shadow-sm);
      margin: 28px 0 36px;
      overflow: hidden;
      transition: box-shadow 0.2s ease, border-color 0.2s ease;
    }

    .screen-card:hover {
      box-shadow: var(--shadow-md);
      border-color: #cbd5e1;
    }

    .screen-card-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 12px;
      padding: 14px 20px;
      background: #f8fafc;
      border-bottom: 1px solid var(--line);
    }

    .screen-card-title-group {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .screen-number {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      padding: 3px 10px;
      border-radius: 6px;
      background: var(--brand-red-light);
      color: var(--brand-red);
      font-size: 12px;
      font-weight: 700;
      border: 1px solid var(--brand-red-border);
    }

    .screen-card-title {
      margin: 0;
      font-size: 16px;
      font-weight: 700;
      color: var(--ink-heading);
    }

    .screen-tags {
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .tag-role {
      padding: 3px 9px;
      border-radius: 999px;
      font-size: 11.5px;
      font-weight: 500;
    }

    .tag-role.sender {
      background: #eff6ff;
      color: #1d4ed8;
      border: 1px solid #bfdbfe;
    }

    .tag-role.receiver {
      background: #ecfdf5;
      color: #047857;
      border: 1px solid #a7f3d0;
    }

    .tag-role.both {
      background: #f5f3ff;
      color: #6d28d9;
      border: 1px solid #ddd6fe;
    }

    .tag-role.exception {
      background: #fffbeb;
      color: #b45309;
      border: 1px solid #fde68a;
    }

    .screen-card-body {
      padding: 20px 22px;
    }

    .screen-card-body .table-responsive {
      margin: 0 0 16px;
    }

    .screen-notes-box {
      background: #f8fafc;
      border-radius: var(--radius-sm);
      border: 1px solid var(--line);
      padding: 14px 18px;
      margin-top: 14px;
    }

    .screen-notes-title {
      font-size: 13px;
      font-weight: 700;
      color: var(--ink-heading);
      margin-bottom: 8px;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .screen-notes-box ul {
      margin: 4px 0;
      padding-left: 20px;
    }

    .screen-notes-box li {
      font-size: 13.5px;
      margin: 4px 0;
      line-height: 1.7;
    }

    /* Flowchart / Mermaid Container */
    .flow-card {
      background: #fff;
      border-radius: var(--radius-md);
      border: 1px solid var(--line);
      box-shadow: var(--shadow-sm);
      padding: 20px;
      margin: 20px 0 28px;
    }

    .flow-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 14px;
      padding-bottom: 10px;
      border-bottom: 1px solid var(--line-subtle);
    }

    .flow-title {
      font-size: 14px;
      font-weight: 700;
      color: var(--ink-heading);
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .flow-badge {
      font-size: 11.5px;
      padding: 2px 8px;
      border-radius: 4px;
      background: #f1f5f9;
      color: var(--ink-muted);
      border: 1px solid #e2e8f0;
    }

    .mermaid {
      text-align: center;
      padding: 10px 0;
    }

    /* Interactive Prototype Showcase */
    .proto-showcase {
      background: #1e293b;
      border-radius: var(--radius-lg);
      border: 1px solid #334155;
      box-shadow: var(--shadow-lg);
      margin: 28px 0 36px;
      overflow: hidden;
    }

    .proto-showcase-bar {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 12px 20px;
      background: #0f172a;
      border-bottom: 1px solid #334155;
    }

    .proto-mac-dots {
      display: flex;
      align-items: center;
      gap: 7px;
    }

    .proto-dot {
      width: 11px;
      height: 11px;
      border-radius: 50%;
    }

    .proto-dot.red {
      background: #ef4444;
    }

    .proto-dot.yellow {
      background: #f59e0b;
    }

    .proto-dot.green {
      background: #10b981;
    }

    .proto-bar-title {
      color: #e2e8f0;
      font-size: 13px;
      font-weight: 600;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .proto-bar-actions {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .proto-action-btn {
      background: #334155;
      color: #cbd5e1;
      border: none;
      padding: 4px 10px;
      border-radius: 5px;
      font-size: 12px;
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      gap: 4px;
      transition: all 0.15s ease;
    }

    .proto-action-btn:hover {
      background: #475569;
      color: #ffffff;
    }

    .proto-frame-wrapper {
      padding: 16px;
      background: #0f172a;
    }

    .proto-frame {
      width: 100%;
      height: 780px;
      border: none;
      border-radius: var(--radius-md);
      display: block;
      background: #f4f4f6;
    }

    .proto-hint-box {
      padding: 12px 20px;
      background: #1e293b;
      color: #94a3b8;
      font-size: 13px;
      border-top: 1px solid #334155;
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 8px;
    }

    /* Floating Back to Top Button */
    .back-to-top {
      position: fixed;
      bottom: 28px;
      right: 28px;
      background: #ffffff;
      color: var(--ink-body);
      border: 1px solid var(--line);
      box-shadow: var(--shadow-md);
      border-radius: 50%;
      width: 44px;
      height: 44px;
      display: flex;
      align-items: center;
      justify-content: center;
      text-decoration: none;
      font-size: 18px;
      transition: all 0.2s ease;
      z-index: 900;
      opacity: 0.9;
    }

    .back-to-top:hover {
      background: var(--brand-red);
      color: #ffffff;
      border-color: var(--brand-red);
      transform: translateY(-3px);
      box-shadow: 0 8px 20px rgba(196, 46, 46, 0.25);
      opacity: 1;
    }

    /* Print Styles */
    @media print {

      .topbar,
      .doc-sidebar,
      .back-to-top,
      .proto-bar-actions {
        display: none !important;
      }

      .doc-layout {
        padding: 0;
      }

      .content-canvas {
        border: none;
        box-shadow: none;
        padding: 0;
      }

      body {
        background: #fff;
      }
    }
  </style>
</head>

<body>

  <!-- Top Sticky Navigation Bar -->
  <header class="topbar">
    <div class="topbar-content">
      <div class="topbar-left">
        <div class="doc-icon">🧧</div>
        <h2 class="topbar-title" style="margin:0;padding:0;border:none;font-size:15px;">一對一發紅包功能 — 產品規格書</h2>
        <div class="topbar-tags">
        </div>
      </div>
      <div class="topbar-right">
        <a href="#2" class="nav-pill primary">📱 互動原型</a>
        <a href="#32" class="nav-pill">📋 畫面規格</a>
        <a href="#34-rd" class="nav-pill">⚙️ 系統設計</a>
        <a href="#4" class="nav-pill">📊 數據驗證</a>
      </div>
    </div>
    <div class="scroll-progress-container">
      <div class="scroll-progress-bar" id="progressBar"></div>
    </div>
  </header>

  <!-- Document Framework -->
  <div class="doc-layout">

    <!-- Sticky Sidebar Table of Contents -->
    <aside class="doc-sidebar">
      <nav class="sidebar-inner" aria-label="章節目錄">
        <div class="sidebar-header">
          <span>文件目錄 Table of Contents</span>
        </div>
        <ul class="toc-menu">
          <li class="toc-item">
            <a href="#1" class="toc-link"><span class="toc-num">01</span> 功能目的</a>
          </li>
          <li class="toc-item">
            <a href="#2" class="toc-link"><span class="toc-num">02</span> 原型圖與互動預覽</a>
          </li>
          <li class="toc-item">
            <a href="#3" class="toc-link"><span class="toc-num">03</span> 功能規格說明</a>
            <ul class="toc-sub">
              <li><a href="#31" class="toc-link">3.1 使用者流程總覽</a></li>
              <li><a href="#32" class="toc-link">3.2 頁面元件規格 (①～⑨)</a></li>
              <li><a href="#33" class="toc-link">3.3 業務規則</a></li>
              <li><a href="#34-rd" class="toc-link">3.4 系統流程與資料設計</a></li>
              <li><a href="#35-edge-cases" class="toc-link">3.5 例外情境 (Edge Cases)</a></li>
              <li><a href="#36_1" class="toc-link">3.6 風控與安全考量</a></li>
              <li><a href="#37" class="toc-link">3.7 資料結構延伸設計</a></li>
            </ul>
          </li>
          <li class="toc-item">
            <a href="#4" class="toc-link"><span class="toc-num">04</span> 數據追蹤與成效驗證</a>
            <ul class="toc-sub">
              <li><a href="#41" class="toc-link">4.1 市場與需求分析</a></li>
              <li><a href="#42" class="toc-link">4.2 埋點規格與數據追蹤</a></li>
              <li><a href="#43-ab" class="toc-link">4.3 成效驗證 (A/B 測試)</a></li>
            </ul>
          </li>
          <li class="toc-item">
            <a href="#5" class="toc-link"><span class="toc-num">05</span> 前提假設</a>
          </li>
        </ul>
        <div class="sidebar-actions">
          <a href="#top" class="sidebar-btn">⬆ 回到頂部</a>
          <a href="#2" class="sidebar-btn" style="color:var(--brand-red);font-weight:600;">📱 快速跳至原型</a>
        </div>
      </nav>
    </aside>

    <!-- Main Content Reader -->
    <main class="doc-main">
      <article class="content-canvas" id="top">

        <!-- Document Header -->
        <header class="doc-header">
          <h1 class="doc-main-title">
            <span>一對一發紅包功能 — 產品規格書</span>
          </h1>
          <div class="doc-meta-row">
          </div>
        </header>

        <!-- Scope Callout Banner -->
        <!-- <div class="callout callout-scope">
          <div class="callout-title">📌 本次範圍聲明</div>
          <p>本規格書明確聚焦於<strong>一對一場景</strong>。群紅包涉及「份數設定」「隨機金額演算法」「搶紅包併發控制」等額外複雜度，暫列為 Phase
            2 候選，本次僅在資料結構與架構上預留擴充空間（詳見 3.7、5. 待確認事項）。</p>
        </div> -->

        <!-- Section 1: Purpose -->
        <section id="1">
          <h2><span class="chapter-badge">01</span> 功能目的</h2>
          <p><strong>背景</strong>：目前聊天室底部工具列已有「照片／拍照／語音／紅包」四個入口，但「紅包」點擊後尚無對應流程。</p>
          <p><strong>目的</strong>：</p>
          <ol>
            <li>補齊聊天室內金流社交互動缺口，提供比單純「轉帳」更具儀式感與人情味的資金互動工具。</li>
            <li>增加聊天室活躍度與趣味性，刺激好友間的日常互動頻率（如生日、節慶、致謝、打賭等情境）。</li>
            <li>驗證用戶在一對一私聊中以「紅包」形式互動的意願，作為後續是否投入「群組紅包（拼手氣）」的決策依據。</li>
          </ol>
          <p><strong>目標</strong>：</p>
          <ol>
            <li>一對一聊天中「發紅包」滲透率提升，帶動聊天室 7 日留存與人均聊天時長成長（詳見第 4.3 節驗證邏輯）。</li>

          </ol>
          <div class="callout callout-info">
            <div class="callout-title">💡 邊界說明</div>
            <p>本次僅規劃一對一、單一預設樣式、錢包餘額單一支付來源的最小可行版本。</p>
          </div>
        </section>

        <!-- Section 2: Prototype -->
        <section id="2">
          <h2><span class="chapter-badge">02</span> 原型圖</h2>
          <p>已提供互動式 HTML 線框稿，涵蓋以下 <strong>9 個畫面／狀態</strong>，點擊下方原型頂部的分頁按鈕即可即時切換操作：</p>

          <div class="table-responsive">
            <table>
              <thead>
                <tr>
                  <th style="width:50px;">#</th>
                  <th style="width:220px;">畫面名稱</th>
                  <th>規格重點與場景說明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>①</td>
                  <td>發紅包-輸入頁</td>
                  <td>點擊聊天室「紅包」按鈕後的第一個畫面，輸入金額與祝福語</td>
                </tr>
                <tr>
                  <td>②</td>
                  <td>支付密碼確認</td>
                  <td>二次驗證，防止誤觸／被盜用</td>
                </tr>
                <tr>
                  <td>③</td>
                  <td>聊天室-未拆狀態</td>
                  <td>紅包以訊息氣泡形式出現在聊天室，尚未被領取</td>
                </tr>
                <tr>
                  <td>④</td>
                  <td>拆紅包動畫（封口）</td>
                  <td>對方點擊氣泡後進入的全螢幕拆紅包頁，未拆前為封口狀態</td>
                </tr>
                <tr>
                  <td>⑤</td>
                  <td>拆紅包結果</td>
                  <td>點擊封口後顯示金額與「收下了」按鈕</td>
                </tr>
                <tr>
                  <td>⑥</td>
                  <td>聊天室-已拆狀態</td>
                  <td>氣泡由紅轉金，顯示「XX 已領取」</td>
                </tr>
                <tr>
                  <td>⑦</td>
                  <td>發送方-領取詳情</td>
                  <td>發送者可點自己送出的紅包氣泡，查看是否已被領取、領取時間與單號</td>
                </tr>
                <tr>
                  <td>⑧</td>
                  <td>異常-餘額不足（發送方）</td>
                  <td>錢包餘額不足時的中斷流程，清楚提示差額並導向儲值</td>
                </tr>
                <tr>
                  <td>⑨</td>
                  <td>異常-對方未開通零錢包（接收方）</td>
                  <td>接收者點擊紅包時尚未開通零錢包，需先完成開通才能收下</td>
                </tr>
              </tbody>
            </table>
          </div>

          <!-- Interactive Prototype Showcase Frame -->
          <div class="proto-showcase">
            <div class="proto-showcase-bar">
              <div class="proto-mac-dots">
                <span class="proto-dot red"></span>
                <span class="proto-dot yellow"></span>
                <span class="proto-dot green"></span>
              </div>
              <div class="proto-bar-title">互動式原型預覽</div>
              <div class="proto-bar-actions">
                <button class="proto-action-btn" onclick="toggleProtoHeight(this)">↕ 調整視窗高度</button>
              </div>
            </div>
            <div class="proto-frame-wrapper">
              <iframe
                src="data:text/html;base64,PCFET0NUWVBFIGh0bWw+CjxodG1sIGxhbmc9InpoLUhhbnQiPgo8aGVhZD4KPG1ldGEgY2hhcnNldD0iVVRGLTgiPgo8dGl0bGU+5LiA5bCN5LiA55m857SF5YyFIC0g5Y6f5Z6LPC90aXRsZT4KPHN0eWxlPgogIDpyb290ewogICAgLS1icmFuZC1yZWQ6I0UyM0QzRDsKICAgIC0tYnJhbmQtcmVkLWRhcms6I0M0MkUyRTsKICAgIC0tYnJhbmQtZ29sZDojRjBDMzZBOwogICAgLS1ibHVlLWJ1YmJsZTojM0I4MkY2OwogICAgLS1ncmV5LWJ1YmJsZTojRURFREVEOwogICAgLS1iZzojRUZFRkY0OwogICAgLS1pbms6IzFBMUExQTsKICAgIC0taW5rLXNvZnQ6IzhBOEE4RTsKICAgIC0tbGluZTojRTVFNUVBOwogIH0KICAqe2JveC1zaXppbmc6Ym9yZGVyLWJveDt9CiAgYm9keXsKICAgIG1hcmdpbjowOwogICAgZm9udC1mYW1pbHk6LWFwcGxlLXN5c3RlbSwiUGluZ0ZhbmcgVEMiLCJNaWNyb3NvZnQgSmhlbmdIZWkiLHNhbnMtc2VyaWY7CiAgICBiYWNrZ3JvdW5kOiNGNEY0RjY7CiAgICBkaXNwbGF5OmZsZXg7CiAgICBmbGV4LWRpcmVjdGlvbjpjb2x1bW47CiAgICBhbGlnbi1pdGVtczpjZW50ZXI7CiAgICBwYWRkaW5nOjI4cHggMTJweCA2MHB4OwogICAgY29sb3I6dmFyKC0taW5rKTsKICB9CiAgaDF7Zm9udC1zaXplOjE2cHg7Y29sb3I6dmFyKC0taW5rLXNvZnQpO2ZvbnQtd2VpZ2h0OjUwMDttYXJnaW46MCAwIDE4cHg7fQogIC50YWJzewogICAgZGlzcGxheTpmbGV4O2ZsZXgtd3JhcDp3cmFwO2dhcDo4cHg7anVzdGlmeS1jb250ZW50OmNlbnRlcjsKICAgIG1heC13aWR0aDo3NjBweDttYXJnaW4tYm90dG9tOjIycHg7CiAgfQogIC50YWJ7CiAgICBwYWRkaW5nOjdweCAxNHB4O2JvcmRlci1yYWRpdXM6OTk5cHg7Ym9yZGVyOjFweCBzb2xpZCB2YXIoLS1saW5lKTsKICAgIGJhY2tncm91bmQ6I2ZmZjtmb250LXNpemU6MTIuNXB4O2N1cnNvcjpwb2ludGVyO2NvbG9yOnZhcigtLWluayk7CiAgICB0cmFuc2l0aW9uOmFsbCAuMTVzOwogIH0KICAudGFiLmFjdGl2ZXtiYWNrZ3JvdW5kOnZhcigtLWluayk7Y29sb3I6I2ZmZjtib3JkZXItY29sb3I6dmFyKC0taW5rKTt9CiAgLnRhYjpob3Zlcntib3JkZXItY29sb3I6dmFyKC0taW5rKTt9CgogIC5waG9uZXsKICAgIHdpZHRoOjM3NXB4OwogICAgaGVpZ2h0OjcyMHB4OwogICAgYmFja2dyb3VuZDojZmZmOwogICAgYm9yZGVyLXJhZGl1czozOHB4OwogICAgYm9yZGVyOjhweCBzb2xpZCAjMWMxYzFlOwogICAgb3ZlcmZsb3c6aGlkZGVuOwogICAgcG9zaXRpb246cmVsYXRpdmU7CiAgICBib3gtc2hhZG93OjAgMjBweCA0MHB4IHJnYmEoMCwwLDAsLjE4KTsKICB9CiAgLm5vdGNoewogICAgcG9zaXRpb246YWJzb2x1dGU7dG9wOjA7bGVmdDo1MCU7dHJhbnNmb3JtOnRyYW5zbGF0ZVgoLTUwJSk7CiAgICB3aWR0aDoxNTBweDtoZWlnaHQ6MjJweDtiYWNrZ3JvdW5kOiMxYzFjMWU7Ym9yZGVyLXJhZGl1czowIDAgMTRweCAxNHB4O3otaW5kZXg6NTA7CiAgfQogIC5zY3JlZW57CiAgICBwb3NpdGlvbjphYnNvbHV0ZTtpbnNldDowO2Rpc3BsYXk6bm9uZTtmbGV4LWRpcmVjdGlvbjpjb2x1bW47YmFja2dyb3VuZDp2YXIoLS1iZyk7CiAgfQogIC5zY3JlZW4uYWN0aXZle2Rpc3BsYXk6ZmxleDt9CgogIC8qIC0tLS0gU3RhdHVzIC8gTmF2IGJhciAtLS0tICovCiAgLnN0YXR1c2JhcntoZWlnaHQ6MjJweDtmbGV4OjAgMCBhdXRvO2JhY2tncm91bmQ6dHJhbnNwYXJlbnQ7fQogIC5uYXZiYXJ7CiAgICBmbGV4OjAgMCBhdXRvO2hlaWdodDo0NHB4O2Rpc3BsYXk6ZmxleDthbGlnbi1pdGVtczpjZW50ZXI7anVzdGlmeS1jb250ZW50OnNwYWNlLWJldHdlZW47CiAgICBwYWRkaW5nOjAgMTRweDtiYWNrZ3JvdW5kOiNmZmY7Ym9yZGVyLWJvdHRvbToxcHggc29saWQgdmFyKC0tbGluZSk7cG9zaXRpb246cmVsYXRpdmU7CiAgfQogIC5uYXZiYXIgLmNlbnRlcnsKICAgIHBvc2l0aW9uOmFic29sdXRlO2xlZnQ6MDtyaWdodDowO3RleHQtYWxpZ246Y2VudGVyO2ZvbnQtc2l6ZToxNXB4O2ZvbnQtd2VpZ2h0OjYwMDsKICB9CiAgLm5hdmJhciAuc2lkZXtmb250LXNpemU6MTRweDtjb2xvcjp2YXIoLS1pbmspO21pbi13aWR0aDo0MHB4O30KICAubmF2YmFyIC5zaWRlLmxpbmt7Y29sb3I6dmFyKC0tYnJhbmQtcmVkKTt9CiAgLm5hdmJhciAuYmFja3tmb250LXNpemU6MjBweDt9CgogIC8qIC0tLS0gU2NyZWVuIDE6IGNvbXBvc2UgLS0tLSAqLwogIC5jb21wb3NlLWJvZHl7ZmxleDoxO292ZXJmbG93LXk6YXV0bztwYWRkaW5nOjA7fQogIC5lbnZlbG9wZS1jYXJkewogICAgbWFyZ2luOjE2cHg7Ym9yZGVyLXJhZGl1czoxNHB4O292ZXJmbG93OmhpZGRlbjsKICAgIGJhY2tncm91bmQ6bGluZWFyLWdyYWRpZW50KDE2MGRlZyx2YXIoLS1icmFuZC1yZWQpIDAlLHZhcigtLWJyYW5kLXJlZC1kYXJrKSAxMDAlKTsKICAgIGNvbG9yOiNmZmY7cGFkZGluZzoyNnB4IDIycHggMjBweDtwb3NpdGlvbjpyZWxhdGl2ZTsKICB9CiAgLmVudmVsb3BlLWNhcmQgLnRhZ3sKICAgIHBvc2l0aW9uOmFic29sdXRlO3RvcDoxNHB4O3JpZ2h0OjE2cHg7Zm9udC1zaXplOjExcHg7YmFja2dyb3VuZDpyZ2JhKDI1NSwyNTUsMjU1LC4xOCk7CiAgICBwYWRkaW5nOjNweCA5cHg7Ym9yZGVyLXJhZGl1czo5OTlweDsKICB9CiAgLmVudmVsb3BlLWNhcmQgLnRve2ZvbnQtc2l6ZToxMi41cHg7b3BhY2l0eTouODU7bWFyZ2luLWJvdHRvbToxNHB4O30KICAuYW1vdW50LXJvd3tkaXNwbGF5OmZsZXg7YWxpZ24taXRlbXM6YmFzZWxpbmU7Z2FwOjZweDttYXJnaW4tYm90dG9tOjZweDt9CiAgLmFtb3VudC1yb3cgLmN1cntmb250LXNpemU6MjBweDtmb250LXdlaWdodDo2MDA7fQogIC5hbW91bnQtcm93IGlucHV0ewogICAgYmFja2dyb3VuZDp0cmFuc3BhcmVudDtib3JkZXI6bm9uZTtvdXRsaW5lOm5vbmU7Y29sb3I6I2ZmZjtmb250LXNpemU6MzRweDtmb250LXdlaWdodDo3MDA7CiAgICB3aWR0aDoxMDAlO3BhZGRpbmc6MDsKICB9CiAgLmFtb3VudC1yb3cgaW5wdXQ6OnBsYWNlaG9sZGVye2NvbG9yOnJnYmEoMjU1LDI1NSwyNTUsLjU1KTtmb250LXdlaWdodDo2MDA7fQogIC5hbW91bnQtaGludHtmb250LXNpemU6MTEuNXB4O29wYWNpdHk6Ljg7fQogIC5kaXZpZGVyLWRhc2h7Ym9yZGVyLXRvcDoxcHggZGFzaGVkIHJnYmEoMjU1LDI1NSwyNTUsLjQpO21hcmdpbjoxNnB4IDA7fQogIC5ibGVzc2luZy1pbnB1dHsKICAgIHdpZHRoOjEwMCU7YmFja2dyb3VuZDp0cmFuc3BhcmVudDtib3JkZXI6bm9uZTtvdXRsaW5lOm5vbmU7Y29sb3I6I2ZmZjtmb250LXNpemU6MTRweDsKICAgIHJlc2l6ZTpub25lO2ZvbnQtZmFtaWx5OmluaGVyaXQ7CiAgfQogIC5ibGVzc2luZy1pbnB1dDo6cGxhY2Vob2xkZXJ7Y29sb3I6cmdiYSgyNTUsMjU1LDI1NSwuNjUpO30KICAuY2hhci1jb3VudHtmb250LXNpemU6MTFweDtvcGFjaXR5Oi43O3RleHQtYWxpZ246cmlnaHQ7fQoKICAuaW5mby1saXN0e21hcmdpbjowIDE2cHg7YmFja2dyb3VuZDojZmZmO2JvcmRlci1yYWRpdXM6MTJweDtvdmVyZmxvdzpoaWRkZW47fQogIC5pbmZvLXJvd3sKICAgIGRpc3BsYXk6ZmxleDtqdXN0aWZ5LWNvbnRlbnQ6c3BhY2UtYmV0d2VlbjtwYWRkaW5nOjEzcHggMTRweDtmb250LXNpemU6MTMuNXB4OwogICAgYm9yZGVyLWJvdHRvbToxcHggc29saWQgdmFyKC0tbGluZSk7CiAgfQogIC5pbmZvLXJvdzpsYXN0LWNoaWxke2JvcmRlci1ib3R0b206bm9uZTt9CiAgLmluZm8tcm93IC5se2NvbG9yOnZhcigtLWluay1zb2Z0KTt9CiAgLmluZm8tcm93IC5ye2NvbG9yOnZhcigtLWluayk7fQogIC5pbmZvLXJvdyAuci5saW5re2NvbG9yOiMzQjgyRjY7fQoKICAuYm90dG9tLWZpeGVke2ZsZXg6MCAwIGF1dG87cGFkZGluZzoxMnB4IDE2cHggMjJweDtiYWNrZ3JvdW5kOiNmZmY7Ym9yZGVyLXRvcDoxcHggc29saWQgdmFyKC0tbGluZSk7fQogIC5idG4tcHJpbWFyeXsKICAgIHdpZHRoOjEwMCU7cGFkZGluZzoxM3B4O2JvcmRlci1yYWRpdXM6MjRweDtib3JkZXI6bm9uZTsKICAgIGJhY2tncm91bmQ6dmFyKC0tYnJhbmQtcmVkKTtjb2xvcjojZmZmO2ZvbnQtc2l6ZToxNS41cHg7Zm9udC13ZWlnaHQ6NjAwOwogIH0KICAuYnRuLXByaW1hcnk6ZGlzYWJsZWR7YmFja2dyb3VuZDojRjNCN0IwO30KICAuYnRuLXN1Ynt0ZXh0LWFsaWduOmNlbnRlcjtmb250LXNpemU6MTEuNXB4O2NvbG9yOnZhcigtLWluay1zb2Z0KTttYXJnaW4tdG9wOjhweDt9CgogIC8qIC0tLS0gU2NyZWVuIDI6IHBheW1lbnQgcGFzc3dvcmQgLS0tLSAqLwogIC5wYXktc3VtbWFyeXt0ZXh0LWFsaWduOmNlbnRlcjtwYWRkaW5nOjM0cHggMjBweCAxOHB4O30KICAucGF5LXN1bW1hcnkgLmxhYmVse2ZvbnQtc2l6ZToxM3B4O2NvbG9yOnZhcigtLWluay1zb2Z0KTt9CiAgLnBheS1zdW1tYXJ5IC5hbXR7Zm9udC1zaXplOjM2cHg7Zm9udC13ZWlnaHQ6NzAwO21hcmdpbjo4cHggMCAycHg7fQogIC5wYXktc3VtbWFyeSAudG97Zm9udC1zaXplOjEyLjVweDtjb2xvcjp2YXIoLS1pbmstc29mdCk7fQogIC5kb3RzLXJvd3tkaXNwbGF5OmZsZXg7anVzdGlmeS1jb250ZW50OmNlbnRlcjtnYXA6MTZweDttYXJnaW46MjZweCAwIDhweDt9CiAgLmRvdHt3aWR0aDoxNHB4O2hlaWdodDoxNHB4O2JvcmRlci1yYWRpdXM6NTAlO2JvcmRlcjoxcHggc29saWQgI0M5QzlDRTt9CiAgLmRvdC5maWxsZWR7YmFja2dyb3VuZDp2YXIoLS1pbmspO2JvcmRlci1jb2xvcjp2YXIoLS1pbmspO30KICAucGF5LW5vdGV7dGV4dC1hbGlnbjpjZW50ZXI7Zm9udC1zaXplOjExLjVweDtjb2xvcjp2YXIoLS1pbmstc29mdCk7bWFyZ2luLWJvdHRvbToyMHB4O30KICAua2V5cGFke2Rpc3BsYXk6Z3JpZDtncmlkLXRlbXBsYXRlLWNvbHVtbnM6cmVwZWF0KDMsMWZyKTtnYXA6MXB4O2JhY2tncm91bmQ6dmFyKC0tbGluZSk7bWFyZ2luLXRvcDphdXRvO30KICAua2V5e2JhY2tncm91bmQ6I2ZmZjt0ZXh0LWFsaWduOmNlbnRlcjtwYWRkaW5nOjE4cHggMDtmb250LXNpemU6MjJweDt9CiAgLmtleS5zdWJ7Zm9udC1zaXplOjEycHg7Y29sb3I6dmFyKC0taW5rLXNvZnQpO30KCiAgLyogLS0tLSBTY3JlZW4gMy81OiBjaGF0IGJ1YmJsZSAtLS0tICovCiAgLmNoYXQtYm9keXtmbGV4OjE7b3ZlcmZsb3cteTphdXRvO3BhZGRpbmc6MTRweCAxMnB4O2Rpc3BsYXk6ZmxleDtmbGV4LWRpcmVjdGlvbjpjb2x1bW47Z2FwOjE0cHg7fQogIC5kYXRlLWNoaXB7YWxpZ24tc2VsZjpjZW50ZXI7Zm9udC1zaXplOjExcHg7Y29sb3I6dmFyKC0taW5rLXNvZnQpO2JhY2tncm91bmQ6I0UyRTJFNjtwYWRkaW5nOjNweCAxMHB4O2JvcmRlci1yYWRpdXM6OHB4O30KICAubXNnLXJvd3tkaXNwbGF5OmZsZXg7Z2FwOjhweDttYXgtd2lkdGg6ODglO30KICAubXNnLXJvdy5tZXthbGlnbi1zZWxmOmZsZXgtZW5kO2ZsZXgtZGlyZWN0aW9uOnJvdy1yZXZlcnNlO30KICAuYXZhdGFye3dpZHRoOjM0cHg7aGVpZ2h0OjM0cHg7Ym9yZGVyLXJhZGl1czo4cHg7YmFja2dyb3VuZDojQjlDNkQ2O2ZsZXg6MCAwIGF1dG87fQogIC5hdmF0YXIubWV7YmFja2dyb3VuZDojM0I4MkY2O30KICAuYnViYmxlLXRleHR7YmFja2dyb3VuZDp2YXIoLS1ncmV5LWJ1YmJsZSk7cGFkZGluZzo5cHggMTJweDtib3JkZXItcmFkaXVzOjEwcHg7Zm9udC1zaXplOjE0cHg7fQogIC5idWJibGUtdGV4dC5tZXtiYWNrZ3JvdW5kOnZhcigtLWJsdWUtYnViYmxlKTtjb2xvcjojZmZmO30KCiAgLnJlZGVudi1idWJibGV7CiAgICB3aWR0aDoyMTBweDtib3JkZXItcmFkaXVzOjEwcHg7b3ZlcmZsb3c6aGlkZGVuOwogICAgYmFja2dyb3VuZDpsaW5lYXItZ3JhZGllbnQoMTYwZGVnLHZhcigtLWJyYW5kLXJlZCkgMCUsdmFyKC0tYnJhbmQtcmVkLWRhcmspIDEwMCUpOwogICAgY29sb3I6I2ZmZjtjdXJzb3I6cG9pbnRlcjsKICB9CiAgLnJlZGVudi1idWJibGUgLnRvcHtkaXNwbGF5OmZsZXg7YWxpZ24taXRlbXM6Y2VudGVyO2dhcDoxMHB4O3BhZGRpbmc6MTRweCAxMnB4IDEwcHg7fQogIC5yZWRlbnYtaWNvbnsKICAgIHdpZHRoOjMycHg7aGVpZ2h0OjMycHg7Ym9yZGVyLXJhZGl1czo1MCU7YmFja2dyb3VuZDpyZ2JhKDI1NSwyNTUsMjU1LC4xNSk7CiAgICBkaXNwbGF5OmZsZXg7YWxpZ24taXRlbXM6Y2VudGVyO2p1c3RpZnktY29udGVudDpjZW50ZXI7Zm9udC1zaXplOjE3cHg7CiAgfQogIC5yZWRlbnYtYnViYmxlIC5tc2d7Zm9udC1zaXplOjEzLjVweDtsaW5lLWhlaWdodDoxLjM7fQogIC5yZWRlbnYtYnViYmxlIC5ib3R0b217CiAgICBib3JkZXItdG9wOjFweCBzb2xpZCByZ2JhKDI1NSwyNTUsMjU1LC4yNSk7cGFkZGluZzo2cHggMTJweDtmb250LXNpemU6MTAuNXB4O29wYWNpdHk6Ljg1OwogICAgZGlzcGxheTpmbGV4O2p1c3RpZnktY29udGVudDpzcGFjZS1iZXR3ZWVuOwogIH0KICAucmVkZW52LWJ1YmJsZS5vcGVuZWR7CiAgICBiYWNrZ3JvdW5kOmxpbmVhci1ncmFkaWVudCgxNjBkZWcsI0Q4QjY3OSAwJSwjQzc5QTRGIDEwMCUpOwogIH0KCiAgLyogLS0tLSBTY3JlZW4gNDogb3BlbiBhbmltYXRpb24gLyByZXN1bHQgLS0tLSAqLwogIC5vcGVuLXNjcmVlbnsKICAgIGJhY2tncm91bmQ6bGluZWFyLWdyYWRpZW50KDE4MGRlZywjQzQyRTJFIDAlLCM3QTFGMUYgMTAwJSk7CiAgICBjb2xvcjojZmZmO2hlaWdodDoxMDAlO2FsaWduLWl0ZW1zOmNlbnRlcjsKICB9CiAgLm9wZW4tbmF2e3dpZHRoOjEwMCU7cGFkZGluZzoxMnB4IDE0cHg7ZGlzcGxheTpmbGV4O2p1c3RpZnktY29udGVudDpzcGFjZS1iZXR3ZWVuO2ZvbnQtc2l6ZToxM3B4O30KICAub3Blbi1hdmF0YXJ7d2lkdGg6NTZweDtoZWlnaHQ6NTZweDtib3JkZXItcmFkaXVzOjEwcHg7YmFja2dyb3VuZDpyZ2JhKDI1NSwyNTUsMjU1LC4yKTttYXJnaW4tdG9wOjZweDt9CiAgLm9wZW4tZnJvbXtmb250LXNpemU6MTNweDttYXJnaW4tdG9wOjEwcHg7b3BhY2l0eTouOTt9CiAgLm9wZW4tYmxlc3Npbmd7Zm9udC1zaXplOjE1cHg7Zm9udC13ZWlnaHQ6NjAwO21hcmdpbjo2cHggMjRweCAwO3RleHQtYWxpZ246Y2VudGVyO30KICAub3Blbi1zZWFsewogICAgbWFyZ2luLXRvcDoyNnB4O3dpZHRoOjg4cHg7aGVpZ2h0Ojg4cHg7Ym9yZGVyLXJhZGl1czo1MCU7CiAgICBiYWNrZ3JvdW5kOnJhZGlhbC1ncmFkaWVudChjaXJjbGUgYXQgMzUlIDMwJSwjRkZFOUIwLCNGMEMzNkEgNjAlLCNEOEE4M0UpOwogICAgZGlzcGxheTpmbGV4O2FsaWduLWl0ZW1zOmNlbnRlcjtqdXN0aWZ5LWNvbnRlbnQ6Y2VudGVyO2ZvbnQtc2l6ZTozNHB4O2NvbG9yOiNCMjNBMkU7CiAgICBib3gtc2hhZG93OjAgNnB4IDE4cHggcmdiYSgwLDAsMCwuMjUpOwogIH0KICAub3Blbi1jdGF7bWFyZ2luLXRvcDoxNHB4O2ZvbnQtc2l6ZToxMnB4O29wYWNpdHk6Ljg1O2xldHRlci1zcGFjaW5nOjFweDt9CiAgLnJlc3VsdC1hbXQtbGFiZWx7Zm9udC1zaXplOjEzcHg7b3BhY2l0eTouODU7bWFyZ2luLXRvcDoyMnB4O30KICAucmVzdWx0LWFtdHtmb250LXNpemU6NDRweDtmb250LXdlaWdodDo3MDA7bWFyZ2luLXRvcDo0cHg7fQogIC5yZXN1bHQtc3Vie2ZvbnQtc2l6ZToxMnB4O29wYWNpdHk6Ljg7bWFyZ2luLXRvcDo2cHg7fQogIC5yZXN1bHQtYnRuewogICAgbWFyZ2luLXRvcDoyNnB4O2JhY2tncm91bmQ6cmdiYSgyNTUsMjU1LDI1NSwuMTUpO2JvcmRlcjoxcHggc29saWQgcmdiYSgyNTUsMjU1LDI1NSwuNSk7CiAgICBjb2xvcjojZmZmO3BhZGRpbmc6OXB4IDMwcHg7Ym9yZGVyLXJhZGl1czoyMnB4O2ZvbnQtc2l6ZToxMy41cHg7CiAgfQogIC5yZXN1bHQtZGV0YWlsLWxpbmt7bWFyZ2luLXRvcDoxNHB4O2ZvbnQtc2l6ZToxMnB4O3RleHQtZGVjb3JhdGlvbjp1bmRlcmxpbmU7b3BhY2l0eTouODU7fQoKICAvKiAtLS0tIFNjcmVlbiA2OiBkZXRhaWwgKHNlbmRlciByZWNlaXB0KSAtLS0tICovCiAgLmRldGFpbC1jYXJke21hcmdpbjoyMHB4IDE2cHg7YmFja2dyb3VuZDojZmZmO2JvcmRlci1yYWRpdXM6MTRweDtvdmVyZmxvdzpoaWRkZW47dGV4dC1hbGlnbjpjZW50ZXI7fQogIC5kZXRhaWwtaGVhZHtiYWNrZ3JvdW5kOmxpbmVhci1ncmFkaWVudCgxNjBkZWcsdmFyKC0tYnJhbmQtcmVkKSx2YXIoLS1icmFuZC1yZWQtZGFyaykpO2NvbG9yOiNmZmY7cGFkZGluZzoyNnB4IDE2cHggMzRweDt9CiAgLmRldGFpbC1oZWFkIC5hbXR7Zm9udC1zaXplOjMwcHg7Zm9udC13ZWlnaHQ6NzAwO30KICAuZGV0YWlsLWhlYWQgLmJsZXNzaW5ne2ZvbnQtc2l6ZToxMi41cHg7b3BhY2l0eTouOTttYXJnaW4tdG9wOjZweDt9CiAgLmRldGFpbC1zdGF0dXN7CiAgICBiYWNrZ3JvdW5kOiNmZmY7bWFyZ2luLXRvcDotMThweDtib3JkZXItcmFkaXVzOjE0cHggMTRweCAwIDA7cGFkZGluZzoxNnB4IDE2cHggNHB4OwogIH0KICAuZGV0YWlsLXN0YXR1cyAubGluZXtmb250LXNpemU6MTNweDtjb2xvcjp2YXIoLS1pbmstc29mdCk7bWFyZ2luLWJvdHRvbToxMHB4O30KICAuZGV0YWlsLXJvd3tkaXNwbGF5OmZsZXg7Z2FwOjEycHg7YWxpZ24taXRlbXM6Y2VudGVyO3BhZGRpbmc6MTBweCAxNnB4O2JvcmRlci1ib3R0b206MXB4IHNvbGlkIHZhcigtLWxpbmUpO3RleHQtYWxpZ246bGVmdDt9CiAgLmRldGFpbC1yb3cgLmF2e3dpZHRoOjMwcHg7aGVpZ2h0OjMwcHg7Ym9yZGVyLXJhZGl1czo1MCU7YmFja2dyb3VuZDojQjlDNkQ2O30KICAuZGV0YWlsLXJvdyAubmFtZXtmb250LXNpemU6MTMuNXB4O30KICAuZGV0YWlsLXJvdyAuc3RhdGV7bWFyZ2luLWxlZnQ6YXV0bztmb250LXNpemU6MTIuNXB4O2NvbG9yOnZhcigtLWluay1zb2Z0KTt9CiAgLmRldGFpbC1yb3cgLnN0YXRlLmNsYWltZWR7Y29sb3I6IzJGQTg0Rjt9CiAgLm1ldGEtaW5mb3twYWRkaW5nOjE0cHggMTZweDtmb250LXNpemU6MTEuNXB4O2NvbG9yOnZhcigtLWluay1zb2Z0KTt0ZXh0LWFsaWduOmxlZnQ7bGluZS1oZWlnaHQ6MS44O30KCiAgLmNhcHRpb257bWF4LXdpZHRoOjM3NXB4O21hcmdpbi10b3A6MTRweDtmb250LXNpemU6MTJweDtjb2xvcjp2YXIoLS1pbmstc29mdCk7dGV4dC1hbGlnbjpjZW50ZXI7bGluZS1oZWlnaHQ6MS42O30KPC9zdHlsZT4KPC9oZWFkPgo8Ym9keT4KCjxoMT7kuIDlsI3kuIDnmbzntIXljIUg4oCUIOm7nuaTiuS4iuaWueaMiemIleWIh+aPm+WOn+Wei+eVq+mdou+8iOacrOaqlOahiOWDheeCuue3muahhuekuuaEj++8jOmdnuacgOe1guimluimuueov++8iTwvaDE+Cgo8ZGl2IGNsYXNzPSJ0YWJzIj4KICA8ZGl2IGNsYXNzPSJ0YWIgYWN0aXZlIiBkYXRhLXRhcmdldD0iczEiPuKRoCDnmbzntIXljIUt6Ly45YWl6aCBPC9kaXY+CiAgPGRpdiBjbGFzcz0idGFiIiBkYXRhLXRhcmdldD0iczIiPuKRoSDmlK/ku5jlr4bnorznorroqo08L2Rpdj4KICA8ZGl2IGNsYXNzPSJ0YWIiIGRhdGEtdGFyZ2V0PSJzMyI+4pGiIOiBiuWkqeWupC3mnKrmi4Y8L2Rpdj4KICA8ZGl2IGNsYXNzPSJ0YWIiIGRhdGEtdGFyZ2V0PSJzNGEiPuKRoyDmi4bntIXljIXli5Xnlas8L2Rpdj4KICA8ZGl2IGNsYXNzPSJ0YWIiIGRhdGEtdGFyZ2V0PSJzNGIiPuKRpCDmi4bntIXljIXntZDmnpw8L2Rpdj4KICA8ZGl2IGNsYXNzPSJ0YWIiIGRhdGEtdGFyZ2V0PSJzNSI+4pGlIOiBiuWkqeWupC3lt7Lmi4Y8L2Rpdj4KICA8ZGl2IGNsYXNzPSJ0YWIiIGRhdGEtdGFyZ2V0PSJzNiI+4pGmIOeZvOmAgeaWuS3poJjlj5boqbPmg4U8L2Rpdj4KICA8ZGl2IGNsYXNzPSJ0YWIiIGRhdGEtdGFyZ2V0PSJzNyI+4pGnIOeVsOW4uC3ppJjpoY3kuI3otrM8L2Rpdj4KICA8ZGl2IGNsYXNzPSJ0YWIiIGRhdGEtdGFyZ2V0PSJzOCI+4pGoIOeVsOW4uC3lsI3mlrnmnKrplovpgJrpm7bpjKLljIU8L2Rpdj4KPC9kaXY+Cgo8ZGl2IGNsYXNzPSJwaG9uZSI+CiAgPGRpdiBjbGFzcz0ibm90Y2giPjwvZGl2PgoKICA8IS0tIFNDUkVFTiAxOiDnmbzntIXljIXovLjlhaXpoIEgLS0+CiAgPGRpdiBjbGFzcz0ic2NyZWVuIGFjdGl2ZSIgaWQ9InMxIj4KICAgIDxkaXYgY2xhc3M9InN0YXR1c2JhciI+PC9kaXY+CiAgICA8ZGl2IGNsYXNzPSJuYXZiYXIiPgogICAgICA8ZGl2IGNsYXNzPSJzaWRlIGJhY2siPuKAuTwvZGl2PgogICAgICA8ZGl2IGNsYXNzPSJjZW50ZXIiPue0heWMhTwvZGl2PgogICAgICA8ZGl2IGNsYXNzPSJzaWRlIGxpbmsiIHN0eWxlPSJ0ZXh0LWFsaWduOnJpZ2h0OyI+6KiY6YyEPC9kaXY+CiAgICA8L2Rpdj4KICAgIDxkaXYgY2xhc3M9ImNvbXBvc2UtYm9keSI+CiAgICAgIDxkaXYgY2xhc3M9ImVudmVsb3BlLWNhcmQiPgogICAgICAgIDxkaXYgY2xhc3M9InRhZyI+8J+npyDlgIvkurrntIXljIU8L2Rpdj4KICAgICAgICA8ZGl2IGNsYXNzPSJ0byI+6YCB57Wm77ya5bCP576OPC9kaXY+CiAgICAgICAgPGRpdiBjbGFzcz0iYW1vdW50LXJvdyI+CiAgICAgICAgICA8c3BhbiBjbGFzcz0iY3VyIj5OVCQ8L3NwYW4+CiAgICAgICAgICA8aW5wdXQgdHlwZT0idGV4dCIgcGxhY2Vob2xkZXI9Iuiri+i8uOWFpemHkemhjSIgdmFsdWU9IjIwMCI+CiAgICAgICAgPC9kaXY+CiAgICAgICAgPGRpdiBjbGFzcz0iYW1vdW50LWhpbnQiPuWWruethuS4iumZkCBOVCQyLDAwMCDCtyDku4rml6XlianppJjpoY3luqYgTlQkOCwwMDA8L2Rpdj4KICAgICAgICA8ZGl2IGNsYXNzPSJkaXZpZGVyLWRhc2giPjwvZGl2PgogICAgICAgIDx0ZXh0YXJlYSBjbGFzcz0iYmxlc3NpbmctaW5wdXQiIHJvd3M9IjIiIHBsYWNlaG9sZGVyPSLmga3llpznmbzosqHvvIzlpKflkInlpKfliKkiPuaBreWWnOeZvOiyoe+8jOWkp+WQieWkp+WIqTwvdGV4dGFyZWE+CiAgICAgICAgPGRpdiBjbGFzcz0iY2hhci1jb3VudCI+OCAvIDQwPC9kaXY+CiAgICAgIDwvZGl2PgogICAgICA8ZGl2IGNsYXNzPSJpbmZvLWxpc3QiPgogICAgICAgIDxkaXYgY2xhc3M9ImluZm8tcm93Ij48ZGl2IGNsYXNzPSJsIj7mlK/ku5jmlrnlvI88L2Rpdj48ZGl2IGNsYXNzPSJyIj7pjKLljIXppJjpoY0gTlQkMSw1MzA8L2Rpdj48L2Rpdj4KICAgICAgICA8ZGl2IGNsYXNzPSJpbmZvLXJvdyI+PGRpdiBjbGFzcz0ibCI+6aCY5Y+W5pyf6ZmQPC9kaXY+PGRpdiBjbGFzcz0iciI+MjQg5bCP5pmC5YWn77yM6YC+5pyf6Ieq5YuV6YCA5ZuePC9kaXY+PC9kaXY+CiAgICAgIDwvZGl2PgogICAgPC9kaXY+CiAgICA8ZGl2IGNsYXNzPSJib3R0b20tZml4ZWQiPgogICAgICA8YnV0dG9uIGNsYXNzPSJidG4tcHJpbWFyeSI+5aGe6Yyi6YCy57SF5YyFIE5UJDIwMDwvYnV0dG9uPgogICAgICA8ZGl2IGNsYXNzPSJidG4tc3ViIj7ntIXljIXnmbzlh7rlvozkuI3lj6/mkqTlm57vvIzoq4vnorroqo3ph5HpoY3oiIflsI3osaE8L2Rpdj4KICAgIDwvZGl2PgogIDwvZGl2PgoKICA8IS0tIFNDUkVFTiAyOiDmlK/ku5jlr4bnorwgLS0+CiAgPGRpdiBjbGFzcz0ic2NyZWVuIiBpZD0iczIiPgogICAgPGRpdiBjbGFzcz0ic3RhdHVzYmFyIj48L2Rpdj4KICAgIDxkaXYgY2xhc3M9Im5hdmJhciI+CiAgICAgIDxkaXYgY2xhc3M9InNpZGUgYmFjayI+4oC5PC9kaXY+CiAgICAgIDxkaXYgY2xhc3M9ImNlbnRlciI+6Ly45YWl5pSv5LuY5a+G56K8PC9kaXY+CiAgICAgIDxkaXYgY2xhc3M9InNpZGUiPjwvZGl2PgogICAgPC9kaXY+CiAgICA8ZGl2IGNsYXNzPSJwYXktc3VtbWFyeSI+CiAgICAgIDxkaXYgY2xhc3M9ImxhYmVsIj7norroqo3nmbzpgIHntIXljIXntaYg5bCP576OPC9kaXY+CiAgICAgIDxkaXYgY2xhc3M9ImFtdCI+TlQkIDIwMDwvZGl2PgogICAgICA8ZGl2IGNsYXNzPSJ0byI+5oGt5Zac55m86LKh77yM5aSn5ZCJ5aSn5YipPC9kaXY+CiAgICA8L2Rpdj4KICAgIDxkaXYgY2xhc3M9ImRvdHMtcm93Ij4KICAgICAgPGRpdiBjbGFzcz0iZG90IGZpbGxlZCI+PC9kaXY+PGRpdiBjbGFzcz0iZG90IGZpbGxlZCI+PC9kaXY+PGRpdiBjbGFzcz0iZG90IGZpbGxlZCI+PC9kaXY+CiAgICAgIDxkaXYgY2xhc3M9ImRvdCI+PC9kaXY+PGRpdiBjbGFzcz0iZG90Ij48L2Rpdj48ZGl2IGNsYXNzPSJkb3QiPjwvZGl2PgogICAgPC9kaXY+CiAgICA8ZGl2IGNsYXNzPSJwYXktbm90ZSI+5b+Y6KiY5a+G56K877yf5pS555So55Sf54mp6L6o6K2Y6amX6K2JPC9kaXY+CiAgICA8ZGl2IGNsYXNzPSJrZXlwYWQiPgogICAgICA8ZGl2IGNsYXNzPSJrZXkiPjE8L2Rpdj48ZGl2IGNsYXNzPSJrZXkiPjI8L2Rpdj48ZGl2IGNsYXNzPSJrZXkiPjM8L2Rpdj4KICAgICAgPGRpdiBjbGFzcz0ia2V5Ij40PC9kaXY+PGRpdiBjbGFzcz0ia2V5Ij41PC9kaXY+PGRpdiBjbGFzcz0ia2V5Ij42PC9kaXY+CiAgICAgIDxkaXYgY2xhc3M9ImtleSI+NzwvZGl2PjxkaXYgY2xhc3M9ImtleSI+ODwvZGl2PjxkaXYgY2xhc3M9ImtleSI+OTwvZGl2PgogICAgICA8ZGl2IGNsYXNzPSJrZXkgc3ViIj7mjIfntIs8L2Rpdj48ZGl2IGNsYXNzPSJrZXkiPjA8L2Rpdj48ZGl2IGNsYXNzPSJrZXkgc3ViIj7liKrpmaQ8L2Rpdj4KICAgIDwvZGl2PgogIDwvZGl2PgoKICA8IS0tIFNDUkVFTiAzOiDogYrlpKnlrqQgLSDmnKrmi4YgLS0+CiAgPGRpdiBjbGFzcz0ic2NyZWVuIiBpZD0iczMiPgogICAgPGRpdiBjbGFzcz0ic3RhdHVzYmFyIj48L2Rpdj4KICAgIDxkaXYgY2xhc3M9Im5hdmJhciI+CiAgICAgIDxkaXYgY2xhc3M9InNpZGUgYmFjayI+4oC5PC9kaXY+CiAgICAgIDxkaXYgY2xhc3M9ImNlbnRlciI+5bCP576OPC9kaXY+CiAgICAgIDxkaXYgY2xhc3M9InNpZGUiPuKLrzwvZGl2PgogICAgPC9kaXY+CiAgICA8ZGl2IGNsYXNzPSJjaGF0LWJvZHkiPgogICAgICA8ZGl2IGNsYXNzPSJkYXRlLWNoaXAiPjA4LzA2IDE1OjA5PC9kaXY+CiAgICAgIDxkaXYgY2xhc3M9Im1zZy1yb3ciPjxkaXYgY2xhc3M9ImF2YXRhciI+PC9kaXY+PGRpdiBjbGFzcz0iYnViYmxlLXRleHQiPuS9oOWlvTwvZGl2PjwvZGl2PgogICAgICA8ZGl2IGNsYXNzPSJtc2ctcm93IG1lIj4KICAgICAgICA8ZGl2IGNsYXNzPSJhdmF0YXIgbWUiPjwvZGl2PgogICAgICAgIDxkaXYgY2xhc3M9InJlZGVudi1idWJibGUiPgogICAgICAgICAgPGRpdiBjbGFzcz0idG9wIj4KICAgICAgICAgICAgPGRpdiBjbGFzcz0icmVkZW52LWljb24iPvCfp6c8L2Rpdj4KICAgICAgICAgICAgPGRpdiBjbGFzcz0ibXNnIj7mga3llpznmbzosqHvvIzlpKflkInlpKfliKk8L2Rpdj4KICAgICAgICAgIDwvZGl2PgogICAgICAgICAgPGRpdiBjbGFzcz0iYm90dG9tIj48c3Bhbj7ntIXljIU8L3NwYW4+PHNwYW4+6bue5pOK5p+l55yLPC9zcGFuPjwvZGl2PgogICAgICAgIDwvZGl2PgogICAgICA8L2Rpdj4KICAgIDwvZGl2PgogICAgPGRpdiBjbGFzcz0iYm90dG9tLWZpeGVkIiBzdHlsZT0icGFkZGluZzoxMHB4IDEycHg7Ij4KICAgICAgPGRpdiBzdHlsZT0iaGVpZ2h0OjM2cHg7Ym9yZGVyOjFweCBzb2xpZCB2YXIoLS1saW5lKTtib3JkZXItcmFkaXVzOjE4cHg7Ij48L2Rpdj4KICAgIDwvZGl2PgogIDwvZGl2PgoKICA8IS0tIFNDUkVFTiA0YTog5ouG57SF5YyF5YuV55Wr77yI5pyq5ouG54uA5oWL77yM562J5b6F6bue5pOK5ouG6ZaL77yJIC0tPgogIDxkaXYgY2xhc3M9InNjcmVlbiBvcGVuLXNjcmVlbiIgaWQ9InM0YSIgc3R5bGU9ImRpc3BsYXk6bm9uZTsiPgogICAgPGRpdiBjbGFzcz0ib3Blbi1uYXYiPjxzcGFuPuKAuTwvc3Bhbj48c3Bhbj7ntIXljIU8L3NwYW4+PHNwYW4+4ouvPC9zcGFuPjwvZGl2PgogICAgPGRpdiBjbGFzcz0ib3Blbi1hdmF0YXIiPjwvZGl2PgogICAgPGRpdiBjbGFzcz0ib3Blbi1mcm9tIj7kvoboh6og5L2gIOeahOe0heWMhTwvZGl2PgogICAgPGRpdiBjbGFzcz0ib3Blbi1ibGVzc2luZyI+5oGt5Zac55m86LKh77yM5aSn5ZCJ5aSn5YipPC9kaXY+CiAgICA8ZGl2IGNsYXNzPSJvcGVuLXNlYWwiPvCfp6c8L2Rpdj4KICAgIDxkaXYgY2xhc3M9Im9wZW4tY3RhIj7pu57mk4rmi4bplos8L2Rpdj4KICA8L2Rpdj4KCiAgPCEtLSBTQ1JFRU4gNGI6IOaLhue0heWMhee1kOaenCAtLT4KICA8ZGl2IGNsYXNzPSJzY3JlZW4gb3Blbi1zY3JlZW4iIGlkPSJzNGIiIHN0eWxlPSJkaXNwbGF5Om5vbmU7Ij4KICAgIDxkaXYgY2xhc3M9Im9wZW4tbmF2Ij48c3Bhbj7igLk8L3NwYW4+PHNwYW4+57SF5YyFPC9zcGFuPjxzcGFuPuKLrzwvc3Bhbj48L2Rpdj4KICAgIDxkaXYgY2xhc3M9Im9wZW4tYXZhdGFyIj48L2Rpdj4KICAgIDxkaXYgY2xhc3M9Im9wZW4tZnJvbSI+5L6G6IeqIOS9oCDnmoTntIXljIU8L2Rpdj4KICAgIDxkaXYgY2xhc3M9InJlc3VsdC1hbXQtbGFiZWwiPuW3sumgmOWPljwvZGl2PgogICAgPGRpdiBjbGFzcz0icmVzdWx0LWFtdCI+TlQkMjAwPC9kaXY+CiAgICA8ZGl2IGNsYXNzPSJyZXN1bHQtc3ViIj7lt7LlrZjlhaXpm7bpjKLljIXvvIzlj6/mlrzjgIzmiJHnmoTpjKLljIXjgI3mn6XnnIs8L2Rpdj4KICAgIDxidXR0b24gY2xhc3M9InJlc3VsdC1idG4iPuaUtuS4i+S6hjwvYnV0dG9uPgogICAgPGRpdiBjbGFzcz0icmVzdWx0LWRldGFpbC1saW5rIj7mn6XnnIvntIXljIXoqbPmg4U8L2Rpdj4KICA8L2Rpdj4KCiAgPCEtLSBTQ1JFRU4gNTog6IGK5aSp5a6kIC0g5bey5ouGIC0tPgogIDxkaXYgY2xhc3M9InNjcmVlbiIgaWQ9InM1Ij4KICAgIDxkaXYgY2xhc3M9InN0YXR1c2JhciI+PC9kaXY+CiAgICA8ZGl2IGNsYXNzPSJuYXZiYXIiPgogICAgICA8ZGl2IGNsYXNzPSJzaWRlIGJhY2siPuKAuTwvZGl2PgogICAgICA8ZGl2IGNsYXNzPSJjZW50ZXIiPuWwj+e+jjwvZGl2PgogICAgICA8ZGl2IGNsYXNzPSJzaWRlIj7ii688L2Rpdj4KICAgIDwvZGl2PgogICAgPGRpdiBjbGFzcz0iY2hhdC1ib2R5Ij4KICAgICAgPGRpdiBjbGFzcz0iZGF0ZS1jaGlwIj4wOC8wNiAxNTowOTwvZGl2PgogICAgICA8ZGl2IGNsYXNzPSJtc2ctcm93Ij48ZGl2IGNsYXNzPSJhdmF0YXIiPjwvZGl2PjxkaXYgY2xhc3M9ImJ1YmJsZS10ZXh0Ij7kvaDlpb08L2Rpdj48L2Rpdj4KICAgICAgPGRpdiBjbGFzcz0ibXNnLXJvdyBtZSI+CiAgICAgICAgPGRpdiBjbGFzcz0iYXZhdGFyIG1lIj48L2Rpdj4KICAgICAgICA8ZGl2IGNsYXNzPSJyZWRlbnYtYnViYmxlIG9wZW5lZCI+CiAgICAgICAgICA8ZGl2IGNsYXNzPSJ0b3AiPgogICAgICAgICAgICA8ZGl2IGNsYXNzPSJyZWRlbnYtaWNvbiI+8J+npzwvZGl2PgogICAgICAgICAgICA8ZGl2IGNsYXNzPSJtc2ciPuaBreWWnOeZvOiyoe+8jOWkp+WQieWkp+WIqTwvZGl2PgogICAgICAgICAgPC9kaXY+CiAgICAgICAgICA8ZGl2IGNsYXNzPSJib3R0b20iPjxzcGFuPue0heWMhTwvc3Bhbj48c3Bhbj7lsI/nvo7lt7LpoJjlj5Y8L3NwYW4+PC9kaXY+CiAgICAgICAgPC9kaXY+CiAgICAgIDwvZGl2PgogICAgICA8ZGl2IGNsYXNzPSJtc2ctcm93Ij48ZGl2IGNsYXNzPSJhdmF0YXIiPjwvZGl2PjxkaXYgY2xhc3M9ImJ1YmJsZS10ZXh0Ij7orJ3orJ3kvaDvvIHmlLbliLDllaYg8J+YijwvZGl2PjwvZGl2PgogICAgPC9kaXY+CiAgICA8ZGl2IGNsYXNzPSJib3R0b20tZml4ZWQiIHN0eWxlPSJwYWRkaW5nOjEwcHggMTJweDsiPgogICAgICA8ZGl2IHN0eWxlPSJoZWlnaHQ6MzZweDtib3JkZXI6MXB4IHNvbGlkIHZhcigtLWxpbmUpO2JvcmRlci1yYWRpdXM6MThweDsiPjwvZGl2PgogICAgPC9kaXY+CiAgPC9kaXY+CgogIDwhLS0gU0NSRUVOIDY6IOeZvOmAgeaWueafpeeci+mgmOWPluips+aDhSAtLT4KICA8ZGl2IGNsYXNzPSJzY3JlZW4iIGlkPSJzNiI+CiAgICA8ZGl2IGNsYXNzPSJzdGF0dXNiYXIiPjwvZGl2PgogICAgPGRpdiBjbGFzcz0ibmF2YmFyIj4KICAgICAgPGRpdiBjbGFzcz0ic2lkZSBiYWNrIj7igLk8L2Rpdj4KICAgICAgPGRpdiBjbGFzcz0iY2VudGVyIj7ntIXljIXoqbPmg4U8L2Rpdj4KICAgICAgPGRpdiBjbGFzcz0ic2lkZSI+PC9kaXY+CiAgICA8L2Rpdj4KICAgIDxkaXYgY2xhc3M9ImRldGFpbC1jYXJkIj4KICAgICAgPGRpdiBjbGFzcz0iZGV0YWlsLWhlYWQiPgogICAgICAgIDxkaXYgY2xhc3M9ImFtdCI+TlQkMjAwPC9kaXY+CiAgICAgICAgPGRpdiBjbGFzcz0iYmxlc3NpbmciPuaBreWWnOeZvOiyoe+8jOWkp+WQieWkp+WIqTwvZGl2PgogICAgICA8L2Rpdj4KICAgICAgPGRpdiBjbGFzcz0iZGV0YWlsLXN0YXR1cyI+CiAgICAgICAgPGRpdiBjbGFzcz0ibGluZSI+MSDkurrlt7LpoJjlj5bvvIzlhbEgTlQkMjAwPC9kaXY+CiAgICAgIDwvZGl2PgogICAgICA8ZGl2IGNsYXNzPSJkZXRhaWwtcm93Ij4KICAgICAgICA8ZGl2IGNsYXNzPSJhdiI+PC9kaXY+CiAgICAgICAgPGRpdiBjbGFzcz0ibmFtZSI+5bCP576OPC9kaXY+CiAgICAgICAgPGRpdiBjbGFzcz0ic3RhdGUgY2xhaW1lZCI+5bey6aCY5Y+WIMK3IDE1OjExPC9kaXY+CiAgICAgIDwvZGl2PgogICAgICA8ZGl2IGNsYXNzPSJtZXRhLWluZm8iPgogICAgICAgIOeZvOmAgeaZgumWk++8mjIwMjYvMDgvMDYgMTU6MDk8YnI+CiAgICAgICAg57SF5YyF5Zau6Jmf77yaUkUyMDI2MDgwNjE1MDkxMjM0NTxicj4KICAgICAgICDpoJjlj5bmnJ/pmZDvvJoyMDI2LzA4LzA3IDE1OjA577yI6YC+5pyf6Ieq5YuV6YCA5Zue6Yyi5YyF77yJCiAgICAgIDwvZGl2PgogICAgPC9kaXY+CiAgPC9kaXY+CgogIDwhLS0gU0NSRUVOIDc6IOeVsOW4uCAtIOmkmOmhjeS4jei2syAtLT4KICA8ZGl2IGNsYXNzPSJzY3JlZW4iIGlkPSJzNyI+CiAgICA8ZGl2IGNsYXNzPSJzdGF0dXNiYXIiPjwvZGl2PgogICAgPGRpdiBjbGFzcz0ibmF2YmFyIj4KICAgICAgPGRpdiBjbGFzcz0ic2lkZSBiYWNrIj7igLk8L2Rpdj4KICAgICAgPGRpdiBjbGFzcz0iY2VudGVyIj7ntIXljIU8L2Rpdj4KICAgICAgPGRpdiBjbGFzcz0ic2lkZSI+PC9kaXY+CiAgICA8L2Rpdj4KICAgIDxkaXYgc3R5bGU9ImZsZXg6MTtkaXNwbGF5OmZsZXg7ZmxleC1kaXJlY3Rpb246Y29sdW1uO2FsaWduLWl0ZW1zOmNlbnRlcjtqdXN0aWZ5LWNvbnRlbnQ6Y2VudGVyO3BhZGRpbmc6MzBweDt0ZXh0LWFsaWduOmNlbnRlcjsiPgogICAgICA8ZGl2IHN0eWxlPSJ3aWR0aDo2NHB4O2hlaWdodDo2NHB4O2JvcmRlci1yYWRpdXM6NTAlO2JhY2tncm91bmQ6I0ZERUNFQztkaXNwbGF5OmZsZXg7YWxpZ24taXRlbXM6Y2VudGVyO2p1c3RpZnktY29udGVudDpjZW50ZXI7Zm9udC1zaXplOjMwcHg7Ij7imqDvuI88L2Rpdj4KICAgICAgPGRpdiBzdHlsZT0iZm9udC1zaXplOjE1cHg7Zm9udC13ZWlnaHQ6NjAwO21hcmdpbi10b3A6MTZweDsiPumMouWMhemkmOmhjeS4jei2szwvZGl2PgogICAgICA8ZGl2IHN0eWxlPSJmb250LXNpemU6MTIuNXB4O2NvbG9yOnZhcigtLWluay1zb2Z0KTttYXJnaW4tdG9wOjhweDtsaW5lLWhlaWdodDoxLjY7Ij4KICAgICAgICDnm67liY3ppJjpoY0gTlQkNTDvvIzkuI3otrPku6XmlK/ku5ggTlQkMjAwIOeahOe0heWMheOAgjxicj7oq4vlhLLlgLzlvozlho3oqabkuIDmrKHjgIIKICAgICAgPC9kaXY+CiAgICAgIDxidXR0b24gY2xhc3M9ImJ0bi1wcmltYXJ5IiBzdHlsZT0ibWFyZ2luLXRvcDoyNHB4O3dpZHRoOjIwMHB4OyI+56uL5Y2z5YSy5YC8PC9idXR0b24+CiAgICA8L2Rpdj4KICA8L2Rpdj4KCiAgPCEtLSBTQ1JFRU4gODog55Ww5bi4IC0g5bCN5pa55pyq6ZaL6YCa6Zu26Yyi5YyFIC0tPgogIDxkaXYgY2xhc3M9InNjcmVlbiIgaWQ9InM4Ij4KICAgIDxkaXYgY2xhc3M9InN0YXR1c2JhciI+PC9kaXY+CiAgICA8ZGl2IGNsYXNzPSJuYXZiYXIiPgogICAgICA8ZGl2IGNsYXNzPSJzaWRlIGJhY2siPuKAuTwvZGl2PgogICAgICA8ZGl2IGNsYXNzPSJjZW50ZXIiPue0heWMhTwvZGl2PgogICAgICA8ZGl2IGNsYXNzPSJzaWRlIj48L2Rpdj4KICAgIDwvZGl2PgogICAgPGRpdiBzdHlsZT0iZmxleDoxO2Rpc3BsYXk6ZmxleDtmbGV4LWRpcmVjdGlvbjpjb2x1bW47YWxpZ24taXRlbXM6Y2VudGVyO2p1c3RpZnktY29udGVudDpjZW50ZXI7cGFkZGluZzozMHB4O3RleHQtYWxpZ246Y2VudGVyOyI+CiAgICAgIDxkaXYgc3R5bGU9IndpZHRoOjY0cHg7aGVpZ2h0OjY0cHg7Ym9yZGVyLXJhZGl1czo1MCU7YmFja2dyb3VuZDojRkZGNEUwO2Rpc3BsYXk6ZmxleDthbGlnbi1pdGVtczpjZW50ZXI7anVzdGlmeS1jb250ZW50OmNlbnRlcjtmb250LXNpemU6MzBweDsiPvCfkZs8L2Rpdj4KICAgICAgPGRpdiBzdHlsZT0iZm9udC1zaXplOjE1cHg7Zm9udC13ZWlnaHQ6NjAwO21hcmdpbi10b3A6MTZweDsiPumWi+mAmumbtumMouWMheaJjeiDveaUtuS4i+e0heWMhTwvZGl2PgogICAgICA8ZGl2IHN0eWxlPSJmb250LXNpemU6MTIuNXB4O2NvbG9yOnZhcigtLWluay1zb2Z0KTttYXJnaW4tdG9wOjhweDtsaW5lLWhlaWdodDoxLjY7Ij4KICAgICAgICDpm7bpjKLljIXnlKjkvoblrZjmlL7ntIXljIXnrYnlsI/poY3mrL7poIXvvIw8YnI+5Y+q6ZyA57Ch5Zau6Kit5a6a5Y2z5Y+v5a6M5oiQ6ZaL6YCa44CCCiAgICAgIDwvZGl2PgogICAgICA8YnV0dG9uIGNsYXNzPSJidG4tcHJpbWFyeSIgc3R5bGU9Im1hcmdpbi10b3A6MjRweDt3aWR0aDoyMDBweDtiYWNrZ3JvdW5kOnZhcigtLWJyYW5kLWdvbGQpO2NvbG9yOiM1QTNFMTI7Ij7nq4vljbPplovpgJo8L2J1dHRvbj4KICAgICAgPGRpdiBzdHlsZT0iZm9udC1zaXplOjExLjVweDtjb2xvcjp2YXIoLS1pbmstc29mdCk7bWFyZ2luLXRvcDoxNHB4OyI+6ZaL6YCa5a6M5oiQ5b6M5bCH6Ieq5YuV54K65L2g5ouG6ZaL57SF5YyFPC9kaXY+CiAgICA8L2Rpdj4KICA8L2Rpdj4KCjwvZGl2PgoKPGRpdiBjbGFzcz0iY2FwdGlvbiI+CiAg56S65oSP6YeN6bue77ya4pGjIOaLhue0heWMheWJjeeCuuOAjOWwgeWPo+OAjeeLgOaFi++8jOm7nuaTiuW+jOaJjeinuOeZvOmWi+WVn+WLleeVq++8mzxicj4KICDikaQg6aCY5Y+W5b6M6IGK5aSp5rCj5rOh55Sx57SF6L2J6YeR5Lim6aGv56S644CM5bey6aCY5Y+W44CN77yb4pGmIOeCuumkmOmhjeS4jei2s+eahOS4reaWt+a1geeoi++8m+KRqCDngrrlsI3mlrnlsJrmnKrplovpgJrpm7bpjKLljIXmmYLnmoTlvJXlsI7jgIIKPC9kaXY+Cgo8c2NyaXB0Pgpkb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCcudGFiJykuZm9yRWFjaCh0YWI9PnsKICB0YWIuYWRkRXZlbnRMaXN0ZW5lcignY2xpY2snLCgpPT57CiAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCcudGFiJykuZm9yRWFjaCh0PT50LmNsYXNzTGlzdC5yZW1vdmUoJ2FjdGl2ZScpKTsKICAgIGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3JBbGwoJy5zY3JlZW4nKS5mb3JFYWNoKHM9PntzLmNsYXNzTGlzdC5yZW1vdmUoJ2FjdGl2ZScpO3Muc3R5bGUuZGlzcGxheT0nbm9uZSc7fSk7CiAgICB0YWIuY2xhc3NMaXN0LmFkZCgnYWN0aXZlJyk7CiAgICBjb25zdCB0YXJnZXQ9ZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQodGFiLmRhdGFzZXQudGFyZ2V0KTsKICAgIHRhcmdldC5zdHlsZS5kaXNwbGF5PSdmbGV4JzsKICAgIHRhcmdldC5jbGFzc0xpc3QuYWRkKCdhY3RpdmUnKTsKICB9KTsKfSk7Cjwvc2NyaXB0Pgo8L2JvZHk+CjwvaHRtbD4K"
                class="proto-frame" loading="lazy"></iframe>
            </div>
            <div class="proto-hint-box">
              <span>💡 提示：此為可直接點擊互動之線框稿，點選分頁可切換各流程畫面，並可上下捲動。</span>
              <!-- <span style="color:#64748b;">畫面解析度標準：iOS 375×812 pt</span> -->
            </div>
          </div>
        </section>

        <!-- Section 3: Specifications -->
        <section id="3">
          <h2><span class="chapter-badge">03</span>功能規格說明</h2>

          <!-- 3.1 Overview -->
          <div id="31">
            <h3><span class="sub-badge">3.1</span> 使用者流程總覽</h3>
            <p>本功能拆為三條關鍵流程：<strong>發送</strong>、<strong>領取</strong>（含對方未開通零錢包的例外分支）、<strong>逾時退回</strong>。</p>

            <!-- Flow 1 -->
            <div class="flow-card">
              <div class="flow-header">
                <div class="flow-title">📌 流程一：發送流程</div>
                <span class="flow-badge">發送者端</span>
              </div>
              <div class="mermaid">
                flowchart TD
                A[點擊紅包<br />進入發紅包頁] --> B[輸入金額<br />填寫金額與祝福語]
                B --> C{檢查額度<br />金額與餘額是否足夠}
                C -- 超額或不足 --> C1[提示錯誤<br />返回輸入頁修改]
                C -- 通過 --> D[支付驗證<br />錢包密碼或生物辨識]
                D -- 驗證失敗 --> D1[提示錯誤<br />並中斷流程]
                D -- 驗證成功 --> E[建立紅包<br />推送聊天氣泡給雙方]
              </div>
            </div>

            <!-- Flow 2 -->
            <div class="flow-card">
              <div class="flow-header">
                <div class="flow-title">📌 流程二：領取流程（含未開通例外）</div>
                <span class="flow-badge">接收者端</span>
              </div>
              <div class="mermaid">
                flowchart TD
                F[點擊紅包<br />接收者查看訊息] --> G{狀態檢查<br />是否待領取或已過期}
                G -- 已過期 --> G1[顯示過期提示]
                G -- 待領取 --> H{錢包檢查<br />是否已開通零錢包}
                H -- 未開通 --> H1[引導開通零錢包]
                H1 -- 開通完成 --> I
                H -- 已開通 --> I[拆紅包<br />點擊封口動畫]
                I --> J[入帳完成<br />更新雙方聊天氣泡]
              </div>
            </div>

            <!-- Flow 3 -->
            <div class="flow-card">
              <div class="flow-header">
                <div class="flow-title">📌 流程三：逾時退回流程</div>
                <span class="flow-badge">系統背景排程</span>
              </div>
              <div class="mermaid">
                flowchart LR
                K[排程掃描<br />找出逾時未領紅包] --> L[退款處理<br />退回發送者錢包] --> M[更新狀態<br />推送雙方系統通知]
              </div>
            </div>

            <blockquote>
              <p>三張流程圖對應畫面①～⑨，細節規格見 3.2。「錢包檢查」是本版新增的例外分支：接收者若從未開通過零錢包，需先完成簡易開通才能入帳，見畫面⑨與 3.5 例外情境。</p>
            </blockquote>
          </div>

          <!-- 3.2 Screen Components -->
          <div id="32">
            <h3><span class="sub-badge">3.2</span> 頁面元件規格</h3>

            <!-- Screen 1 -->
            <div class="screen-card" id="screen-1">
              <div class="screen-card-header">
                <div class="screen-card-title-group">
                  <!-- <span class="screen-number">①</span> -->
                  <h4 class="screen-card-title">畫面①：發紅包輸入頁</h4>
                </div>
                <div class="screen-tags">
                  <span class="tag-role sender">發送者視角</span>
                  <span class="tag-role both">核心起點</span>
                </div>
              </div>
              <div class="screen-card-body">
                <div class="table-responsive">
                  <table>
                    <thead>
                      <tr>
                        <th style="width:180px;">元件名稱</th>
                        <th>規格與行為定義</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td>導覽列</td>
                        <td>左：返回；中：「紅包」；右：「記錄」入口（進入「我的紅包記錄」列表）</td>
                      </tr>
                      <tr>
                        <td>紅包卡片</td>
                        <td>顯示收款對象暱稱（帶入聊天對象，不可更改）</td>
                      </tr>
                      <tr>
                        <td>金額輸入框</td>
                        <td>純數字鍵盤；預設空白／可設定「常用金額」快捷按鈕（如 100/200/500/1000）；最小 NT$1，最大依單筆限額；不允許輸入小數點後超過 2 位</td>
                      </tr>
                      <tr>
                        <td>額度提示文案</td>
                        <td>「單筆上限 NT$X，今日剩餘額度 NT$Y」，即時依已用額度更新</td>
                      </tr>
                      <tr>
                        <td>祝福語輸入框</td>
                        <td>預設文案「恭喜發財，大吉大利」；可清空自行輸入；上限 40 字；即時字數統計；需做敏感詞過濾</td>
                      </tr>
                      <tr>
                        <td>支付方式列</td>
                        <td>MVP 僅支援「錢包餘額」單一支付來源，顯示目前餘額，不提供切換其他支付工具的入口</td>
                      </tr>
                      <tr>
                        <td>領取期限說明</td>
                        <td>固定文案：「24 小時內，逾期自動退回」</td>
                      </tr>
                      <tr>
                        <td>主按鈕「塞錢進紅包 NT$X」</td>
                        <td>金額為 0 或未輸入時按鈕呈灰階不可點擊；按鈕文案內金額需與輸入框同步</td>
                      </tr>
                      <tr>
                        <td>次要提示文字</td>
                        <td>「紅包發出後不可撤回，請確認金額與對象」</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
                <div class="screen-notes-box">
                  <div class="screen-notes-title">⚡ 互動與前端防護細節</div>
                  <ul>
                    <li>金額輸入框需做防抖與格式化（如自動加千分位顯示，但實際傳輸值為純數字）。</li>
                    <li>點擊「塞錢進紅包」時前端先做基本驗證（金額 &gt; 0、未超過單筆/當日限額、餘額足夠），驗證失敗於當頁 Toast 提示，不進入下一步。</li>
                    <li>驗證通過才呼叫下一步（支付密碼頁）。</li>
                  </ul>
                </div>
              </div>
            </div>

            <!-- Screen 2 -->
            <div class="screen-card" id="screen-2">
              <div class="screen-card-header">
                <div class="screen-card-title-group">
                  <!-- <span class="screen-number">②</span> -->
                  <h4 class="screen-card-title">畫面②：支付密碼確認</h4>
                </div>
                <div class="screen-tags">
                  <span class="tag-role sender">發送者視角</span>
                  <span class="tag-role exception">安全驗證</span>
                </div>
              </div>
              <div class="screen-card-body">
                <div class="callout callout-info" style="margin-top:0;">
                  <div class="callout-title">💡 支付範圍說明</div>
                  <p>紅包的資金來源僅限「錢包餘額」，因此支付驗證只有一種路徑（App 內建支付密碼／生物辨識），不涉及信用卡直接扣款需導向發卡行 3D 驗證頁、或行動支付（Apple
                    Pay／Google Pay／LINE Pay 等）SDK
                    驗證失敗需顯示第三方回傳錯誤等分支。若餘額不足，導向既有「儲值」流程即可。
                  </p>
                </div>
                <div class="table-responsive">
                  <table>
                    <thead>
                      <tr>
                        <th style="width:180px;">元件名稱</th>
                        <th>規格與行為定義</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td>摘要區</td>
                        <td>顯示「確認發送紅包給 {暱稱}」、金額、祝福語（唯讀）</td>
                      </tr>
                      <tr>
                        <td>密碼點位</td>
                        <td>6 位數字密碼點狀輸入，逐位填滿</td>
                      </tr>
                      <tr>
                        <td>生物辨識</td>
                        <td>若裝置支援 Face ID / 指紋，且使用者已開啟此設定，優先跳出系統原生生物辨識彈窗，成功後略過數字鍵盤</td>
                      </tr>
                      <tr>
                        <td>數字鍵盤</td>
                        <td>3×4 標準鍵盤，含隨機亂數排列設定與刪除鍵</td>
                      </tr>
                      <tr>
                        <td>錯誤處理</td>
                        <td>密碼錯誤：Toast「密碼錯誤，還可輸入 N 次」；連續錯誤 5 次鎖定支付密碼 30 分鐘並提示走「忘記密碼」流程</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
                <div class="screen-notes-box">
                  <div class="screen-notes-title">⚡ 互動與事務細節</div>
                  <ul>
                    <li>密碼驗證與扣款需在同一支付服務事務內完成（見 3.4 交易一致性）。</li>
                    <li>支付成功後：關閉此頁，返回聊天室並自動捲動到最新訊息（新紅包氣泡）。</li>
                    <li>支付失敗（如餘額不足、風控攔截）：導向對應中斷頁（見畫面⑧、3.5 例外情境）。</li>
                  </ul>
                </div>
              </div>
            </div>

            <!-- Screen 3 & 6 -->
            <div class="screen-card" id="screen-3-6">
              <div class="screen-card-header">
                <div class="screen-card-title-group">
                  <!-- <span class="screen-number">③/⑥</span> -->
                  <h4 class="screen-card-title">畫面③／⑥：聊天室內紅包氣泡</h4>
                </div>
                <div class="screen-tags">
                  <span class="tag-role both">雙方視角</span>
                  <span class="tag-role sender">即時訊息</span>
                </div>
              </div>
              <div class="screen-card-body">
                <div class="table-responsive">
                  <table>
                    <thead>
                      <tr>
                        <th style="width:200px;">狀態分類</th>
                        <th>視覺樣式與文字呈現</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td>待領取（發送者視角）</td>
                        <td>紅色氣泡＋祝福語預覽＋「紅包」標籤＋副標「點擊查看」</td>
                      </tr>
                      <tr>
                        <td>待領取（接收者視角）</td>
                        <td>同上紅色氣泡，但點擊後直接導向拆紅包全螢幕頁而非詳情頁</td>
                      </tr>
                      <tr>
                        <td>已領取</td>
                        <td>氣泡底色由紅轉金／灰，副標改為「{暱稱}已領取」，發送者與接收者兩端皆同步即時更新</td>
                      </tr>
                      <tr>
                        <td>已過期（未領取超過 24hr）</td>
                        <td>氣泡呈現灰階，副標「該紅包已過期」，點擊後可查看詳情但不可再拆開領取</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
                <div class="screen-notes-box">
                  <div class="screen-notes-title">⚡ 即時通道與快取同步</div>
                  <ul>
                    <li>氣泡狀態需為「即時」或「近即時」同步（建議透過既有聊天訊息的長輪詢/WebSocket 通道推送狀態變更事件，不需要使用者手動刷新）。</li>
                    <li>同一則紅包訊息在雙方聊天視窗中的訊息 ID 需一致，狀態變更需同時更新兩端本地訊息列表快取。</li>
                  </ul>
                </div>
              </div>
            </div>

            <!-- Screen 4 & 5 -->
            <div class="screen-card" id="screen-4-5">
              <div class="screen-card-header">
                <div class="screen-card-title-group">
                  <!-- <span class="screen-number">④/⑤</span> -->
                  <h4 class="screen-card-title">畫面④／⑤：全螢幕拆紅包頁</h4>
                </div>
                <div class="screen-tags">
                  <span class="tag-role receiver">接收者視角</span>
                  <span class="tag-role both">儀式感體驗</span>
                </div>
              </div>
              <div class="screen-card-body">
                <div class="table-responsive">
                  <table>
                    <thead>
                      <tr>
                        <th style="width:180px;">元件名稱</th>
                        <th>規格與動效定義</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td>頂部資訊</td>
                        <td>發送者頭像、暱稱「來自 {發送者} 的紅包」</td>
                      </tr>
                      <tr>
                        <td>祝福語</td>
                        <td>置中大字顯示，如「恭喜發財，大吉大利」</td>
                      </tr>
                      <tr>
                        <td>封口圖示</td>
                        <td>點擊觸發拆開動畫（建議 400–600ms 的縮放/翻轉動效），動畫結束才顯示金額，避免使用者感覺「秒到帳」缺乏儀式感</td>
                      </tr>
                      <tr>
                        <td>結果顯示</td>
                        <td>金額大字呈現（如 NT$200）、「已存入零錢包」提示</td>
                      </tr>
                      <tr>
                        <td>主按鈕「收下了」</td>
                        <td>點擊後返回聊天室</td>
                      </tr>
                      <tr>
                        <td>次要連結「查看紅包詳情」</td>
                        <td>導向詳情頁，顯示發送時間、單號等</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
                <div class="screen-notes-box">
                  <div class="screen-notes-title">⚡ 互動防重細節</div>
                  <ul>
                    <li>已領取過的紅包，若使用者再次點擊氣泡，直接導向「已拆」結果頁（不可重複拆／不可重複入帳）。</li>
                  </ul>
                </div>
              </div>
            </div>

            <!-- Screen 7 (Restored & Enhanced) -->
            <div class="screen-card" id="screen-7">
              <div class="screen-card-header">
                <div class="screen-card-title-group">
                  <!-- <span class="screen-number">⑦</span> -->
                  <h4 class="screen-card-title">畫面⑦：發送方-領取詳情</h4>
                </div>
                <div class="screen-tags">
                  <span class="tag-role sender">發送者視角</span>
                  <span class="tag-role both">狀態追蹤</span>
                </div>
              </div>
              <div class="screen-card-body">
                <div class="table-responsive">
                  <table>
                    <thead>
                      <tr>
                        <th style="width:180px;">元件名稱</th>
                        <th>規格與行為定義</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td>導覽列</td>
                        <td>左：返回按鈕；中：「紅包詳情」；右：無或「說明」</td>
                      </tr>
                      <tr>
                        <td>紅包金額與祝福語卡片</td>
                        <td>置中大字呈現紅包總金額（如 NT$200）與原設定之祝福語文案</td>
                      </tr>
                      <tr>
                        <td>領取狀態摘要</td>
                        <td>顯示「1 人已領取，共 NT$200」或「尚未被領取，將於 XX:XX 過期」或「已逾時退回發送者錢包」</td>
                      </tr>
                      <tr>
                        <td>領取明細列表</td>
                        <td>顯示收款人頭像、暱稱（如「小美」）、領取狀態與時間（如「已領取 · 15:11」）</td>
                      </tr>
                      <tr>
                        <td>底部詮釋資訊 (Metadata)</td>
                        <td>發送時間（精確至分/秒）、紅包單號（唯一 ID）、領取截止期限（24 小時後）</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
                <div class="screen-notes-box">
                  <div class="screen-notes-title">⚡ 互動與退款關聯</div>
                  <ul>
                    <li>發送者可隨時從聊天室自身發出的紅包氣泡點擊進入，確認好友是否已領取。</li>
                    <li>若紅包超過 24 小時未被領取，狀態變更為「已逾時退回」，並提示退回至錢包之時間與交易單號。</li>
                  </ul>
                </div>
              </div>
            </div>

            <!-- Screen 8 (Restored & Enhanced) -->
            <div class="screen-card" id="screen-8">
              <div class="screen-card-header">
                <div class="screen-card-title-group">
                  <!-- <span class="screen-number">⑧</span> -->
                  <h4 class="screen-card-title">畫面⑧：異常-餘額不足（發送方）</h4>
                </div>
                <div class="screen-tags">
                  <span class="tag-role sender">發送者視角</span>
                  <span class="tag-role exception">異常中斷流程</span>
                </div>
              </div>
              <div class="screen-card-body">
                <div class="table-responsive">
                  <table>
                    <thead>
                      <tr>
                        <th style="width:180px;">元件名稱</th>
                        <th>規格與行為定義</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td>提示圖示＋文案</td>
                        <td>警告圓形圖示 ⚠️；主標「錢包餘額不足」；副標提示「目前錢包餘額 NT$X，發送此紅包尚差 NT$Y」</td>
                      </tr>
                      <tr>
                        <td>主按鈕「立即儲值」</td>
                        <td>導向現有儲值/綁卡流程，儲值成功後可選擇是否自動返回發紅包頁並保留原輸入金額</td>
                      </tr>
                      <tr>
                        <td>次要操作「修改金額」</td>
                        <td>返回畫面①修改發送金額，保留原收款對象與祝福語，避免使用者重複填寫</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
                <div class="screen-notes-box">
                  <div class="screen-notes-title">⚡ 攔截與回復機制</div>
                  <ul>
                    <li>發送者於輸入頁提交或支付密碼頁時，系統若偵測錢包可用餘額不足，立即攔截並進入此頁，禁止直接發出請求。</li>
                    <li>完成儲值返回後，自動刷新錢包餘額資訊。</li>
                  </ul>
                </div>
              </div>
            </div>

            <!-- Screen 9 -->
            <div class="screen-card" id="screen-9">
              <div class="screen-card-header">
                <div class="screen-card-title-group">
                  <!-- <span class="screen-number">⑨</span> -->
                  <h4 class="screen-card-title">畫面⑨：異常-對方未開通零錢包（接收者視角）</h4>
                </div>
                <div class="screen-tags">
                  <span class="tag-role receiver">接收者視角</span>
                  <span class="tag-role exception">金流認證分支</span>
                </div>
              </div>
              <div class="screen-card-body">
                <div class="callout callout-info" style="margin-top:0;">
                  <div class="callout-title">💡 金流合規與邊界情況說明</div>
                  <p>
                    零錢包能實際收受金額，本質上是金融/支付產品，開通流程通常涉及既有錢包/會員系統既定的身份驗證規則（手機號碼驗證等基本層級或依法規要求需要更完整的實名認證，如證件上傳、人臉辨識），不一定能即時完成。此畫面只負責「導去既有開通流程」，不假設能秒速接續拆紅包；實際驗證規則與所需時間由既有錢包/會員系統定義。
                  </p>
                  <p style="margin-top:6px;">紅包功能的前提就是 App
                    本身已具備錢包/金流基礎功能，「對方完全未開通」理論上多發生在全新或從未使用過金流功能的使用者身上。</p>
                </div>
                <div class="table-responsive">
                  <table>
                    <thead>
                      <tr>
                        <th style="width:200px;">情境 / 元件</th>
                        <th>規格與行為定義</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td>觸發時機</td>
                        <td>接收者點擊紅包氣泡，系統偵測其帳號尚未完成錢包開通/驗證</td>
                      </tr>
                      <tr>
                        <td>提示文案</td>
                        <td>「開通零錢包才能收下紅包」＋一句話說明（需完成身份驗證後才能存入款項）</td>
                      </tr>
                      <tr>
                        <td>主按鈕「立即開通」</td>
                        <td>導向既有錢包/會員系統的開通與身份驗證流程（實際步驟沿用既有機制，本次不重新定義）</td>
                      </tr>
                      <tr>
                        <td>驗證為即時完成（如僅需手機驗證）</td>
                        <td>導回領取流程，進入畫面④拆紅包頁</td>
                      </tr>
                      <tr>
                        <td>驗證非即時（如需審核）</td>
                        <td>提示「驗證中，完成後可回到此則訊息領取」，紅包維持「待領取」狀態，接收者需自行回來點擊完成領取，並非系統自動接續</td>
                      </tr>
                      <tr>
                        <td>若在領取期限內未完成驗證</td>
                        <td>比照一般規則於 24 小時後過期，金額自動全數退回發送者</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>

          </div>

          <!-- 3.3 Business Rules -->
          <div id="33">
            <h3><span class="sub-badge">3.3</span> 業務規則</h3>
            <div class="table-responsive">
              <table>
                <thead>
                  <tr>
                    <th style="width:180px;">規則項目</th>
                    <th style="width:200px;">建議預設值</th>
                    <th>詳細說明與風控考量</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td>單筆金額上限</td>
                    <td>NT$2,000（依法規/風控調整）</td>
                    <td>依用戶身分認證等級（實名/未實名）分級設定</td>
                  </tr>
                  <tr>
                    <td>單筆金額下限</td>
                    <td>NT$1</td>
                    <td>避免 0 元或負數</td>
                  </tr>
                  <tr>
                    <td>每日發送額度</td>
                    <td>NT$8,000</td>
                    <td>依帳號分級可調整，需可由後台設定</td>
                  </tr>
                  <tr>
                    <td>每日發送筆數上限</td>
                    <td>20 筆</td>
                    <td>防止異常刷單/洗錢風險</td>
                  </tr>
                  <tr>
                    <td>祝福語字數</td>
                    <td>40 字</td>
                    <td>需過濾敏感詞、URL、聯絡方式（防詐騙話術）</td>
                  </tr>
                  <tr>
                    <td>領取期限</td>
                    <td>24 小時</td>
                    <td>逾時系統自動退回原支付帳戶，並發送系統通知</td>
                  </tr>
                  <tr>
                    <td>支付驗證</td>
                    <td>支付密碼或生物辨識</td>
                    <td>每筆皆須驗證，不可記住密碼跳過</td>
                  </tr>
                  <tr>
                    <td>好友關係限制</td>
                    <td>僅限已互為好友的一對一聊天</td>
                    <td>非好友/已封鎖狀態不顯示紅包入口或直接阻擋</td>
                  </tr>
                  <tr>
                    <td>重複領取</td>
                    <td>嚴格禁止</td>
                    <td>後端需以「紅包 ID + 接收者 ID」做唯一鎖，防止併發重複入帳</td>
                  </tr>
                  <tr>
                    <td>撤回</td>
                    <td>不支援</td>
                    <td>紅包發出即扣款成功，不提供撤回；僅能等待對方拒領或超時退回</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>

          <!-- 3.4 Technical & Data Design (For RD) -->
          <div id="34-rd">
            <h3><span class="sub-badge">3.4</span> 系統流程與資料設計（供參考）</h3>

            <div class="callout callout-tech">
              <div class="callout-title">⚙️ 核心資料表設計：<code>red_envelope</code></div>
              <p>一對一紅包核心資料表，包含唯一識別、金額、狀態機與去重冪等鍵：</p>
            </div>

            <div class="table-responsive">
              <table>
                <thead>
                  <tr>
                    <th style="width:160px;">欄位名稱</th>
                    <th style="width:140px;">型別</th>
                    <th>欄位說明與索引定義</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td><code>id</code></td>
                    <td><span class="badge-type">VARCHAR(32)</span></td>
                    <td>紅包單號（Primary Key，唯一）</td>
                  </tr>
                  <tr>
                    <td><code>sender_id</code></td>
                    <td><span class="badge-type">BIGINT</span></td>
                    <td>發送者 User ID（建立索引）</td>
                  </tr>
                  <tr>
                    <td><code>receiver_id</code></td>
                    <td><span class="badge-type">BIGINT</span></td>
                    <td>接收者 User ID（一對一場景固定單一，建立索引）</td>
                  </tr>
                  <tr>
                    <td><code>conversation_id</code></td>
                    <td><span class="badge-type">BIGINT</span></td>
                    <td>所屬聊天室 ID，供訊息渲染定位與查詢</td>
                  </tr>
                  <tr>
                    <td><code>amount</code></td>
                    <td><span class="badge-type">DECIMAL(10,2)</span></td>
                    <td>紅包金額（精確至小數兩位）</td>
                  </tr>
                  <tr>
                    <td><code>message</code></td>
                    <td><span class="badge-type">VARCHAR(120)</span></td>
                    <td>祝福語文案</td>
                  </tr>
                  <tr>
                    <td><code>status</code></td>
                    <td><span class="badge-type">ENUM</span></td>
                    <td><code>pending</code> 待領取／<code>claimed</code> 已領取／<code>expired</code>
                      已過期已退回／<code>failed</code> 支付失敗未建立</td>
                  </tr>
                  <tr>
                    <td><code>pay_channel</code></td>
                    <td><span class="badge-type">ENUM</span></td>
                    <td>支付管道，MVP 預設 <code>wallet</code>（錢包餘額）</td>
                  </tr>
                  <tr>
                    <td><code>created_at</code></td>
                    <td><span class="badge-type">DATETIME</span></td>
                    <td>建立發送時間</td>
                  </tr>
                  <tr>
                    <td><code>claimed_at</code></td>
                    <td><span class="badge-type">DATETIME|NULL</span></td>
                    <td>領取時間（未拆為 NULL）</td>
                  </tr>
                  <tr>
                    <td><code>expire_at</code></td>
                    <td><span class="badge-type">DATETIME</span></td>
                    <td>過期時間（建立時間 + 24 小時）</td>
                  </tr>
                  <tr>
                    <td><code>idempotency_key</code></td>
                    <td><span class="badge-type">VARCHAR(64)</span></td>
                    <td>前端請求去重鍵，防止使用者連續重複點擊造成重複扣款（唯一約束）</td>
                  </tr>
                </tbody>
              </table>
            </div>

            <!-- Detailed Flows -->
            <div class="screen-card">
              <div class="screen-card-header">
                <div class="screen-card-title-group">
                  <h4 class="screen-card-title" style="margin:0;">核心 API 流程步驟說明</h4>
                </div>
              </div>
              <div class="screen-card-body">
                <p><strong>關鍵流程一：發送 API</strong></p>
                <ol>
                  <li>前端送出「建立紅包」請求（含 <code>idempotency_key</code>）。</li>
                  <li>後端：檢查好友關係、額度、風控規則 → 呼叫支付/錢包服務凍結並扣款 → 扣款成功後寫入 <code>red_envelope</code>（status=pending）→
                    寫入一則聊天訊息（type=red_envelope, ref_id=紅包單號）→ 透過即時通道推送給雙方裝置。</li>
                  <li>扣款失敗 → 回傳對應錯誤碼（餘額不足/超額/風控攔截），前端導向對應中斷頁，不建立訊息。</li>
                </ol>

                <p style="margin-top:20px;"><strong>關鍵流程二：領取 API</strong></p>
                <ol>
                  <li>接收者點擊紅包訊息 → 前端呼叫「領取」API，帶紅包單號。</li>
                  <li>後端先檢查紅包狀態（是否 pending／是否已過期），再檢查接收者的零錢包是否已開通：
                    <ul>
                      <li>若尚未開通 → 回傳「需先開通零錢包」錯誤碼，前端導向畫面⑨引導開通，開通完成後前端自動重新呼叫一次「領取」API。</li>
                      <li>若已開通 → 進入下一步。</li>
                    </ul>
                  </li>
                  <li>以資料庫層級鎖（如 <code>SELECT ... FOR UPDATE</code> 或條件式
                    <code>UPDATE ... WHERE status='pending'</code>）確保同一紅包只會被扣成功一次，即使前端重複觸發或多裝置同時點擊。
                  </li>
                  <li>領取成功 → 金額入接收者錢包 → 更新 <code>status=claimed</code>、<code>claimed_at</code> → 推送雙端聊天氣泡狀態更新事件。</li>
                  <li>若已被領取（重複請求）→ 直接回傳目前狀態與結果，不重複扣款（保證冪等性）。</li>
                </ol>

                <p style="margin-top:20px;"><strong>關鍵流程三：逾時退回排程</strong></p>
                <ul>
                  <li>排程（如每 5–10 分鐘掃描一次，或使用延遲佇列/訊息排程更即時）掃描 <code>status=pending</code> 且
                    <code>expire_at &lt; now()</code> 的紅包 → 退款至發送者錢包 → 更新 <code>status=expired</code> → 推送雙端狀態更新 +
                    系統提示訊息（如「紅包已過期，金額已退回」）。
                  </li>
                </ul>

                <p style="margin-top:20px;"><strong>交易一致性保證：</strong></p>
                <ul>
                  <li>扣款與寫入紅包記錄需在同一分散式交易或採用「先扣款成功再寫入」＋「補償機制（若寫入失敗則退款）」，避免出現「扣款成功但紅包未建立」的孤兒交易。</li>
                  <li>領取入帳與狀態更新需具備冪等鍵與唯一約束，避免高併發下重複入帳。</li>
                  <li>一對一場景僅有 1 位可領取人，併發風險遠低於群紅包（多人搶同一筆），但仍須用「條件式更新（狀態從 pending 轉 claimed
                    時檢查前值）」而非「先查後寫」，避免同一使用者從多裝置或重複請求造成雙重入帳。</li>
                </ul>
              </div>
            </div>
          </div>

          <!-- 3.5 Edge Cases -->
          <div id="35-edge-cases">
            <h3><span class="sub-badge">3.5</span> 例外情境（Edge Cases）</h3>
            <div class="table-responsive">
              <table>
                <thead>
                  <tr>
                    <th style="width:260px;">例外情境</th>
                    <th>系統與前端處理規範</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td>餘額不足（發送方）</td>
                    <td>中斷發送流程，提示差額並導向儲值流程（畫面⑧）</td>
                  </tr>
                  <tr>
                    <td>支付密碼連續錯誤</td>
                    <td>鎖定支付密碼 30 分鐘，提示忘記密碼身分確認流程</td>
                  </tr>
                  <tr>
                    <td>接收方尚未開通零錢包</td>
                    <td>導向開通引導頁（畫面⑨），開通完成後自動接續拆紅包流程；若放棄開通則紅包比照一般規則等待 24 小時後過期退回</td>
                  </tr>
                  <tr>
                    <td>發送方想用信用卡/行動支付直接支付紅包</td>
                    <td>MVP 不支援，僅能用錢包餘額；餘額不足時導向既有儲值流程補足餘額後再送出</td>
                  </tr>
                  <tr>
                    <td>對方已將我刪除／封鎖</td>
                    <td>不顯示紅包入口，或點擊後提示「無法發送」</td>
                  </tr>
                  <tr>
                    <td>對方帳號已被停用／凍結</td>
                    <td>攔截於建立紅包前，提示「對方帳號狀態異常無法接收紅包」</td>
                  </tr>
                  <tr>
                    <td>網路中斷導致「已扣款但未看到結果」</td>
                    <td>前端需具重試查詢機制（依 <code>idempotency_key</code> 查詢最終狀態），避免使用者重複下單造成二次扣款疑慮</td>
                  </tr>
                  <tr>
                    <td>使用者於拆紅包頁反覆快速點擊封口</td>
                    <td>前端點擊節流（Debounce/Throttle）＋後端樂觀鎖冪等處理，僅第一次點擊有效，其餘直接回傳已領取結果</td>
                  </tr>
                  <tr>
                    <td>超過每日發送額度／筆數限制</td>
                    <td>於輸入頁提交時即攔截，Toast 提示「已達今日發送上限」</td>
                  </tr>
                  <tr>
                    <td>兩端裝置系統時間不同步造成過期誤判</td>
                    <td>過期判斷一律以伺服器時間 <code>expire_at</code> 為準，前端僅作顯示渲染，不執行本地過期判定</td>
                  </tr>
                  <tr>
                    <td>紅包金額文字／祝福語含敏感詞或外部聯絡方式</td>
                    <td>送出前後端雙重過濾，違規則提示重新輸入，嚴防詐騙話術（如「加LINE領紅包」等釣魚攻擊）</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>

          <!-- 3.6 Risk & Security -->
          <div id="36_1">
            <h3><span class="sub-badge">3.6</span> 風控與安全考量</h3>
            <div class="callout callout-scope">
              <div class="callout-title">🔒 帳號與資金防護架構</div>
              <ul>
                <li><strong>大額紅包二次驗證</strong>：超過設定門檻（如單筆 &gt; NT$1,000 或當日累計 &gt;
                  NT$5,000）建議觸發二次驗證或延遲入帳（風控審核），降低帳號遭盜用後被快速轉移洗錢之風險。</li>
                <li><strong>異常模式即時監控</strong>：同一帳號短時間內對多個不同對象發送大量紅包（可能為洗錢或詐騙話術配合），應觸發風控告警並可暫時凍結該帳號紅包發送功能。</li>
                <li>
                  <strong>設備與行為指紋</strong>：需防範「同一設備操控多個帳號互轉套利」「自動化腳本高頻發送/領取測試風控邊界」等情形，建議比照現有反詐騙機制，記錄裝置指紋與請求頻率，異常時觸發二次驗證或人工審核。
                </li>
                <li><strong>敏感內容過濾</strong>：祝福語內容需與現有聊天內容審核機制共用敏感詞、釣魚 URL 與非法關鍵字過濾規則。</li>
                <li><strong>全生命週期交易流水</strong>：所有紅包交易（建立、扣款、入帳、退回、開通分支）均須留存不可篡改之完整交易流水日誌，供客服申訴與風控稽核。</li>
              </ul>
            </div>
          </div>

          <!-- 3.7 Scalability Design -->
          <!-- <div id="37">
            <h3><span class="sub-badge">3.7</span> 資料結構延伸性設計（非本次實作範圍）</h3>
            <p>MVP 階段 <code>red_envelope</code> 表以 <code>receiver_id</code>
              單一欄位設計即可支撐一對一場景，不需要為此新增額外表格。僅先在此記錄一個未來擴充方向，供之後若要做群紅包時參考：</p>
            <p>若要擴充<strong>群紅包（多人可領、拼手氣隨機分配）</strong>，建議改為「<strong>紅包主表 +
                領取明細子表（1:N）</strong>」的設計，主表存總金額/總份數/分配演算法，子表存每筆領取紀錄。本次不需要為此調整 MVP 的資料庫設計，僅記錄此方向以降低未來遷移成本。</p>
          </div> -->
        </section>

        <!-- Section 4: Data & Metrics -->
        <section id="4">
          <h2><span class="chapter-badge">04</span>數據追蹤與成效驗證規劃</h2>

          <!-- 4.1 Market & Needs -->
          <div id="41">
            <h3><span class="sub-badge">4.1</span> 市場與需求分析</h3>
            <p><strong>競品參考借鏡</strong></p>
            <ul>
              <li>
                <strong>微信紅包</strong>：一對一/群組紅包皆支援，群紅包採「拼手氣」隨機分配，帶動極高的節慶期間開啟率；本次僅做一對一固定金額，屬於微信紅包的子集，可作為未來擴充群紅包/拼手氣的基礎架構。
              </li>
              <li><strong>LINE Pay</strong>：一對一轉帳無「拆封儀式感」設計，較偏向純工具性；本功能刻意保留「封口＋拆開動畫」的儀式感，強化社交趣味性而非單純轉帳。</li>
              <li><strong>Messenger/WhatsApp 的紅包/禮金模組</strong>（部分地區）：多與節慶活動綁定，作為行銷導流工具。</li>
            </ul>
            <p><strong>建議的用戶訪談重點（正式上線前建議訪談 5–8 位目標用戶）</strong></p>
            <ol>
              <li>使用者在什麼情境下會想在聊天中「發錢」給朋友？（節慶祝福／代墊費用／分帳／單純娛樂打賭）</li>
              <li>對「祝福語」「封口拆開動畫」等社交儀式感元素的重視程度？</li>
              <li>對金額上限、24 小時領取期限等規則的接受度。</li>
              <li>是否介意此功能與轉帳（AA分帳）功能混淆，是否需要分開兩種入口文案（如「紅包」vs「轉帳」）。</li>
            </ol>
            <p><strong>建議調研方法</strong>：可先以問卷/焦點小組確認核心動機與金額習慣區間，再以可用性測試驗證本原型的操作流暢度（尤其是支付密碼頁與拆紅包動畫的體感速度）。</p>
          </div>

          <!-- 4.2 Tracking -->
          <div id="42">
            <h3><span class="sub-badge">4.2</span> 埋點規格與數據追蹤</h3>
            <p>建議至少埋設以下事件，事件命名可依既有埋點規範調整（已依一對一場景精簡）：</p>

            <div class="table-responsive">
              <table>
                <thead>
                  <tr>
                    <th style="width:240px;">事件名稱 (Event ID)</th>
                    <th style="width:200px;">觸發時機</th>
                    <th>上報關鍵屬性 (Attributes)</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td><code>red_envelope_entry_click</code></td>
                    <td>點擊聊天室「紅包」按鈕</td>
                    <td>conversation_id, sender_id</td>
                  </tr>
                  <tr>
                    <td><code>red_envelope_compose_view</code></td>
                    <td>進入輸入頁</td>
                    <td>-</td>
                  </tr>
                  <tr>
                    <td><code>red_envelope_submit_click</code></td>
                    <td>點擊「塞錢進紅包」</td>
                    <td>amount, message_length, is_default_message</td>
                  </tr>
                  <tr>
                    <td><code>red_envelope_submit_fail</code></td>
                    <td>前端/後端驗證失敗</td>
                    <td>fail_reason（餘額不足/超額/風控/其他）</td>
                  </tr>
                  <tr>
                    <td><code>red_envelope_pay_start</code></td>
                    <td>進入支付密碼/生物辨識驗證頁</td>
                    <td>red_envelope_id</td>
                  </tr>
                  <tr>
                    <td><code>red_envelope_pay_fail</code></td>
                    <td>支付驗證失敗</td>
                    <td>fail_reason（密碼錯誤/餘額不足/風控攔截）, attempt_count</td>
                  </tr>
                  <tr>
                    <td><code>red_envelope_pay_success</code></td>
                    <td>支付驗證成功並建立紅包</td>
                    <td>amount, red_envelope_id</td>
                  </tr>
                  <tr>
                    <td><code>red_envelope_bubble_impression</code></td>
                    <td>紅包氣泡在聊天室曝光</td>
                    <td>red_envelope_id, viewer_role（sender/receiver）</td>
                  </tr>
                  <tr>
                    <td><code>red_envelope_open_click</code></td>
                    <td>接收者點擊氣泡進入拆紅包頁</td>
                    <td>red_envelope_id, time_since_sent</td>
                  </tr>
                  <tr>
                    <td><code>red_envelope_wallet_check_fail</code></td>
                    <td>接收者未開通/未驗證錢包（畫面⑨觸發）</td>
                    <td>red_envelope_id, wallet_status（not_activated/pending_verification）</td>
                  </tr>
                  <tr>
                    <td><code>red_envelope_claim_success</code></td>
                    <td>拆開並成功入帳</td>
                    <td>red_envelope_id, amount, time_since_sent</td>
                  </tr>
                  <tr>
                    <td><code>red_envelope_expired</code></td>
                    <td>系統判定逾時退回</td>
                    <td>red_envelope_id</td>
                  </tr>
                  <tr>
                    <td><code>red_envelope_detail_view</code></td>
                    <td>發送者查看領取詳情頁</td>
                    <td>red_envelope_id</td>
                  </tr>
                </tbody>
              </table>
            </div>

            <div class="screen-card">
              <div class="screen-card-header">
                <div class="screen-card-title-group">
                  <h4 class="screen-card-title" style="margin:0;">數據監控指標與資料品質體系</h4>
                </div>
              </div>
              <div class="screen-card-body">
                <p><strong>通用屬性（建議每個事件都攜帶）</strong>：<br>
                  <code>user_id</code>、<code>is_new_user</code>（近 7
                  日新註冊，用於分析「對方未開通錢包」情境的實際發生比例）、<code>device_type</code>、<code>app_version</code>、<code>conversation_id</code>。
                </p>

                <p><strong>核心監測指標（Dashboard 建議）</strong>：</p>
                <ul>
                  <li><strong>紅包功能滲透率</strong>＝發送過紅包的一對一聊天室數 / 活躍一對一聊天室總數</li>
                  <li><strong>發送成功率</strong>＝<code>pay_success</code> / <code>submit_click</code></li>
                  <li><strong>平均發送金額與分佈區間</strong></li>
                  <li><strong>拆開率</strong>＝<code>claim_success</code> / <code>pay_success</code></li>
                  <li><strong>平均拆開時效</strong>（<code>time_since_sent</code> 中位數）</li>
                  <li><strong>逾時退回率</strong>＝<code>expired</code> / <code>pay_success</code></li>
                  <li><strong>因餘額不足而中斷的比例</strong>（判斷是否需要更輕量的儲值引導）</li>
                  <li><strong>對方未開通錢包觸發率</strong>＝<code>wallet_check_fail</code> /
                    <code>open_click</code>（用來驗證這個邊界情況實際發生頻率，決定畫面⑨的開通體驗值不值得再投入優化）
                  </li>
                </ul>

                <p><strong>數據品質監控（上線後建議每日對帳）</strong>：</p>
                <ul>
                  <li><strong>完整性</strong>：<code>submit_click</code> 數量應約等於 <code>pay_start</code> 數量，落差過大代表前端埋點可能有遺漏。
                  </li>
                  <li><strong>一致性</strong>：<code>pay_success</code> 加總金額，理論上應等於 <code>claim_success</code> 加總金額 ＋
                    <code>expired</code> 加總金額 ＋ 仍在待領取狀態（尚未到期）的金額。
                  </li>
                  <li><strong>財務對帳</strong>：每日與既有錢包/金流系統的交易紀錄做金額核對，避免「埋點顯示發送成功，但實際未扣款/入帳」的落差，這類落差通常代表交易一致性（3.4 節）出了問題。
                  </li>
                </ul>
              </div>
            </div>
          </div>

          <!-- 4.3 A/B Testing -->
          <div id="43-ab">
            <h3><span class="sub-badge">4.3</span> 成效驗證方法（A/B 測試設計）</h3>
            <p><strong>假設（Hypothesis）</strong>：在一對一聊天中提供紅包功能，能提升聊天室互動頻率與留存，而非單純是「轉帳」的替代品被冷落。</p>

            <div class="table-responsive">
              <table>
                <thead>
                  <tr>
                    <th style="width:160px;">實驗維度</th>
                    <th>設計規範與執行細節</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td>實驗分組</td>
                    <td>實驗組開放紅包入口／對照組維持現狀（入口存在但點擊無反應，或延後開放），用戶隨機分配，確保兩組在新舊用戶比例、地區、裝置等特徵上分佈相近。</td>
                  </tr>
                  <tr>
                    <td>樣本量規劃</td>
                    <td>正式跑實驗前先用統計檢定力（Power）試算所需最小樣本數（如抓 80% 檢定力、5% 顯著水準），避免樣本太小導致「有效果但測不出來」的假陰性結論。</td>
                  </tr>
                  <tr>
                    <td>實驗週期</td>
                    <td>建議至少涵蓋一個完整使用者行為週期（如 7 天），並避開重大節慶（如過年）以免結果被特殊時段放大或干擾，除非本來就是想驗證節慶情境下的效果。</td>
                  </tr>
                  <tr>
                    <td>主要指標 (North Star)</td>
                    <td>開通紅包功能的用戶，其<strong>次週聊天室 DAU／留存率</strong>是否顯著高於對照組；一對一聊天室的<strong>人均訊息互動次數</strong>是否提升。</td>
                  </tr>
                  <tr>
                    <td>次要指標</td>
                    <td>紅包滲透率、拆開率；錢包儲值轉換率是否因「餘額不足」中斷流程而提升（驗證是否帶動支付生態使用）。</td>
                  </tr>
                  <tr>
                    <td>護欄指標 (Guardrails)</td>
                    <td>客訴率／風控攔截率不應顯著上升；因誤觸/誤解額度規則、或畫面⑨開通卡關產生的客服工單量未明顯增加。</td>
                  </tr>
                  <tr>
                    <td>統計檢定方法</td>
                    <td>比例型指標（如發送率、拆開率、留存率）：用 Z 檢定比較兩組比例差異（α=0.05，p &lt; 0.05 視為顯著）；連續型指標（如互動次數、平均金額）：用 T 檢定。</td>
                  </tr>
                </tbody>
              </table>
            </div>

            <p><strong>決策矩陣</strong></p>
            <div class="table-responsive">
              <table>
                <thead>
                  <tr>
                    <th style="width:140px;">實驗結果</th>
                    <th style="width:280px;">判斷標準</th>
                    <th>產品上線決策</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td><span style="color:#059669;font-weight:700;">🟢 顯著提升</span></td>
                    <td>核心指標 p &lt; 0.05，且護欄指標無顯著惡化</td>
                    <td><strong>全量上線</strong></td>
                  </tr>
                  <tr>
                    <td><span style="color:#d97706;font-weight:700;">🟡 無顯著差異</span></td>
                    <td>核心指標 p ≥ 0.05</td>
                    <td>保留原版，或針對假設中的具體環節（如儀式感、文案、場景引導）優化後再測一輪</td>
                  </tr>
                  <tr>
                    <td><span style="color:#dc2626;font-weight:700;">🔴 顯著下降</span></td>
                    <td>核心指標顯著變差，或護欄指標明顯惡化</td>
                    <td>暫緩全量上線，回溯分析原因（如額度規則造成誤解、⑨開通流程卡關過多）</td>
                  </tr>
                </tbody>
              </table>
            </div>

            <div class="callout callout-tech">
              <div class="callout-title">🔍 進階分析與判讀邏輯</div>
              <ul>
                <li>
                  <strong>漏斗分析</strong>：<code>entry_click → submit_click → pay_success → claim_success</code>，定位主要流失環節。若流失集中在
                  <code>pay_start → pay_success</code>，通常代表支付驗證體驗有問題；若集中在
                  <code>open_click → claim_success</code>，需檢查是否為畫面⑨（未開通錢包）卡住了太多人。
                </li>
                <li><strong>分群分析</strong>：新用戶 vs 老用戶、有無觸發畫面⑨例外流程的使用者，分開比較轉化率與滿意度，確認這條例外路徑是否顯著拖累整體體驗。</li>
                <li><strong>防範留存選擇偏誤</strong>：單純比較「發送過紅包的用戶」與「沒發送過的用戶」留存率，容易把「本來就比較活躍的人」誤判為「功能帶來的效果」，因此仍需以隨機分組的 A/B
                  測試結果為準，相關性分析只能當輔助參考。</li>
                <li>
                  <strong>判讀邏輯</strong>：若實驗組在統計顯著水準下於留存/互動指標優於對照組，且護欄指標無顯著惡化，則判定功能有效，可全量上線；若僅拉升「發送/拆開」等功能自身指標，但未反映在留存或互動頻率上，則說明功能本身有使用但未創造真正的社交黏著價值，需檢視是否為儀式感/文案/場景引導不足（如未在節慶/生日等情境主動提示使用）。
                </li>
              </ul>
            </div>
          </div>
        </section>

        <!-- Section 5: Assumptions & Dependencies -->
        <section id="5">
          <h2><span class="chapter-badge">05</span>前提假設</h2>

          <div class="screen-card">
            <div class="screen-card-header">
              <div class="screen-card-title-group">
                <h4 class="screen-card-title" style="margin:0;">前提假設</h4>
              </div>
            </div>
            <div class="screen-card-body">
              <ul>
                <li><strong>紅包資金來源 MVP 階段僅支援「錢包餘額」</strong>，不支援信用卡直接扣款或行動支付作為紅包支付管道；額度分級沿用既有錢包/實名認證機制，不重新設計 KYC 流程。</li>
                <li><strong>延伸功能排期</strong>：自訂紅包封面、手續費、聊天輸入區自然語言捷徑等延伸功能均不在本次範圍，待 MVP 上線驗證數據後再評估。</li>
                <li><strong>群組紅包排期</strong>：群組紅包（拼手氣隨機分配）非本次範圍，僅在 3.7 節記錄未來資料結構擴充方向。</li>
                <li><strong>金流架構本身不在本次規格重新設計</strong>：多數社交/通訊 App 不會自建持牌電子支付，而是優先串接既有的行動支付/電子支付服務（如 LINE
                  Pay、微信支付、街口支付等），只有平台規模夠大、有明確金融策略時才會自行申請執照自建。本規格書假設「錢包餘額」這個概念已經存在（不論背後是自建還是串接第三方），紅包功能只呼叫既有的扣款/入帳/驗證介面，不重新定義金流架構。
                </li>
              </ul>
            </div>
          </div>

        </section>




      </article>
      <p>※ 此文件使用Claude、Perplexity、Gemini Pro 協助生成。</p>
    </main>
  </div>

  <!-- Back to top floating button -->
  <a href="#top" class="back-to-top" title="回到頂部" aria-label="回到頂部">↑</a>

  <script>
    // Initialize Mermaid diagrams
    mermaid.initialize({ startOnLoad: true, theme: "neutral", securityLevel: "loose" });

    // Reading Scroll Progress Bar & ScrollSpy
    const progressBar = document.getElementById('progressBar');
    const tocLinks = document.querySelectorAll('.toc-link');
    const sections = Array.from(tocLinks).map(link => {
      const id = link.getAttribute('href').replace('#', '');
      return document.getElementById(id);
    }).filter(Boolean);

    window.addEventListener('scroll', () => {
      const winScroll = document.documentElement.scrollTop || document.body.scrollTop;
      const height = document.documentElement.scrollHeight - document.documentElement.clientHeight;
      const scrolled = (winScroll / height) * 100;
      if (progressBar) {
        progressBar.style.width = scrolled + '%';
      }

      // ScrollSpy Active Tracking
      let currentSectionId = '';
      sections.forEach(section => {
        const sectionTop = section.offsetTop - 120;
        if (winScroll >= sectionTop) {
          currentSectionId = section.getAttribute('id');
        }
      });

      if (currentSectionId) {
        tocLinks.forEach(link => {
          if (link.getAttribute('href') === '#' + currentSectionId) {
            link.classList.add('active');
          } else {
            link.classList.remove('active');
          }
        });
      }
    });

    // Toggle Prototype Viewport Height
    function toggleProtoHeight(btn) {
      const frame = document.querySelector('.proto-frame');
      if (!frame) return;
      if (frame.style.height === '920px') {
        frame.style.height = '780px';
        btn.textContent = '↕ 調整視窗高度';
      } else {
        frame.style.height = '920px';
        btn.textContent = '↕ 還原標準高度';
      }
    }
  </script>
</body>

</html>
