# General
![github_actions_gtest_example-Release](https://github.com/bastianhjaeger/github_actions_gtest_example/workflows/github_actions_gtest_example-Release/badge.svg?branch=master)
![github_actions_gtest_example-Debug](https://github.com/bastianhjaeger/github_actions_gtest_example/workflows/github_actions_gtest_example-Debug/badge.svg?branch=master)

Example on how to integrate gtest test into [github actions](https://github.com/features/actions). 



While there exist other links in the www on how to use it (which did not work for my setup), this repo quickly shows how to add a workflow / action to github that executes your gtest tests and responses with a simple ok / not-ok to highlight on github.

It is plain and simple, but maybe it helps you. 

# Setup Actions

the workflow file you need (e.g. in you repo under `.github/workflows/release.yml`) could look like this

```yaml
name: my-workflow-name

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Install gtest manually
      run: sudo apt-get install libgtest-dev && cd /usr/src/gtest && sudo cmake CMakeLists.txt && sudo make && sudo cp *.a /usr/lib && sudo ln -s /usr/lib/libgtest.a /usr/local/lib/libgtest.a && sudo ln -s /usr/lib/libgtest_main.a /usr/local/lib/libgtest_main.a
    - uses: actions/checkout@v1
    - name: configure
      run: mkdir build && cd build && cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_FLAGS="-Werror" ..
    - name: make
      run: cd build && make
    - name: Run Test
      run: /home/runner/work/github_actions_gtest_example/github_actions_gtest_example/build/test/github_actions_gtest_example.test
```

The important part is the *"Install gtest manually"* section. This is the plain and simple way to add it.

# Result

After some time (depending on you project and test complexity) you see the results like this:

<img src="doc/actions_build_example_01.png" alt="actions_build_example_01" style="zoom:50%;" />

<img src="doc/actions_build_example_02_detail.png" alt="actions_build_example_02_detail" style="zoom:50%;" />

<img src="doc/actions_build_example_03_overview.png" alt="actions_build_example_03_overview" style="zoom:50%;" />


# Build local
```bash
git clone git@github.com:bastianhjaeger/github_actions_gtest_example.git
cd github_actions_gtest_example
mkdir build && cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
make -j8
```
