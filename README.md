## VikCraft Interactive Data Table

VikCraft Interactive Data Table is a lightweight yet powerful JavaScript library for creating highly customizable and interactive data tables. It supports both client-side and server-side data processing, offering features like sorting, searching, pagination, column filtering, grouping, inline editing, and Excel export.

### ✨ Features

* **Client-Side & Server-Side Data**: Seamlessly switch between processing data locally or fetching it from an API.
* **Sorting**: Click on column headers to sort data ascending or descending.
* **Global Search**: A single search box to filter data across all columns.
* **Column Filtering**: Advanced filtering options for each column based on data type (string, number).
* **Pagination**: Navigate through large datasets with customizable page lengths.
* **Column Resizing**: Adjust column widths by dragging headers.
* **Grouping**: Drag and drop columns into a dedicated zone to group rows by common values.
* **Inline Editing**: Double-click cells to directly edit their content with various input types (text, number, dropdown, checkbox).
* **Checkboxes**: Enable row selection with individual and "select all" checkboxes.
* **Custom Actions**: Add custom buttons and define actions for each row.
* **Excel Export**: Export the filtered and sorted data directly to an Excel (`.xlsx`) file.
* **Theming**: Built-in 'default', 'dark', and 'narrow' themes, with easy customization via CSS.
* **Custom Cell & Row Styling**: Apply dynamic CSS classes or inline styles based on cell or row data.

---

### 🚀 Getting Started

#### 1. Include the Library

You'll need three files: `vikcraft-data-table.css`, `vikcraft-data-table.js`, and the SheetJS library for Excel export.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>VikCraft Interactive Data Table</title>
    
    <script src="[https://cdn.sheetjs.com/xlsx-0.20.2/package/dist/xlsx.full.min.js](https://cdn.sheetjs.com/xlsx-0.20.2/package/dist/xlsx.full.min.js)"></script>

    <link href="[https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap](https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap)" rel="stylesheet">
    
    <link rel="stylesheet" href="vikcraft-data-table.css">

    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f2f5;
            padding: 20px;
        }
        h1 {
            text-align: center;
            color: #333;
        }
        /* Custom styles for your specific table instance can go here */
    </style>
</head>
<body>

    <h1>My Interactive Data Table</h1>
    
    <div id="my-table-container"></div>

    <script src="vikcraft-data-table.js"></script>
    <script>
        document.addEventListener('DOMContentLoaded', function () {
            // Your table initialization code will go here
        });
    </script>
