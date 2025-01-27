# Summary

Regular expressions are used to match patterns in strings.  Strings are sequences of characters and regular expressions excel at both finding strings through patterns and enforcing patterns on strings.  If you need to find anything beyond _exact_ character strings with or without general case insensitivity then it is time to consider leaving behind standard tools for finding text and embracing regular expressions.

This walkthrough is meant to be used as a loose script and general resource to be shared as part of a standalone, 120-minute workshop as part of a regular workshop series offered by Prairies DRI.

1. Overview
2. Setup
3. Search
4. Extract
5. Report
6. Modify

# 1. Why do we want to learn regular expression?

It saves tons of time and make your life much easier when dealing with "big data".

### (1) Two examples:

#### <1>. **Search, extraction, and re-organization**: i.e. when working on a thesis or project

	 ([0-9.]{1,}) to ([0-9.]{1,})\)
  
change to

	\1-\2)
  
#### <2>. **Validation**: Email validation

We can validate email in Excel table with Visual Basics of Application (VBA).


### (2) Principles to illustrate

These are  three things you need to know to be successful with regular expressions

1. Your data.
2. How "other people" will mess up what you expect about your data.
3. Where to get help—and double check!—reading and writing regular expressions.


# 2. Setup

We'll be using both [https://regex101.com/](https://regex101.com/) and [https://colab.research.google.com/](https://colab.research.google.com/) so each participant should have each open in a separate tab.

#### Everyone should open browser and point it to [https://regex101.com/](https://regex101.com/) and [https://colab.research.google.com/](https://colab.research.google.com/).


# 3. Search

### (1). Exact String Matching

Our focus for examples will be Canadian postal codes. These are a fairly straight forward pattern that could have a lot of alternatives to control for. No need to load in a set of these, we'll build our dataset as we go.

Type "A1A1A1" into the `TEXT STRING` box.  In the `REGULAR EXPRESSION` box and we'll begin building a regular expression to find or enforce Canadian postal codes.  Type:

	A1A1A1

As you are typing you'll notice that different parts of the sample text are highlighted in blue. If you type something other than is in the `TEST STRING` box then the highlighting will disappear, this includes typing too many characters.  This is the regex engine parsing away as you enter the search expression.  When complete you'll see the number of matches, steps, and time taken to carryout the search.  In this case it should look something like `1 match (7 steps, 1.0ms)`. Even with a long text the search search is pretty fast, much faster than you or I scrolling through the text to find a text string.

So, it can match exactly what we type.  Can it ignore things that do not mach?  Type something else:

	hello
	
Note that it doesn't matter where the `hello` is as long as it doesn't break up the original string.

What about a different postal code? Type your own postal code into the `TEST STRING` box.

	M5W1E6
	
It won't detect this new postal code. Why? Because we've been _very_ specific, providing a regular expression that will match `A1A1A1` exactly. We can see this by opening the EXPLANATION PANEL to the right of the REGULAR EXPRESSION box.  


### (2) Variable Pattern match

#### <1> Varible of strings
Example 1. Postcode:

	A1A1A1

Code:

	[A-Z][0-9][A-Z][0-9][A-Z][0-9]

Example 2. Grade:

	Mickey Mouse	A
	Donald Duck	B
	Peppa Pig	C
	Paw Patrol	A
 	
Who got B or higher?

	\s(A|B)\b

#### <2> Varible of numbers
Daily snowfall amount (mm):

	Sun	4.5
 	Mon	7.3
	Tue	9.4
	Wed	7.9
	Thu	2.7
	Fri	3.4
 	Sat	5.2

Which day(s) in this week have snowfall more than 5 mm?

	[5-9]\.[0-9]

> "." here is a special character in regex syntax, which means any character. However, with "\" in front, it represents itself "." in this case.

#### <3> Variable of string lengths
Annual snowfall amount (cm):

	Washington	1640.0
 	California	1045.0
	New York	544.0
	Texas		51.0
	Georgia		9.7
	Florida		0.2
 	Hawaii		0.0

Which state(s) have more than 100 cm snowfall?

	[0-9]{3,4}\.[0-9]

#### <4> Insertion or deletion
Postcode:

 	A1A 1A1
  	A1A1A1

Code:

	[A-Z][0-9][A-Z]\s?[0-9][A-Z][0-9]

Practice: can you find all cities from USA?

	Edmonton, Canada
	New York City, USA
 	Washington, United States of America
	Calgary, CA
	Toronto, Canada
	Vancouver, CA
 	Los Angeles, U.S.
  	Chicago, U.S.A
   	Seattle, US

Reference answer:


<details>
  <summary>Click to see the answer!</summary>
  
  ```,\s(U\.?S\.?A?|United States)\b```
  
</details>


#### Cheet Sheet 1

	.		# any character except new line
 	\d		# a digit
  	[36]		# 3 or 6
   	[3-6]		# 3 or 4 or 5 or 6
	[^3-6]		# anything not 3, 4, 5, or 6
	\D		# a non-digit
 	\w		# a word character: letter or digit or _
	\W		# a non-word character
	\t		# tab
 	\n		# new line
	+		# at least 1 repeat, same as {1,}
 	?		# 0 or 1 repeat, same as {,1}
	*		# any number of repetitions. Same as {,}
	\s		# a whitespace
	\S		# a non-whitespace character
	|		# or
	\.		# to match a dot "."
	^		# start of the string
	$		# end of the string
		

