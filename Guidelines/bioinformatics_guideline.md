## Ling lab bioinformatic guidelines ##

This guidelines are loosely based upon:
- [Bioinformatics zen](http://www.bioinformaticszen.com)
- Excellent book ['Advanced R'](http://adv-r.had.co.nz/)

To work on bioinformatics projects, so that it doesn't become a complete mess, you should at least know (in any order):

- R
- Linux command line basics [This, for example](http://linuxcommand.org/)
- GNU Make
- Git (and github)
- Markdown for readme files

Good to know (descending order of importance, increasing order of difficulty):
- Shell programming
- Python
- C++
- Haskell

### General ###

- Always use Makefile to put pipelines together. It should be possible to recalcuate the whole project using the single makefile command. [Read more](http://www.bioinformaticszen.com/post/functional/)
- Always use git.
- All operations should be only executed on condor using 'csubmit.sh'. (TODO) this won't work with makefile. I'm writing a new script that will take care of this.
- All files should use LF endings. Set up your IDE accordingly.
- All raw data should be softlinked from Raw_Data_Archive
- No part of the analysis should be conducted in the R enterperer line or os command line (shell scripts are of course fine)
- If you catch yourself copy-pasting from one file to another - don't to it, wrap it in a function instead and put into Scripts/yourlanguage/Functions/ (or equivalent) directory.
- If you catch yourself copying a function from one project to another - stop and move it to a package of some sorts.

### Git and remote repositories ###

- Commit only project ./Scripts folder to the remote repo.
- Be sure that scripts do not contain any sensitive patient data. Use private repository if have doubts.
- Do not use git-lfs to store files other then scripts. It is important that patient-related files do not leave inner network.
- Main "Scripts" folder should only be updated by 'git' commands. If you use any kind of automatic deployment for testing or development purposes, never deploy to the directory of the git repo. Create another directory instead.
For example, if you work on "Scripts" directory and use automatic deployment, deploy to "ScriptDeployment" directory, not to "Scripts" itself.

(TODO)
*Think how to organize all github repos*
*Probably use*

### Data ###

- Data, if correct, is immutable. Scripts should never modify data files in place, only create modified copies.
- Prefer .RData to text to pass data between R scripts, if possible.
- Prefer picle to text to pass data between Python scripts.

### Project structure ###

#### All projects ####

##### Root project directory #####

    Annotations/
    Configs/
    Logs/
      CondorLogs/
      RoutFiles/
    Persistance/
    Plots/
    QC/
    Results/
    RawData/
    Scripts/
    Temp/
    Makefile
    Readme.md

##### General #####

  - .RData files should always be saved to a Temp folder
  - Project root should be inside Active_Projects/Project_Name/Private/ . Soflink into your home if you will.
  - RawData/ is always softlinked, not copied, frow Raw_Data_Archive
  - If you start a new project, good chances it done before by someone. In this case, you can fork a project template from (TODO) the github repo.

##### Scripts directory #####

- This project is always a git root
- Each programming language used to perform the analysis should have a separate directory.
- Scripts should be grouped by subdirectories based on script logic.
- Each subdirectory should have a Readme.md file, which describes the logic the scripts are based upon, and what each script does
- IDE settings, additional files and memos should never deployed to a common server. Ok to add to .gitignore

Example:

      Scripts/
        .git
        R/
          Analysis1/
            Subanalysis1/
          Analysis2/
          Functions/
          Preprocessing/
          Postprocessing/
          QC/
          Plots/
        Ruby
          Analysis1/
            Subanalysis1/
          Analysis2/
          Preprocessing/
          QC/
        Haskell/
      .gitignore

##### Persistance directory #####

- Each persistance file type used to perform the analysis should have a separate directory:

      Persistance/RData/
      Persistance/Txt/
      Persistance/Pickle/
      Persistance/HDF5/

### File names ###

- File names should be meaningful, in other words always infer their content.
- File names should be lowercase and separated by '_':

      perform_eqtl_analysis.sh

### Coding style ###

#### Shell scripts ####

- Always use '#!/usr/bin/env bash' shebang line

#### R ####

For more details, read [Hadley Wickham conding style](http://adv-r.had.co.nz/Style.html).
Here is the short summary:

##### *General* ######

- Don't use save, load, save.image or load.image for persistance. Use saveRDS or loadRDS instead.
- Separate the scripts on the logical sections

      # Load data ---------------------------
      # Plot data ---------------------------

- Reusable functions should be saved in Functions/ directory
- Try to reduce OOP usage. Only use one OOP system in the project, preferably R6.

##### *Logging* #####

- Never use print or cat to log messages. Use a package like futile-logging instead

##### *Files* ######

- All scripts should have .R extension
- All scripts should have '#!/usr/bin/env Rscript' shebang line.
- If files need to be run in sequence, prefix them with numbers:

        0-download.R
        1-parse.R
        2-explore.R

##### *Variable names* ######

- Variable and function names should be lowercase. Use an underscore (_) to separate words within a name
- No meaningless variable names like 'a' or 'result' allowed.

##### *Assignment operation* #####

- Use '<-' instead of '=' for assignment.
- Use whitespace around '<-'

Example:

    a \<- b,

  not

    a = b

  or

    a\<-b


##### *Assignment operation* #####

### Software recomendations ###

Why IDE is a personal choise, I need to notice that set of IDEs from Jetbrains is almost perfrect in every regard.
I personally reccomend to use PyCharm IDE with R plugin installed. The main benefit is that it has an option of automatically deploy files to the Purple server.
