---
layout: post
title: C programming with Sublime Text
---

C is one of my favourite programming languages and Sublime Text one of my favourite text editors. However, I didn't used to code in C with Sublime, don't ask me why. So when I decided to combine them the first issue I faced was how to integrate makefiles to Sublime.

As I like to trust on my own building enviroment I will write a custom build system. If we go to _Tools->Build System->New Build System..._, we can create a new build configuration. Configuration is made by means of a JSON file. Following the one we need to use a makefile to compile our code.

{% highlight JSON linenos %}
{
	"cmd": ["make"],
	"selector": "source.makefile",
	"variants":
	[
		{
			"name": "Clean",
			"cmd": ["make", "clean"]
		},
		{
			"name": "All",
			"cmd": ["make", "all"]
		}
	]
}
{% endhighlight %}

Variants will be used to call _make clean_ and _make all_ using a Sublime custom shortcut. So... how do we create the shortcut? Easy. Another JSON file.

If we go to _Preferences->Key Bindings-User_ we can add the shortcuts we want to the existing JSON file.

{% highlight JSON linenos %}
[

    { "keys": ["ctrl+shift+a"], "command": "build", "args": {"variant": "All"} },

    { "keys": ["ctrl+shift+c"], "command": "build", "args": {"variant": "Clean"} },

]
{% endhighlight %}

And that's it. To sum up a simple example of how we can use this configuration. We create a simple C program.

{% highlight C linenos %}
/*File: hello.c*/
#include <stdio.h>
int main(void)
{
	printf("Hello Sublime!\n");
	return 0;	
}
{% endhighlight %}

And now, our makefile.

{% highlight makefile linenos %}

CFLAGS = -ansi -pedantic -Wall -O3
CC = gcc
all: hello.c
hello: hello.o
	$(CC) hello.o -o hello
hello.o: hello.c
	$(CC) $(CFLAGS) -c -o hello.o hello.c 

.PHONY: clean
clean:
	rm -f *.0 *~

{% endhighlight %}

Ready. If we put this files under the same folder, and we press _Ctrl+Shift+A_ with the scope in our makefile, sublime will call _make all_ and we will have our little program compiled. Sublime will show us the output of the compilation process and we will know if everything went well. 

We can also run the program with a shortcut from Sublime. However, I prefer to run the code directly from a terminal as we can take advantage of the full functionality of the terminal.

For a snippet of code like this, we can use a simpler compilation script, or maybe use Sublime default compilation system. Let's see how to do that.

## Compiling a single file

In this case we have to create a custom compiling script as follows.

{% highlight JSON linenos %}
{
	"cmd" : ["gcc $file_name -o $file_base_name -ansi -pedantic -Wall -lm"],
	"selector" : "source.c",
	"working_dir" : "$file_path",
	"shell" : "true"
}
{% endhighlight %}


_For the record, this article was written using Sublime Text 3_
