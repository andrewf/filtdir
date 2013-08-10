Filtdir filters directories. That is, you give it a source directory and an
output directory, and for each file in the source, it runs an external
program you specify with three arguments:

 - $1: The source directory
 - $2: The base file name of the source file
 - $3: The output directory

This program is expected to do your custom processing and write to the correct
file. The reason the filename resolution is not done automatically, for
example, by simply redirecting the output of the script into the dest file, is
in case you want to change the filename or not create a file at all. The
directory arguments don't have slashes, so the source file name is `"$1/$2"`,
and if you don't want to change the filename, the dest file name is `"$3/$2"`.

You call it like `filtdir srcdir destdir your_program` (be sure to do `./your_program` if appropriate). Personally, I use it for Jinja2 processing in
a simple static site generator and converting flac files to mp3s in my
music collection. 

I took care to handle spaces in filenames, but colons or regex special chars in directory names are probably bad news. If it gets on my nerves, I'll probably rewrite the thing in python.
