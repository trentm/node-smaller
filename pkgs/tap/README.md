# @smaller/tap

A "smaller"-take on [node-tap](https://github.com/tapjs/node-tap) which trades
some functionality for a smaller install footprint.  In my limited checks this
reduces the install size from about 35MB to 4.5MB.

Differences with node-tap:

- Support for code coverage has been removed, by excluding the nyc
  and coveralls dependencies.  Note that this removal is done with minimal
  patching, so the options for coverage remain, but `tap` will likely crash if
  you attempt to use them.
