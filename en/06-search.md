# 6. Search

### Search field input + submit

- Type a valid product name into the search field and press **Enter** — verify that a results page loads with relevant products.
- Type a valid product name into the search field and click the **search icon/button** — verify the same results page loads (both submission methods work).
- Type a search term and submit via keyboard shortcut (Enter) while focus is inside the field — verify no page-level form submission conflict occurs.
- Click into the search field without typing anything and press Enter — verify the system either prevents submission or shows a meaningful empty-query response (not a crash or blank page).
- Tab to the search field using keyboard only, type a term, and press Enter — verify full keyboard-accessible submission works.
- Type a single character (e.g., "a") and submit — verify results load without errors (short query edge case).
- Type 200+ characters in the search field and submit — verify the field accepts or gracefully truncates input without throwing a server error.
- Submit a search on mobile using the virtual keyboard "Search/Go" button — verify the results page loads correctly on mobile.
- Verify the search field has a visible placeholder text that hints at the expected input (e.g., "Search products…").
- Verify the search field is correctly labeled for screen readers (accessible label or aria-label present).

### Autocomplete / suggest

- Type 2–3 characters of a known product name — verify a dropdown with autocomplete suggestions appears within 300ms.
- Type a partial brand name — verify brand suggestions appear in the autocomplete dropdown.
- Type a partial category name — verify category suggestions appear (if supported).
- Select a suggestion from the autocomplete dropdown using a mouse click — verify the user is taken directly to the correct product or results page.
- Navigate the autocomplete dropdown using arrow keys and press Enter to select — verify keyboard navigation works correctly.
- Type quickly and pause — verify the dropdown debounces correctly and does not fire a request on every single keystroke.
- Type a term with no matches — verify the autocomplete either shows "No suggestions" or closes gracefully rather than showing an empty broken dropdown.
- Verify autocomplete suggestions show product images and/or prices (if the design requires it).
- Type a misspelled term with 1–2 typos — verify autocomplete still surfaces relevant suggestions (fuzzy matching).
- Press Escape while the autocomplete dropdown is open — verify the dropdown closes and focus returns to the search field.
- Submit via autocomplete suggestion selection — verify the selected term populates the search field before navigating.
- On mobile, verify the autocomplete dropdown does not get hidden behind the virtual keyboard.

### Search results display

- Search for a product that exists — verify results show product image, name, price, and an "Add to Cart" or link-to-product action.
- Verify the total result count is displayed (e.g., "42 results for 'chair'").
- Verify the search term is echoed back on the results page (e.g., "Results for: chair").
- Click on a result item — verify the correct product detail page opens.
- Verify results are paginated or implement infinite scroll consistently (no duplicate items, no missing items at page boundaries).
- Change the number of results per page (if the option exists) — verify the correct count appears and pagination updates accordingly.
- Verify that out-of-stock products are either excluded from results or clearly marked as unavailable.
- Search for a product with a long name — verify the product name in the result card does not overflow or break the layout.
- Verify that product cards in search results maintain consistent image aspect ratios (no stretched or missing images).
- Verify structured data / rich snippets are not broken on the search results page (page still renders valid HTML).

### Search results sorting

- On the search results page, sort by **Price: Low to High** — verify results are reordered accordingly with the cheapest item first.
- Sort by **Price: High to Low** — verify results are reordered with the most expensive item first.
- Sort by **Relevance** (default) — verify the most closely matching product appears first.
- Sort by **Newest** — verify the most recently added products appear first.
- Sort by **Name A–Z** — verify alphabetical ascending order.
- Sort by **Name Z–A** — verify alphabetical descending order.
- Sort by **Rating** — verify highest-rated products appear first.
- Change the sort option and verify the URL updates (or state is maintained) so the sorted view is shareable/bookmarkable.
- After selecting a sort order, navigate to page 2 — verify the sort order is preserved across pages.
- Verify that sorting does not reset active filters.

### Search results filtering

