# UI lab results

Tool parts that I chose:
- Part 3: Robot Framework
- Part 4: Cypress

## Part 0
- Step 0.1, Chrome version: 157.0.8103.39
- Step 0.3, ChromeDriver version: 157.0.8103.39
- Step 0.4, locator of Pizza, Please!: `button[name='pizza-please']` (or `name=pizza-please`)
- Step 0.4, locator of Excluded tools: `css=select` (or `select`)
- Step 0.4, locator of Must be vegetarian: `id=default-checkbox` (or `#default-checkbox`)
- Step 0.4, locator of Custom Pizza Name: `input#pizza-name`
- Step 0.5, pizza name, tool and calories of the run by hand: Lab Pizza 20261105-154729, Pizza cutter, 400 calories per slice

| Step | Actual result | Pass or fail |
|------|---------------|--------------|
| 1 | The heading reads "Looking to break out of your pizza routine?" | Pass |
| 2 | Advanced switch toggled on; Max Calories per Slice, Excluded tools, Must be vegetarian, and Custom Pizza Name appear | Pass |
| 3 | Fields show 600, Knife selected in Excluded tools, vegetarian checkbox ticked, and pizza name "Lab Pizza 20261105-154729" | Pass |
| 4 | Recommendation shows pizza name "Lab Pizza 20261105-154729", tool "Pizza cutter" (different from Knife), and 400 calories per slice (<= 600) | Pass |
| 5 | Clicked "Love it!" button without logging in; message displays "Please log in first." in #rate-result | Pass |
| 6 | Navigated to /admin, signed in as admin/admin; page shows "Latest pizza recommendations" | Pass |
| 7 | Found pizza "Lab Pizza 20261105-154729" in the list with tool "Pizza cutter", matching step 4 | Pass |

## Part 3: Robot Framework
- robot --version: Robot Framework 7.5.1 (Python 3.14.7 on win32)
- Step 3.2, failure message for the wrong title: Title should have been 'Web Form' but was 'Web form'.
- Step 3.8, run 1, result, elapsed time and pizza name: PASS, 8.70 s, Lab Pizza 20261105-154729
- Step 3.8, run 2, result, elapsed time and pizza name: PASS, 8.44 s, Lab Pizza 20261105-154805

## Part 4: Cypress
- node --version, npm --version and npx cypress version: node v24.19.0, npm 11.17.0, npx cypress 16.2.1
- Step 4.3, failure message for the wrong title: Timed out retrying after 10000ms: expected 'Web form' to equal 'Web Form'
- Step 4.6, tool and calories in the answer of POST /api/pizza: Tool: Scissors, Calories per slice: 400
- Step 4.7, the line of the admin list that shows your pizza: Lab Pizza 20261105-155149 (tool=Scissors, ingredients_number=5) (newest)
- Step 4.8, run 1, result, duration and pizza name: All specs passed! (2 passed), 00:10, Lab Pizza 20261105-155149
- Step 4.8, run 2, result, duration and pizza name: All specs passed! (2 passed), 00:10, Lab Pizza 20261105-155301
- Step 4.9 (optional), browser and result: Microsoft Edge 156 (headless), All specs passed! (2 passed, duration 00:10)

## Part B
1. The tests locate the "Max Calories per Slice" box through its preceding label rather than its id because all three number input boxes on the QuickPizza page share the identical attribute `id="floating_filled"`. In standard HTML, an id must be unique across the document, so querying by `#floating_filled` matches three distinct elements (`$$('#floating_filled').length` returns 3) and cannot distinguish the calories field reliably. To allow tests to use a direct ID, the developers should assign unique IDs to each field, such as `id="max-calories-per-slice"`, or attach a dedicated testing attribute like `data-testid="max-calories"`. This change helps testers by providing an unambiguous, fast, and resilient locator that does not depend on DOM hierarchy or label text phrasing that could change during localization or redesigns.

