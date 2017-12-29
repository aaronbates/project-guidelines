# SASS guidelines

:construction: work in-progress :construction:

- Wrap all maths operations in paranthesis with a single space between values, variables and operators.
- Treat `@extend` with suspicion, avoid if you can.
- Avoid string concatenation if you have the luxury

- Avoid nesting if you can — only nest if you must scope styles to parents and there are multiple elements to nest.

### `@extend`

Where possible avoid `@extend`, it has a number of problems:

- Performance is worse than a mixin -- gzip favours repetition, mixins achieve a better compression delta.
- `@extend` is greedy and will extend every instance of a class it finds. Can lead to ["selector chain hell"](https://twitter.com/gaelmetais/status/564109775995437057).
- It moves code around -- source order matters in CSS, avoid moving selectors around a project.
- It obscures code by hiding complexity when debugging. A multiple class approach leads to more transparency.

### Further reading

- [sass-guidelin.es](https://sass-guidelin.es)
- [Airbnb CSS styleguide](https://github.com/airbnb/css)
