---
title:  "Using VS Code in Windows 10 for Remote Development in EC2"
image:  /assets/images/blog_posts/remote_ssh_open_config.png
author: Ram Balachandran
# comments: false
# date:   2020-08-23 08:30:00 +0530
categories:
    - DevOps 
layout: post
---

I have used many IDE environments over the past years ranging from emacs, pycharm, eclipse among others. Emacs used to be my favorite editor since I was able to customize it to my needs. For example, one of the things I loved about Emacs was its seamless ability to work on code in remote servers through the [Tramp](https://www.gnu.org/software/emacs/manual/html_node/tramp/index.html#Top) package. However, in a corporate career, you are mostly destined to work in local Windows environment and customizing Emacs to work with windows was becoming too much of a pain.

 Recently, I switched over to Visual Studio Code (VS Code) which is slowly becoming my favourite IDE. It somehow strikes the perfect balance between working out of the box and customizability. Further, VS Code has strong interoperability across multiple languages.  I recently had to start remote development in EC2 and I was worried I might not be able to customize VS code to perform remote development. However, it turned out that VS Code provided the best framework for such remote development through `ssh`. I'm documenting here the steps I look to ensure a seamless access to EC2 through VS Code in Windows 10. 

### Machine and Software Information
Local Machine: *Windows 10*

Remote Machine: *Ubuntu 18.04 in AWS EC2*

### Steps to be Followed
1. First, you need to install the [Remote-SSH](vscode:extension/ms-vscode-remote.remote-ssh) extension. 
2. For this extension, to work we will need an OpenSSH compatible SSH Client. Unfortunately, *PuTTY* does not satisfy this requirement and as a result we need to install *Windows OpenSSH Client*.
3. We can check if the client is installed in the local machine through the following windows power shell command.

    ```powershell
    Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'
    # which should return output that appears something like

    Name  : OpenSSH.Client~~~~0.0.1.0
    State : NotPresent
    Name  : OpenSSH.Server~~~~0.0.1.0
    State : NotPresent
    ```

4. If not present, we can install them using the following commands.

    ```powershell
    # Install the OpenSSH Client
    Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

    # Install the OpenSSH Server
    Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0

    # Both of these should return output like:

    Path          :
    Online        : True
    RestartNeeded : False
    ```

5. Now we need to configure the EC2 instance to be able to connect to it. From the command palette (which can be accessed through `F1` or `Ctrl+Shift+P`), select `Remote-SSH: Open Configuration File` as shown below.

    ![Remote Open Config](/assets/images/blog_posts/remote_ssh_open_config.png)

6. You need to access the ssh config file which is typically in `C:\Users\<UserName>\.ssh\config`. Add the following information to the config file.

    ```ssh
    Host ec2-aws
        HostName <hostname>
        User ubuntu
        IdentityFile `C:\Users\<UserName>\aws_keys\test-key-pair.pem
    ```
    The Host is a name you provide to identify the connection. The `<hostname>`is unique for an EC2 instance. This information can be obtained from browser based client or command line interface (CLI). Typically, I use an Ubuntu machine and connect to the machine with the username `Ubuntu`. An important thing to remember is that the `*.pem` file that is used to connect to the AWS needs to have restricted permissions. Without such restricted permission, VS Code will not be able to use it to connect to EC2. So, its best to place it within `C:\Users\<UserName>\` so it can derive restricted permissions.

7. Upon configuration, we can connect to the EC2 instance through the `Remote-SSH: Connect to Host` as shown below 

    ![Remote Host Connect](/assets/images/blog_posts/remote_ssh_connect_to_host.png)

### Conclusion
It is also a good practice to idenity the type of Remote machine (which in my case is Linux), since I think VS Code does customization based on this information. Now VS Code will provide you access to all the files/folders remote machine which you can use to do seamless remote development.


<!---
# Multi Line  Equation in MathJax: https://stackoverflow.com/a/21565829/1652217
# How to make Mathjax work with jekyll: 
Copy _layouts/post.html to the working directory (You can get the path from `bundle info <theme-name>` which in this case is `minima`)
Copy the following scriptline into `post.html` and it should work off the box.

```
script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
```


You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
--->