- Apply a **price range** filter — verify only products within the selected price range are shown.
- Apply a **category** filter — verify only products from that category are shown.
- Apply a **brand** filter — verify only products of that brand are shown.
- Apply a **color** filter — verify only products with that color variant are shown.
- Apply a **size** filter — verify only products available in that size are shown.
- Apply a **rating** filter (e.g., 4 stars and above) — verify only products meeting that threshold appear.
- Apply a filter and verify the result count updates immediately to reflect the filtered set.
- Apply a filter and navigate to the next page — verify the filter remains active.
- Apply a filter and then change the sort order — verify both are applied simultaneously.
- Verify that filter options show item counts (e.g., "Leather (12)") so the user knows how many results each option yields.
- Verify that filter options with 0 results are either hidden or shown as disabled/grayed out.

### No results display

- Search for a term that definitely does not exist in the catalog (e.g., "xyzabc123notaproduct") — verify a "No results found" message is shown.
- Verify the no-results page does not show an error, a blank white page, or a 404.
- Verify the no-results page includes the searched term in the message (e.g., "No results for 'xyzabc123'").
- Verify the no-results page offers helpful alternatives: a search field to try again, suggested popular searches, or featured/recommended products.
- Verify a "Did you mean…?" suggestion is shown when the zero-results query is close to a real product name.
- Verify a navigation link back to the homepage or top categories is present on the no-results page.
- Search for a valid term, apply filters that eliminate all results — verify the no-results state within filtered search is handled the same way (not a crash).

### Typo tolerance (fuzzy search)

- Search for a product name with one character transposed (e.g., "chiar" instead of "chair") — verify correct products are returned.
- Search with a missing letter (e.g., "cushin") — verify results still surface the correct product.
- Search with one extra letter (e.g., "cusshion") — verify results are returned.
- Search with a common German–English typo or locale-specific misspelling — verify the system handles it gracefully.
- Search with two simultaneous typos — verify whether results are returned or a "did you mean?" prompt is shown.
- Verify that fuzzy results are ranked so the closest match appears first, not last.
- Search for a product using its plural form when the catalog uses singular (or vice versa) — verify results appear (stemming/lemmatization).

### Synonyms

- Search for a synonym of a product category (e.g., "couch" instead of "sofa") — verify relevant products are returned.
- Search for an informal/colloquial name for a product (e.g., "throw" instead of "decorative pillow") — verify results appear.
- Search using a German term on an English shop (or vice versa if multilingual) — verify the behavior is defined (either shows results via synonym mapping or clearly shows none).
- Verify that synonym mappings are consistent: if "couch → sofa" returns results, then "sofa" should return at least the same set.
- Search for an acronym or abbreviation (e.g., "DK" for "dark") — verify whether synonym expansion applies.
- After a synonym-matched search, verify the highlighted terms in results reflect the actual synonym used, not misleading text.

### Search by article number / SKU

- Enter a known SKU/article number exactly — verify the exact product is returned as the top result or the only result.
- Enter a partial SKU (first 4–5 characters) — verify relevant products matching the partial code are returned (if partial SKU search is supported).
- Enter an SKU in lowercase when the catalog stores it in uppercase — verify case-insensitive matching works.
- Enter a completely invalid/non-existent SKU — verify the no-results page is shown cleanly.
- Enter an SKU with hyphens or special formatting (e.g., "AB-123-X") — verify the search handles the special characters and still finds the product.
- Verify that a product found by SKU shows the correct product detail page when clicked.

### Search by category

- Search for a category name (e.g., "armchairs") — verify the category page or a filtered product list is returned, not just unrelated products.
- Verify the search results page differentiates between category results and product results (separate sections or labels).
- Click a category result in the search results — verify the correct category page opens with all its products listed.
- Search for a parent category name — verify both the parent and child category results are surfaced.
- Verify that category-specific search results show the category description or thumbnail, not a blank entry.

### Search by brand

- Search for a known brand name — verify products from that brand appear prominently.
- Verify the search results page includes a brand filter or facet that is pre-selected or highlighted when a brand name is searched.
- Search for a brand name using a partial name (e.g., first 3 letters) — verify suggestions or results appear.
- Search for a brand name that matches no products — verify the no-results page is shown with suggestions.
- Verify that brand name matches in product titles are ranked higher than brand mentions in descriptions.

