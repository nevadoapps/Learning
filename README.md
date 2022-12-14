# GitHub Usage

Table of Contents
- Basic and Advanced Github Formatting
  - Headings
  - Styling
  - Quoting Code
  - Creating Highlighting Code Blocks

<details>
  <summary>Basic and Advanced Github Formatting
</summary>
<p>
<details>
  <summary>
    Headings
  </summary>
  <p>
  To create a heading, add one to six # symbols before your heading text. The number of # you use will determine the size of the heading.

  Example:
  - # The largest heading
  - ## The second largest heading
  - ###### The smallest heading
</p>
</details>

<details>
  <summary>
    Styling
  </summary>
  <p>
You can create tables with pipes | and hyphens -,. Hyphens are used to create each column's header, while pipes seperate each column. You must include a blank line before your table in order for it to correctly render.

| Style | Syntax | Keyboard Shortcut | Example | Output |
| --- | --- | --- | --- | --- |
| Bold | ** ** or __ __ | Ctrl + B | <quote>** This is bold text **</quote> | **This is bold text** |
| Italic |	* * or _ _ | Ctrl+I | * This text is italicized *	| *This text is italicized* |
| Strikethrough | 	~~ ~~	| | ~~ This was mistaken text ~~ | ~~This was mistaken text~~ |
| Bold and nested italic	| ** ** and _ _	| | ** This text is _extremely_ important ** |	This text is extremely important |
| All bold and italic |	*** ***	| | *** All this text is important ***	| ***All this text is important*** |
| Subscript	| <sub> </sub> | | <sub> This is a subscript text </sub> |	This is a subscript text| 
| Superscript	| <sup> </sup> | | <sup>This is a superscript text</sup> |	This is a superscript text |
</p>
</details>

<details>
  <summary>
  Quoting Code
  </summary>
  <p>
  You can call out code or command within a sentence with single backticks. (AltGr + , / ş or Ctrl + E) The text within the backticks will not be formatted. 

  Example: 
  git status 
  git add
  git commit

  Some basic Git commands are:
  ```
  git status
  git add
  git commit
  ```
  </p>
</details>

<details>
  <summary>
    Creating Highlighting Code Blocks
  </summary>
  <p>
  You can create fenced code blocks by placing triple backticks ``` before and after the code block. We recommend placing a blank line before and after code blocks to make the raw formatting easier to read.

  Example:
  using System.Collections as;

  ```csharp
  using System.Collections;
  ```
  </p>
</details>


<p>
</details>
