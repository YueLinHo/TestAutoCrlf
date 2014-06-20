## The different EOL behavior between ...

| git | Version | EOL behavior |
|-----|:-------:|:------------:|
| Official Git | 2.0.0 | Mixed EOL |
| Git for Windows | 1.9.4 | Mixed EOL|
| libgit2-based software | 0.21.0-rc1 | All CRLF |

## Testing Repsoitory

 1. Set core.autocrlf = false
 1. Add/Commit all files with different EOLs 
 1. Set core.autocrlf = true

## Tests and Results

### Official Git 2.0.0 (using Vagrant)

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
 14. **Run** ```git config core.autocrlf true```
 15. **Delete** all files in the working tree via Windows Explorer.
 16. **Run** ```git checkout HEAD -f -- "MIX-more_LF.txt"```
   * Got the file with **mixed EOLs**
 17. **Run** ```git checkout HEAD -f -- "MIX-more_CRLF.txt"```
   * Got the file with **mixed EOLs**

(Check the EOL via Notepad++ on Windows)

Double check : Run Step 3-5, 10-11, 13-17

### Git for Windows 1.9.4

  ```
$ git checkout HEAD -f -- "MIX-more_LF.txt"
$ git checkout HEAD -f -- "MIX-more_CRLF.txt"
  ```

   **Still mixed EOLs**

### TortiseGit 1.8.9.0 / Revert (Using Git for Windows)

 1. Delete them
 2. TortiseGit -> Revert

   **Still mixed EOLs**
   
### TortoiseGit 1.8.9.0 : Save revision to... (Using libgit2)

 1. Open TortoiseGit Log Message dialog
 2. Click the commit which add all file
 3. Right click on testing file
 4. Click "Save revision to..."

   **All CRLF EOLs**
   
#### Other information

I use VS2013 to trace [the source code](https://github.com/TortoiseGit/TortoiseGit).
Called function are:
 * git_blob_filtered_content() of blob.c
   * git_filter_list_apply_to_blob() of filter.c
     * git_filter_list_apply_to_data() of filter.c
       * fe->filter->apply() (crlf_apply() of crlf.c)
         * **crlf_apply_to_workdir()** of crlf.c
           * **line_ending()** of crlf.c
            * git_buf_text_lf_to_crlf() of buf_text.c
              * git_buf_put() of buffer.c

And...

in the line_ending(),

ca->crlf_action is -1 (GIT_CRLF_GUESS)

ca->eol is 0 (GIT_EOL_UNSET)

And **line_ending() return "\r\n"** finally. (GIT_EOL_NATIVE == GIT_EOL_CRLF is true)

**Maybe I will re-trace the source code to get where the var ca is set.**
