# PowerShell scripting

- [PowerShell scripting](#powershell-scripting)
  - [Variables](#variables)
  - [Run commands](#run-commands)
  - [Functions](#functions)
  - [Folders](#folders)
  - [Scopes](#scopes)
  - [Loops](#loops)
    - [foreach](#foreach)
  - [Exceptions](#exceptions)

## Variables

Set variable:

```ps1
$var1 = Get-Process chrome
```

## Run commands

Run commands or expressions on the local computer:

```ps1
Invoke-Expression "git --version"
```

## Functions

Defining and calling function that accepts 1 parameter:

```ps1
function Failure($msg) {
    Write-Host $msg -ForegroundColor Red
}

Failure("Failure message")
```

## Folders

Check if folder exists:

```ps1
if (Test-Path -Path "path/to/folder") {
    
}
```

Create new directory supressing output:

```ps1
New-Item -ItemType Directory -Force -Path src
```

## Scopes

Use `$script scope` if you want your counter to reset each time you execute the same script:

```ps1
$counter = 1;

function Next() {
    Write-Host $script:counter

    $script:counter++
}

Next # 1
Next # 2
Next # 3
```

With `$global` scope you will get `4 5 6` on the second script run, not `1 2 3`.

## Loops

### foreach

Example of a simple `foreach` loop:

```ps1
foreach ($item in $items) {
    if (Test-Path -Path "src/${item}") {
        continue
    }
    else {    
        DoWork()
    }
}
```

## Exceptions

```ps1
try {
    Invoke-Expression "git --version"
}
catch {
    Failure("Git command not found. Make sure that git is in your PATH variable")
}

```
