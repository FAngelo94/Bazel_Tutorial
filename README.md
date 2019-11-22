# Bazel Example

In this example we step it up and showcase how to integrate multiple ```cc_library``` targets from different packages.

 In Bazel, subdirectories containing BUILD files are known as packages. The new property ```visibility``` will tell Bazel which package(s) can reference this target, in this case the ```//main``` package can use ```hello-time``` library. 

```
cc_library(
    name = "hello-time",
    srcs = ["hello-time.cc"],
    hdrs = ["hello-time.h"],
    visibility = ["//main:__pkg__"],
)
```

To use our ```hello-time``` libary, an extra dependency is added in the form of //path/to/package:target_name, in this case, it's ```//lib:hello-time```

```
cc_binary(
    name = "hello-world",
    srcs = ["hello-world.cc"],
    deps = [
        ":hello-greet",
        "//lib:hello-time",
    ],
)
```

To build this example you use (notice that 3 slashes are required in windows)
```
bazel build //main:hello-world

# In Windows, note the three slashes

bazel build ///main:hello-world
```

## How to see the dependencies
Print the text the file dependencies during the build:
```
bazel query --notool_deps --noimplicit_deps 'deps(//main:hello-world)' \  --output graph
```

It is possible to build a graphics of dependencies with xdot using this command:
```
xdot < (bazel query --notool_deps --noimplicit_deps 'deps(//main:hello-world)' \  --output graph)
```

It is also possible put the output of bazel query in a file and then run this command to show the graphics of dependences:
```
xdot < (cat <name_file>)
```
