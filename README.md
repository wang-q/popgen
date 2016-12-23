

#

## arara and latexindent

* `indent.yaml` for arara

```bash
# curl -O https://raw.githubusercontent.com/cmhughes/latexindent.pl/master/indent.yaml
# sed -i".bak" "s/indent.pl/indent/" indent.yaml

cat <<'EOF' > indent.yaml
!config
# indent rule for arara
# author: Paulo Cereda, Chris Hughes
# last updated: 11/9/2013
# requires arara 3.0+
#
# Sample usage:
#
# % arara: indent
# % arara: indent: {overwrite: yes}
# % arara: indent: {output: myfile.tex, silent: no}
# % arara: indent: {output: myfile.tex, silent: yes, overwrite: yes}
# % arara: indent: {trace: true}
# % arara: indent: {localSettings: true}
# % arara: indent: {onlyDefault: on}
# % arara: indent: { cruft: /home/cmhughes/Desktop }
#
# Directories with spaces will cause issues in the cruft call.
#
# Note: output will take priority above overwrite
identifier: indent
name: Indent
command: <arara> @{ isWindows( "cmd /c latexindent.exe", "latexindent" ) } @{silent} @{trace} @{localSettings} @{cruft}@{ isNotEmpty( cruft, '="'.concat(parameters.cruft).concat('"') ) } @{overwrite}  @{onlyDefault} @{output} "@{file}" @{ isNotEmpty( output, '"'.concat(parameters.output).concat('"') ) }
arguments:
- identifier: overwrite
  flag: <arara> @{ isTrue( parameters.overwrite, "-w" ) }
- identifier: silent
  flag: <arara> @{ isTrue( parameters.silent, "-s" ) }
- identifier: trace
  flag: <arara> @{ isTrue( parameters.trace, "-t" ) }
- identifier: localSettings
  flag: <arara> @{ isTrue( parameters.localSettings, "-l" ) }
- identifier: output
  flag: <arara> @{ isNotEmpty( parameters.output, "-o" ) }
- identifier: onlyDefault
  flag: <arara> @{ isTrue( parameters.onlyDefault, "-d" ) }
- identifier: cruft
  flag: <arara> @{ isNotEmpty( parameters.cruft, "-c" ) }

EOF

sudo mv indent.yaml /usr/local/texlive/2016/texmf-dist/scripts/arara/rules/
```

* `defaultSettings.yaml` for latexindent

