# GitHub Documentation Syntax

Table of Contents
- Basic and Advanced Github Formatting
  - Headings
  - Styling
  - Quoting Code
  - Creating Highlighting Code Blocks
  - Relative Links
  - Lists
  - Task lists
  - Footnotes

## Basic and Advanced Github Formatting
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

<details>
  <summary>
  Relative links
  </summary>
  <p>
  You can define relative links and image paths in your rendered files to help readers navigate to other files in your repository.

  A relative link is a link that is relative to the current file. For example, if you have a README file in your root of your repository, and you have another file in docs/CONTRIBUTING.md, the relative link to CONTRIBUTING.md in your README.md might look like:

  Syntax:
  ```
  [Contributing guidelines for this project](docs/CONTRIBUTING.md)
  ```
  Output:
  [Contributing guidelines for this project](docs/CONTRIBUTING.md)
  </p>
</details>

<details>
  <summary>
  Lists
  </summary>
  <p>
  You can make an unordered list by preceding one or more lines of text with -, *, or +.

  Syntax:

  <quote>
  - CSharp
  + CSharp
  * CSharp
  </quote>

  Output:
  - CSharp
  + CSharp
  * CSharp
  </p>

  To order your list, precede each line with a number.
  Syntax:
  <quote>
  1. CSharp
  2. CSharp
  3. CSharp    
  </quote>

  Output:
  1. CSharp
  2. CSharp
  3. CSharp
</details>

<details>
  <summary>
  Task lists
  </summary>
  <p>
To create a task list, preface list items with a hyphen and space followed by [ ]. T
To mark a task as complete, use [x].

Syntax:
<quote>
  - [x] #739
  - [ ] https://github.com/octo-org/octo-repo/issues/740
  - [ ] Add delight to the experience when all tasks are complete :tada:
</quote>

Output:
- [x] #739
- [ ] https://github.com/octo-org/octo-repo/issues/740
- [ ] Add delight to the experience when all tasks are complete :tada:
  </p>
</details>

<details>
  <summary>
  Footnotes
  </summary>
  <p>
  You can use add footnotes to your content by using this bracket syntax:

  Example:

  Here is a simple footnote[^1].

  [^1]: My footnote reference.

  Output:
Here is a simple footnote[^1].

[^1]: My footnote reference.  
  </p>
</details>