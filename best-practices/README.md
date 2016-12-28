最佳实践
========

编写优秀程序的指南。

一般准则
--------

* 不要盲目遵循这些指南，而应该尽量去理解它们，并且在当有怀疑就问出来。
* 不要开发一个内建库提供的功能。
* 不要吞掉异常或者『静默失败』（译者注：所谓静默失败，是指当程序运行出错过，
  既不返回正确的结果，也不抛出异常或者返回错误代码，而是静默的忽略这个错误）。
* 不要写『猜测未来功能』的代码（译者注：不懂所指）。
* 仅在真正异常时才抛出异常。
* [保持代码简洁]。

[保持代码简洁]: http://www.readability.com/~/ko2aqda2

面向对象的设计
--------------

* 避免全局变量。
* 避免过长的参数列表。
* 限制一个对象的 collaborators (这个对象依赖的实体)。
* 限制一个对象的依赖（依赖这个对象的实体）。
* 偏好组合而不是继承。
* 偏好小方法。最好在 1 至 5 行之间。
* 偏好小的类，它只实现维一的 responsibility。当一个类超过 100 行时，
  它做的事可能就太多了。
* [讲，不要问].

[讲，不要问]: http://robots.thoughtbot.com/post/27572137956/tell-dont-ask

(译者注：Ruby 开始的几个 section 没有翻译，请跳到 Shell section 阅读)

Ruby
----

* Avoid optional parameters. Does the method do too much?
* Avoid monkey-patching.
* Generate necessary [Bundler binstubs] for the project, such as `rake` and
  `rspec`, and add them to version control.
* Prefer classes to modules when designing functionality that is shared by
  multiple models.
* Prefer `private` when indicating scope. Use `protected` only with comparison
  methods like `def ==(other)`, `def <(other)`, and `def >(other)`.

[Bundler binstubs]: https://github.com/sstephenson/rbenv/wiki/Understanding-binstubs

Ruby Gems
---------

* Declare dependencies in the `<PROJECT_NAME>.gemspec` file.
* Reference the `gemspec` in the `Gemfile`.
* Use [Appraisal] to test the gem against multiple versions of gem dependencies
  (such as Rails in a Rails engine).
* Use [Bundler] to manage the gem's dependencies.
* Use [Travis CI] for Continuous Integration, indicators showing whether GitHub
  pull requests can be merged, and to test against multiple Ruby versions.

[Appraisal]: https://github.com/thoughtbot/appraisal
[Bundler]: http://bundler.io
[Travis CI]: http://travis-ci.org

Rails
-----

* [Add foreign key constraints][fkey] in migrations.
* Avoid bypassing validations with methods like `save(validate: false)`,
  `update_attribute`, and `toggle`.
* Avoid instantiating more than one object in controllers.
* Avoid naming methods after database columns in the same class.
* Don't change a migration after it has been merged into master if the desired
  change can be solved with another migration.
* Don't reference a model class directly from a view.
* Don't return false from `ActiveModel` callbacks, but instead raise an
  exception.
* Don't use instance variables in partials. Pass local variables to partials
  from view templates.
* Don't use SQL or SQL fragments (`where('inviter_id IS NOT NULL')`) outside of
  models.
* Generate necessary [Spring binstubs] for the project, such as `rake` and
  `rspec`, and add them to version control.
* If there are default values, set them in migrations.
* Keep `db/schema.rb` or `db/development_structure.sql` under version control.
* Use only one instance variable in each view.
* Use SQL, not `ActiveRecord` models, in migrations.
* Use the [`.ruby-version`] file convention to specify the Ruby version and
  patch level for a project.
* Use `_url` suffixes for named routes in mailer views and [redirects].  Use
  `_path` suffixes for named routes everywhere else.
* Use a [class constant rather than the stringified class name][class constant in association]
  for `class_name` options on ActiveRecord association macros.
* Validate the associated `belongs_to` object (`user`), not the database column
  (`user_id`).
