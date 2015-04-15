docs.gl
=======

[docs.gl](http://docs.gl) is a public domain web scaffolding for the OpenGL documentation.
The actual documentation website provided by Khronos is in frames and poorly formatted,
difficult to navigate and search. docs.gl aims to improve the form factor and quality of
the OpenGL documentation.

Contributions Welcome
---------------------

docs.gl should be thought of as a public wiki backed by GitHub. If you think you can improve
the documentation, please consider submitting a pull request. I've uploaded all of the GL
version specifications [here](https://dl.dropboxusercontent.com/u/4205810/all-opengl-docs.zip) for convenience.

How To Build It
---------------

docs.gl is a python script that reads each man page, processes it, and outputs static HTML.
There is no database or server side scripting. All of the templating and processing is done
at build time by the python script. It is executed like this:

	python compile.py

If you're building for a final release, then you can use the `--full` parameter which does
HTML minification and Unicode processing as well. It looks like this:

	python compile.py --full

If you are running Windows, there are a build.bat and a build_full.bat for convenience. When
the script is done building, the completed site will be in a folder named `htdocs`.

####GLSL

Building glsl requires additional commands.

GLSL docs have to be pulled out from the GL docs and into their own folder.
    
    python find_glsl.py
    
You can now run the equivalent of `compile.py` for GLSL:

    python compile_glsl.py

The `--full` parameter is still available as follow:

    python compile_glsl.py --full

If you are running Windows, there is `build_full_glsl.bat` for convenience. When the script is done building, the completed site will be in a folder named `glsl_htdocs`.

####GL and GLSL 

To have both API in a single website both glsl/gl folders and opengl_spec.py glsl_spec.py have to be available.(`glsl_spec.py` can be generated by `read_glsl_spec.py`)

If you haven't already build the glsl docs you'll have to run `find_glsl.py` before building the full docs with the following:

    python compile_all.py

The `--full` parameter is still available as follow:

    python compile_all.py --full    

If you are running Windows, there is `build_full_all.bat` for convenience. When the script is done building, the completed site will be in a folder named `merge_htdocs`.

File Structure
--------------

Six directories called `es1` `es2` `es3` `gl2` `gl3` and `gl4` contain the manual pages for
each OpenGL command. They are only the inner HTML with header/footer elements such as `head`
and `html` stripped out. These pages are read and processed by `compile.py` to produce the
final site.

The `html` directory contains further resources for compiling the site. Files sitting
directly in the `html` directory are template files which are used to further process the
manual pages. Inside the template files are tokens that `compile.py` searches for and
dynamically replaces during the compilation process. For example, the token `{$current_api}`
will be replaced with `3.3` or `4.2` depending on the latest version that the command being
compiled is available for. All content in the site that changes based on what manual page is
being compiled is handled in this way.

Files in the `copy` folder are used without changes.

Files in the `examples` folder contain source code examples used in the manual pages. In
these files a special token is used to provide a link to a command page. For example
`{%glBindTexture}` will provide a link to http://docs.gl/gl4/glBindTexture if the example
appears in a GL4 page. If the example appears in a gl3 page, the link will be updated
accordingly.

In the main directory there are some additional helper python scripts. `opengl.py` contains
helper tables, (eg which commands appear in which versions) tables specifying which examples
should appear on which pages, and tables specifying the categories that appear in the table
of contents of each page. `insert_additional.py`, `make_copyright.py`, and `strip.py` are
special one-time-use programs that were used to process the manual pages after they were
initially downloaded from Khronos. They could be reused for additional processing.
`read_spec.py` reads the XML OpenGL spec file found in `specs/gl.xml` and outputs
`opengl_spec.py`, which contains the raw data on which commands belong to which versions.
`opengl_spec.py` should only have to be re-generated when Khronos updates the spec.

To Do
-----

* GLSL functions
* Extensions
* Display a message if the user makes an invalid search
* Display on each page "Core in version/core since version" like the [OpenGL Wiki does](http://www.opengl.org/wiki/GlBindTexture)
* Integrate information from the [common mistakes](http://www.opengl.org/wiki/Common_Mistakes) file into the page for each command they pertain to
* Update each page with any information that may be in the spec but missing in the manual
* Download the entire site as a zip file
* Display on each page which commands are likely to cause a pipeline stall (eg glGet)

Thanks to the great people in Freenode ##opengl who provided me good feedback while I was
writing this.
