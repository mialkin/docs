# PowerShell

Windows has two command shells: the **Command shell** and **PowerShell**. Each shell is a software program that provides direct communication between you and the operating system or application, providing an environment to automate IT operations.

**Command Prompt** is one of the command-line interface programs used to execute commands in Windows operating systems.

PowerShell was designed to extend the capabilities of the Command shell to run PowerShell commands called _cmdlets_. Cmdlets are similar to Windows Commands but provide a more extensible scripting language. You can run Windows Commands and PowerShell cmdlets in Powershell, but the Command shell can only run Windows Commands and not PowerShell cmdlets.

> For the most robust, up-to-date Windows automation, we recommend using PowerShell instead of Windows Commands.

## What is PowerShell?

PowerShell is a cross-platform task automation and configuration management framework, consisting of a command-line shell and scripting language. Unlike most shells, which accept and return text, PowerShell is built on top of the .NET Common Language Runtime (CLR), and accepts and returns .NET objects. This fundamental change brings entirely new tools and methods for automation.

## What is Windows Terminal?

**Windows Terminal** is a modern terminal application for users of command-line tools and shells like Command Prompt, PowerShell, and Windows Subsystem for Linux (WSL).

## Commands

| Command                                                | Alias    | Description                                                                                                                                                                   |
| ------------------------------------------------------ | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Clear-Host                                             | cls      | Clear display in the host program                                                                                                                                             |
| Copy-Item FILE_PATH -Destination FOLDER_PATH           | cp       |
| Get-Alias                                              | None     | Get aliases in the current session                                                                                                                                            |
| Get-Alias ALIAS_NAME                                   | None     | Get description of the alias                                                                                                                                                  |
| Get-Location                                           | pwd, gl  | Get an object that represents the current directory, much like the print working directory (pwd) command                                                                      |
| gci env:                                               |          | Display all environment variables                                                                                                                                             |
| Get-ChildItem                                          | ls       |
| Get-Command                                            | gcm      | Get all commands that are installed on the computer, including cmdlets, aliases, functions, filters, scripts, and applications                                                |
| Get-Command -noun SEARCH_TERM                          |          | Get commands that have SEARCH_TERM in their names                                                                                                                             |
| Get-Help COMMAND_NAME                                  | None     | Display information about PowerShell concepts and commands, including cmdlets, functions, Common Information Model (CIM) commands, workflows, providers, aliases, and scripts |
| Get-Process -Name chrome \| Get-Member                 |          | Get returned object properties                                                                                                                                                |
| Get-Service "s\*"                                      | gsv      | Get objects, whose name starts with "s", that represent the services on a computer                                                                                            |
| Get-Service \| Where-Object {$\_.Status -eq "Running"} | gsv      |                                                                                                                                                                               |
| Get-Help COMMAND_NAME -Online                          | None     | Open documentation page in a web browser                                                                                                                                      |
| Invoke-Expression "git --version"                      |          | Run commands or expressions on the local computer                                                                                                                             |
| New-Item -ItemType Directory -Force -Path src          | Out-Null | Create new directory supressing output                                                                                                                                        |
| Set-Location                                           | cd, sl   |
| Start-Transcript                                       | None     | Create a record of all or part of a PowerShell session to a text file                                                                                                         |
| Write-Host YOUR_TEXT                                   | None     | Writes customized output to a host                                                                                                                                            |

## Links

-   [↑ Learn PowerShell in Y minutes](https://learnxinyminutes.com/docs/powershell/)
