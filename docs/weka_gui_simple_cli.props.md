

# File
`weka/gui/SimpleCLI.props`

# Description
Contains properties for the SimpleCLI, e.g., the command history. Whenever the user issues a command, the history gets saved in the user's home directory (`$HOME/SimpleCLI.props` on Linux or `%USERPROFILE%/SimpleCLI.props` on Windows).

# Version
* > 3.5.6

# Fields
* `HistorySize`
> the maximum number of most recent commands to store in the properties file (in the user's home directory).
* `Command X`
> lists command **X** of the history, with **X** being an integer starting from 0.

# See also
* [Properties file](properties_file.md)