### Search term highlighting in results

- Search for a single-word term — verify that word is visually highlighted (bold, color, underline) wherever it appears in the result titles or descriptions.
- Search for a multi-word phrase — verify each matching word is highlighted individually in the results.
- Verify highlighting does not break HTML (e.g., no visible `<mark>` or `<b>` tags rendered as raw text).
- Verify highlighting is applied to both the product name and the short description (if shown) in the result card.
- Search for a term that appears in a product description but not in the product title — verify highlighting still appears in the description snippet.
- Verify that highlighted text remains legible: sufficient color contrast between the highlight color and background (WCAG AA).
- Search using a synonym that triggered a result — verify the matched synonym keyword is highlighted (not the raw query if different).

### Search history

- Perform several searches while logged in — verify a search history list appears in the autocomplete dropdown on subsequent visits.
- Clear search history (if the option is provided) — verify all previous queries are removed from the dropdown.
- Log out and log back in — verify search history persists for logged-in users (server-side persistence).
- Verify search history is stored per user and not shared across accounts.
- On a device with cookies blocked, verify search history gracefully fails or is omitted rather than throwing an error.
- Verify the search history list shows a maximum sensible number of entries (e.g., 5–10), not an unbounded list.
- Click an item from search history — verify the search is re-executed with that term.

### Popular search terms

- Verify a list of popular/trending search terms is shown when the search field is focused but empty.
- Click on a popular search term — verify a search is executed for that term and results are shown.
- Verify the popular search terms are contextually relevant to the shop's product catalog (not generic random words).
- Verify the popular search terms section is labeled clearly (e.g., "Trending", "Popular searches").
- Verify popular search terms update over time (not hard-coded forever); confirm with the dev team that there is a mechanism for this.
- On mobile, verify the popular search terms list is scrollable and does not push critical content off screen.

### Empty search handling

- Focus the search field, type nothing, and press Enter — verify the system either prevents submission (button disabled, form validation) or shows an appropriate message like "Please enter a search term."
- Focus the search field, type a space only, and press Enter — verify the system treats this as an empty query and shows the appropriate message.
- Verify that submitting an empty search does not route to a broken results page or display a server error (500).
- Verify the URL does not contain an empty `?q=` parameter that could be indexed by search engines.
- After triggering the empty-search error state, type a valid term and submit — verify the error clears and results load normally.

### Special characters in search

- Search using common punctuation: `&`, `-`, `/`, `.`, `'` — verify no server error and results are shown or a clean no-results page.
- Search using HTML-significant characters: `<`, `>`, `"` — verify they are properly escaped and do not cause rendering issues in the results page or URL.
- Search using encoded characters or URL entities (e.g., `%20`, `%26`) typed directly — verify the system decodes them correctly or treats them as literals without breaking.
- Search using emoji characters — verify the search either returns results or a clean no-results page without a crash.
- Search using parentheses or brackets (e.g., `(blue)`) — verify query parsing does not interpret them as syntax and handles them as literals.
- Search using a backslash `\` — verify no server-side regex or query parsing error is triggered.
- Search using a null byte or control characters (paste from clipboard) — verify the server handles this safely without an error.

### XSS protection on search input

- Enter `<script>alert('xss')</script>` into the search field and submit — verify the script does not execute; the term should be rendered as escaped text (e.g., `&lt;script&gt;...`).
- Enter `<img src=x onerror=alert(1)>` in the search field and submit — verify no alert fires and the input is sanitized.
- Enter `javascript:alert(1)` as a search term — verify it is treated as a plain text query and not interpreted as a URL or script.
- Check the page source of the search results page for the reflected query value — verify it is HTML-encoded (e.g., `&lt;` not `<`).
- Verify the search term in the URL query string is properly encoded (e.g., `?q=%3Cscript%3E`), not raw HTML.
- Test a stored XSS scenario: if "recent searches" are saved and redisplayed, enter a script tag as a search term, then revisit the page — verify the stored term is also escaped when rendered.
- Verify that error messages triggered by malicious input do not reflect the raw input string unsanitized.
