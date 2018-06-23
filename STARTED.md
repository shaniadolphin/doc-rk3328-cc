# Firefly Document Template

This is the Firefly's document template, 
which serves as a board document startup and 
also the sharing document pool by all the boards.

## How to Create a New Board Document

First clone this repository. Then replace the "Firefly-Board" with the actual board name to get started:
```
en/conf.py:21:project = 'Firefly-Board Manual'
en/conf.py:22:_project_filename = 'Firefly-Board_Manual'
en/conf.py:23:copyright = '2018, Firefly Team'
en/conf.py:24:author = 'Firefly Team'
en/index.rst:1:Welcome to Firefly-Board Manual

zh_CN/conf.py:21:project = 'Firefly-Board Manual'
zh_CN/conf.py:22:_project_filename = 'Firefly-Board_Manual'
zh_CN/conf.py:23:copyright = '2018, Firefly Team'
zh_CN/conf.py:24:author = 'Firefly Team'
zh_CN/index.rst:1:Welcome to Firefly-Board Manual
```

 - `conf.py` contains the configuration of the project. Only the project infos listed above needs to change. 
 - `index.rst` is the **master** file of the document project, which will be transformed to the default HTML file `index.html`. It organizes the document structure by using the `toctree` directive:
    ```
    .. toctree::
      :maxdepth: 2
      :caption: Getting Started

      intro
    ```
    There can be multiple `toctree` directives in `index.rst`. Each `toctree` directive can have its own optional `caption`, which serves as a distinct category separator making the table of contents clearer to read. The numeric `maxdepth` option indicates the depth of the tree; by default, all levels are included. To see more options and usage, please check the `toctree` [document](http://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#toctree-directive).


**Directory Layout**:
```text
en/
  |-  conf.py
  |- index.rst
  |- img/
  |- share/
  `- *.md
zh/
  |- conf.py
  |- index.rst
  |- img/
  |- share/
  `- *.md
 img/
 ```

 - The `en` directory contains the English documents, and the `zh_CN` directory contains the Chinese documents. They are independent document projects which contain its own configuration (`conf.py`) and Makefile.
 - The `img` directory contains images shared by all the projects. To use these images, you need to create a symlink to them in `en/img` or `zh_CN/img`.
 - The `en/share` and `zh_CN/share` directories contain documents shared by all the boards, which are excluded from building by default. You need to create a symlink in `en` or `zh_CN` directory and add the corresponding pages to `index.rst`.
 - Name all the markdown pages and images in English.
 - Usually, do not put the markdown pages in subdirectory. This keep linking simple and consistent.
 
Markdown Preprocessor is supported. See the [demo](en/share/mdpp-demo.mdpp) for the syntax and usage.

> **Note**: The `.mdpp` file will be transformed to `.md` file unconditionally, which means that they are generated everytime you build the document. If you do not like this behaviour, you can edit the Makefile by removing `FORCE_MAKE` in the `%.md` target. But then you have to add the dependency rules of `.md` files and all the included `.mdpp` files to the Makefile manually.

## How to Build the Document

First, you need to create the sphinx builder environment,
which resides at `~/sphinx-markdown` .

``` shell
sudo pip3 install virtualenv
virtualenv --python=python3 ~/sphinx-markdown
source ~/sphinx-markdown/bin/activate
pip install -r requirements.txt
```

This only needs to do once, then proceed next.

### Create the HTML output

```
# Make html
make

# If the HTML pages seems weird, you need to have a clean rebuild.
# Rebuild html with:
make clean html
```

The HTML output will be:
 - en/_build/html/index.html
 - zh_CN/_build/html/index.html

You can browse to the local index.html in the browser. But note that in local
browsing mode, the search result will not have context.

Or you can fire up a simple web server:

```
python -m SimpleHTTPServer 8000
```

Then go to the url `http://<ip>:8000/en/_build/html` for English or
`http://<ip>:8000/zh_CN/_build/html` for Chinese in the browser to view the
result. This is the recommended way to preview the HTML document.

### Create the PDF output

First you need to install the texlive packages:

```
sudo apt install texlive-xetex texlive-lang-cjk
```

Once latex is ready, produce the PDF with:
```
make pdf
```

The PDF files will be put in:
 - en/_build/latex/*.pdf
 - zh_CN/_build/latex/*.pdf

## How to Deploy the Document

### Deploy to Web Server

Assume that the web directory is `/var/www/html/FireflyBoard`, which you have write permission.

Then use rsync to copy all the static HTML files:

```
make clean html

# Deploy the contents:
rsync -av --delete en/_build/html/ /var/www/html/FireflyBoard/en/
rsync -av --delete zh_CN/_build/html/ /var/www/html/FireflyBoard/zh_CN/
```

### Deploy to ReadTheDocs

First, go to https://readthedocs.org, and login.

Then we import the Chinese and English document in sequency, and finally set Chinese document as a translation of the English document.

**Import the Chinese document**

1. Click the username at the top right.
2. Click `Import a Project` button.
3. Click `Import Manually` button.
4. Enter the project info.
    - Enter the board name plus `CHN` in the `Name` field, e.g. `Firefly-Board-CHN` .
    - Enter the `Repository URL`.
    - Click the `Next` button.
5. The project is now created. In the project control panel, click the `Admin` button.
    - In `Settings` page, chanage the `Language` to `Simplified Chinese`, then click the `Save` button at the bottom.
    - In `Advanced Settings` page, change `Python configuration file` to `zh_CN/conf.py`,  then click the `Save` button at the bottom.
6. In the project control panel, click the `Build` button to check the building status. The top one should be "Passed". If not, click the entry to see the detail of building.
7. Click the light green `View Docs` button at the top right to view the HTML document.

**Import the English document**

1. Click the username at the top right.
2. Click `Import a Project` button.
3. Click `Import Manually` button.
4. Enter the project info.
    - Enter the board name `Name` field, e.g. `Firefly-Board` .
    - Enter the `Repository URL`.
    - Click the `Next` button.
5. The project is now created. In the project control panel, click `Admin` button.
    - In `Settings` page, make sure the `Language` is `English`, then click the `Save` button at the bottom.
    - In `Advanced Settings` page, change `Python configuration file` to `en/conf.py`, then click the `Save` button at the bottom.
6. In the project control panel, click the `Build` button to check the building status. The top one should be "Passed". If not, click the entry to see the detail of building.
7. Click the light green `View Docs` button at the top right to view the HTML document.


**Set as Translation**

1. Click the username at the top right.
2. Select `Firefly-Board` project, which should be an English document.
3. In the project control panel, click `Admin` button.
    - In `Translations` page, select the `Firefly-Board-CHN` project, then click the `Add` button.
4. Click the light green `View Docs` button at the top right to view the HTML document.
5. Click the dropdown box in the bottom left of the page.
6. Switch the language to check the result.

Okay, you have done all the setup. The next time you `git push` the changes to
the github repository, `GitHub` will notify this change to `MakeTheDocs` by
webhook, and `MakeTheDocs` will rebuild the document automatically. You need to
check the building status from time to time to make sure everything goes
smoothly.

---
Tested in Ubuntu 16.04 .

Special thanks to [sphinx-markdown-test](https://github.com/ericholscher/sphinx-markdown-test)
to get the project started.

