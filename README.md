# spark dotnet repl

Microsoft are creating an actual repl and it looks pretty awesome but I really miss using spark-shell and being able to quickly iterate on spark code to make sure it does what I want. I wrote this so I could write spark dotnet code in a repl and have it behave the same as spark-shell.

This is only ever going to be useful for anyone who uses https://github.com/dotnet/spark

See it in action: https://youtu.be/iEpXyzPVGGc

# Setup

You will either need to set these environment variables:

SPARK_HOME
JAVA_HOME

or put them in the app.config (spark-repl.exe.config) - there are example entries in there.

The way it works is it starts the dotnet backend using debug, I redirect stdout and stderr into this console and use the roslyn parser stuff to execute the csharp code.

# What can it do?

It can call any of the Microsoft.Spark functions (or any other c# function), a couple of things to note:

- the way I execute the csharp and keep the context if you do something like `a == b` the result is thrown away so you can evaluate any expressions by putting a `$` at the beginning:

```csharp
"abc" == "def"
```

shows no result

```csharp
$"abc" == "def"
```

shows "False"

You can also save the result into a variable and then the result will be shown:

```csharp
var result = "abc" == "def"
```

shows the result. Or you can do the good old `Console.WriteLine("abc" == "def")` which works as great as you would expect.

# Intellisense

This was harder to implement and as this is a stop gap and I wanted to get it done in an evening, this doesn't hae intellisense but if you have an object you can see the available methods and properties:

```csharp
var catalog = spark.Catalog()
:catalog cache
```

This shows the signatures of any method on catalog that contains the work cache - so you can quickly see what you can do, it just doesn't do it for you.

# Output guff

With spark there is a lot of output, you can disable the output using:

```csharp
:spark error off
:spark output off
```

turn it back on using:

```csharp
:spark error on
:spark output on
```

## list all your variables

by doing:

```csharp
:variables
```

and see methods using:

```csharp
:variableName
```

and filter the signatures using:

```csharp
:variableName filter
```

## that is it

You need to make sure everythign is setup correctly and it should be groovy - any issues create an issue here.
