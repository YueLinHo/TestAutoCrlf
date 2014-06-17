# Test the different behavior on EOL between Git For Windows and libgit2

 1. Set core.autocrlf = false
 1. Add/Commit all files with different EOLs 
 1. Set core.autocrlf = true

# Test and Reslut

## Git for Windows 1.9.2

  ```
$ git checkout HEAD -f -- "MIX-more_LF.txt"
$ git checkout HEAD -f -- "MIX-more_CRLF.txt"
  ```

   **Still mixed EOLs**

## TortiseGit / Revert (Using Git for Windows)

 1. Delete them
 2. TortiseGit -> Revert

   **Still mixed EOLs**
   
## TortoiseGit : Save revision to... (Using libgit2)

 1. Open TortoiseGit Log Message dialog
 2. Click the commit which add all file
 3. Right click on testing file
 4. Click "Save revision to..."

   **All CRLF EOLs**
   
## Git 2.0.0 on Linux (using Vagrant)

Based on https://github.com/msysgit/msysgit/wiki/Vagrant

 1. Installe Virtual Box
 2. Installe Vagrant
 3. **Run** C:\msysGit\msys.bat
 4. **Run** ```vagrant up```
 5. **Run** ```vagrant ssh```
 6. Run ```sudo apt-get install python-software-properties```
 7. Run ```sudo add-apt-repository ppa:git-core/ppa```
 8. Run ```sudo apt-get update```
 9. Run ```sudo apt-get install git```
 10. **Run** ```git --version``` shows ```git version 2.0.0```
 11. **Run** ```cd /vagrant```
 12. Run ```git clone https://github.com/YueLinHo/TestAutoCrlf.git```
 13. **Run** ```cd TestAutoCrlf```
 14. **Delete** all files in the working tree via Windows Explorer.
 15. **Run** ```git checkout HEAD -f -- "MIX-more_LF.txt"``` -> Got the file with **mixed EOLs**
 16. **Run** ```git checkout HEAD -f -- "MIX-more_CRLF.txt"``` -> Got the file with **mixed EOLs**

(Check the EOL via Notepad++ on Windows)

Double Check it: Run Step 3-5, 10-11, 13-16
