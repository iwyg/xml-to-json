xml-to-json
===========

A complete XML to JSON transformation utility adapted from  
[Doeke Zanstra's][1] xml2json utility 

This version is adapted from [VladG's][2] [xsl-utility][3]


This version:

- resorbs* repetitive elements:

  	<days>
			<day>Monday</day>
			<day>Tuesday</day>
			<day>Wendsday</day>
		</days>

		====> {days: ["Monday", "Tuesday", "Wendsday"] }

		*Nota bene: If only 1 element exists, it will be output as-is

		<days>
			<day>Monday</day>
		</days>

		====> {days: {day: "Monday"} }


- empty text nodes are transformed to empty strings
		<days>
			<day></day>
			<day>Monday</day>
		</days>

		====> {days: ["", "Monday"] }


- numbers are output as integers
- nodes with text equal to "null" are transformed to items with value set to Javascript `null`

		<days>
			null
		</days>

		====> {days: null }

- arrays: it is possible to explicitly trigger an array by using a tagname defined in `<wc:array/>`, e.g.

		<node>
			<item>foo</item>
		</node>

		====>	{"node": ["foo"]}

- attributes

		<node id="12"><content>…</content></node>

		====>	{"node": {"@attributes":{"id":"12"}, "content": …}

- attributes within arrays:

		<array>
			<item id="1">A</item>
			<item id="2">B</item>
		</array>

		====>	{"array": [{"@attributes": {"id":"1"}, "value": "A"},{"@attributes": {"id":"2"}, "value": "B"}]}

- attributes on array parents:

		<array id="1">
			<item>A</item>
			<item>B</item>
		</array>

		====>	{"array": {"@attributes": {"id":"1"}, "values": ["A","B"]}}



[1]:http://code.google.com/p/xml2json-xslt/
[2]:https://github.com/vlad-ghita
[3]:http://getsymphony.com/download/xslt-utilities/view/64195/