</body>
</html>
```

#### 2. Initialize the Table
Create an instance of VikCraftDataTable by providing the selector of your table container and a configuration object.

```javascript
document.addEventListener('DOMContentLoaded', function () {
    const sampleData = [
        { id: 1, name: 'Alice', email: 'alice@example.com', age: 28, city: 'New York', salary: 70000, active: true },
        { id: 2, name: 'Bob', email: 'bob@example.com', age: 35, city: 'Los Angeles', salary: 85000, active: false },
        { id: 3, name: 'Charlie', email: 'charlie@example.com', age: 22, city: 'Chicago', salary: 60000, active: true },
    ];

    const tableConfig = {
        data: sampleData, // Your data array (client-side)
        columns: [
            { key: 'id', label: 'ID', sortable: true, filterable: true, type: 'number' },
            { key: 'name', label: 'Full Name', sortable: true, filterable: true, type: 'string', editable: true, editType: 'text' },
            { key: 'email', label: 'Email Address', sortable: true, filterable: true, type: 'string', editable: true, editType: 'text' },
            { key: 'age', label: 'Age', sortable: true, filterable: true, type: 'number', editable: true, editType: 'number' },
            { key: 'city', label: 'City', sortable: true, filterable: true, type: 'string', editable: true, editType: 'dropdown', editOptions: ['New York', 'Los Angeles', 'Chicago'] },
            { 
                key: 'salary', 
                label: 'Salary', 
                sortable: true, 
                filterable: true, 
                type: 'number',
                cellStyler: function(cellValue, rowData) {
                    if (cellValue > 80000) {
                        return { className: 'highlight-salary', style: { backgroundColor: '#e6ffe6' } }; // Apply a CSS class
                    }
                }
            },
            { key: 'active', label: 'Active', sortable: true, filterable: false, editable: true, editType: 'checkbox' },
        ],
        
        // Enable desired features
        enableCheckboxes: true,
        enableSearch: true,
        enableColumnFilter: true,
        enablePagination: true,
        enablePageLength: true,
        enableColumnResize: true,
        enableGrouping: true,
        enableEditing: true,
        enableExcelExport: true,

        // Optional configurations
        defaultPageLength: 5,
        pageLengthOptions: [5, 10, 15, 20],
        defaultGroupBy: 'city', // Group by city initially

        // Define row actions
        actions: [
            { label: 'View', className: 'btn-view', onClick: (row) => alert(`Viewing: ${row.name}`) },
            { label: 'Delete', className: 'btn-delete', onClick: (row) => confirm(`Delete ${row.name}?`) && console.log('Deleted!') }
        ],

        // Callbacks for interactions
        onCheckboxChange: (isChecked, rowData, selectedRows) => {
            console.log(`Row ${rowData.name} ${isChecked ? 'selected' : 'deselected'}. Selected rows:`, selectedRows.map(r => r.name));
        },
        onSelectAll: (isChecked, allSelectedRows) => {
            console.log(`${allSelectedRows.length} rows were ${isChecked ? 'selected' : 'deselected'}.`);
        },
        onCellEdit: (columnKey, newValue, rowData) => {
            console.log(`Cell "${columnKey}" in row "${rowData.name}" changed to:`, newValue);
            // Here you would typically send an API request to update your backend
        },
        onRowCreate: function(rowData, tr) {
            // Example: Style rows where salary is null (e.g., inactive employees)
            if (rowData.salary === null) {
                tr.classList.add('row-inactive');
            }
        },

        // Excel Export Specifics
        excelExportConfig: {
            title: 'Employee Data Report',
            subtitle: `Export Date: ${new Date().toLocaleDateString()}`,
            filename: 'employee_data.xlsx'
        }
    };

    const myTable = new VikCraftDataTable('#my-table-container', tableConfig);

    // Example of changing theme dynamically
    // myTable.setTheme('dark'); 
});
```


-----
## ⚙️ Configuration Options

The `userConfig` object passed to `VikCraftDataTable` can contain the following properties:

| Option               | Type                | Default               | Description                                                                                                    |
| :------------------- | :------------------ | :-------------------- | :------------------------------------------------------------------------------------------------------------- |
| `data`               | `Array<Object>`     | `[]`                  | (Client-side only) An array of JS objects representing the table rows. Keys must match a `column.key`.       |
| `columns`            | `Array<Object>`     | `[]`                  | **Required**. Array of column definition objects. See Column Definition.                                       |
| `columnGroups`       | `Array<Object>`     | `[]`                  | Array of objects defining multi-level headers. Each has `label` (string) and `colspan` (number).               |
| `actions`            | `Array<Object>`     | `[]`                  | Array of action button definitions. See Actions Definition.                                                    |
| `enableCheckboxes`   | `boolean`           | `true`                | Enable row-selection checkboxes.                                                                               |
| `enableSearch`       | `boolean`           | `true`                | Enable global search input.                                                                                    |
| `enableColumnFilter` | `boolean`           | `true`                | Enable individual column filters.                                                                              |
| `enablePagination`   | `boolean`           | `true`                | Enable pagination.                                                                                             |
| `enablePageLength`   | `boolean`           | `true`                | Enable “Show X entries” dropdown.                                                                              |
| `enableColumnResize` | `boolean`           | `true`                | Enable column width resizing.                                                                                  |
| `enableGrouping`     | `boolean`           | `true`                | Enable drag-and-drop grouping.                                                                                 |
| `enableEditing`      | `boolean`           | `false`               | Enable inline editing (requires `enableEditing: true`).                                                        |
| `enableExcelExport`  | `boolean`           | `false`               | Enable “Export to Excel” button.                                                                               |
| `defaultPageLength`  | `number`            | `10`                  | Default rows per page.                                                                                         |
| `pageLengthOptions`  | `Array<number>`     | `[5,10,25,50,100]`    | Options for “Show X entries” dropdown.                                                                         |
| `defaultGroupBy`     | `string \| null`    | `null`                | Key of the column to group by initially.                                                                       |
| `theme`              | `string`            | `'default'`           | Initial theme (`'default'`, `'dark'`, `'narrow'`).                                                             |
| `serverSide`         | `boolean`           | `false`               | If `true`, data is fetched from `ajax` endpoint.                                                               |
| `ajax`               | `string \| null`    | `null`                | **Server-side only**. URL for fetching data.                                                                    |
| `method`             | `string`            | `'GET'`               | **Server-side only**. HTTP method for `ajax` (`'GET'`/`'POST'`).                                                 |
| `customFilters`      | `Object`            | `{}`                  | **Server-side only**. Extra key-value pairs for `ajax` requests.                                                |
| `onCheckboxChange`   | `function`          | `null`                | Callback `(isChecked, rowData, selectedRows)` on single checkbox change.                                       |
| `onSelectAll`        | `function`          | `null`                | Callback `(isChecked, allSelectedRows)` on “select all” change.                                                |
| `onCellEdit`         | `function`          | `null`                | Callback `(columnKey, newValue, rowData)` after inline edit.                                                   |
| `onRowCreate`        | `function`          | `null`                | Callback `(rowData, trElement)` for custom row styling.                                                        |
| `excelExportConfig`  | `Object`            | `{}`                  | Config for Excel export. See Excel Export Configuration.                                                        |

---

## 📚 Column Definition

Each object in the `columns` array can have the following properties:

| Property      | Type            | Default       | Description                                                                                               |
| :------------ | :-------------- | :------------ | :-------------------------------------------------------------------------------------------------------- |
| `key`         | `string`        | **Required**  | The key from your data object for this column.                                                            |
| `label`       | `string`        | `key`         | The display text for the column header.                                                                   |
| `sortable`    | `boolean`       | `false`       | If `true`, the column can be sorted.                                                                      |
| `filterable`  | `boolean`       | `false`       | If `true`, a filter input appears for this column.                                                        |
| `type`        | `string`        | `'string'`    | Filter type (`'string'` or `'number'`).                                                                   |
| `width`       | `string`        | `auto`        | CSS width (e.g., `'100px'`, `'10%'`).                                                                     |
| `editable`    | `boolean`       | `false`       | If `true`, allows inline editing (requires `enableEditing: true`).                                         |
| `editType`    | `string`        | `'text'`      | Editor type (`'text'`, `'number'`, `'dropdown'`, `'checkbox'`).                                           |
| `editOptions` | `Array<string>` | `[]`          | **Required for `'dropdown'`**. Options for the dropdown editor.                                           |
| `cellStyler`  | `function`      | `null`        | `(cellValue, rowData) => { className, style }` for custom cell styles.                                     |

---

#### `cellStyler` Example

```javascript
{
  key: 'salary',
  label: 'Salary',
  type: 'number',
  cellStyler: (value, row) => {
    if (value > 100000) {
      return { className: 'highlight-salary', style: { backgroundColor: '#e6ffe6' } };
    } else if (value < 50000) {
      return { className: 'low-salary' };
    }
  }
}
```

---

## 📊 Excel Export Configuration

The `excelExportConfig` object can contain:

| Property   | Type     | Default                            | Description                                    |
| :--------- | :------- | :--------------------------------- | :--------------------------------------------- |
| `title`    | `string` | `'Data Report'`                    | The main title displayed at the top of the Excel sheet. |
| `subtitle` | `string` | `'Generated by VikCraft Data Table'` | A secondary subtitle for the Excel sheet.      |
| `filename` | `string` | `'report.xlsx'`                    | The filename for the exported Excel file.      |

---

## 🌍 Server-Side Mode

When `serverSide` is `true`, the table will make `fetch` requests to the `ajax` endpoint for data.

The server is expected to return a JSON object with the following structure:

```json
{
  "data": [
    // Array of row objects (non-grouped or top-level)
    // OR for grouped data:
    // {
    //   "groupName": "New York",
    //   "itemCount": 2,
    //   "items": [ {id: 1, ...}, {id: 4, ...} ]
    // }
  ],
  "recordsTotal": 100,      // Total records before filtering
  "recordsFiltered": 50,    // Records after filtering (for pagination)
  "isGrouped": false        // True if the response contains grouped data
}
```

The table sends the following parameters to the server:

- `page`: Current page number.  
- `limit`: Rows per page.  
- `globalFilter`: Value of the global search input.  
- `sort`: Key of the column to sort by.  
- `direction`: Sort direction (`asc` or `desc`).  
- `groupBy`: Key of the column to group by (if grouping is active).  
- `collapsed[]`: Array of collapsed group names (if grouping is active).  
- `filter_{columnKey}`: JSON string of `{ "value": "filterText", "operator": "equals" }` for each active column filter.  
- Any extra key-value pairs defined in `customFilters`.  

---

**Example Server-Side Request Parameters (GET):**

```http
GET http://localhost:8000/api/employees/
?page=1
&limit=10
&globalFilter=smith
&sort=name
&direction=asc
&filter_age={"value":"30","operator":">="}
&custom_status=active
```

---

## 🎨 Styling and Theming

The library comes with base styles in `vikcraft-data-table.css`. All styles are namespaced under `.vikcraft-data-table-wrapper` to prevent conflicts.

**Built-in Themes:**
- `default`: The standard light theme.
- `dark`: A dark mode theme.
- `narrow`: Reduces padding for a more compact table.

You can set the initial theme via the `theme` configuration option or change it dynamically using:

```javascript
myTable.setTheme('dark');
```

---

### Custom Styling

You can override or extend the default styles by adding your CSS after `vikcraft-data-table.css`. The provided example demonstrates how to add custom styles for `high-experience`, `status-active`, `status-inactive`, and `row-inactive` classes. These are applied via `cellStyler` and `onRowCreate` callbacks.

```css
/* Custom style for high-experience cells (e.g., salary > 85000) */
.vikcraft-data-table-wrapper td.high-experience {
    font-weight: bold;
    color: #0056b3;
}
.vikcraft-data-table-wrapper.theme-dark td.high-experience {
    color: #8ab4f8; /* Dark theme override */
}

/* Custom style for entire inactive rows (e.g., salary is null) */
.vikcraft-data-table-wrapper tr.row-inactive {
    background-color: #fff0f1;
}
.vikcraft-data-table-wrapper.theme-dark tr.row-inactive {
    background-color: #5c1f26;
    color: #f5c5cb;
}
```
---

## 🧩 Public Methods

After initializing `VikCraftDataTable`, you can access its public methods:

- **`myTable.setTheme(themeName)`**  
  - `themeName`: `string` (`'default'`, `'dark'`, `'narrow'`)  
  Changes the visual theme of the table.

- **`myTable.getSelectedRows()`** → `Array<Object>`  
  Returns an array of data objects for all currently selected rows.

- **`myTable.reloadData(newFilters = {})`**  
  - `newFilters`: `Object` (optional) – Key-value pairs to merge into existing `customFilters`.  
  Reloads the table data and resets to page 1.

---

## 📜 License

This project is open-source and available under the **MIT License**.

