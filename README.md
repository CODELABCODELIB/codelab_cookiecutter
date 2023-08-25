## Ruchella's Coding Conventions
Coding conventions are a set of guidelines and rules that define how code should be written and formatted in a consistent and standardized way within a project. Have you ever tried to rerun an analysis you did several months or even years ago? While you might have written the code, it becomes very hard to make sense of anything after some time passes. That is where coding conventions come in. These conventions are essential for promoting readability, maintainability, and collaboration.

If you are just starting out learning how to program then coding conventions are the last thing you're probably thinking about. You just want your code to work. However coding conventions will make the life of future you (when you need to understand what you did) and other people who want to use your code much easier. After it runs you can 'clean' your code by following these coding conventions. 

This document contains coding conventions I use and have developed by learning the hard way on what I forgot in the future. In the long run it's meant to make your life much easier. This document mostly applies to matlab codes. Also check out my documentation on 'Data management' [TODO insert link].

_Note: If you are programming in a specific language that that language already has its own coding conventions (There is a whole book written on it for [matlab ](http://cnl.sogang.ac.kr/cnlab/lectures/programming/matlab/Richard_Johnson-MatlabStyle2_book.pdf))._

# Getting Started
What is included in this document: 
- [Folder Structures](#folder-structures) : Learn about the recommended way to organize your project's folders and directory structure. A well-structured project helps you keep track of your code, data, and documentation.
- [Version Control and Collaboration](#version-control-and-collaboration) : Learn best practices for version control that enables collaboration with other members of the lab
- [Naming Conventions](#naming-conventions) : How to name your files, variables, etc. 
- [Error Warnings and Breaks](#error-warnings-and-breaks) : Discover how to implement error handling, warnings, and breaks in your code. Prevent your code from crashing.
- [Documentation](#documentation) : What to include in the documentation of your code 
- [Commenting](#commenting) : Master the art of commenting within your code. Follow the provided template to document functions, inputs, outputs, and important sections of your code.


## Folder Structures
Here is a good way to organize every folder you work with
```
Project Root
|-- data : any data required to run the codes or data results from processed codes
|   |-- raw : save your results datasets here
|   |-- processed : save your results datasets here
|   |   |-- figures : Save your result plots here
|-- docs : documentation of the pre processing codes
|-- src :  All your codes go here
|   |-- main_your_project.m : one main script that calls the other functions
|   |-- playground : functions to explore the data and any code you don't end up using (deprecated)
|   |-- testing : functions to test your code
|   |-- EEG : functions for EEG analysis
|   |-- smartphone : functions for smartphone behavioral analysis
|   └── utils  :Utility  functions these are general functions that just aid your code in running like nanzscore.m
|-- README.md main documentation on how to get started 
|-- requirements.txt  dependencies of your project
```
An extra note on 'main_your_project.m' make sure not to call your functions in the command line because then its impossible to know exactly what you did if you need to debug in the future. 

### Startup via Linux

Run this bash script to create this directory

```Bash
    chmod u+x setup.sh
    ./setup.sh
```
After running the script make sure you can add any other specific folders you might need such as the EEG analysis folder.

### Alternative startup

Run this matlab script to create and add your directory to path. First create the folder for your project and go in that folder then run these commands:

```Matlab
%% create the folders
mkdir docs
mkdir data/raw
mkdir data/processed
mkdir src/playground
mkdir src/utils
%% create the files
% you can edit this path
addpath(genpath('.'))
edit ./src/main.m
edit ./README.md
edit ./requirements.txt
```

## Version Control and Collaboration
Before you start a project create a github repository. When you create the repository carefully consider whether your code can be made public. If you have copied code from a private repository then this code cannot be made public in your repository. Add the right collaborators to your repository. 

Version control should be done via github and not by saving a file with a different name such as `testing_v1.m` or `testing_v2.m` unless `testing_v1` is used somewhere else there is no point of creating a seperate file. If you want to recover something from  `testing_v1` just look at the code history on github. 

Best practice is to commit and push your code as soon as you are done with a section (for example you finished the codes used to load the data). If you do not finish the section you can commit your progress at the end of the day. Also commit your code if you are going to make major changes incase you want to revisit a previous version. Write descriptive commit messages so you can find when you added something. Do not just write messages such as 'code update', instead write something like: 'load EEG struct and calculate ERP'

Before you commit your code makesure you create a `.gitignore` file. You should never push your data to github. This is an example gitignore based on the example 'Folder Structure' provided above. 
```
data
src/playground
*~
```

_Note: If you are using another programing language such as python create and activate an [enviroment](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#) to control the version of the packages you are using._

## Naming Conventions
These dictate how variables, functions, classes, and other code elements should be named. You can use 
- `snake_case` all lowercase words are seperated by '_'
- `PascalCase` all first letters upper case
- `camelCase` first letter of a word lowercase all subsequent words have the first letter uppercase

Essentially it doesnt matter which one you choose as long as you just use that one. You can choose to mix them only in the case for example using `snake_case` for datasets and `camelCase` for variables. I use `snake_case` for everything. 

Most importantly use clear and descriptive names for your variables and file names, it make it easier for everyone (including future you) to understand the purpose of each element. That means do not use just one letter such as 'i' in a for loop. 

## Error warnings and breaks
To avoid errors, especially when other people use your code make sure that the arguments that go into your function are correct. I like to use matlab function declaration, read the full documentation of how it works [here](https://nl.mathworks.com/help/matlab/ref/arguments.html). A quick example is :
```Matlab
arguments
    EEG struct; % Check if input is a cell
    labels (1,:) {mustBeNumeric};  % check the size of labels and make sure there is only numeric values
    options.threshold {mustBeGreaterThanOrEqual(options.threshold,0)} = 5; % check if threshold is at least a value of 0
    options.plot_data logical = 0; %  make sure onlya value of 0 or 1 is accepted
end
```

After the arguments before starting your analysis, some things can be further validated for example:
```Matlab
% validate that all 64 electrodes are present
if size(EEG.data,1) < 64
    error('Missing blink electrodes')
end
```
Choose wisely between an `error` when your code won't work at all without this, a `warning` your code will still work but the results might differ, or even `fprintf` just display a message.

## Documentation
I like to write my documentation with [spinx](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html) it uses reStructuredText (rst) files to create beautiful documentation. 
In the docs I have provided the Makefile to create the HTML, simply write 'make HTML' and it will build a folder with your RST files as HTML. 

What to include in your documentation? Here is a quick markdown inside markdown starting place:
```
# Project Title
Brief description or introduction about the project.

## Getting Started
Briefly explain how to get started with the project. Include prerequisites and any initial setup required.

## Features
List the key features or functionalities of the project.

## Dependencies
Include any dependencies and links on how to install them.

## Usage
Explain how to use the project. Include examples and usage scenarios.

## Quick overview repository structure
Explain what is in the repository where can the user find which functions

## References
Any references of toolboxes or papers relevant for the project

# About
Year, Leiden University Released under the [Mention the project's license and provide a link to the full license text](https://) license.

## Citations or Authors
Write the citation of your paper or if multiple contributors to your project include them and a link to their github or socials.
```

## Commenting
For each function make sure to document the input and outputs. I generally follow these principles:  

- **Function Description:** Begin with a brief explanation of the function's purpose in one line.
```Matlab
%% Explain what the function does in one line
```
- **Usage:** Illustrate various function calls using required and optional arguments. Use this format:
 ```Matlab
 % - [output_1, output_2] = example_function(required_argument_1, required_argument_2)
 %                        - example_function(..., optional_argument_1)
 %                        - example_function(..., optional_argument_n)
 %                       - example_function(..., 'optional_name_argument_1', 'optional_name_value_argument_1')
```
- **Input(s):** List and describe required input arguments, using this format:
```Matlab
% Input(s):
%    required_argument_1 = explanation 
%    required_argument_2 = explanation 
```
- **Optional Input Parameter(s):** Enumerate optional input arguments with default values, as shown:
```Matlab
% Optional input parameter(s):
%    optional_argument_1 (default : x) = explanation
%    optional_argument_n (default : x) = explanation
%    optional_name_argument_1 (default : x) = explanation - possible values 
```
- **Output(s):** Describe output variables formatted as:
```Matlab
% Output(s):
%   output_1 = explanation
```
- **Long explanations** : for any argument descriptions with long explanations go up until the maximum characters then continue on newline with the explanation indented
```Matlab
%   output_2 = Long explanation up until the maximum characters then newline
%              finish the long explanation
```
- **Author and Date:** Provide your name, affiliation, and the date when you wrote the function. If the function is edited, include the date of editing, your name, and affiliation.
```Matlab
% Your name, Affiliation, day/month/year written
% Edited  day/month/year written, Your name, Affiliation
```

Here is a version you can copy paste and use in your own code:
```Matlab
%% Explain what the function does in one line
%
% **Usage:**
%   - [] = example_function()
%                         - example_function(...,)
%
% Input(s):
%    required_argument_1 =  
%
% Optional input parameter(s):
%    optional_argument_1 (default : ) = 
%    optional_name_argument_1 (default : ) = 
%
% Output(s):
%   output_1 = 
%
% Your name, Affiliation, day/month/year written
```

Finally, aside from documenting a function make sure to also add comments inside the function explaining especially the complicated parts of the code. If your code does multiple things seperate them into sections with:
```Matlab
%% Explanation of section
```

If you are using a script and it is very long also add larger seperation blocks such as:
```Matlab
%% %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%% start of a block with multiple sections %%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
```
However, at that point it might be better to create multiple files with the different blocks 






