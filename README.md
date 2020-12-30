# Layout

Layout is a lightweight markup language that builds interactive web pages



## Introduction 

Layout describes components of your web page using content enclosed between begin and end words

	`
	begin component
		more components and content
	end component
	`
The begin and end words should be followed by matching labels to begin and end a component as shown above.

Layout supports few components, making it easy to remember and work with. Some components are visible on the web page and others are not.

Visible components are found inside the layout component. The layout component can be thought of as your page and it is used as the first component which will contain all other visible components.

## Visible Components

There are two types of visible components: *structural* and *content*. The final visible content is placed inside the content component.


- Structural Components
	1. layout
	2. section
	3. row

- Content Components
	1. col or column

- Content Types
	1. image
	2. video
	3. text
	4. website
	5. button

	`
	begin layout
		begin section
			begin row 1
				begin column 1
					# visible content here
				end column 1
				begin column 2
					# visible content here
				end column 2
			end row 1
			begin row 2
				# more components here
			end row 2
		end section
	end layout
	`

## Rows and Columns

Rows and Columns need an additional label to indicate the row number and column number. 

Columns make use of content types followed by the value as shown below:

	`
	begin column 1
		use image
		value photo.png
	end column 1
	`
	This means that photo.png is a picture in the same directory as the file.
	
	## Automatic values
	
	Instead of specifying the value as photo.png, you can specify the value as auto and Layout will automatically use an image with the name that matches the row and column number
	
	`
	begin row 2
		begin column 1
			use image
			value auto
		end column 1
	end row 2
	`
	
	The code above assumes there is an image named r2c1.png 
		- the extention ".png" does not matter, it can also be r2c1.jpg and this would still work.
		- automatic values can only be used for content types *image* and *video*

## Styles

Styles are applied inside a component before a new *component* or *content* begins

	`
	begin section
		black background-color
		white color
		photo.png background-image
		50 width on cells
		
		begin row 1
			blue background-color
			100 width
			begin column 1
				pink color
				use text
				value hello world
			end column 1
			begin column 2
				*will use white color inherited from section*
			end column 2
		end row 1
		begin row 2
			*will use black background-color*
		end row 2
	end section
	`
	## Widths and Heights
		
		- Widths ands heights are always equal unless both specified
		- They use the percentage of the page size unless px is specified *50px width*
		- Using width and height style on the section only sets with width and height of the section 
			- Speciying "on cells" will set the width on all the cells of the table within the rows and columns
			- *row width is not cell width*
	

## Invisible Components

Invisible components are there to help visible components so as to write less.
They need additional labels using #hashtags which must be unique 

	1. reuse
	2. repeat
	3. action
	
## reuse

The reuse component can store any components and styles within itself and can be reused by other components.

	`
	begin reuse #mycode
		*components and content  here*
	end reuse
	
	begin layout
		begin section
			reuse #mycode
		end section
	end layout
	`
	
	The reuse component is not displayed, but all of its content is copied over where it reused.
	Note the difference between using a content type e.g. use image compared to using a reusable component created by you: reuse #mycode
	
	## reuse with value labels
	
		- The #mycode is the label for your reuse component, but within itself, it can use value labels using the @symbol
		- After the reuse label, you add "with" followed by as many value labels 
		
		`
		begin reuse #makename with @columnNo @myname @mysurname
			begin column @columnNo
				use text
				value @myname @mysurname
			end column @columnNo
		end reuse
		
		begin layout
			begin section
				begin row 1
					reuse #makename 1 John Doe
					reuse #makename 2 Jane Doe
				end row 1
			end section
		end layout
		`
		
		- The above code will replace "reuse #makename 1 John Doe" with:
			`
			begin column 1
				use text
				value John Doe
			end column 1
			begin column 2
				use text
				value Jane Doe
			end column 2
			`
		- By making use of the reuse with labelled values, and reusing it by passing those values, less code was written
		- *Note* that each value passed is matched according to the order passed.
		- *Note* that the reuse components should be created before they are used.

## repeat

The repeat component repeats its contents over a range of values
The repeat component does not need its own label as it is not reused from a different section 
	
	`
	begin repeat from 1 to 10 by 1 with @label
		*more components and content*
	end repeat
	`
	This will create the inner content 10 times because:
		- The start and end numbers are included in the range
		- "by 1" means the range changes by increments of 1
		- It is also possible to repeat from 10 to 1
	
	The @label keeps the current value of the range and this can be used inside the repeat
	
		`
		begin repeat from 1 to 3 by 1 with @row
			begin row @row
				begin repeat from 1 to 2 by 1 with @col
					begin col @col
						use image
						value auto
					end col @col
				end repeat
			end row @row
		end repeat
		`
