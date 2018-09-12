# Renaming files in a folder using bash in one line

    for file in *.json; do mv "$file" "${file/002_0003_/003_0002_}"; done

This example renames all `.json` files in the current directory by replacing
`002_0003_` with `003_0002_` . Test out the command by doing a dry run - replace
`mv` with `echo`.