* Use `db/seeds.rb` for data that is required in all environments.
* Use `dev:prime` rake task for development environment seed data.
* Prefer `cookies.signed` over `cookies` to [prevent tampering].
* Prefer `Time.current` over `Time.now`
* Prefer `Date.current` over `Date.today`
* Prefer `Time.zone.parse("2014-07-04 16:05:37")` over `Time.parse("2014-07-04 16:05:37")`
* Use `ENV.fetch` for environment variables instead of `ENV[]`so that unset
  environment variables are detected on deploy.
* [Use blocks][date-block] when declaring date and time attributes in FactoryGirl factories.
* Use `touch: true` when declaring `belongs_to` relationships.

[date-block]: /best-practices/samples/ruby.rb#L10
[fkey]: http://robots.thoughtbot.com/referential-integrity-with-foreign-keys
[`.ruby-version`]: https://gist.github.com/fnichol/1912050
[redirects]: http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.30
[Spring binstubs]: https://github.com/sstephenson/rbenv/wiki/Understanding-binstubs
[prevent tampering]: http://blog.bigbinary.com/2013/03/19/cookies-on-rails.html
[class constant in association]: https://github.com/thoughtbot/guides/blob/master/style/rails/sample.rb

Testing
-------

* Avoid `any_instance` in rspec-mocks and mocha. Prefer [dependency injection].
* Avoid `its`, `specify`, and `before` in RSpec.
* Avoid `let` (or `let!`) in RSpec. Prefer extracting helper methods,
  but do not re-implement the functionality of `let`. [Example][avoid-let].
* Avoid using `subject` explicitly *inside of an* RSpec `it` block.
  [Example][subject-example].
* Avoid using instance variables in tests.
* Disable real HTTP requests to external services with
  `WebMock.disable_net_connect!`.
* Don't test private methods.
* Test background jobs with a [`Delayed::Job` matcher].
* Use [stubs and spies] \(not mocks\) in isolated tests.
* Use a single level of abstraction within scenarios.
* Use an `it` example or test method for each execution path through the method.
* Use [assertions about state] for incoming messages.
* Use stubs and spies to assert you sent outgoing messages.
* Use a [Fake] to stub requests to external services.
* Use integration tests to execute the entire app.
* Use non-[SUT] methods in expectations when possible.

[dependency injection]: http://en.wikipedia.org/wiki/Dependency_injection
[subject-example]: ../style/testing/unit_test_spec.rb
[avoid-let]: ../style/testing/avoid_let_spec.rb
[`Delayed::Job` matcher]: https://gist.github.com/3186463
[stubs and spies]: http://robots.thoughtbot.com/post/159805295/spy-vs-spy
[assertions about state]: https://speakerdeck.com/skmetz/magic-tricks-of-testing-railsconf?slide=51
[Fake]: http://robots.thoughtbot.com/post/219216005/fake-it
[SUT]: http://xunitpatterns.com/SUT.html

Bundler
-------

* Specify the [Ruby version] to be used on the project in the `Gemfile`.
* Use a [pessimistic version] in the `Gemfile` for gems that follow semantic
  versioning, such as `rspec`, `factory_girl`, and `capybara`.
* Use a [versionless] `Gemfile` declarations for gems that are safe to update
  often, such as pg, thin, and debugger.
* Use an [exact version] in the `Gemfile` for fragile gems, such as Rails.

[Ruby version]: http://bundler.io/v1.3/gemfile_ruby.html
[exact version]: http://robots.thoughtbot.com/post/35717411108/a-healthy-bundle
[pessimistic version]: http://robots.thoughtbot.com/post/35717411108/a-healthy-bundle
[versionless]: http://robots.thoughtbot.com/post/35717411108/a-healthy-bundle

Postgres
--------

* Avoid multicolumn indexes in Postgres. It [combines multiple indexes]
  efficiently. Optimize later with a [compound index] if needed.
* Consider a [partial index] for queries on booleans.
* Constrain most columns as [`NOT NULL`].
* [Index foreign keys].
* Use an `ORDER BY` clause on queries where the results will be displayed to a
  user, as queries without one may return results in a changing, arbitrary
  order.

