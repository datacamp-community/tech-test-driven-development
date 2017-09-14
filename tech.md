Nowadays Test Driven Development (TDD) is very opinionated (some might even think that [TDD is dead](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html)) but it is hard to argue that it has become a standard of how we write applications.

Unfortunately, a lot of developers miss the core idea of TDD, that the tests have to be written first and implementation is kind of a side effect which makes the tests pass, not the other way around.

It is very common to spot developers that implement first and add tests later on, to validate what they just had written. To be honest *just* writing specs is to TDD as Java is to JavaScript.

In order to do TDD efficiently, we need to setup a feedback loop from our tests so we can follow the proper workflow.

In Ruby world we can use [`guard`](https://github.com/guard/guard) along with specific extensions like [`guard-rails`](https://github.com/ranmocy/guard-rails), [`guard-rspec`](https://github.com/guard/guard-rspec) and [`guard-bundler`](https://github.com/guard/guard-bundler)

After installing gems and bootstrapping `guard` with:

```
    $ bundle exec guard init rspec
```

We can tweak our newly created `Guardfile` a bit.

1) Change `cmd` to run your tests to one using bin stubs to take advantage of using [`spring`](https://github.com/rails/spring) and execute your tests faster.

2) Use `keep` option of `failed_mode` to make `guard` remember failed specs which were executed. Guard will then add those to run list and keep executing until they pass.

3) Add option to clean a terminal screen after each tests execution to always see the output from the last run.

```
    guard :rspec, cmd: "./bin/rspec", failed_mode: :keep do
        ...
        clearing :on
        ...
    end
```

With such simple setup, we have created a constant feedback loop from our tests. From now, on any change in code will trigger related specs: for example, a change in `models/user.rb` will trigger `spec/models/user_spec.rb` automatically so we can see the output from tests execution without even switching terminals.

[![asciicast](https://asciinema.org/a/vfwXW5obAZJ20n8TilsHyBfyQ.png)](https://asciinema.org/a/vfwXW5obAZJ20n8TilsHyBfyQ)

