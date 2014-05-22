Global Change Lab buildpack
===========================
This is a buildpack for the [Global Change Lab](https://github.com/bitblueprint/Global-Change-Lab) django app.

A buildpack is a _de facto_ standard for defining how apps should be built on cloud providers (PaaSs).
A buildpack consists of three scripts `detect`, `compile` and `release` which are all placed in the `bin`
folder of the buildpack.
All other files in the buildpack are auxillary files that get loaded, read or in other ways help the 
three main scripts.

When you upload your app to your cloud provider (for example with `git push`) the cloud provider
(heroku, cloudControl, Stackato or some PaaS) will execute the three scripts in sequence
in order to build your app:

1. `bin/detect` is a sort of "sanity-check" whether the app you uploaded
   matches the expectations of the build pack.
2. `bin/compile` runs any preparation work your app needs in order to run
   e.g. installs required packages, collects static files...
3. `bin/release` returns data to the cloud provider that can speficy
   what command to run in order to run your server, or other
   information that the cloud provider might need.

When the buildpack has been run, the cloud provider will execute the jobs specified in the file
`Procfile` in the app directory. (Save for some intermediate steps, that you can largely ignore)
If `Procfile` is not present the cloud provider will execute jobs from
the property `default_process_types` returned by `bin/release`.

_Read more:_

* [cloudControl buildpacks](https://www.cloudcontrol.com/dev-center/Platform%20Documentation#buildpacks-and-the-procfile)
* [heroku documentation on buildpacks](https://devcenter.heroku.com/articles/buildpack-api)

Usage
-----
On cloudControl you use the buildpack this way:

~~~bash
$ cctrlapp APP_NAME create custom --buildpack https://github.com/bitblueprint/gcl-buildpack-python.git
~~~

The buildpack will use Pip to install your dependencies, vendoring a copy of the Python runtime into your web container.

_Read more:_
[cloudControl: Custom buildpack feature](https://www.cloudcontrol.com/dev-center/Guides/Third-Party%20Buildpacks/Third-Party%20Buildpacks):

Choose your Python version
--------------------------

You can also specify arbitrary Python release with a `runtime.txt` file in the application repository root, otherwise `python-2.7.3` will be choosen as a default one:

~~~bash
$ cat runtime.txt
python-3.3.2
~~~
