
# File
`weka/core/logging/Logging.props`

# Description
Defines the type of logging Weka performs. The default is to log regular log messages from the GUI and everything that is output to `stdout` and `stderr` to `$HOME/weka.log`.

# Version
* > 3.5.8

# Fields
* `Logger`
> The logger class to use for logging. See Javadoc of respective logger class (derived from `weka.core.logging.Logger`) for more information.
* `MinLevel`
> Sets the minimum level a log messages needs to have in order to end up in the log. `ALL` will log everything, `OFF` turns logging off.
* `DateFormat`
> The [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601) format of the date. Here is the default value:
> `yyyy-MM-dd HH:mm:ss`

* `LogFile`
> In case a logger class logs to a file, like `weka.core.logging.FileLogger` and `weka.core.logging.OutputLogger`, this file will be used. These loggers clear the log-file everytime Weka is started.

# See also
* [Properties file](properties_file.md)

# Links
* [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601)
