# Archiving & Compression in Linux

Archiving and compression are essential for managing files, reducing
storage space, and transferring data efficiently. Linux provides several
commands for archiving and compressing files.

------------------------------------------------------------------------

## 1. **tar (Tape Archive)**

`tar` is used to archive multiple files into a single file. By itself,
`tar` does not compress data, but it is often used with compression
tools like `gzip` or `bzip2`.

### Common Options:

-   `-c` → Create archive
-   `-x` → Extract archive
-   `-v` → Verbose (show progress)
-   `-f` → Specify filename
-   `-z` → Use gzip compression
-   `-j` → Use bzip2 compression

### Examples:

-   Create an archive:

``` bash
tar -cvf archive.tar file1 file2 dir/
```

-   Extract an archive:

``` bash
tar -xvf archive.tar
```

-   Create a compressed archive with gzip:

``` bash
tar -czvf archive.tar.gz file1 file2
```

-   Extract a gzip-compressed archive:

``` bash
tar -xzvf archive.tar.gz
```

-   Create a compressed archive with bzip2:

``` bash
tar -cjvf archive.tar.bz2 file1 file2
```

-   Extract a bzip2-compressed archive:

``` bash
tar -xjvf archive.tar.bz2
```

------------------------------------------------------------------------

## 2. **gzip**

`gzip` is a compression tool that compresses files using the GNU zip
algorithm.

### Features:

-   Replaces the original file with a compressed file ending in `.gz`.
-   Works best with single files (not multiple files at once).

### Examples:

-   Compress a file:

``` bash
gzip file.txt
```

(This creates `file.txt.gz`) - Decompress a file:

``` bash
gunzip file.txt.gz
```

-   View compressed file contents without extracting:

``` bash
zcat file.txt.gz
```

------------------------------------------------------------------------

## 3. **bzip2**

`bzip2` compresses files using the Burrows--Wheeler algorithm. It
usually offers better compression than `gzip` but is slower.

### Examples:

-   Compress a file:

``` bash
bzip2 file.txt
```

(This creates `file.txt.bz2`) - Decompress a file:

``` bash
bunzip2 file.txt.bz2
```

-   Test compressed file integrity:

``` bash
bzip2 -t file.txt.bz2
```

------------------------------------------------------------------------

## 4. **zip**

`zip` is used to compress and archive files into a `.zip` format, widely
used across different operating systems.

### Examples:

-   Create a zip archive:

``` bash
zip archive.zip file1 file2 dir/
```

-   Add a file to an existing archive:

``` bash
zip archive.zip newfile.txt
```

-   Create a password-protected zip archive:

``` bash
zip -e secure.zip file.txt
```

------------------------------------------------------------------------

## 5. **unzip**

`unzip` is used to extract files from a `.zip` archive.

### Examples:

-   Extract a zip archive:

``` bash
unzip archive.zip
```

-   Extract to a specific directory:

``` bash
unzip archive.zip -d /path/to/destination
```

-   List contents of a zip file without extracting:

``` bash
unzip -l archive.zip
```

------------------------------------------------------------------------

# Summary

-   **tar** → Archive multiple files (optionally with compression).
-   **gzip** → Compress single files (`.gz`).
-   **bzip2** → Better compression than gzip (`.bz2`).
-   **zip** → Create compressed `.zip` files (multi-platform support).
-   **unzip** → Extract `.zip` files.

These tools make it easy to archive and compress data for storage and
transfer in Linux systems.
