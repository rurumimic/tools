# sed, a stream editor

[gnu](https://www.gnu.org/software/sed/manual/sed.html)

## Running sed

### Overview

```bash
sed SCRIPT INPUTFILE
``

`sed` filters the contents of the standard input.  
`sed` writes output to standard output.  

```bash
sed 's/hello/world/' input.txt > output.txt
sed 's/hello/world/' < input.txt > output.txt
cat input.txt | sed 's/hello/world/' - > output.txt
```

- `-i`: edit files in-place instead of printing to standard output.

- `-n`: suppress output.
- `-n '45p'`: suppress output, and print specific lines.
- `-n '2,4p; 3p; 3p; 3p'`: suppress output, and print lines 2, 4 once, and line 3 four times.
- `-n '1p; $p'`: suppress output, and print the first line of the first file and the last line of the last file.

- `-e`, `-f`: can be combined and can appear multiple times.
- `-e SCRIPT`: with script.
- `-f SCRIPT_FILE`: with script file.

### Options

```bash
sed OPTIONS... [SCRIPT] [INPUTFILE...]
```

- `--version`
- `--help`
- `-n`, `--quiet`, `--silent`: disalbe the automatic printing. only produce output when explicilty told to via the _p_ command.
- `--debug`
- `-e script`, `--expression=script`
- `-f script-file`, `--file=script-file`
- `-i[SUFFIX]`, `--in-place[=SUFFIX]`: edit files in-place. implies `-s`.
	- when the end fo the file is reached, the temporary file is renamed to the output file's original name.
  - the extension, if supplied, is used to modify the name of the old file before renaming the temporary file.
	  - == making a backup copy
		- contain one or more `*` characters: each asterisk is replaced with the current filename. for prefix or moving files into another directory(provided the directory already exists).
			- `-i../path/prefix.*`
		- doesn't contain a *: appended as a suffix.
  - with `-n` and without `p`: output file will be empty.
- `-l N`, `--line-length=N`: line-wrap length default value is 70. with _l_ command. `-l N 'l'`, `-n 'l 5'`.
- `--posix`: GNU sed includes several extensions to POSIX sed.
- `-b`, `--binary`: open input files in binary mode. when this options is specified in Windows, `\r\n` is not processed.
- `--follow-symlinks`: with option `-i`.
- (`-E`), `-r`, `--regexp-extended`: use exetended regular expressions. 
- `-s`, `--separate`: consider files as separate.
- `--sandbox`: `e/w/r` commands are rejected. 
- `-u`, `--unbuffered`: buffer both input and output as minimally as practical. useful with `tail -f`
- `-z`, `--null-data`, `--zero-terminated`: separate lines by NUL characters

## sed scripts

### sed script overview

command syntax:

```bash
[addr]X[options]
```

- X: a single-letter sed command.
- addr: an optional line address.
	- X will be executed only on the matched lines.
  - a single line number, a regular expression, a range of lines
- options: used for some sed commands.

### the s command

_substitute_: the most important in sed.

syntax: `s/regexp/replacement/flags`

## Addresses: selecting lines

### addresses overview

```bash
sed 's/hello/world/' # all lines

sed '144s/hello/world/' # line 144
sed '4,17s/hello/world/' # in lines 4 to 17
sed '/apple/s/hello/world/' # in lines containing the world 'apple'

sed '4,17!s/hello/world/' # in lines 1 ~ 3 and 18 ~
sed '/apple/!s/hello/world/' # in lines not containing the world 'apple'
```

## Advanced sed: cycles and buffers

### How sed works

sed maintains two data buffers:

- _active pattern_ space
- _auxiliary hold_ space

both are initially empty.

1. read one line from the input stream.
1. remove any trailing newline.
1. place it in the pattern space.
1. each command can have an address(a condition code) associated to it.
1. verify the condition and execute commands.
1. the contents of pattern space are printed out to the output stream.
1. then the next cycle starts for the next input line.
1. unless special commands (like 'D') are used, the pattern space is deleted between two cycles.
1. the hold space keeps its data between cycles.

### Hold and Pattern Buffers
### Multiline techniques - using D,G,H,N,P
### Branching and Flow Control