#### What do we learn from this:
> #1 It's very important to understand your data.
> 
> #2 You may want to do multiple testing before applying your code. Validate the output after each step.
> 
> #3 Make a good plan when generating the data. Think about how to make it consistent, straightforward, reproducible and easy to process.

# 4. Extract

We can "group" the patterns and "capture" the matches.

### (1) Single group

Print out the cities in US:
	Edmonton, Canada
	New York City, US
 	Washington, US
	Calgary, CA
	Toronto, Canada
	Vancouver, CA
 	Los Angeles, US
  	Chicago, US
   	Seattle, US

Code:
	(.*),\sUS\b

### (2) Multiple groups

Get the first name and last name

	John Michael Smith
 	Emma Johnson
  	Tony H. Brown

Code:

 	^(\w{1,}) .* (\w{0,})$

> Practice: get the students' names, grades and courses:

	John got A in Maths, B in Arts, and C in Physics.
 	Emma got C in English, A in Chemistry, and B in History.
  	Tony got B in French, B in Maths, and D in Arts.

# 5. Report
Open [https://colab.research.google.com/]() in your browser, login with your google account.

### (1) Exact match

	import re
	input = "University of Alberta"
	output = re.search(r"of", input)

> What does it report?

 	print(output)
 	print(output.start())
  	print(output.end())

### (2) Pattern match

	import re
	cities = ["Vancouver BC", "Calgary AB", "Edmonton Alberta", "Toronto ON"]
	for city in cities:
		output=re.search(r"(AB|Alberta)", city)
		if output:
			print(city+" is in Alberta")
		else:
			print(city+" is not in Alberta")

### (3) Capture the information by grouping

Get the city name and the province: "Vancouver BC", "Calgary AB", "Edmonton Alberta", "Toronto ON"

	import re
	cities = ["Vancouver BC", "Calgary AB", "Edmonton Alberta", "Toronto ON"]
	for city in cities:
		output=re.search(r"(\w{1,}) (\w{1,})", city)
		print("City:" + output.group(1) + ", Province:" + output.group(2))

> Practice: get the names, grades and courses of those students who _don't_ take Maths: 

	John got A in Maths, B in Arts, and C in Physics.
 	Emma got C in English, A in Chemistry, and B in History.
  	Tony got B in French, B in Maths, and D in Arts.


### (4) Find all matches

For the last example about students' grades in different courses, what if some students take 2 or 4 or more courses. For example:

	John got A in Maths, B in Arts, A in chemistry, and C in Physics.
 	Emma got C in English, and B in History.
  	Tony got B in French, B in Maths, and D in Arts.

We need to use another function _re.finditer_ to capture all matches.

	import re
	students = [
		"John got A in Maths, B in Arts, A in chemistry, and C in Physics", 
		"Emma got C in English, A in Chemistry and B in History", 
		"Tony got B in French, and D in Arts"
	]
	for student in students:
		get_name = re.search(r"^(\w+)", student)
		print("Grades for Student: " + get_name.group(1))
		all_matches = re.finditer(r"([ABCDF]) in (\w+)", student)
		for match in all_matches:
			course = match.group(2)
			grade = match.group(1)
			print(course + ":" + grade)
			

#  6. Modify

### (1) Change format

Change date format from 01/24/2025 to 2025-01-24

	import re
	input = "01/24/2025"
	output = re.sub(r"(\d{2})\/(\d{2})\/(\d{4})", r"\3-\1-\2", input)
	print(output)


### (2) Convert units: cm to mm

	import re
	input_text = "The iPhone 16 features a 6.1-inch OLED display. It is 14.76 cm long, 7.16 cm high, and 0.78 cm deep."
	pattern = r"(\d+(\.\d+)?) cm"
	def convert_to_mm(match):
		cm_value = float(match.group(1))
		mm_value = round(cm_value * 10, 1)
		return f"{mm_value} mm"
	output_text = re.sub(pattern, convert_to_mm, input_text)
	print(output_text)


## Practice

### Question 1:
A start codon is the first codon in a messenger RNA (mRNA) transcript that is translated into a protein by a ribosome. The start codon is typically ATG, but can be TTG and GTG in _E.coli_. Use regular expression to find all start codon in the following DNA sequence.

	GATCTGACTAGACATCAGGCCCGGATGCAAC

### Question 2:
Use python code and regular expression to change the author names:

> Original: Tom Jerry Li

> Changed: T.J. Li

## Extended materials

#### Tutorials

* [http://www.rexegg.com/]()
* [http://www.regular-expressions.info/]()
* [https://www.sitepoint.com/demystifying-regex-with-practical-examples/]()
* [https://www.loggly.com/blog/regexes-the-bad-better-best/]()

#### _The_ Book

* [http://shop.oreilly.com/product/9781565922570.do]()

#### Online Testers
* [https://regex101.com/]() 
* [http://www.regexpal.com/]() = [http://www.regextester.com/]()
* [http://regexr.com/]()
* [http://myregexp.com/]()

## Survey
Click [here](https://forms.gle/GDg848Jk4sRr6LaN6). Thank you!