[`NOT NULL`]: http://www.postgresql.org/docs/9.1/static/ddl-constraints.html#AEN2444
[combines multiple indexes]: http://www.postgresql.org/docs/9.1/static/indexes-bitmap-scans.html
[compound index]: http://www.postgresql.org/docs/9.2/static/indexes-bitmap-scans.html
[partial index]: http://www.postgresql.org/docs/9.1/static/indexes-partial.html
[Index foreign keys]: https://tomafro.net/2009/08/using-indexes-in-rails-index-your-associations

Background Jobs
---------------

* Store IDs, not `ActiveRecord` objects for cleaner serialization, then re-find
  the `ActiveRecord` object in the `perform` method.

Email
-----

* Use [SendGrid] or [Amazon SES] to deliver email in staging and production
  environments.
* Use a tool like [ActionMailer Preview] to look at each created or updated mailer view
  before merging. Use [MailView] gem unless using Rails version 4.1.0 or later.

[Amazon SES]: http://robots.thoughtbot.com/post/3105121049/delivering-email-with-amazon-ses-in-a-rails-3-app
[SendGrid]: https://devcenter.heroku.com/articles/sendgrid
[MailView]: https://github.com/37signals/mail_view
[ActionMailer Preview]: http://api.rubyonrails.org/v4.1.0/classes/ActionMailer/Base.html#class-ActionMailer::Base-label-Previewing+emails

Web
---

* Avoid a Flash of Unstyled Text, even when no cache is available.
* Avoid rendering delays caused by synchronous loading.
* Use https instead of http when linking to assets.

JavaScript
----------
* Use the latest stable JavaScript syntax with a transpiler, such as [babel].
* Include a `to_param` or `href` attribute when serializing ActiveRecord models,
  and use that when constructing URLs client side, rather than the ID.

[babel]: http://babeljs.io/

HTML
----

* Don't use a reset button for forms.
* Prefer cancel links to cancel buttons.
* Use `<button>` tags over `<a>` tags for actions.

CSS
---

* Use Sass.
* Use [Autoprefixer][autoprefixer] to generate vendor prefixes based on the
  project-specific browser support that is needed.

[autoprefixer]: https://github.com/postcss/autoprefixer

Sass
----

* Prefer `overflow: auto` to `overflow: scroll`, because `scroll` will always
  display scrollbars outside of OS X, even when content fits in the container.
* Use `image-url` and `font-url`, not `url`, so the asset pipeline will re-write
  the correct paths to assets.
* Prefer mixins to `@extend`.

Browsers
--------

* Avoid supporting versions of Internet Explorer before IE10.

Objective-C
-----------

