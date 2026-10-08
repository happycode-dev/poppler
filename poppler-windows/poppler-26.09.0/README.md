# Poppler 26.09.0 for Windows

This folder is the Windows build of **Poppler**, the same PDF library that the Linux package `poppler-utils` comes from.

Poppler reads PDF files. It does not edit them like a word processor. The programs in this folder extract text, render pages to images, list fonts, split and merge PDFs, and pull out embedded files.

On Linux you install one package:

```bash
sudo apt install poppler-utils
```

On Windows there is no matching installer in this package. You copy this folder onto the PC and run the `.exe` files in `Library\bin`. Those `.exe` files are the Windows versions of `pdftotext`, `pdfinfo`, `pdftoppm`, and the other tools.

Linux binaries do not run on Windows. Copy this Windows folder. Do not copy `/usr/bin/pdftotext` from a Linux machine.

This build was checked on this PC: `pdfinfo.exe` and `pdftotext.exe` both print `version 26.09.0`.

| Item | Value |
| --- | --- |
| Poppler version | 26.09.0 |
| Package | Release-26.09.0-0 |
| Source | https://github.com/oschwartz10612/poppler-windows/releases/tag/v26.09.0-0 |
| Built from | conda-forge `poppler-feedstock` (the Windows binaries plus their DLLs) |
| Project | https://poppler.freedesktop.org/ |

No internet is required after this folder is on the PC. There is no installer and no registry entry.

## What to copy

Copy the **entire** `poppler-26.09.0` folder, or copy `Release-26.09.0-0.zip` and unzip it on the offline PC.

A typical location:

```text
C:\poppler\poppler-26.09.0\
```

Keep the layout below. The programs load DLLs from the same directory as the `.exe`. The encoding files live in `share\poppler`, beside the `Library` folder.

```text
poppler-26.09.0\
├── README.md                  this file
├── Library\
│   ├── bin\                   programs and DLLs (this is the folder you use)
│   │   ├── pdftotext.exe
│   │   ├── pdfinfo.exe
│   │   ├── pdftoppm.exe
│   │   ├── ...other tools...
│   │   ├── poppler.dll
│   │   └── ...other DLLs...
│   ├── include\               C/C++ headers, only needed to compile against Poppler
│   ├── lib\                   import libraries, only needed to compile against Poppler
│   └── share\man\man1\        manual pages
└── share\poppler\             encoding data (CJK, Cyrillic, Unicode maps)
    ├── cidToUnicode\
    ├── cMap\
    ├── nameToUnicode\
    └── unicodeMap\
```

For day-to-day use you only need `Library\bin` and `share\poppler`. Leave `include` and `lib` in place if you copy the whole folder. They do not hurt, and they are required only when you compile your own program against Poppler.

## Put the tools on PATH

The tools are here:

```text
C:\poppler\poppler-26.09.0\Library\bin
```

### Current Command Prompt only

```bat
set PATH=C:\poppler\poppler-26.09.0\Library\bin;%PATH%
pdftotext -v
```

### Every new Command Prompt for your user

1. Press Win+R, run `sysdm.cpl`, open **Advanced**, then **Environment Variables**.
2. Under **User variables**, select **Path**, then **Edit**, then **New**.
3. Add:

   ```text
   C:\poppler\poppler-26.09.0\Library\bin
   ```

4. Close every Command Prompt and open a new one.
5. Run:

   ```bat
   where pdftotext
   pdftotext -v
   ```

   `where` should print the path under `C:\poppler\...`. `pdftotext -v` should print `pdftotext version 26.09.0`.

You can skip PATH and call the program by its full path:

```bat
"C:\poppler\poppler-26.09.0\Library\bin\pdftotext.exe" -layout "C:\docs\file.pdf" "C:\docs\file.txt"
```

## The programs

These match the Linux `poppler-utils` commands. On Windows each name has `.exe` on the end. In Command Prompt you can type the name without `.exe`.

