# Unity

## Version Control

[ignore參考](https://godstamps.blogspot.com/2018/09/unity-git-2018.html)

```.gitignore
# ===========================
# Default Collab Ignore Rules
# ===========================

# OS Generated
# ============
.DS_Store
._*
.Spotlight-V100
.Trashes
Icon?
ehthumbs.db
[Tt]humbs.db
[Dd]esktop.ini

# Visual Studio / MonoDevelop generated
# =====================================
[Ee]xported[Oo]bj/
*.userprefs
*.csproj
*.pidb
*.suo
*.sln
*.user
*.unityproj
*.booproj

# Unity generated
# ===============
[Oo]bj/
[Bb]uild
sysinfo.txt
*.stackdump

# Others?
# =======
/Temp/
/Library/
```

## Test Runner

`Assembly Definition(asmdef)`:  
Unity專門的reference控管檔案，在任意地方create這個file看起來可以直接吃到所有main project的script?