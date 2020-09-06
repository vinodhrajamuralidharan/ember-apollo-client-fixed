# Changelog

All notable changes to this project will be documented in this file.

This project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## v2.1.0 - 2020-04-08

### :boom: Breaking Change

- Upgrade to Ember v3.17 & Drop node v8 (#342) @josemarluedke

### :bug: Bug Fix

- Use WeakMap for storing observables and unsubscribeFn (#343) @josemarluedke

### :memo: Documentation

- Update README to show new fragment import syntax (#344) @josemarluedke

### :house: Internal

- Upgrade to Prettier v2.0.0 (#345) @josemarluedke
- Add tests to make sure refresh a route renders new data (#337) @josemarluedke

### :package: Dependencies

- Bump acorn from 5.7.3 to 5.7.4 (#339) @dependabot

***

#### Contributors

@dependabot, @dependabot[bot] and @josemarluedke

***

For full changes, see the [comparison between v2.0.0 and v2.1.0](https://github.com/ember-graphql/ember-apollo-client-fixed/compare/v2.0.0...v2.1.0)


## v2.0.0 - 2019-12-11

After a long cycle of beta releases and a lot of changes, this is the first
release out of beta of `ember-apollo-client-fixed` v2.0.0. We believe the majority of
people using this addon have been using the beta version in production apps for
quite a long time; this release makes it official and recommended to use the
latest version. New apps should use this version and avoid using deprecated features.
For apps using v1.x, please take a look at all the breaking changes. Note that we
have changed the minimum required Ember version.

Here are all the changes since the last v1 release.

### :boom: Breaking Change

- Don't create `EmberObject` for returned data  (#310) @josemarluedke
  Before this change, `ember-apollo-client-fixed` would always wrap the returned data
  from the GraphQL (when not an array) with `EmberObject`. In this PR we removed
  that and instead we return plain objects.

- Remove usage of `Evented` mixin (#313) @josemarluedke
   `subscribe` no longer returns an Ember Object and
    does not apply the `Evented` Ember mixin. Instead, it just returns an native class and
    it triggers an event when new data comes in. You can use this event to add
    a listener like so:

  ```js
  //import { addListener, removeListener } from '@ember/object/events';

  const result = await this.apollo.subscribe(
    {
      subscription: mySubscription,
    }
  );

  const handleEvent = event => {
    console.log('event received', event)
  };

  // Add listener to new data
  addListener(result, 'event', handleEvent);

  // Remove the listener from new data
  removeListener(result, 'event', handleEvent);
  ```

  The `lastEvent` property should be accessed directly or use `get` from Ember
  Object.

  ```js
  console.log(result.lastEvent);

  // or

  // import { get } from '@ember/object';

  console.log(get(result, 'lastEvent'));
  ```

- Remove unused prod dependencies (#306) @josemarluedke
    It's not recommended to import dependencies of other dependencies in your
    application. Therefore, if you depend on any apollo package that this
    addons depends on, you should add to your `package.json`. You should also add
    ember-auto-import to make sure you are able to import them.
- Mixins are deprecated in favor of `queryManager` computed macro. (#262) @josemarluedke
- Return the actual returned data, not a copy (#312) @josemarluedke
- `graphql` has been added as a peer dependency and removed from the addon's direct dependency. Please run `ember install ember-apollo-client-fixed` to add the default dependencies to your project or run `yarn add -D graphql`.
- If you are using Ember versions 3.4 or 3.5 you must add `ember-native-class-polyfill` to your application. Please run `ember install ember-native-class-polyfill`. Later Ember versions are not required to have this polyfill. (#261) @josemarluedke
- Dropped support for Ember LTS 2.18. We now only support the last LTS version (3.4) and above. (#261) @josemarluedke
- The way to overwrite `clientOptions`, `link` and `chache` has been changed from computed properties to functions. Previous behavior will be removed in the next major version. (#261) @josemarluedke
    Please refer to readme for examples.
- Drop support for node.js v6 (#295) @josemarluedke
- Switch from custom webpack build setup to ember-auto-import. (#159) @jasonmit
- The old build configs (`include`, `exclude`) have been removed. Additional dependencies can be used via ember-auto-import and a regular npm install. (#171) @bgentry
- The old, deprecated mixin import paths have been removed. (#171) @bgentry
- The deprecated `middlewares` option has been removed from the apollo service. Users should switch to override `link` instead. (#171) @bgentry

## :bug: Bug Fix

- unsubscribeAll does not forget active subscriptions (#325, #330) @FabHof
- Fix IE 11 Symbol.iterator (#321) @vsergiu93

### :rocket: Enhancement

- Add deprecation for for default value of keepGraphqlFileExtension (#318) @josemarluedke
- Add public alternative to unsubscribe from watchQuery (#310) @josemarluedke
- Upgrade broccoli-graphql-filter to v0.4 to allow new import syntax (#300) @josemarluedke
  Before the recommended way was to access a property `_apolloUnsubscribe` from
  the result query.
- Add `queryManager` computed macro with support for classic Ember Object and ES6 classes through decorators (decorator syntax requires Ember v3.10.0).
- Add built-in support for GraphQL subscriptions. (#173) @coladarci
- Add configuration options to keep graphql file extension. (#230) @josemarluedke


## :memo: Documentation

- Fix references in the README to apollo docs (#323) @brunoocasali
- Fix documentation for unsubscribeAll(onlyStale) (#320) @dmzza
- Fix documentation about watchQuery (#317) @dmzza
- Clarify status of graphql-tag in README (#314) @jgwhite

## :house: Internal

- Refactor mutation tests to acceptance instead of unit tests (#322) @josemarluedke
- Fix ESLint issues with latest version (#326) @josemarluedke
- Remove Episode GraphQL type from dummy app (#316) @josemarluedke
- Rework tests and dummy app (#315) @josemarluedke
- Cleanup to use async/await and move away from Ember get/set when possible (#309) @josemarluedke
- Add tests with delayed responses (#308) @josemarluedke
- Using nested fragments creates invalid graphql document (#298) @lstrzebinczyk


## :wrench: Tooling

- Only run tests on ubuntu & Improve testem (#329) @josemarluedke
- Run tests in GitHub Actions (#328) @josemarluedke
- Add Release Drafter as GitHub Actions (#307) @josemarluedke
- Refactor testem to avoid test failures (#331) @josemarluedke

### :package: Dependencies

- Upgrade project dependencies & Ember 3.14 (#327) @josemarluedke
- Remove unused dev dep & Upgrade to Ember v3.13 & Cleanup dependencies (#305) @josemarluedke
- Upgrade dependencies (#302) @josemarluedke
- Upgrade to Ember 3.12 & other dependencies (#301) @josemarluedke

***

### Contributors

@FabHof, @brunoocasali, @buschtoens, @dmzza, @vsergiu93, @lstrzebinczyk, @jasonmit, @coladarci, @brunoocasali, @jgwhite, @christophermlne, @bgentry and @josemarluedke

***

For full changes, see the [comparison between v2.0.0-beta.8 and v2.0.0](https://github.com/ember-graphql/ember-apollo-client-fixed/compare/v1.1.0...v2.0.0)


## v2.0.0-beta.8 - 2019-12-11

## :bug: Bug Fix

- unsubscribeAll does not forget active subscriptions (#325, #330) @FabHof

## :memo: Documentation

- Fix references in the README to apollo docs (#323) @brunoocasali

## :house: Internal

- Upgrade project dependencies & Ember 3.14 (#327) @josemarluedke
- Refactor mutation tests to acceptance instead of unit tests (#322) @josemarluedke
- Fix ESLint issues with latest version (#326) @josemarluedke

## :wrench: Tooling

- Only run tests on ubuntu & Improve testem (#329) @josemarluedke
- Run tests in GitHub Actions (#328) @josemarluedke

***

### Contributors

@FabHof, @brunoocasali and @josemarluedke


## v2.0.0-beta.7 - 2019-10-21

### :rocket: Enhancement

- Add deprecation for for default value of keepGraphqlFileExtension (#318) @josemarluedke

### :bug: Bug Fix

- Fix IE 11 Symbol.iterator (#321) @vsergiu93

### :memo: Documentation

- Fix documentation for unsubscribeAll(onlyStale) (#320) @dmzza
- Fix documentation about watchQuery (#317) @dmzza

***

### Contributors

@dmzza, @josemarluedke, @pull and @vsergiu93

***


## v2.0.0-beta.6 - 2019-10-11

### :boom: Breaking Change

- Don't create EmberObject for returned data  (#310) @josemarluedke
  Before this change, `ember-apollo-client-fixed` would always wrap the returned data
  from the GraphQL (when not an array) with `EmberObject`. In this PR we removed
  that and instead we return plain objects.

- Remove usage of Evented mixin (#313) @josemarluedke
   `subscribe` no longer returns an Ember Object and
    does not apply the Evented Ember mixin. Instead, it just returns an native class and
    it triggers an event when new data comes in. You can use this event to add
    a listener like so:

  ```js
  //import { addListener, removeListener } from '@ember/object/events';

  const result = await this.apollo.subscribe(
    {
      subscription: mySubscription,
    }
  );

  const handleEvent = event => {
    console.log('event received', event)
  };

  // Add listener to new data
  addListener(result, 'event', handleEvent);

  // Remove the listener from new data
  removeListener(result, 'event', handleEvent);
  ```

  The `lastEvent` property should be accessed directly or use `get` from Ember
  Object.

  ```js
  console.log(result.lastEvent);

  // or

  // import { get } from '@ember/object';

  console.log(get(result, 'lastEvent'));
  ```

- Remove unused prod dependencies (#306) @josemarluedke
    It's not recommended to import dependencies of other dependencies in your
    application. Therefore, if you depend on any apollo package that this
    addons depends on, you should add to your `package.json`. You should also add
    ember-auto-import to make sure you are able to import them.

- Return the actual returned data, not a copy (#312) @josemarluedke

### :rocket: Enhancement

- Add public alternative to unsubscribe from watchQuery (#310) @josemarluedke
- Upgrade broccoli-graphql-filter to v0.4 to allow new import syntax (#300) @josemarluedke
  Before the recommended way was to access a property `_apolloUnsubscribe` from
  the result query.

### :memo: Documentation

- Clarify status of graphql-tag in README (#314) @jgwhite

### :house: Internal

- Remove Episode GraphQL type from dummy app (#316) @josemarluedke
- Rework tests and dummy app (#315) @josemarluedke
- Cleanup to use async/await and move away from Ember get/set when possible (#309) @josemarluedke
- Add tests with delayed responses (#308) @josemarluedke
- Using nested fragments creates invalid graphql document (#298) @lstrzebinczyk

### :wrench: Tooling

- Add Release Drafter as GitHub Actions (#307) @josemarluedke

### :package: Dependencies

- Remove unused dev dep & Upgrade to Ember v3.13 & Cleanup dependencies (#305) @josemarluedke
- Upgrade dependencies (#302) @josemarluedke
- Upgrade to Ember 3.12 & other dependencies (#301) @josemarluedke

***


## [v2.0.0-beta.5] - 2019-08-19

### Added

- Add `queryManager` computed macro with support for classic Ember Object and ES6 classes through decorators (decorator syntax requires Ember v3.10.0).

### Deprecated

- Mixins are deprecated in favor of `queryManager` computed macro.

## [v2.0.0-beta.4] - 2019-04-22

### Breaking

- `graphql` has been added as a peer dependency and removed from the addon's direct dependency. Please run `ember install ember-apollo-client-fixed` to add the default dependencies to your project or run `yarn add -D graphql`.
- If you are using Ember versions 3.4 or 3.5 you must add `ember-native-class-polyfill` to your application. Please run `ember install ember-native-class-polyfill`. Later Ember versions are not required to have this polyfill.
- Dropped support for Ember LTS 2.18. We now only support the last LTS version (3.4) and above.
- The way to overwrite `clientOptions`, `link` and `chache` has been changed from computed properties to functions. Previous behavior will be removed in the next major version.
- Drop support for node.js v6


## [v2.0.0-beta.3] - 2019-03-09

### Added

- Add built-in support for GraphQL subscriptions. Thanks [@coladarci](https://github.com/coladarci)! ([#173](https://github.com/bgentry/ember-apollo-client-fixed/pull/173))

## [v2.0.0-beta.2] - 2019-02-19

### Changed

- Add configuration options to keep graphql file extension. Thanks [@josemarluedke](https://github.com/josemarluedke)! ([#230](https://github.com/bgentry/ember-apollo-client-fixed/pull/230))

## [v2.0.0-beta.1] - 2018-09-18

### Changed

- Switch from custom webpack build setup to ember-auto-import. Thanks [@jasonmit](https://github.com/jasonmit)! ([#159](https://github.com/bgentry/ember-apollo-client-fixed/pull/159))
- The old build configs (`include`, `exclude`) have been removed. Additional dependencies can be used via ember-auto-import and a regular npm install.
- The old, deprecated mixin import paths have been removed.
- The deprecated `middlewares` option has been removed from the apollo service. Users should switch to override `link` instead.

## [v1.1.0] - 2018-09-14

### Changed

- Addon config is now fetched lazily thanks to [@lennyburdette](https://github.com/lennyburdette) in [#166](https://github.com/bgentry/ember-apollo-client-fixed/pull/166).
- The old mixin import paths have been deprecated ([#167](https://github.com/bgentry/ember-apollo-client-fixed/pull/167)). Thanks [@jasonmit](https://github.com/jasonmit)!