* Setup new projects using [Liftoff](https://github.com/thoughtbot/liftoff) and
  follow provided directory structure.
* Prefer categories on `Foundation` classes to helper methods.
* Prefer string constants to literals when providing keys or key paths to methods.

Shell
-----

* 不要解析 `ls` 的输出。[这里][parsingls]有细节说明以及替代方法。
* 有的进程接受文件作为参数，不要将文件内容 `cat` 到 `stdin` 作为这样程序的输入。
* 使用 `echo` 不要带选项、转义、变量（这些情况下要使用 `printf`）。
* 不要使用 `/bin/sh` [shebang][] 除非你的脚本将至少在如下环境测试和运行：
  纯 Sh，POSIX 兼容模式下的 Dash (因为它可能运行在 Debian)，
  以及 POSIX 兼容模式下的 Bash（因为它可能运行在 OSX）。
* 当使用 `/bin/sh` [shebang][] 时不要使用任何非 POSIX 的 [features][bashisms]。
* 调用 `cd` 时要有代码处理改变目录失败的情况。
* 调用 `rm` 时如果参数有变量，要确保变量非空。
* 偏好 "$@" 而不是 "$\*"，除非你明确知道你想干什么。
* 偏好 `awk '/re/ { ... }'` 而不是 `grep re | awk '{ ... }'`。
* 偏好 `find -exec {} +` 而不是 `find -print0 | xargs -0`。
* 偏好 `for` 循环而不是 `while read` 循环。
* 偏好 `grep -c` 而不是 `grep | wc -l`。
* 偏好 `mktemp` 而不是使用 `$$` 来给一个临时文件以『独特』命名。
* 偏好 `sed '/re/!d; s//.../'` 而不是 `grep re | sed 's/re/.../'`。
* 偏好 `sed 'cmd; cmd'` 而不是 `sed -e 'cmd' -e 'cmd'`。
* 偏好这样使用 `if` 语句来检查退出状态 (`if grep -q ...; `, 而不是
  `if [ -n "$(grep ...)" ];`)。
* 偏好读取环境变量而不是进程的输出 (用 `$TTY` 而不是 `$(tty)`，用 `$PWD` 而不是
  `$(pwd)`，等等)。
* 使用 `$( ... )`, 而不是反引号来获取命令的输出。
* 使用 `$(( ... ))`, 而不是 `expr` 来执行算法表达式
* 使用 `1` 和 `0`, 而不是 `true` 和 `false` 来表示布尔变量。
* 使用 `find -print0 | xargs -0`，而不是 `find | xargs`。
* 使用双引号包住所有 `"$variable"` 而不是使用 `"$( ... )"` 表达式，
  除非你想让它们按词分割并且/或者被解释为 globs。
* 使用 `local` 关键字修饰函数作用域的变量。
* 使用 [shellcheck][] 来发现一般问题。

[shebang]: http://en.wikipedia.org/wiki/Shebang_(Unix)
[parsingls]: http://mywiki.wooledge.org/ParsingLs
[bashisms]: http://mywiki.wooledge.org/Bashism
[shellcheck]: http://www.shellcheck.net/

Bash
----

在 Shell 最佳实践之外，

* 偏好 `${var,,}` 和 `${var^^}` 而不是 `tr` 来修改大小写。
* 偏好 `${var//from/to}` 而不是 `sed` 做简单的字符串替换。
* 偏好 `[[` 而不是 `test` 或者 `[`。
* 在 `while read` 循环中，偏好进程替换而不是管道。
* 当你不需要结果时，使用 `((` 或者 `let`，而不是 `$((`。

(译者注：下边几个 section 没有翻译)

Haskell
-------

* Avoid partial functions (`head`, `read`, etc).
* Compile code with `-Wall -Werror`.

Ember
-----

* Avoid using `$` without scoping to `this.$` in views and components.
* Prefer to make model lookup calls in routes instead of controllers (`find`,
  `findAll`, etc.).
* Prefer adding properties to controllers instead of models.
* Don't use jQuery outside of views and components.
* Prefer to use predefined `Ember.computed.*` functions when possible.
* Use `href="#"` for links that have an action.
* Prefer dependency injection through `Ember.inject` over initializers, globals
  on window, or namespaces. ([sample][inject])
* Prefer sub-routes over maintaining state.
* Prefer explicit setting of boolean properties over `toggleProperty`.
* Prefer testing your application with [QUnit][ember-test-guides].

[ember-test-guides]: https://guides.emberjs.com/v2.2.0/testing/

Testing

* Prefer `findWithAssert` over `find` when fetching an element you expect to
  exist

[inject]: samples/ember.js#L1-L11

Angular
-------

* [Avoid manual dependency annotations][annotations]. Disable mangling or use a
  [pre-processor][ngannotate] for annotations.
* Prefer `factory` to `service`. If you desire a singleton, wrap the singleton
  class in a factory function and return a new instance of that class from the
  factory.
* Prefer the `translate` directive to the `translate` filter for [performance
  reasons][angular-translate].
* Don't use the `jQuery` or `$` global. Access jQuery via `angular.element`.

[annotations]: http://robots.thoughtbot.com/avoid-angularjs-dependency-annotation-with-rails
[ngannotate]: https://github.com/kikonen/ngannotate-rails
[angular-translate]: https://github.com/angular-translate/angular-translate/wiki/Getting-Started#using-translate-directive

Ruby JSON APIs
--------------

* Review the recommended practices outlined in Heroku's [HTTP API Design Guide]
  before designing a new API.
* Use a fast JSON parser, e.g. [`oj`][oj]
* Write integration tests for your API endpoints. When the primary consumer of
  the API is a JavaScript client maintained within the same code base as the
  provider of the API, write [feature specs]. Otherwise write [request specs].

[HTTP API Design Guide]: https://github.com/interagent/http-api-design
[oj]: https://github.com/ohler55/oj
[feature specs]: https://www.relishapp.com/rspec/rspec-rails/docs/feature-specs/feature-spec
[request specs]: https://www.relishapp.com/rspec/rspec-rails/docs/request-specs/request-spec




*以下为本文档的英文原文*
======================


Best Practices
==============

A guide for programming well.

General
-------

* These are not to be blindly followed; strive to understand these and ask
  when in doubt.
* Don't duplicate the functionality of a built-in library.
* Don't swallow exceptions or "fail silently."
* Don't write code that guesses at future functionality.
* Exceptions should be exceptional.
* [Keep the code simple].

[Keep the code simple]: http://www.readability.com/~/ko2aqda2

Object-Oriented Design
----------------------

* Avoid global variables.
* Avoid long parameter lists.
* Limit collaborators of an object (entities an object depends on).
* Limit an object's dependencies (entities that depend on an object).
* Prefer composition over inheritance.
* Prefer small methods. Between one and five lines is best.
* Prefer small classes with a single, well-defined responsibility. When a
  class exceeds 100 lines, it may be doing too many things.
* [Tell, don't ask].

[Tell, don't ask]: http://robots.thoughtbot.com/post/27572137956/tell-dont-ask

Ruby
----

* Avoid optional parameters. Does the method do too much?
* Avoid monkey-patching.
* Generate necessary [Bundler binstubs] for the project, such as `rake` and
  `rspec`, and add them to version control.
* Prefer classes to modules when designing functionality that is shared by
  multiple models.
* Prefer `private` when indicating scope. Use `protected` only with comparison
  methods like `def ==(other)`, `def <(other)`, and `def >(other)`.

[Bundler binstubs]: https://github.com/sstephenson/rbenv/wiki/Understanding-binstubs

Ruby Gems
---------

* Declare dependencies in the `<PROJECT_NAME>.gemspec` file.
* Reference the `gemspec` in the `Gemfile`.
* Use [Appraisal] to test the gem against multiple versions of gem dependencies
  (such as Rails in a Rails engine).
* Use [Bundler] to manage the gem's dependencies.
* Use [Travis CI] for Continuous Integration, indicators showing whether GitHub
  pull requests can be merged, and to test against multiple Ruby versions.

[Appraisal]: https://github.com/thoughtbot/appraisal
[Bundler]: http://bundler.io
[Travis CI]: http://travis-ci.org

Rails
-----

* [Add foreign key constraints][fkey] in migrations.
* Avoid bypassing validations with methods like `save(validate: false)`,
  `update_attribute`, and `toggle`.
* Avoid instantiating more than one object in controllers.
* Avoid naming methods after database columns in the same class.
* Don't change a migration after it has been merged into master if the desired
  change can be solved with another migration.
* Don't reference a model class directly from a view.
* Don't return false from `ActiveModel` callbacks, but instead raise an
  exception.
* Don't use instance variables in partials. Pass local variables to partials
  from view templates.
* Don't use SQL or SQL fragments (`where('inviter_id IS NOT NULL')`) outside of
  models.
* Generate necessary [Spring binstubs] for the project, such as `rake` and
  `rspec`, and add them to version control.
* If there are default values, set them in migrations.
* Keep `db/schema.rb` or `db/development_structure.sql` under version control.
* Use only one instance variable in each view.
* Use SQL, not `ActiveRecord` models, in migrations.
* Use the [`.ruby-version`] file convention to specify the Ruby version and
  patch level for a project.
* Use `_url` suffixes for named routes in mailer views and [redirects].  Use
  `_path` suffixes for named routes everywhere else.
* Use a [class constant rather than the stringified class name][class constant in association]
  for `class_name` options on ActiveRecord association macros.
* Validate the associated `belongs_to` object (`user`), not the database column
  (`user_id`).
* Use `db/seeds.rb` for data that is required in all environments.
* Use `dev:prime` rake task for development environment seed data.
* Prefer `cookies.signed` over `cookies` to [prevent tampering].
* Prefer `Time.current` over `Time.now`
* Prefer `Date.current` over `Date.today`
* Prefer `Time.zone.parse("2014-07-04 16:05:37")` over `Time.parse("2014-07-04 16:05:37")`
* Use `ENV.fetch` for environment variables instead of `ENV[]`so that unset
  environment variables are detected on deploy.
* [Use blocks][date-block] when declaring date and time attributes in FactoryGirl factories.
* Use `touch: true` when declaring `belongs_to` relationships.

[date-block]: /best-practices/samples/ruby.rb#L10
[fkey]: http://robots.thoughtbot.com/referential-integrity-with-foreign-keys
[`.ruby-version`]: https://gist.github.com/fnichol/1912050
[redirects]: http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.30
[Spring binstubs]: https://github.com/sstephenson/rbenv/wiki/Understanding-binstubs
[prevent tampering]: http://blog.bigbinary.com/2013/03/19/cookies-on-rails.html
[class constant in association]: https://github.com/thoughtbot/guides/blob/master/style/rails/sample.rb

Testing
-------

* Avoid `any_instance` in rspec-mocks and mocha. Prefer [dependency injection].
* Avoid `its`, `specify`, and `before` in RSpec.
* Avoid `let` (or `let!`) in RSpec. Prefer extracting helper methods,
  but do not re-implement the functionality of `let`. [Example][avoid-let].
* Avoid using `subject` explicitly *inside of an* RSpec `it` block.
  [Example][subject-example].
* Avoid using instance variables in tests.
* Disable real HTTP requests to external services with
  `WebMock.disable_net_connect!`.
* Don't test private methods.
* Test background jobs with a [`Delayed::Job` matcher].
* Use [stubs and spies] \(not mocks\) in isolated tests.
* Use a single level of abstraction within scenarios.
* Use an `it` example or test method for each execution path through the method.
* Use [assertions about state] for incoming messages.
* Use stubs and spies to assert you sent outgoing messages.
* Use a [Fake] to stub requests to external services.
* Use integration tests to execute the entire app.
* Use non-[SUT] methods in expectations when possible.

[dependency injection]: http://en.wikipedia.org/wiki/Dependency_injection
[subject-example]: ../style/testing/unit_test_spec.rb
[avoid-let]: ../style/testing/avoid_let_spec.rb
[`Delayed::Job` matcher]: https://gist.github.com/3186463
[stubs and spies]: http://robots.thoughtbot.com/post/159805295/spy-vs-spy
[assertions about state]: https://speakerdeck.com/skmetz/magic-tricks-of-testing-railsconf?slide=51
[Fake]: http://robots.thoughtbot.com/post/219216005/fake-it
[SUT]: http://xunitpatterns.com/SUT.html

Bundler
-------

* Specify the [Ruby version] to be used on the project in the `Gemfile`.
* Use a [pessimistic version] in the `Gemfile` for gems that follow semantic
  versioning, such as `rspec`, `factory_girl`, and `capybara`.
* Use a [versionless] `Gemfile` declarations for gems that are safe to update
  often, such as pg, thin, and debugger.
* Use an [exact version] in the `Gemfile` for fragile gems, such as Rails.

[Ruby version]: http://bundler.io/v1.3/gemfile_ruby.html
[exact version]: http://robots.thoughtbot.com/post/35717411108/a-healthy-bundle
[pessimistic version]: http://robots.thoughtbot.com/post/35717411108/a-healthy-bundle
[versionless]: http://robots.thoughtbot.com/post/35717411108/a-healthy-bundle

Postgres
--------

* Avoid multicolumn indexes in Postgres. It [combines multiple indexes]
  efficiently. Optimize later with a [compound index] if needed.
* Consider a [partial index] for queries on booleans.
* Constrain most columns as [`NOT NULL`].
* [Index foreign keys].
* Use an `ORDER BY` clause on queries where the results will be displayed to a
  user, as queries without one may return results in a changing, arbitrary
  order.

[`NOT NULL`]: http://www.postgresql.org/docs/9.1/static/ddl-constraints.html#AEN2444
[combines multiple indexes]: http://www.postgresql.org/docs/9.1/static/indexes-bitmap-scans.html
[compound index]: http://www.postgresql.org/docs/9.2/static/indexes-bitmap-scans.html
[partial index]: http://www.postgresql.org/docs/9.1/static/indexes-partial.html
[Index foreign keys]: https://tomafro.net/2009/08/using-indexes-in-rails-index-your-associations

Background Jobs
---------------

* Store IDs, not `ActiveRecord` objects for cleaner serialization, then re-find
  the `ActiveRecord` object in the `perform` method.

Email
-----

* Use [SendGrid] or [Amazon SES] to deliver email in staging and production
  environments.
* Use a tool like [ActionMailer Preview] to look at each created or updated mailer view
  before merging. Use [MailView] gem unless using Rails version 4.1.0 or later.

[Amazon SES]: http://robots.thoughtbot.com/post/3105121049/delivering-email-with-amazon-ses-in-a-rails-3-app
[SendGrid]: https://devcenter.heroku.com/articles/sendgrid
[MailView]: https://github.com/37signals/mail_view
[ActionMailer Preview]: http://api.rubyonrails.org/v4.1.0/classes/ActionMailer/Base.html#class-ActionMailer::Base-label-Previewing+emails

Web
---

* Avoid a Flash of Unstyled Text, even when no cache is available.
* Avoid rendering delays caused by synchronous loading.
* Use https instead of http when linking to assets.

JavaScript
----------
* Use the latest stable JavaScript syntax with a transpiler, such as [babel].
* Include a `to_param` or `href` attribute when serializing ActiveRecord models,
  and use that when constructing URLs client side, rather than the ID.

[babel]: http://babeljs.io/

HTML
----

* Don't use a reset button for forms.
* Prefer cancel links to cancel buttons.
* Use `<button>` tags over `<a>` tags for actions.

CSS
---

* Use Sass.
* Use [Autoprefixer][autoprefixer] to generate vendor prefixes based on the
  project-specific browser support that is needed.

[autoprefixer]: https://github.com/postcss/autoprefixer

Sass
----

* Prefer `overflow: auto` to `overflow: scroll`, because `scroll` will always
  display scrollbars outside of OS X, even when content fits in the container.
* Use `image-url` and `font-url`, not `url`, so the asset pipeline will re-write
  the correct paths to assets.
* Prefer mixins to `@extend`.

Browsers
--------

* Avoid supporting versions of Internet Explorer before IE10.

Objective-C
-----------

* Setup new projects using [Liftoff](https://github.com/thoughtbot/liftoff) and
  follow provided directory structure.
* Prefer categories on `Foundation` classes to helper methods.
* Prefer string constants to literals when providing keys or key paths to methods.

Shell
-----

* Don't parse the output of `ls`. See [here][parsingls] for details and
  alternatives.
* Don't use `cat` to provide a file on `stdin` to a process that accepts
  file arguments itself.
* Don't use `echo` with options, escapes, or variables (use `printf` for those
  cases).
* Don't use a `/bin/sh` [shebang][] unless you plan to test and run your
  script on at least: Actual Sh, Dash in POSIX-compatible mode (as it
  will be run on Debian), and Bash in POSIX-compatible mode (as it will
  be run on OSX).
* Don't use any non-POSIX [features][bashisms] when using a `/bin/sh`
  [shebang][].
* If calling `cd`, have code to handle a failure to change directories.
* If calling `rm` with a variable, ensure the variable is not empty.
* Prefer "$@" over "$\*" unless you know exactly what you're doing.
* Prefer `awk '/re/ { ... }'` to `grep re | awk '{ ... }'`.
* Prefer `find -exec {} +` to `find -print0 | xargs -0`.
* Prefer `for` loops over `while read` loops.
* Prefer `grep -c` to `grep | wc -l`.
* Prefer `mktemp` over using `$$` to "uniquely" name a temporary file.
* Prefer `sed '/re/!d; s//.../'` to `grep re | sed 's/re/.../'`.
* Prefer `sed 'cmd; cmd'` to `sed -e 'cmd' -e 'cmd'`.
* Prefer checking exit statuses over output in `if` statements (`if grep
  -q ...; `, not `if [ -n "$(grep ...)" ];`).
* Prefer reading environment variables over process output (`$TTY` not
  `$(tty)`, `$PWD` not `$(pwd)`, etc).
* Use `$( ... )`, not backticks for capturing command output.
* Use `$(( ... ))`, not `expr` for executing arithmetic expressions.
* Use `1` and `0`, not `true` and `false` to represent boolean
  variables.
* Use `find -print0 | xargs -0`, not `find | xargs`.
* Use quotes around every `"$variable"` and `"$( ... )"` expression
  unless you want them to be word-split and/or interpreted as globs.
* Use the `local` keyword with function-scoped variables.
* Identify common problems with [shellcheck][].

[shebang]: http://en.wikipedia.org/wiki/Shebang_(Unix)
[parsingls]: http://mywiki.wooledge.org/ParsingLs
[bashisms]: http://mywiki.wooledge.org/Bashism
[shellcheck]: http://www.shellcheck.net/

Bash
----

In addition to Shell best practices,

* Prefer `${var,,}` and `${var^^}` over `tr` for changing case.
* Prefer `${var//from/to}` over `sed` for simple string replacements.
* Prefer `[[` over `test` or `[`.
* Prefer process substitution over a pipe in `while read` loops.
* Use `((` or `let`, not `$((` when you don't need the result

Haskell
-------

* Avoid partial functions (`head`, `read`, etc).
* Compile code with `-Wall -Werror`.

Ember
-----

* Avoid using `$` without scoping to `this.$` in views and components.
* Prefer to make model lookup calls in routes instead of controllers (`find`,
  `findAll`, etc.).
* Prefer adding properties to controllers instead of models.
* Don't use jQuery outside of views and components.
* Prefer to use predefined `Ember.computed.*` functions when possible.
* Use `href="#"` for links that have an action.
* Prefer dependency injection through `Ember.inject` over initializers, globals
  on window, or namespaces. ([sample][inject])
* Prefer sub-routes over maintaining state.
* Prefer explicit setting of boolean properties over `toggleProperty`.
* Prefer testing your application with [QUnit][ember-test-guides].

[ember-test-guides]: https://guides.emberjs.com/v2.2.0/testing/

Testing

* Prefer `findWithAssert` over `find` when fetching an element you expect to
  exist

[inject]: samples/ember.js#L1-L11

Angular
-------

* [Avoid manual dependency annotations][annotations]. Disable mangling or use a
  [pre-processor][ngannotate] for annotations.
* Prefer `factory` to `service`. If you desire a singleton, wrap the singleton
  class in a factory function and return a new instance of that class from the
  factory.
* Prefer the `translate` directive to the `translate` filter for [performance
  reasons][angular-translate].
* Don't use the `jQuery` or `$` global. Access jQuery via `angular.element`.

[annotations]: http://robots.thoughtbot.com/avoid-angularjs-dependency-annotation-with-rails
[ngannotate]: https://github.com/kikonen/ngannotate-rails
[angular-translate]: https://github.com/angular-translate/angular-translate/wiki/Getting-Started#using-translate-directive

Ruby JSON APIs
--------------

* Review the recommended practices outlined in Heroku's [HTTP API Design Guide]
  before designing a new API.
* Use a fast JSON parser, e.g. [`oj`][oj]
* Write integration tests for your API endpoints. When the primary consumer of
  the API is a JavaScript client maintained within the same code base as the
  provider of the API, write [feature specs]. Otherwise write [request specs].

[HTTP API Design Guide]: https://github.com/interagent/http-api-design
[oj]: https://github.com/ohler55/oj
[feature specs]: https://www.relishapp.com/rspec/rspec-rails/docs/feature-specs/feature-spec
[request specs]: https://www.relishapp.com/rspec/rspec-rails/docs/request-specs/request-spec
