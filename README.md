Some alfred workflows for quick work in rails

# Format nested modules and classes to a single line
Generally used for calling in console.

This assumes the most recent thing in the clipboard history is the copied nested structure from the ruby file. 

It takes this:
```ruby
module Productivity
  module Writing
    class CopyPasta
```

To this:
```ruby
Productivity::Writing::CopyPasta
```

# Convert CircleCI underlying commands and seed into a bisect command (standard)
This quickly builds out an rspec command with the `--bisect` option to help get a reproducible failure (useful for transients). This can take a while to run (sometimes hours) and occupy your test database while it is going.

First, copy _just_ the seed number:

![image](https://user-images.githubusercontent.com/8879305/116628051-312d9900-a903-11eb-828b-9c5b29668de1.png)

Then copy the underlying commands:

![image](https://user-images.githubusercontent.com/8879305/116628126-53271b80-a903-11eb-9453-ec1ed043fca1.png)

In the above example, just double-click line 155 and copy to get it all. 

Running the workflow will output the command, all ready to go:
```sh
$ DISABLE_SPRING=1 bin/rspec --seed 47771 "spec/utilities/calculator_spec.rb[1:1]" "apps/api/graphql/spec/groups_spec.rb" --bisect
```

# Convert CircleCI underlying commands and seed into a bisect command (parallel) 
This does the same as above except outputs a command to run the bisect in a parallel database, allowing you to continue working freely while you have the bisect running.

Before running this one, you have to set up the [parallel_tests](https://github.com/grosser/parallel_tests) gem. 

Steps are the same as above: copy the seed and _then_ the underlying commands (yes, it IS order dependent).

Output looks a bit different:
```sh
$ /usr/bin/env TEST_ENV_NUMBER=2 DISABLE_SPRING=1 parallel_rspec -n 1 -e "bin/rspec "spec/utilities/calculator_spec.rb[1:1]" "apps/api/graphql/spec/groups_spec.rb" --bisect --seed 47771"
```

Convert CircleCI underlying commands and seed into a bisect command that runs in a parallel database (requires setting up parallel_tests gem)
