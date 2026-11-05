# UI Testing Lab: Robot Framework & Cypress

This repository contains automated UI test suites for **Selenium Web Form** and **QuickPizza** as part of ITDS362 Software Quality Assurance and Testing.

The two chosen tools for this lab are:
1. **Robot Framework** (Part 3)
2. **Cypress** (Part 4)

---

## Directory Overview & Running Tests

### 1. Robot Framework (`robot/`)
* **Description:** Contains keyword-driven test scenarios (`first.robot`, `web_form.robot`, and `pizza_scenario.robot`) utilizing `SeleniumLibrary` and a modular Page Object resource file (`resources/quickpizza.resource`). The test evidence for full runs is stored in `robot/results/`.
* **Prerequisites:** Python 3.10+ and Google Chrome matching `CHROMEDRIVER`. Activate virtual environment (`.\.venv\Scripts\Activate.ps1` or `source .venv/bin/activate`).
* **Run Command:**
  ```powershell
  cd robot
  robot -d results/run1 -v BROWSER:headlesschrome pizza_scenario.robot
  ```

---

### 2. Cypress (`cypress/`)
* **Description:** Contains end-to-end spec files (`cypress/e2e/web-form.cy.js` and `cypress/e2e/pizza-scenario.cy.js`) with custom commands (`cypress/support/commands.js`) and API interception for QuickPizza. Full run videos and screenshots are recorded in `cypress/cypress/videos/` and `cypress/cypress/screenshots/`.
* **Prerequisites:** Node.js 22+ and npm. Run `npm install` inside the `cypress/` directory.
* **Run Command:**
  ```powershell
  cd cypress
  npx cypress run --browser chrome
  ```
