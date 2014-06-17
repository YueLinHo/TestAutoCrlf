# Test the different behavior on EOL between Git For Windows and libgit2

 1. Set core.autocrlf = false
 1. Add/Commit all files with different EOLs 
 1. Set core.autocrlf = true
 1. Checkout these files

# Result - file with mixed EOL

 * CGit For Windows : still mixed EOL
 * libgit2 : all CRLF
   