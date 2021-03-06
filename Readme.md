Web Traffic Generator
=====================

## 1. Description of the software
This tool takes as input a list of web pages and visits them by means of Selenium Web Browser (relying on Firefox).
It produces as output the HARs generated by each page.
HAR is the [HTTP Archive](http://www.softwareishard.com/blog/har-12-spec/), a JSON data structure containing 
information and performance statistics about the webpage download.

For information about this Readme file and this tool please write to [martino.trevisan@polito.it](mailto:martino.trevisan@polito.it)

## 2. Dependencies
This software has been tested under Ubuntu Linux and Mac OSX. You need python3 and Firefox installed.
Please use Firefox <= 46.0. With >= 47.0 this software might not work.
To install Firefox 46 follow [this](http://www.askmetutorials.com/2016/04/install-firefox-46-on-ubuntu-1604-1404.html) link.

These packages are required:
```
python3 python3-pip
```


You must install Selenium python3 package as well as `numpy` and `scipy`
```
sudo pip3 install selenium numpy scipy
```

To run Firefox within a virtual display you need the package `xvfb` and its python package.
In this way you can run this tool in a machine without a X session active (e.g., from `ssh`).
Run these commands:
```
sudo apt-get install xvfb
sudo pip3 install pyvirtualdisplay
```

If you can't install pip3, you can do it following 
[this](http://stackoverflow.com/questions/6587507/how-to-install-pip-with-python-3) link.

Finally, you must download the Har Export Trigger extension for Firerfox, available [here](http://www.softwareishard.com/blog/har-export-trigger/).
You must only download the *.xpi file; you don't need to install the extension in your Firefox.
A fast way to retrieve it is to run the command:

```
wget https://github.com/firebug/har-export-trigger/releases/download/harexporttrigger-0.5.0-beta.10/harexporttrigger-0.5.0-beta.10.xpi
```

## 3. Usage
To run this tool, you must execute this command line:
```
usage: ./web_traffic_generator.py [-h] [-e har_export] [-r real_backoff]
                                [-b static_backoff] [-t timeout]
                                [-s start_page] [-v]
                                input_file out_dir
```

Please don't execute it as an argument of the python3 interpreter.
So `python3 web_traffic_generator.py` does NOT work.

positional arguments:
*  `input_file`            File where are stored the pages, one per row
*  `output_dir`            Output directory where HAR files are saved, one for each page.

optional arguments:
*  `-h, --help`            show this help message and exit
*  `-e har_export, --har_export har_export`
                        Path to Har Export extension xpi file. If not set,
                        searches it in the code path (the same path of the web_traffic_generator.py file).
*  `-r real_backoff, --real_backoff real_backoff`
                        Use real backoff distribution with maximum value
                        <real_backoff> seconds
*  `-b static_backoff, --static_backoff static_backoff`
                        Use a static (always the same) backoff with value <static_backoff>
                        seconds
*  `-t timeout, --timeout timeout`
                        Timeout in seconds after declaring failed a visit.
                        Default is 30.
*  `-v, --virtual_display`
                        Use a virtual display instead of the physical one. Requires `xvfb` package.

## 4. Output format
This tool creates an output file where it stores the output HAR of each requested URL.
For each input URL, the corresponding HAR file created in the provided directory.
The name of the HAR file reflects the time when the visit was performed and requested domain  (e.g., `visit_2016_06_15_13_41_22_www_google_it.har`).
Some HAR could be missing, due to failed downloads (very slow pages, crashed browser ecc...)

In the HAR file, at the position "log" -> "pages" -> "title", you can find a string representing the OnLoad time and
the original requested URL, separated by space, for example

`4.22002387046814 http://www.libero.it`

The OnLoad measure is important for performance, while knowing the actual requested URL can be useful for complicated web pages.


