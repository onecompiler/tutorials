HTML tables allow developers to arrange data into rows and columns.

`<table>` tag is used to create a table, `<tr>` tag is used to create table rows, and `<td>` tag is used to create data cells.

**Note:** While HTML5 still supports attributes like `border`, `cellpadding`, and `cellspacing` for backward compatibility, it's recommended to use CSS for styling tables instead. These attributes are considered deprecated in favor of CSS properties.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Table</title>
  </head>
  <body>
      <table style="border-collapse: collapse; border: 1px solid black;">
         <tr>
            <td style="border: 1px solid black; padding: 5px;">Row 1, Col 1</td>
            <td style="border: 1px solid black; padding: 5px;">Row 1, Col 2</td>
            <td style="border: 1px solid black; padding: 5px;">Row 1, Col 3</td>
         </tr>
         
         <tr>
            <td style="border: 1px solid black; padding: 5px;">Row 2, Col 1</td>
            <td style="border: 1px solid black; padding: 5px;">Row 2, Col 2</td>
            <td style="border: 1px solid black; padding: 5px;">Row 2, Col 3</td>
         </tr>
         
         <tr>
            <td style="border: 1px solid black; padding: 5px;">Row 3, Col 1</td>
            <td style="border: 1px solid black; padding: 5px;">Row 3, Col 2</td>
            <td style="border: 1px solid black; padding: 5px;">Row 3, Col 3</td>
         </tr>
      </table>
  </body>
</html>
```
### Try yourself [here](https://onecompiler.com/html/3vvxwgzab)


You can also define table headings using `<th>` tag.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Table example</title>
  </head>
  <body>
      <table style="border: 3px solid black; width: 300px; height: 150px; border-spacing: 5px;">
      <caption>Country and its official language</caption>
         <tr>
            <th style="border: 1px solid black; padding: 5px;">Country</th>
            <th style="border: 1px solid black; padding: 5px;">Language</th>
         </tr>
         <tr>
            <td style="border: 1px solid black; padding: 5px;">Russia</td>
            <td style="border: 1px solid black; padding: 5px;">Russian</td>
         </tr>
         <tr>
            <td style="border: 1px solid black; padding: 5px;">US</td>
            <td style="border: 1px solid black; padding: 5px;">English</td>
         </tr>
         <tr>
            <td style="border: 1px solid black; padding: 5px;">UK</td>
            <td style="border: 1px solid black; padding: 5px;">English</td>
         </tr>
         <tr>
            <td style="border: 1px solid black; padding: 5px;">Japan</td>
            <td style="border: 1px solid black; padding: 5px;">Japanese</td>
         </tr>
         <tr>
            <td style="border: 1px solid black; padding: 5px;">India</td>
            <td style="border: 1px solid black; padding: 5px;">Hindi</td>
         </tr>
         <tr>
            <td style="border: 1px solid black; padding: 5px;">France</td>
            <td style="border: 1px solid black; padding: 5px;">French</td>
         </tr>
      </table>
  </body>
</html>
```
### Try yourself [here](https://onecompiler.com/html/3vvy3su9r)

## Table attributes and styling

### Modern HTML5 Best Practices

In modern HTML5, it's recommended to use CSS for styling tables instead of HTML attributes. Here's a comparison:

* **Deprecated attributes** (still work but not recommended):
  * `border` - Use CSS `border` property instead
  * `cellpadding` - Use CSS `padding` on `<td>` and `<th>` elements
  * `cellspacing` - Use CSS `border-spacing` property
  * `width` and `height` - Use CSS `width` and `height` properties

### CSS Alternatives

```css
/* Instead of border="1" */
table {
  border: 1px solid black;
  border-collapse: collapse;
}

/* Instead of cellpadding="5" */
td, th {
  padding: 5px;
}

/* Instead of cellspacing="5" */
table {
  border-spacing: 5px;
}

/* Instead of width="300" height="150" */
table {
  width: 300px;
  height: 150px;
}
```

### Structural attributes (still valid in HTML5)

* ### Colspan and Rowspan

These attributes are still valid and used to merge cells:
    * ***colspan attribute:*** Merges two or more columns into a single cell
    * ***rowspan attribute:*** Merges two or more rows into a single cell

* ### Caption 

You can add a caption to the table using the `<caption>` element (not an attribute).

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Table example</title>
  </head>
  <body>
      <table style="border: 3px solid black; width: 300px; height: 150px; border-spacing: 5px;">
      <caption>Country and its official language</caption>
         <tr>
            <th style="border: 1px solid black; padding: 5px;">Country</th>
            <th style="border: 1px solid black; padding: 5px;">Language</th>
         </tr>
         <tr>
            <td style="border: 1px solid black; padding: 5px;">Russia</td>
            <td style="border: 1px solid black; padding: 5px;">Russian</td>
         </tr>
         <tr>
            <td style="border: 1px solid black; padding: 5px;">US</td>
            <td style="border: 1px solid black; padding: 5px;" rowspan="2">English</td> <!-- Notice we are merging two rows here-->
         </tr>
         <tr>
            <td style="border: 1px solid black; padding: 5px;">UK</td>
         </tr>
         <tr>
            <td style="border: 1px solid black; padding: 5px;">Japan</td>
            <td style="border: 1px solid black; padding: 5px;">Japanese</td>
         </tr>
         <tr>
            <td style="border: 1px solid black; padding: 5px;">India</td>
            <td style="border: 1px solid black; padding: 5px;">Hindi</td>
         </tr>
         <tr>
            <td style="border: 1px solid black; padding: 5px;">France</td>
            <td style="border: 1px solid black; padding: 5px;">French</td>
         </tr>
      </table>
  </body>
</html>

```

### Try yourself [here](https://onecompiler.com/html/3vvydg4eh)