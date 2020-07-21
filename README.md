# Pure JavaScript filtering for any plain HTML table

Super simple filtering without frameworks and complex components. Simply copy & paste.

It works, assuming you have the following structure of your table:

```html
<table>
  <thead>
  </thead>
  <tbody>
    <tr>
      ...
    </tr>
  </tbody>
</table>
```


```javascript

Array.from(document.querySelectorAll('table.sortable')).forEach((table) => {
function getRandomInt(max) {
  return Math.floor(Math.random() * Math.floor(max));
}
	const thead = table.querySelector('thead');
	const headings = Array.from(thead.querySelectorAll('th')).map((th) => (th.innerText.trim()));
	const filtering = document.createElement('tr');

	function getUniqueItems(index) {
		const x= Array.from(table.querySelectorAll('tbody tr > *:nth-child(' + (index + 1) + ')')).map((v) => (v.innerText.trim()));
		return [...new Set(x)].sort();
	}

	headings.forEach((h, idx) => {
		const f = document.createElement('th');
		const i = document.createElement('input');
		const l = document.createElement('datalist');
		l.setAttribute('id', 'list-'+getRandomInt(900000000));
		const uniqueItems = getUniqueItems(idx);
		uniqueItems.forEach((i) => {
			const o = document.createElement('option');
			o.value = i;
			l.appendChild(o);
		});
		f.appendChild(l);
		i.setAttribute('list', l.getAttribute('id'));
		i.setAttribute('onkeyup', 'searchBoxChange(this, '+idx + ');')
		f.appendChild(i);
		filtering.appendChild(f);
	})
	thead.insertBefore(filtering, thead.firstChild);
});

function searchBoxChange(theInput, colIdx) {
	const tbl = theInput.closest('table');
	const str = theInput.value;
	function rowMatches(row) {
		const c =row.querySelector('*:nth-child('+(colIdx + 1)+')');
		return c.innerText.toLowerCase().indexOf(str.toLowerCase()) > -1;
	}
	function selectRow(row) {
	row.style = ''
	}
	function deselectRow(row) {
	row.style = 'display:none';
	}
	Array.from(tbl.querySelectorAll('tbody tr')).forEach((row) => {
		if (rowMatches(row)) selectRow(row)
		else deselectRow(row);
	})
}
```
