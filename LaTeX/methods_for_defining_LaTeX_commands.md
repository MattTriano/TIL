# Methods for defining LaTeX commands

Base LaTeX provides (at least) the following commands for defining the behavior associated with a name. In these examples, `defn` represents the definition of behavior or properties to be encapsulated in an object that can be invoked by the name `a_name`.

* `\newcommand{\a_name}{defn}`
	* This defines a new command or variable, and will throw an error if the given name (`a_name`) is already defined.
* `\renewcommand{\a_name}{defn}`
	* This redefines and already defined command or variable, and will throw an error if the given name (`a_name`) has not yet been defined.
* `\providecommand{\a_name}{defn}`
	* This defines a new command or variable only if the given name (`a_name`) has not yet been defined, otherwise `\providecommand` does nothing.
* `\def{\a_name}{defn}`
	* This defines a command or variable without doing any of the checks provided by the above methods.