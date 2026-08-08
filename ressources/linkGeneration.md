# PoE 1 Item Text to Trade Search URL (`?q=`)

Specification for programmatically parsing in-game item clipboard text (Ctrl+C) into a direct Path of Exile 1 trade site search URL via URI percent-encoding.

---

## 1. Pipeline Overview

```
[ In-Game Item Text ] 
       │
       ▼
[ Step 1: Parse & Extract Modifiers ]
       │
       ▼
[ Step 2: Map Strings to Stat IDs ]
       │
       ▼
[ Step 3: Build JSON Payload Schema ]
       │
       ▼
[ Step 4: Serialize & Percent-Encode ]
       │
       ▼
[ Step 5: Assemble Final Search URL ]

```

---

## 2. Technical Specification

### Step 1: Parse Item Clipboard Text

In-game item text follows a standardized line-separated layout using `--------` as section dividers.

* **Target Section:** Locate explicit or implicit modifier lines (typically after requirements, item level, and base stats).
* **Extract Numbers:** Separate text strings from their numeric values using regular expressions (e.g., `+120 to maximum Life` $\rightarrow$ text template: `+# to maximum Life`, numeric value: `120`).

### Step 2: Map Modifiers to Trade Stat IDs

Match extracted text templates against the PoE Trade API stat lookup dictionary (`[https://www.pathofexile.com/api/trade/data/stats](https://www.pathofexile.com/api/trade/data/stats)`).

* **Format:** Every modifier maps to a unique hash string prefixed by its domain (e.g., `explicit`, `implicit`, `pseudo`).
* **Example Mapping:**
* `+# to maximum Life` $\rightarrow$ `explicit.stat_3299347043`
* `+#% to Fire Resistance` $\rightarrow$ `explicit.stat_3372524247`



### Step 3: Build JSON Query Payload

Construct a standard PoE search object containing the identified stat IDs and numerical thresholds.

```json
{
  "query": {
    "status": { "option": "online" },
    "stats": [
      {
        "type": "and",
        "filters": [
          {
            "id": "explicit.stat_3299347043",
            "value": { "min": 120 }
          },
          {
            "id": "explicit.stat_3372524247",
            "value": { "min": 45 }
          }
        ]
      }
    ]
  },
  "sort": { "price": "asc" }
}

```

### Step 4: Serialization and Encoding

1. **Minify JSON:** Convert the JSON object into a single line string, removing all formatting, spaces, and line breaks.
2. **Percent-Encode:** Apply RFC 3986 standard URL encoding (`encodeURIComponent` in JS, `urllib.parse.quote` in Python) to safely escape reserved characters (`{`, `}`, `"`, `:`, `,`).

### Step 5: URL Construction

Append the encoded query string as the `?q=` parameter to the target league search endpoint:

$$\text{URL} = \text{[https://www.pathofexile.com/trade/search/](https://www.pathofexile.com/trade/search/)}\langle\text{League}\rangle\text{?q=}\langle\text{EncodedJSON}\rangle$$

---

## 3. Implementation Example (JavaScript)

```javascript
// 1. Raw JSON search payload
const payload = {
  "query": {
    "status": { "option": "online" },
    "stats": [
      {
        "type": "and",
        "filters": [
          { "id": "explicit.stat_3299347043", "value": { "min": 120 } },
          { "id": "explicit.stat_3372524247", "value": { "min": 45 } }
        ]
      }
    ]
  },
  "sort": { "price": "asc" }
};

// 2. Minify JSON object into a string
const minifiedJson = JSON.stringify(payload);

// 3. Percent-encode reserved characters for safe URL transport
const encodedQuery = encodeURIComponent(minifiedJson);

// 4. Construct final trade search URL
const league = "Settlers";
const tradeUrl = `https://www.pathofexile.com/trade/search/${league}?q=${encodedQuery}`;

console.log(tradeUrl);

```

### Code Explanation:

* **Line 2–16:** Defines the search criteria using official PoE stat ID hashes and sets a minimum value threshold for each modifier.
* **Line 19:** `JSON.stringify(payload)` strips all structural whitespace and outputs a compact raw JSON string.
* **Line 22:** `encodeURIComponent(minifiedJson)` converts characters like `"` to `%22`, `{` to `%7B`, and `}` to `%7D` so web browsers parse the string as a URL parameter rather than breaking syntax.
* **Line 25–26:** Interpolates the current league string and encoded query string into the base PoE trade search route.

---

## 4. Reference Links & Resources

* **[PoEDB — Trade API Reference](https://poedb.tw/de/API%3ATrade)**
Complete community breakdown of stat IDs, payload structures, and search routes.
* **[Path of Exile Official Stats Endpoint](https://www.google.com/url?sa=E&source=gmail&q=https://www.pathofexile.com/api/trade/data/stats)**
Official JSON mapping of every in-game stat string to its respective `stat_id` hash.
* **[Chuanhsing / poe-api Open API Schema](https://github.com/Chuanhsing/poe-api)**
OpenAPI specifications for PoE trade request and response models.
* **[Snoxh / poe-trade-official-site-format](https://www.google.com/search?q=https://github.com/Snoxh/poe-trade-official-site-format)**
Community implementation reference mapping in-game item strings to official trade JSON structures.