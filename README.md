# fuff-tool
### Cybersecurity Cookbook

#### Recipe: Using ffuf for Web Fuzzing in Kali Linux

Ingredients:
- Kali Linux
- ffuf tool
- Target web application for fuzzing
- Basic knowledge of HTTP and web applications

Instructions:

1. Install ffuf:
    - Kali Linux often comes with many security tools pre-installed, but you may need to install ffuf manually:
      ```bash
      sudo apt update
      sudo apt install ffuf
      ```
    - If ffuf is not available in the package repository, you can install it using Go:
      ```bash
      sudo apt install golang
      go install github.com/ffuf/ffuf@latest
      ```
    - Make sure the Go bin directory is in your PATH:
      ```bash
      export PATH=$PATH:$(go env GOPATH)/bin
      ```

2. Verify Installation:
    - Check that ffuf is installed correctly by running:
      ```bash
      ffuf -h
      ```

3. Download Wordlists:
    - Kali Linux includes wordlists, but you can also download more from repositories like SecLists:
      ```bash
      git clone https://github.com/danielmiessler/SecLists.git
      ```

4. Basic Usage of ffuf:
    - Use ffuf to fuzz directories on a web server:
      ```bash
      ffuf -u http://example.com/FUZZ -w /usr/share/wordlists/dirb/common.txt
      ```
    - Alternatively, using SecLists:
      ```bash
      ffuf -u http://example.com/FUZZ -w ~/SecLists/Discovery/Web-Content/common.txt
      ```

5. Parameter Fuzzing:
    - Fuzz GET parameters:
      ```bash
      ffuf -u http://example.com/page.php?param=FUZZ -w ~/SecLists/Discovery/Web-Content/burp-parameter-names.txt
      ```

6. Fuzzing POST Data:
    - Fuzz POST data by specifying the method and data:
      ```bash
      ffuf -u http://example.com/login -w ~/SecLists/Discovery/Web-Content/burp-parameter-names.txt -X POST -d "username=FUZZ&password=knownpassword"
      ```

7. Advanced Options:
    - Use filters and matchers to refine your fuzzing results:
      - Match HTTP status codes:
        ```bash
        ffuf -u http://example.com/FUZZ -w ~/SecLists/Discovery/Web-Content/common.txt -mc 200
        ```
      - Filter responses by size:
        ```bash
        ffuf -u http://example.com/FUZZ -w ~/SecLists/Discovery/Web-Content/common.txt -fs 4242
        ```

8. Example: Fuzzing for Hidden Files:
    - Use ffuf to find hidden files and directories on a web server:
      ```bash
      ffuf -u http://example.com/FUZZ -w ~/SecLists/Discovery/Web-Content/common.txt -e .php,.html,.txt
      ```

Tips:
- Customize wordlists based on the target application's structure and technology.
- Combine ffuf with other tools like Burp Suite for a more comprehensive testing approach.
- Regularly update ffuf and your wordlists to keep up with the latest developments.


---

This recipe provides detailed steps for installing and using ffuf on Kali Linux, a popular platform for cybersecurity professionals.
# This is an example of a ffuf configuration file.

[http]
    cookies = [
        "cookiename=cookievalue"
    ]
    data = "post=data&key=value"
    followredirects = false
    headers = [
        "X-Header-Name: value",
        "X-Another-Header: value"
    ]
    ignorebody = false
    method = "GET"
    proxyurl = "http://127.0.0.1:8080"
    raw = false
    recursion = false
    recursion_depth = 0
    recursion_strategy = "default"
    replayproxyurl = "http://127.0.0.1:8080"
    timeout = 10
    url = "https://example.org/FUZZ"

[general]
    autocalibration = false
    autocalibrationstrings = [
        "randomtest",
        "admin"
    ]
    autocalibration_strategy = "basic"
    autocalibration_keyword = "FUZZ"
    autocalibration_perhost = false
    colors = false
    delay = ""
    maxtime = 0
    maxtimejob = 0
    noninteractive = false
    quiet = false
    rate = 0
    scrapers = "all"
    stopon403 = false
    stoponall = false
    stoponerrors = false
    threads = 40
    verbose = false
    json = false

[input]
    dirsearchcompat = false
    extensions = ""
    ignorewordlistcomments = false
    inputmode = "clusterbomb"
    inputnum = 100
    inputcommands = [
        "seq 1 100:CUSTOMKEYWORD"
    ]
    request = "requestfile.txt"
    requestproto = "https"
    wordlists = [
        "/path/to/wordlist:FUZZ",
        "/path/to/hostlist:HOST"
    ]

[output]
    debuglog = "debug.log"
    outputdirectory = "/tmp/rawoutputdir"
    outputfile = "output.json"
    outputformat = "json"
    outputcreateemptyfile = false

[filter]
    mode = "or"
    lines = ""
    regexp = ""
    size = ""
    status = ""
    time = ""
    words = ""

[matcher]
    mode = "or"
    lines = ""
    regexp = ""
    size = ""
    status = "200,204,301,302,307,401,403,405,500"
    time = ""
    words = ""