| Program | What it does |
| --- | --- |
| `pdftotext` | PDF to plain text, HTML metadata, or TSV |
| `pdfinfo` | Page count, page size, title, dates, metadata |
| `pdftoppm` | PDF pages to PNG, JPEG, TIFF, or PPM images |
| `pdftocairo` | PDF pages to PNG, JPEG, TIFF, SVG, EPS, or PDF (Cairo renderer; can also print) |
| `pdftohtml` | PDF to HTML or XML |
| `pdftops` | PDF to PostScript |
| `pdfimages` | List or extract images embedded in the PDF |
| `pdffonts` | List fonts used in the PDF |
| `pdfseparate` | Split one PDF into single-page PDFs |
| `pdfunite` | Merge several PDFs into one |
| `pdfdetach` | List or save files embedded in the PDF |
| `pdfattach` | Attach a file to a PDF |

Any tool prints its options with `-h`. Example: `pdftotext -h`.

### pdftotext — text

```bat
pdftotext "C:\docs\file.pdf" "C:\docs\file.txt"
pdftotext -layout "C:\docs\file.pdf" "C:\docs\file.txt"
pdftotext -f 2 -l 5 -layout "C:\docs\file.pdf" "C:\docs\pages.txt"
pdftotext -eol dos "C:\docs\file.pdf" "C:\docs\file.txt"
pdftotext -tsv "C:\docs\file.pdf" "C:\docs\boxes.tsv"
pdftotext -bbox-layout "C:\docs\file.pdf" "C:\docs\boxes.html"
```

If you omit the output file, text goes to the console:

```bat
pdftotext -layout "C:\docs\file.pdf" -
```

Useful options:

| Option | Meaning |
| --- | --- |
| `-f N` / `-l N` | First and last page |
| `-layout` | Keep columns and spacing close to the page |
| `-raw` | Keep the order stored in the PDF, ignore visual columns |
| `-nodiag` | Drop diagonal text |
| `-eol dos` | Windows line endings (CRLF). `unix` or `mac` are the other choices |
| `-nopgbrk` | Do not insert form-feed characters between pages |
| `-enc UTF-8` | Output encoding. `pdftotext -listenc` lists names |
| `-tsv` | One row per word, with page and bounding box |
| `-bbox` / `-bbox-layout` | Word boxes as HTML |
| `-upw PASS` / `-opw PASS` | User or owner password |
| `-q` | Quiet |

`-layout` is the right first try for invoices, forms, and multi-column pages. `-raw` is the right try when `-layout` merges two columns into one line.

Scanned pages have no text layer. `pdftotext` will write an empty or nearly empty file. Those pages need OCR (for example Tesseract). Poppler only reads text that is already in the PDF.

### pdfinfo — document info

```bat
pdfinfo "C:\docs\file.pdf"
pdfinfo -box "C:\docs\file.pdf"
pdfinfo -isodates "C:\docs\file.pdf"
pdfinfo -meta "C:\docs\file.pdf"
pdfinfo -custom "C:\docs\file.pdf"
pdfinfo -f 1 -l 3 "C:\docs\file.pdf"
```

A normal report includes title, author, page count, page size, PDF version, and whether the file is encrypted. `-box` adds MediaBox, CropBox, BleedBox, TrimBox, and ArtBox. `-f` and `-l` print per-page sizes for that range.

### pdftoppm — page images

```bat
pdftoppm -png "C:\docs\file.pdf" "C:\docs\page"
pdftoppm -png -r 200 -f 1 -l 3 "C:\docs\file.pdf" "C:\docs\page"
pdftoppm -jpeg -r 150 "C:\docs\file.pdf" "C:\docs\page"
pdftoppm -png -singlefile "C:\docs\file.pdf" "C:\docs\cover"
```

`-png -r 200` writes `page-1.png`, `page-2.png`, and so on, at 200 DPI. The default resolution is 150 DPI. `-singlefile` writes one image and does not add a page number.

Other output switches: `-jpeg`, `-tiff`, `-gray`, `-mono`. `-scale-to 2000` fits each page inside a 2000×2000 pixel box.