```bash
cat <<'EOF' > defaultSettings.yaml
#
# defaultSettings.yaml for latexindent.pl,
#                      a script that aims to
#                      beautify .tex, .sty, .cls files
#
# (or latexindent.exe if you're on Windows)
#
# You're welcome to change anything you like in here, but
# it would probably be better to have your own user settings
# files somewhere else- remember that this file may be overwritten
# anytime that you update your distribution. Please see the manual
# for details of how to setup your own settings files.
#
# Please read the manual first to understand what each switch does :)

# Default value of indentation
defaultIndent: "    "

# default file extension of backup file (if original is overwritten with -w switch)
# for example, if your .tex file is called
#       myfile.tex
# and you specify the backupExtension as BACKUP.bak then your
# backup file will be
#       myfileBACKUP.bak
backupExtension: .bak

# only one backup per file; if onlyOneBackUp is 0 then the
# number on the extension increments by 1 each time
# (this is in place as a safety measure) myfile.bak0, myfile.bak1, myfile.bak2
#
# if you set onlyOnebackUp to 1, then the backup file will
# be overwritten each time (not recommended until you trust the script)
onlyOneBackUp: 0

# some users may only want a set number of backup files,
# say at most 3; in which case, they can change this switch.
# If maxNumberOfBackUps is set to 0 (or less) then infinitely
# many backups are possible, unless onlyOneBackUp is switched on
maxNumberOfBackUps: 1

# some users may wish to cycle through back up files, for example,
# with maxNumberOfBackUps: 4, they may wish to delete the oldest
# back up file, and keep only the most recent.
#
#    copy myfile.bak1 to myfile.bak0
#    copy myfile.bak2 to myfile.bak1
#    copy myfile.bak3 to myfile.bak2
#    copy myfile.bak4 to myfile.bak3
#
# the back up will be written to myfile.bak4
cycleThroughBackUps: 0

# indent preamble
indentPreamble: 0

# always look for split { }, which means that the user doesn't
# have to complete checkunmatched, checkunmatchedELSE
alwaysLookforSplitBraces: 1

# always look for split [ ], which means that the user doesn't
# have to complete checkunmatchedbracket
alwaysLookforSplitBrackets: 1

# remove trailing whitespace from all lines
removeTrailingWhitespace: 0

# environments that have tab delimiters, add more
# as needed
lookForAlignDelims:
   tabular:
      delims: 1
      alignDoubleBackSlash: 1
      spacesBeforeDoubleBackSlash: 2
   tabularx:
      delims: 1
   longtable: 1
   tabu: 1
   array: 1
   matrix: 1
   bmatrix: 1
   Bmatrix: 1
   pmatrix: 1
   vmatrix: 1
   Vmatrix: 1
   align: 1
   align*: 1
   alignat: 1
   alignat*: 1
   aligned: 1
   cases: 1
   dcases: 1
   listabla: 1

# if you have indent rules for particular environments
# or commands, put them in here; for example, you might just want
# to use a space " " or maybe a double tab "\t\t"
indentRules:
   myenvironment: "    "
   anotherenvironment: "        "
   chapter: " "
   section: " "
   item: "    "

#  verbatim environments- environments specified
#  in this hash table will not be changed at all!
verbatimEnvironments:
    verbatim: 1
    lstlisting: 1

#  no indent blocks (not necessarily verbatim
#  environments) which are marked as %\begin{noindent}
#  or anything else that the user puts in this hash
#  table
noIndentBlock:
    noindent: 1
    cmhtest: 1

# if you don't want to have additional indentation
# in an environment put it in this hash table; note that
# environments in this hash table will inherit
# the *current* level of indentation they just won't
# get any *additional*.
noAdditionalIndent:
    myexample: 1
    mydefinition: 1
    problem: 1
    exercises: 1
    mysolution: 1
    foreach: 0
    widepage: 1
    comment: 1
    \[: 0
    \]: 0
    document: 1
    frame: 0

# if you want to add indentation after
# a heading, such as \part, \chapter, etc
# then populate it in here - you can add
# an indent rule to indentRules if you would
# like something other than defaultIndent
#
# you can also change the level if you like,
# or add your own title command
indentAfterHeadings:
    part:
       indent: 0
       level: 1
    chapter:
       indent: 0
       level: 2
    section:
       indent: 0
       level: 3
    subsection:
       indent: 0
       level: 4
    subsection*:
       indent: 0
       level: 4
    subsubsection:
       indent: 0
       level: 5
    paragraph:
       indent: 0
       level: 6
    subparagraph:
       indent: 0
       level: 7

# if you want the script to look for \item commands
# and format it, as follows (for example),
#       \begin{itemize}
#           \item content here
#                 next line is indented
#                 next line is indented
#           \item another item
#       \end{itemize}
# then populate indentAfterItems. See also itemNames
indentAfterItems:
    itemize: 1
    enumerate: 1
    list: 1

# if you want to use other names for your items (such as, for example, part)
# then populate them here- note that you can trick latexindent.pl
# into indenting all kinds of commands (within environments specified in
# indentAfterItems) using this technique.
itemNames:
    item: 1
    myitem: 1

# if you want to indent if, else, fi constructs such as, for example,
#
#       \ifnum#1=2
#               something
#       \else
#               something else
#       \fi
#
# then populate them in constructIfElseFi
constructIfElseFi:
    ifnum: 1
    ifdim: 1
    ifodd: 1
    ifvmode: 1
    ifhmode: 1
    ifmmode: 1
    ifinner: 1
    if: 1
    ifcat: 1
    ifx: 1
    ifvoid: 1
    ifeof: 1
    iftrue: 1
    ifcase: 1

# latexindent can be called without a file extension,
# e.g, simply
#       latexindent myfile
# in which case the choice of file extension is chosen
# according to the choices made in fileExtensionPreference
# Other file extensions can be added.
fileExtensionPreference:
    .tex: 1
    .sty: 2
    .cls: 3
    .bib: 4

# preferences for information displayed in the log file
logFilePreferences:
    showEveryYamlRead: 1
    showAlmagamatedSettings: 0
    endLogFileWith: '--------------'
    traceModeIncreaseIndent: '>>'
    traceModeAddCurrentIndent: '||'
    traceModeDecreaseIndent: '<<'
    traceModeBetweenLines: "\n"

# \begin{document} and \end{document} are treated differently
# by latexindent within filecontents environments
fileContentsEnvironments:
    filecontents: 1
    filecontents*: 1


# *** NOTE ***
# If you have specified alwaysLookforSplitBraces: 1
# and alwaysLookforSplitBrackets: 1 then you don't need
# to worry about completing
#
#       checkunmatched
#       checkunmatchedELSE
#       checkunmatchedbracket
#
# in other words, you don't really need to edit anything
# below this line- it used to be necessary for older
# versions of the script, but not anymore :)
#***      ***

# commands that might split {} across lines
# such as \parbox, \marginpar, etc
checkunmatched:
    parbox: 1
    vbox: 1

# very similar to %checkunmatched except these
# commands might have an else construct
checkunmatchedELSE:
    pgfkeysifdefined: 1
    DTLforeach: 1
    ifthenelse: 1

# commands that might split []  across lines
# such as \pgfplotstablecreatecol, etc
checkunmatchedbracket:
    pgfplotstablecreatecol: 1
    pgfplotstablesave: 1
    pgfplotstabletypeset: 1
    mycommand: 1
    psSolid: 1

EOF

sudo mv defaultSettings.yaml /usr/local/texlive/2016/texmf-dist/scripts/latexindent/

```