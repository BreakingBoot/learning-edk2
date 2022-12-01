# CodeQL with EDKII

There is already a database created for the EDKII code, so this document will describe how to get it locally and execute the default analysis scripts.

## Environment
This was tested on VisualStudio with the CodeQL extension downloaded. 

## Downloading Database
The database can be downloaded directly in VisualStudio by selecting the "Download Database from GitHub" and then enter the EDKII repository by entering either:
```
https://github.com/tianocore/edk2
```
or
```
tianocore/edk2
```

## Running the Default Scripts
To run the default scripts, you will need to clone the CodeQl repository:
```
git clone https://github.com/github/codeql
```
and then run the `.ql` scripts in the `cpp/ql/src` folder. There are multiple subfolders depending on what you want to look for specifcally. They can also be ran in VisualStudio if you click "Run Query" and then navigate to the script or scripts you want. You can select up to 20 scripts to run at a time.