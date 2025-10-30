# make4ht_bold_td_to_th_extension
Automatically convert bold table cells to HTML &lt;th> header elements in make4ht LaTeX to HTML conversions.

## Description

This make4ht extension automatically detects bold table data cells (`<td>`) (created using `\textbf{}`) in LaTeX tables and converts them to HTML table header cells (`<th>`) with appropriate `scope` attributes for improved accessibility. The extension only processes cells in the first row or first column of tables, following common table header patterns.

## Features

- Easy to use: Just use `\textbf{}` to indicate `<th>` headers in your LaTeX tables
- Adds `scope` attributes (`col` for column headers, `row` for row headers)
- Conservative: only processes first row and first column to avoid false positives

## Installation

Download `bold_td_to_th.mk4` and place it in your LaTeX project directory.

## Usage

### Basic Usage

Use the `-e` flag to specify the extension:

```bash
make4ht -e bold_td_to_th.mk4 yourdocument.tex
```

### Automatic Detection (Same-Name File)

If you rename the extension file to be the same as your `.tex` file, make4ht will automatically detect it:

```bash
# If you have: mydocument.tex and mydocument.mk4
make4ht mydocument.tex
```

## Examples

### Example 1: Column Headers

**LaTeX:**
```latex
\begin{table}[h]
  \centering
  \caption{Sales by Quarter}
  \begin{tabular}{rrrr}
    \textbf{Q1} & \textbf{Q2} & \textbf{Q3} & \textbf{Q4} \\
    120 & 140 & 135 & 150 \\
  \end{tabular}
\end{table}
```

**Generated HTML:**
```html
<table>
  <tr>
    <th scope="col">Q1</th>
    <th scope="col">Q2</th>
    <th scope="col">Q3</th>
    <th scope="col">Q4</th>
  </tr>
  <tr>
    <td>120</td>
    <td>140</td>
    <td>135</td>
    <td>150</td>
  </tr>
</table>
```

### Example 2: Row Headers

**LaTeX:**
```latex
\begin{table}[h]
  \centering
  \caption{Assembly Line Samples}
  \begin{tabular}{lrrr}
    \textbf{Line 1} & 250 & 300 & 275 \\
    \textbf{Line 2} & 190 & 210 & 178 \\
    \textbf{Line 3} &  60 &  90 &  78 \\
  \end{tabular}
\end{table}
```

**Generated HTML:**
```html
<table>
  <tr>
    <th scope="row">Line 1</th>
    <td>250</td>
    <td>300</td>
    <td>275</td>
  </tr>
  <tr>
    <th scope="row">Line 2</th>
    <td>190</td>
    <td>210</td>
    <td>178</td>
  </tr>
  <tr>
    <th scope="row">Line 3</th>
    <td>60</td>
    <td>90</td>
    <td>78</td>
  </tr>
</table>
```

### Example 3: Both Column and Row Headers

**LaTeX:**
```latex
\begin{table}[h]
  \centering
  \caption{Exam Scores}
  \begin{tabular}{lrrr}
    \textbf{Student} & \textbf{Exam 1} & \textbf{Exam 2} & \textbf{Exam 3} \\
    \textbf{Adams} & 88 & 90 & 79 \\
    \textbf{Baker} & 76 & 82 & 73 \\
  \end{tabular}
\end{table}
```

**Generated HTML:**
```html
<table>
  <tr>
    <th scope="col">Student</th>
    <th scope="col">Exam 1</th>
    <th scope="col">Exam 2</th>
    <th scope="col">Exam 3</th>
  </tr>
  <tr>
    <th scope="row">Adams</th>
    <td>88</td>
    <td>90</td>
    <td>79</td>
  </tr>
  <tr>
    <th scope="row">Baker</th>
    <td>76</td>
    <td>82</td>
    <td>73</td>
  </tr>
</table>
```

## How It Works

1. The extension scans all `<td>` cells in the first row and first column of each table
2. It checks if the cell's content is entirely wrapped in bold `<span>` elements (class `ec-lmbx-*`)
3. If a cell is bold, it converts the `<td>` to `<th>`
4. Scope Assignment: 
   - First row cells get `scope="col"` (column headers)
   - First column cells get `scope="row"` (row headers)

## Requirements

- **make4ht** (included in TeX Live and MiKTeX)
- Any LaTeX engine (pdfLaTeX, XeLaTeX, LuaLaTeX)

## Limitations

- Only processes cells in the **first row** or **first column**
- Only detects cells where **all text content** is bold
- Requires header cell content to be wrapped in the `\textbf{}` command (other bold methods may not be detected)
- Bold cells in the middle of tables (not in first row/column) remain as `<td>`

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## See Also

- [make4ht documentation](https://ctan.org/pkg/make4ht)
- [WCAG 2.1 Table Guidelines](https://www.w3.org/WAI/WCAG21/Understanding/info-and-relationships.html)
- [HTML table accessibility](https://www.w3.org/WAI/tutorials/tables/)
