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

You call it like `filtdir srcdir destdir your_program` (be sure to do
`./your_program` if appropriate). Personally, I use it for Jinja2 processing
in a simple static site generator and converting flac files to mp3s in my
music collection. Try running `filtdir any-directory some-other-directory ./dummycmd`
to get a feel for what the script does. The script will create
`some-other-directory` if it doesn't exist and fill it up with a shadow
of the source directory's structure, so you'll probably
want to use a non-existent name at first and delete it afterwards.

I took care to handle spaces in filenames, but colons or regex special
chars in directory names are probably bad news. If it gets on my nerves,
I'll probably rewrite the thing in python.

To finish off, here's my music-conversion script. The implementation of `flac2mp3`
is left as an exercise for the reader:
```bash
#!/usr/bin/env bash

if [[ "$2" =~ \.flac$ ]]; then
    mp3name=$(echo $2 | sed s/flac$/mp3/)
    # exit if we don't actually need to update the output
    if ! [[ "$1/$2" -nt "$3/$mp3name" ]]; then
        exit
    fi;
    echo "deflaccing $1/$2"
    flac2mp3 "$1/$2" "$3/$mp3name"
else
    # exit if we don't actually need to update the output
    if ! [[ "$1/$2" -nt "$3/$2" ]]; then
        exit
    fi;
    echo "copying $1/$2"
    cp "$1/$2" "$3/$2"
fi
```
