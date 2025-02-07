```r
# SCRIPT INFORMATION ====
# Collection: All
# Asset: R coding standards
# Inputs: N/A
# Outputs: N/A
# Author: Stephanie McLean
# Last updated (and why): 8/03/2023 (first version of file)


# SCRIPT HEADERS ====

# Each script should have a header detailing what the script is, inputs, outputs, the author and when it was last updated.
# SCRIPT INFORMATION ===
# Collection: Student 
# Asset: SB265_00001
# Inputs: [dbo].[SB265_00001QA]
# Outputs: Figure 1.csv
# Author: _______
# Last updated (and why): 2021/22 (changed output file folder location)


# SORTING SCRIPTS INTO SUBHEADINGS OR SECTIONS ====

# Use the following code to separate code into relevant sections or subheadings. 
# Sections can easily be found on the left hand side of the script 
# Example: INSTALL AND LOAD PACKAGES ====#


# NOTATION AND NAMING ====

# --- FILE NAMES --- #
# File names should be meaningful and end in .R 
# Example: SB265_00001.R 

# If files need to be run in an order then prefix them with numbers 
# Example: 0_SB265_00001.R, 1_SB265_00001_csv.R 


# --- OBJECT NAMES --- # 
# Variables and function names should be lowercase. 
# Words within a name should be separated by an (_). 
# Names should be concise and meaningful. 
# Example: sample_one NOT sampleone 

# Avoid using names of existing functions or variables 
# Example: 
T <- 10 #T is short for TRUE in R 


# SPACING ====
# Place spaces around all infix operators (=, +, -, <-, etc.). 
# The same rule applies when using = in function calls. 
# Put a space after a comma, and never before (just like in regular English).
# Example: 
average <- mean(feet / 12 + inches, na.rm = TRUE)

# $, @, :, :: and ::: don’t need spaces around them.
# Example:
x <- 1:10
base::get
data$variable

# Place a space before left parentheses, except in a function call.
# Example:
if (debug) do(x)
plot(x, y)

# Spaces are not required around code in parentheses or square brackets.
# Example: 
if (debug) message("debug mode")
species["tiger", ]

# CURLY BRACKETS ====
# An opening curly bracket should never go on its own line and should always be followed by a new line.
# A closing curly bracket should always go on its own line, unless it’s followed by else.
# Always indent the code inside curly brackets.
# Examples:
if (y < 0 && debug) {
  message("Y is negative")
}

if (y == 0) {
  log(x)
} else {
  y ^ x
}

# Curly brackets and new lines can be avoided, if a statement after if is very short.
# Example:
if (is_used) return(value)


# LINE LENGTH ====
# Try to keep code to 80 characters per line to keep it readable. 


# INDENTATION ====
# When indenting your code, use two spaces. Never use tabs or mix tabs and spaces.
# The only exception is if a function definition runs over multiple lines. In that case, indent the second line to where the definition starts:
  
  long_function_name <- function(a = "a long argument", 
                                 b = "another argument",
                                 c = "another long argument") {
    # As usual code is indented by two spaces.
  }


# ASSIGNMENT ====
# Use <-, not =, for assignment.
# Example:
x <- 5


# COMMENTING ====
# Use # followed by a space to add comments to code.
# Comments should explain the why, not the what.
# To comment/uncomment large chunks of code use Command+Shift+C
# Example: 
i <- 1 # define iterator
NOT
i <- 1 # set i to 1


# new line stuff (if, functions, for loops??)
# layout script so doing loading, set up initially so have what is needed
# library() instead of require()

```