2. Step 7 compares the tool displayed in the administrative recommendation list with the tool recorded during the customer recommendation step; this check is a Consistency Check oracle (also known as a cross-referencing or invariant test oracle). It verifies that different representations and views of the same entity within the system remain synchronized and consistent. One fault in QuickPizza that this check would catch is a backend persistence or data-mapping bug where the recommendation engine selects a tool (e.g., `Scissors`), but the database insertion routine either drops the value, reverts to a hardcoded default like `Knife`, or misattributes the record to another concurrent session. In our test run, both the recommendation banner and the admin entry confirmed `tool=Scissors`, proving data integrity across subsystems.

3. Test case QP-01 currently checks that the vegetarian checkbox is selected and that the HTTP request sends `vegetarian: true`, but it never validates whether the recommended ingredients are actually vegetarian. To check this on the page, the test should inspect the `<ul>` list under `Ingredients:` in the recommendation block and verify that every listed item belongs to an approved vegetarian list, or conversely assert that no banned non-vegetarian items (such as `Ham`, `Pepperoni`, `Bacon`, `Chicken`, or `Beef`) appear in the ingredient list. Alternatively, using Cypress `cy.intercept`, the test can assert directly on the JSON response payload with `expect(response.body.pizza.vegetarian).to.be.true` alongside an assertion against `response.body.pizza.ingredients`. To implement this check, the test requires either a defined test oracle containing the recipe ingredient dictionary from QuickPizza, or access to the backend response object to verify the classification flag.

4. Between Robot Framework and Cypress, the two tools demonstrated distinct approaches to automated UI testing. In terms of waiting, Robot Framework relies on explicit SeleniumLibrary keywords like `Wait Until Element Is Visible` with a configured timeout (`timeout=10s`), whereas Cypress automatically retries every `cy.get` and DOM assertion up to `defaultCommandTimeout: 10000` without requiring manual wait declarations. Writing QP-01 was considerably faster and more fluent in Cypress thanks to native JavaScript method chaining and intuitive request interception via `cy.intercept()`, whereas Robot Framework required maintaining strict tabular whitespace indentation and separating keywords into a resource file. For output readability, Cypress generated clear `.mp4` video recordings showing the real-time Command Log alongside page execution, while Robot Framework produced comprehensive `log.html` and `report.html` files with expandable keyword execution trees and screenshots. In our runs, Robot Framework completed in approximately 8.5 seconds per run, while Cypress completed its entire spec suite in 10 seconds.

5. For the automated UI tests in Project 2, I would choose Cypress as the primary automation tool. First, Cypress's built-in automatic waiting and retry mechanism dramatically reduces test flakiness on modern, asynchronous web applications without cluttering test code with repetitive wait statements. Second, Cypress provides native network interception (`cy.intercept`), allowing tests to validate backend API responses and simulate edge cases seamlessly while automatically generating video recordings and screenshots for comprehensive test submission evidence.

6. In Robot Framework (Part 3), if QuickPizza changed the locator of the calories box, we would only need to update the locator in one place: the `${MAX CALORIES}` variable defined inside `robot/resources/quickpizza.resource` (line 11). The resource file acts as a Page Object layer that encapsulates all DOM locators and technical steps behind human-readable business keywords such as `Fill In The Pizza Request`. Because the test file `pizza_scenario.robot` only invokes this keyword and never references the raw XPath, changes in page implementation are completely isolated from the scenario files, keeping maintenance overhead minimal.

7. In Cypress (Part 4), QP-01 must be executed as a single test because QuickPizza is deployed across multiple backend instances where a session cookie keeps a browser connected to one specific server holding that session's created pizzas. By design, Cypress clears all cookies, browser storage, and state between separate `it()` blocks to ensure test independence. If steps 6 and 7 were isolated into a second test, the session cookies would be wiped, causing the subsequent admin visit to potentially connect to a different backend node or unlinked session where the pizza generated in Step 4 does not exist in the "Latest pizza recommendations" list, resulting in an element assertion timeout and test failure.
