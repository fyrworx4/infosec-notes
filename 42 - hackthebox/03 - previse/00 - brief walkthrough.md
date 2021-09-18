# brief walkthrough of previse

* used burpsuite to get into web interface
* downloaded sitebackup.zip to view contents of website (.php files)
* found an exec() function, which was used to get the user shell
* performed path injection to obtain root