### pdftocairo — Cairo render

```bat
pdftocairo -png -r 200 "C:\docs\file.pdf" "C:\docs\page"
pdftocairo -svg "C:\docs\file.pdf" "C:\docs\page.svg"
pdftocairo -pdf -f 1 -l 1 "C:\docs\file.pdf" "C:\docs\first.pdf"
```

Use `pdftocairo` when you want SVG, a transparent PNG (`-transp`), or a Cairo rendering. Use `pdftoppm` for a straightforward PNG or JPEG of each page. Python `pdf2image` calls `pdftoppm`.

### pdfimages — embedded images

```bat
pdfimages -list "C:\docs\file.pdf"
pdfimages -png "C:\docs\file.pdf" "C:\docs\img"
pdfimages -all -p "C:\docs\file.pdf" "C:\docs\img"
```

`-list` prints a table and writes nothing. The extract commands write the images that are stored inside the PDF. They do not re-render the page. A photo on a page is extracted. Text drawn as vectors is not an image and will not appear.

### pdffonts — fonts

```bat
pdffonts "C:\docs\file.pdf"
pdffonts -subst "C:\docs\file.pdf"
```

The table shows each font name, type (TrueType, Type 1, CID, Type 3), encoding, and whether the font is embedded. If `emb` is `no`, the viewer substitutes another font and the page can look different on another PC.

### pdfseparate and pdfunite — split and merge

`pdfseparate` requires `%d` in the output name. `%d` becomes the page number.

```bat
pdfseparate "C:\docs\file.pdf" "C:\docs\page-%d.pdf"
pdfseparate -f 3 -l 5 "C:\docs\file.pdf" "C:\docs\part-%d.pdf"
pdfunite "C:\docs\a.pdf" "C:\docs\b.pdf" "C:\docs\c.pdf" "C:\docs\merged.pdf"
```

### pdfdetach and pdfattach — attachments

```bat
pdfdetach -list "C:\docs\file.pdf"
pdfdetach -saveall "C:\docs\file.pdf"
pdfdetach -savefile "notes.txt" -o "C:\docs\notes.txt" "C:\docs\file.pdf"
pdfattach "C:\docs\file.pdf" "C:\docs\extra.csv" "C:\docs\with-attachment.pdf"
```

### pdftohtml and pdftops

```bat
pdftohtml -s -noframes "C:\docs\file.pdf" "C:\docs\file.html"
pdftohtml -xml "C:\docs\file.pdf" "C:\docs\file.xml"
pdftops "C:\docs\file.pdf" "C:\docs\file.ps"
```

`-s` writes one HTML file for the whole document. `-xml` writes an XML file with positioned text, which some downstream tools parse.

## Passwords

For an encrypted PDF, pass the password to the tool that opens it:

```bat
pdftotext -upw "secret" "C:\docs\locked.pdf" "C:\docs\locked.txt"
pdfinfo -upw "secret" "C:\docs\locked.pdf"
```

`-upw` is the user password (open password). `-opw` is the owner password. If you see an error about a password, the file is encrypted and the command needs one of those flags.

## Python

Poppler is not a pip package. `pip install poppler` does not install these programs. Install the Python library you want, and point it at `Library\bin`.

`pdf2image` shells out to `pdftoppm`:

```python
from pdf2image import convert_from_path

pages = convert_from_path(
    r"C:\docs\file.pdf",
    dpi=200,
    poppler_path=r"C:\poppler\poppler-26.09.0\Library\bin",
)
pages[0].save(r"C:\docs\page1.png", "PNG")
```

If `Library\bin` is on PATH, you can omit `poppler_path`.

Calling `pdftotext` yourself:

```python
import subprocess

subprocess.run(
    [
        r"C:\poppler\poppler-26.09.0\Library\bin\pdftotext.exe",
        "-layout",
        r"C:\docs\file.pdf",
        r"C:\docs\file.txt",
    ],
    check=True,
)
```

