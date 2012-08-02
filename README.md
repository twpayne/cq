# cq

`cq` is a command line utility to query JavaScript libraries that follow the [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml). It provides a fast and easy way to interogate the library's functions.  It's not much more than an specialised, semi-intelligent `grep` with an index.


## Configuration

Create a file called `.cqrc` in your home directory that tells `cq` where to find your JavaScript libraries, for example:

```ini
[closure-library]
path = %(HOME)s/src/closure-library
```

Substitutions like `%(HOME)s` can use environment variables; the trailing `s` indicates that the variable should be treated as a string.


## Execution

`cq` takes one or more arguments that are matched using SQLite's `LIKE` operator.  If only one match is found then that match's source code is printed, otherwise a list of all matches is printed.

Single match example:
```sh
$ cq goog.math.Coordinate.sum
/**
 * Returns the sum of two coordinates as a new goog.math.Coordinate.
 * @param {!goog.math.Coordinate} a A Coordinate.
 * @param {!goog.math.Coordinate} b A Coordinate.
 * @return {!goog.math.Coordinate} A Coordinate representing the sum of the two
 *     coordinates.
 */
goog.math.Coordinate.sum = function(a, b) {
  return new goog.math.Coordinate(a.x + b.x, a.y + b.y);
};
```

Multiple match example:
```sh
$ cq goog.math.Coordinate.%
goog.math.Coordinate.azimuth
goog.math.Coordinate.difference
goog.math.Coordinate.distance
goog.math.Coordinate.equals
goog.math.Coordinate.magnitude
goog.math.Coordinate.prototype.clone
goog.math.Coordinate.squaredDistance
goog.math.Coordinate.sum
```

## Behind the scenes

`cq` creates an index in `%(HOME)s/.cqdb`.  This index is automatically recreated when `cq` is run for the first time, or if `cq` thinks that an indexed file is out-of-date.  You can force re-indexing with the `--reindex` command line option.

`cq` relies on SQLite for all pattern matching.

`cq` is fragile.  It assumes that the code is perfectly formatted according to the Google JavaScript Style Guide.  If you're writing code to be indexed by `cq` then it's strongly recommended to use the [Closure Linter](https://developers.google.com/closure/utilities/) with the `--strict` option to check your code.


## License

Copyright (c) 2012, Tom Payne <twpayne@gmail.com>
All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.

2. Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
