JSRepeater is a simple and intuitive Javascript Templating system provided as a jQuery plugin.
See the following example that binds JSON data to a div.

# $('#myDiv').fillTemplate(myData);

Binding to an hierarchy

Have a look at the JSON object below, it could be a list of categories and subcategories for a computer store.

	var Categories = 
		[{"Name" : "Memory", 
				 "Subcategories" : [{"Name" : "2 GB"},{"Name" : "3GB"},{"Name" : "4GB"}]}, 
		 {"Name" : "Hard Drives", 
				 "Subcategories" : [{"Name" : "250 GB"},{"Name" : "500 GB"},{"Name" : "500+ GB"}]} 
		 {"Name" : "Graphics", 
				 "Subcategories" : [{"Name" : "Integrated"},{"Name" : "Dedicated"}]}] 
			 
The problem is that when we loop through the categories we want to loop through the subcategories as well. We do this with the context attribute. The repeater loops through each category and fills the template with the category information. When it comes to a 'context' attribute though it evaluates that against the current category and then loops through the result. The <li context='Subcategories'> is like a mini template inside the outer template. Inside this template ${Name} will refer not to the name of the category but the name of the Subcategory.

	<ol id='CategoryList' > 
		<li>${Name} 
			<ol>
				<li context='Subcategories'>${Name}</li>
			</ol> 
		</li> 
	</ol>
	
And our result is this

1. Memory
	* GB
	* 3 GB
	* 4 GB
2. Hard Drives
	* 250 GB
	* 500 GB
	* 500+ GB
3. Graphics
	* Integrated
	* Dedicated
	
	
	
**Alternating Items**

One of the more usual requirements for a repeater is to be able to display alternating items differently. Let's use a list of days of the week as an example

	[{"Name" : "Monday"},{"Name" : "Tuesday"} etc ... ]

To tell the repeater to display X for an odd row and Y for an even row we would use %{X:Y}. You are not limited to two options however, %{X:Y:Z} will display X in the first row, Y in the second and Z in the third and then start the sequence again from the beginning.

In the next template we switch the class of the <li> between a class named 'odd' and a class named 'even'

	<ul id='Listofdays' >
		<li class='%{odd:even}'>${Name}</li>
	</ul>

In the stylesheet we format the list and define a different background colour for the odd and even classes.

	#Listofdays { list-style-type: none; }
	#Listofdays .odd { background-color: #f0f8ff; }
	#Listofdays .even { background-color: #ffffff; }

This produces the result below

Monday
Tuesday
Wednesday
Thursday
Friday
Saturday
Sunday

The full format of the alternating directive is %{first|X:Y|last} where the value of 'first' is sent to the document for the first item in the list, for subsequent items X and Y are alternatively sent out and the value of 'last' is written to the document in the case of the last item on the list. 
If we need to put a line at the top of the first row and one at the bottom of the last row as well as alternate row colours we could use the following template.

	<ul id='Listofdays2' >
		<li class='%{first|} %{odd:even} %{||last}'>${Name}</li>
	</ul>

Which when matched with the CSS styles below

	#Listofdays2 { list-style-type: none; }
	#Listofdays2 .first { border-top:solid 1px black; }
	#Listofdays2 .odd { background-color: #f0f8ff; }
	#Listofdays2 .even { background-color: #ffffff; }
	#Listofdays2 .last { border-bottom:solid 1px black; }

Would produce

Monday
Tuesday
Wednesday
Thursday
Friday
Saturday
Sunday
	