## Encoding data (`share\poppler`)

`share\poppler` is **poppler-data**. It holds encoding tables so Poppler can map CJK (Chinese, Japanese, Korean) and Cyrillic character collections to Unicode. Poppler still opens ordinary Latin PDFs if this folder is missing. Text in those other scripts can come out as the wrong characters or as empty when the tables are missing.

Leave the folder here, next to `Library`:

```text
C:\poppler\poppler-26.09.0\share\poppler
```

The tools also look one level above `Library\bin`, which is `Library\share\poppler`. This zip ships the tables in the top `share\poppler` folder and ships only manual pages under `Library\share`. If CJK or Cyrillic text is wrong after a normal run, copy the tables into the second location and try again:

```bat
xcopy /E /I "C:\poppler\poppler-26.09.0\share\poppler" "C:\poppler\poppler-26.09.0\Library\share\poppler"
```

poppler-data uses its own license, separate from the GPL license of the Poppler programs. See `share\poppler\COPYING`, `COPYING.adobe`, and `COPYING.gpl2`. The short upstream note is `share\poppler\README`.

## If it fails on the offline PC

Open a **new** Command Prompt after any PATH change. An old window keeps the old PATH.

| What you see | What to do |
| --- | --- |
| `'pdftotext' is not recognized` | PATH does not contain `Library\bin`, or the Command Prompt was opened before the PATH change. Run `where pdftotext`. |
| `The code execution cannot proceed because XXX.dll was not found` | The `.exe` was copied without the rest of `Library\bin`. Copy the whole `Library\bin` folder. The DLL has to sit next to the `.exe`. |
| `VCRUNTIME140.dll` or `MSVCP140.dll` is missing | Those files are already in this `Library\bin`. The program is loading a different copy, or `Library\bin` was not copied in full. |
| The program starts, then says it cannot open the PDF | Check the path and quotes. `pdftotext "C:\My Documents\file.pdf" "C:\My Documents\out.txt"` |
| Password / encrypted error | Add `-upw` or `-opw`. |
| Text file is empty | The page is a scan (a picture), or the text is drawn as outlines. `pdfinfo` still works. `pdffonts` shows no useful text fonts. Use OCR for the text. |
| CJK or Cyrillic looks wrong | Keep `share\poppler`. If it is already there, copy it to `Library\share\poppler` as shown above. |
| `pdf2image` raises `PDFInfoNotInstalledError` or cannot find `pdftoppm` | Pass `poppler_path=r"C:\poppler\poppler-26.09.0\Library\bin"`, or add that folder to PATH and restart the Python process. |

Confirm the copy with:

```bat
"C:\poppler\poppler-26.09.0\Library\bin\pdfinfo.exe" -v
"C:\poppler\poppler-26.09.0\Library\bin\pdftotext.exe" -v
dir "C:\poppler\poppler-26.09.0\Library\bin\poppler.dll"
dir "C:\poppler\poppler-26.09.0\share\poppler\nameToUnicode"
```

All four should succeed. The two version commands should print `version 26.09.0`.

## Manual pages

`Library\share\man\man1\` contains a page per tool (`pdftotext.1`, `pdfinfo.1`, and the rest). They are the same text as the Linux man pages. On Windows, `-h` on the `.exe` is the practical reference.

## Compiling your own program

`Library\include\poppler` and `Library\lib` are for C or C++ projects that link to `poppler.dll` / `poppler-cpp.dll`. Command-line use and Python `pdf2image` do not need them.

## License and upstream

Poppler is free software from the Poppler developers (GPL). See the copyright line printed by `pdftotext -v`. This zip only repackages the conda-forge Windows build and poppler-data. Bugs in PDF parsing belong to Poppler (https://poppler.freedesktop.org/ or https://gitlab.freedesktop.org/poppler/poppler). Bugs in the conda package belong to https://github.com/conda-forge/poppler-feedstock. Bugs in this zip layout belong to https://github.com/oschwartz10612/poppler-windows.
