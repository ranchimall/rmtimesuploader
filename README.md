# RM Times Content Creator — Article Publisher Agent

> A client-side web application for parsing Microsoft Word (`.docx`) documents, organizing article structure, previewing content, and publishing directly to RanchiMall Times.

---

## Contents

- [Overview](#overview)
- [Publishing Workflow](#publishing-workflow)
- [Integrated Technologies](#integrated-technologies)
- [Key Features](#key-features)
- [User Guide & Step-by-Step Walkthrough](#user-guide--step-by-step-walkthrough)
  - [1. Uploading the Article Document](#1-uploading-the-article-document)
  - [2. Structure Review & Section Selection](#2-structure-review--section-selection)
  - [3. Fullscreen Selection Mode](#3-fullscreen-selection-mode)
  - [4. Editing Titles & Headings](#4-editing-titles--headings)
  - [5. Confirming & Building the Article](#5-confirming--building-the-article)
  - [6. Exporting JSON & Preview HTML](#6-exporting-json--preview-html)
  - [7. Publishing to RanchiMall Times](#7-publishing-to-ranchimall-times)
- [Interface Reference](#interface-reference)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Technical Architecture](#technical-architecture)

---

## Overview

The **Article Publisher Agent** streamlines and automates the entire publishing pipeline for **RanchiMall Times**. Authors and editors can upload standard Microsoft Word (`.docx`) documents, automatically parse and classify headings/sections, review and fine-tune article structure with live controls, preview the formatted output, and publish securely to RanchiMall SuperNodes.

All document processing and cryptographic operations are executed entirely within the client's browser environment—ensuring complete data privacy and high performance.

---

## Publishing Workflow

The complete end-to-end publishing pipeline consists of seven distinct stages:

| Stage | Component | Purpose |
| :---: | :--- | :--- |
| **1** | **Browser Client** | Reads the `.docx` file locally using **JSZip** and **DOMParser**. No server-side processing occurs. |
| **2** | **Content Extraction** | Extracts headings, paragraphs, tables, formatting, and embedded images while preserving document structure. |
| **3** | **GitHub Image Hosting** | Uploads extracted images to the `ranchimall/rmtimes` repository and replaces local references with hosted image URLs. |
| **4** | **Cryptographic Authentication** | Uses `floCrypto.js` to derive the author's FLO identity from their WIF key and digitally authenticate the submission. |
| **5** | **Decentralized Submission** | Sends the authenticated publishing request to RanchiMall SuperNodes via `floCloudAPI.sendGeneralData()`. |
| **6** | **Editorial Approval** | RM Times administrators review the submitted article before publication. |
| **7** | **Publication** | Once approved, the article becomes available on the live RM Times website. |

---

## Integrated Technologies

| Technology | Role |
| :--- | :--- |
| **JSZip** | Reads and extracts `.docx` archives entirely in the browser. |
| **DOMParser** | Parses Word XML into HTML content. |
| **GitHub REST API** | Hosts embedded images and hero images in the `ranchimall/rmtimes` repository. |
| **floCrypto.js** | Generates FLO identity and signs submissions. |
| **floCloudAPI** | Stores authenticated publishing requests on RanchiMall SuperNodes. |

---

## Key Features

* **Pure Client-Side Parsing**: Extracts text, inline styles, bullet lists, embedded tables, and images directly in the browser.
* **Smart Heading Classifier**: Auto-identifies potential section headings based on Word styles and typography heuristics.
* **Clean Section Grouping**: Groups paragraphs dynamically under `Section 1`, `Section 2`, `Section 3`, etc.
* **Fullscreen Selection Mode**: Expands the entire structure review panel to full viewport height (`100vw × 100vh`) with ample vertical space for long articles.
* **Inline Header Editing**: Rename any section header or document title directly inside the UI.
* **Collapsible Sections**: Expand or collapse individual sections or all sections at once.
* **One-Click Export**:
  * **JSON Export**: Clean, structured article payload with headings, sections, read-time calculation, and metadata.
  * **HTML Preview**: Standalone, fully styled HTML file ready for local offline verification.
* **Direct SuperNode Publishing**: Direct integration with RanchiMall SuperNodes and Floyd blockchain identity.

---

## User Guide

### 1. Uploading the Article Document
1. Open `article_publisher_agent_latest.html` in any modern web browser.
2. Click **"Choose Article .docx File"** and select your Word document.
3. The parser processes the document and automatically displays the **Review Detected Structure** panel.

### 2. Structure Review & Section Selection
* **Checkbox Logic**:
  * **Checked Items**: Treated as **Section Titles** (rendered as `<h2>` headings in the published article).
  * **Unchecked Items**: Treated as standard **Body Paragraphs** nested under the preceding section header.
* **Dynamic Re-numbering**: Checking or unchecking an item immediately updates the structure and renumbers sections (`Section 1`, `Section 2`, etc.) in real-time.
* **Collapse / Expand All**: Use the toolbar buttons to quickly collapse all sections for a bird's-eye view, or expand them to inspect body text.

### 3. Fullscreen Selection Mode
For long articles with dozens of sections and paragraphs:
1. Click the **`⛶ Fullscreen View`** button in the top-right corner of the Review panel.
2. The panel expands to fill your entire browser window.
3. Enjoy an expanded, high-visibility workspace with smooth scrolling.
4. To exit:
   * Click **`✕ Exit Fullscreen`**
   * Or press the **`Escape`** (`Esc`) key on your keyboard.

### 4. Editing Titles & Headings
* **Article Title**: Edit the main document title directly in the **"Article Title"** input box.
* **Section Titles**: Click the **"Edit"** button next to any section header row to rename it inline. Press `Enter` or click `Save` to apply.

### 5. Confirming & Building the Article
1. Once your structure is configured, click **"Confirm & Build Article"**.
2. A green confirmation badge displays article stats (Section count, estimated reading time, image count).
3. The review checklist automatically collapses to keep the page tidy, and can be reopened anytime by clicking **"Review / Edit Sections"**.

### 6. Exporting JSON & Preview HTML
Once confirmed, two download buttons appear near the top file picker:
* **Download JSON**: Generates a formatted JSON payload containing the structured article data.
* **Download Preview HTML**: Generates a self-contained HTML preview file matching the RanchiMall Times visual layout.

### 7. Publishing to RanchiMall Times
1. *(Optional)* Click **"Choose Hero Image"** to attach a cover banner for the article.
2. Choose your Login Type (e.g., Keystore file, Floyd key, or direct credentials).
3. Click **"Start Direct Creator & Publish"**.
4. Monitor the live log feed to verify upload, image sync, and blockchain transaction confirmation.

---

## Interface Reference

| Control / Element | Location | Description |
| :--- | :--- | :--- |
| **Choose Article .docx File** | Top Area | Uploads and parses the input `.docx` file. |
| **⛶ Fullscreen View** | Review Header | Expands the selection checklist to full screen (`Esc` to exit). |
| **Article Title Input** | Review Top | Editable field for the main publication headline. |
| **Collapse All / Expand All** | Review Toolbar | Toggles all section bodies simultaneously. |
| **Section Item Checkbox** | Item Row | Toggles between Section Title (Checked) and Paragraph (Unchecked). |
| **Edit Button** | Section Row | Inline editor for renaming section headers. |
| **Reset to Detected** | Review Footer | Reverts all selections and edits back to the initial auto-detected state. |
| **Confirm & Build Article** | Review Footer | Finalizes the structure, calculates read time, and prepares publish payload. |
| **Download JSON / Preview** | File Picker Area | Exports the generated article schema or HTML preview. |

---

##  Keyboard Shortcuts

| Shortcut | Context | Action |
| :--- | :--- | :--- |
| `Esc` | Fullscreen Mode | Exits Fullscreen View back to standard inline layout. |
| `Enter` | Section Title Inline Edit | Saves the modified section title. |
| `Esc` | Section Title Inline Edit | Cancels inline editing and restores previous title text. |

---

## Technical Architecture

```
[ .docx Word Document ]
         │
         ▼
[ Client-Side OOXML Parser (JSZip & DOMParser) ]
  ├── Document Body XML (`word/document.xml`)
  ├── Styles Mapping (`word/styles.xml`)
  ├── Relationships & Media (`word/media/*`)
  └── Tables & Formatting
         │
         ▼
[ Flat Items Representation ]
  ├── Auto-Classification (Heuristics & Styles)
  └── Interactive Heading State Map
         │
         ▼
[ Structure Review UI (Normal / Fullscreen) ]
  ├── Live DOM Grouping & Renumbering (`Section N`)
  ├── Inline Title Renaming
  └── Toggleable Section Nodes
         │
         ▼
[ Content Sanitization (DOMPurify) & Payload Builder ]
         │
         ├──► [ JSON & Preview HTML Export ]
         ├──► [ GitHub REST API Image Hosting ]
         └──► [ RanchiMall SuperNodes via floCloudAPI & floCrypto.js ]
```

---

## 📄 License & Attribution

Internal tooling developed for **RanchiMall Times**.
All document processing and cryptographic operations are executed locally within the client's browser environment.
