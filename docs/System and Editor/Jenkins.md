# Jenkins

## .Net Build Example

Execute Windows batch command

```bat
%NUGET% restore SEDatafeedChecker.sln
```

Build a Visual Studio project or solution using MSBuild

MSBuild Build File `SEDatafeedChecker.sln`

Windows PowerShell(Nunit)

```bat
$workspace = "$env:workspace"
$targetDll = $workspace+ "\SEDatafeedTest\bin\Debug\SEDatafeedTest.dll"
$result = $workspace + '\TestResult_All.xml'
$resultp = "/result=$result"
C:\"Program Files (x86)"\NUnit.org\nunit-console\nunit3-console.exe $targetDll $resultp
```

## Archive the artifacts in Post-build Actions

Archive the artifacts

Files to archive `SEDatafeedChecker\bin\Debug\**\*`

使用`\**\*`可以包含子目錄

## Copy artifacts from another project in Build

Which build `Latest successful build`

Artifacts to copy `SEDatafeedChecker\bin\Debug\**\*`

`Flatten directories`打勾可以把建置的artifacts直接複製到專案底下，可以省去輸入路徑

## Jenkins Parameter

注意有時候是`%xxx%`有時候是`${}`，看你用的是windows batch還是powershell

## Good to read

[精通 Jenkins Pipeline](https://medium.com/getamis/精通-jenkins-pipeline-part1-e8ef48d3543e?fbclid=IwAR2TZZN7p8VNJIq1_y11GW9R2U3Spg2h4EAtQG1yNmtFL1GvIdK4NDpwLrs)