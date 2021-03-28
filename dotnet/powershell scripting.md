# PowerShell scripting

- [PowerShell scripting](#powershell-scripting)
  - [Executing script](#executing-script)
  - [macOS installation](#macos-installation)
  - [Comments](#comments)
  - [Variables](#variables)
  - [Run commands](#run-commands)
  - [Functions](#functions)
  - [Folders](#folders)
  - [Scopes](#scopes)
  - [Loops](#loops)
    - [foreach](#foreach)
  - [Exceptions](#exceptions)
  - [Arrays](#arrays)
  - [Parentheses](#parentheses)

## Executing script

To execute PowerShell scripts you may need to bypass execution polices set on your computer:

```ps1
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
.\script.ps1
Set-ExecutionPolicy Restricted -Scope CurrentUser
```

## macOS installation

```bash
brew install --cask powershell
pwsh --version

brew update
brew upgrade powershell --cask
```

## Comments

```ps1
# Single line comment

<#
  Multi line
  comment
#>
```

## Variables

Set variable:

```ps1
$var1 = Get-Process chrome
```

Get variable's type:

```ps1
$var1.GetType()
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

## Arrays

```ps1
$items = "one", "two", "three", "four"
$status.Length
$status[2] # Get element with index 2
```

## Parentheses

```ps1
Write-Host "pwd:" (Get-Location)
```
