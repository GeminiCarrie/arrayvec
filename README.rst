
arrayvec
========

A vector with fixed capacity.

Please read the `API documentation here`__

__ https://docs.rs/arrayvec

|build_status|_ |crates|_ |crates2|_

.. |build_status| image:: https://travis-ci.org/bluss/arrayvec.svg
.. _build_status: https://travis-ci.org/bluss/arrayvec

.. |crates| image:: http://meritbadge.herokuapp.com/arrayvec
.. _crates: https://crates.io/crates/arrayvec

.. |crates2| image:: http://meritbadge.herokuapp.com/nodrop
.. _crates2: https://crates.io/crates/nodrop

Recent Changes (arrayvec)
-------------------------

- 0.5.0 (not released yet)

  - Add ``FromStr`` impl for ``ArrayString`` by @despawnerer
  - Use a union in the implementation of ``ArrayString`` (stable Rust),
    while this is only used for ``ArrayVec`` on nightly.
  - Add method ``try_extend_from_slice`` to ``ArrayVec``, which is always
    effecient by @Thomasdezeeuw.
  - Add method ``remaining_capacity`` by @Thomasdezeeuw
  - Improve performance of the ``extend`` method.
  - The index type of zero capacity vectors is now itself zero size, by
    @clarcharr
  - Use ``drop_in_place`` for truncate and clear methods. This affects drop order
    and resume from panic during drop.

- 0.4.11

  - In Rust 1.36 or later, use newly stable MaybeUninit. This extends the
    soundness work introduced in 0.4.9, we are finally able to use this in
    stable. We use feature detection (build script) to enable this at build
    time.

- 0.4.10

  - Use ``repr(C)`` in the ``union`` version that was introduced in 0.4.9, to
    allay some soundness concerns.

- 0.4.9

  - Use ``union`` in the implementation on when this is detected to be supported
    (nightly only for now). This is a better solution for treating uninitialized
    regions correctly, and we'll use it in stable Rust as soon as we are able.
    When this is enabled, the ``ArrayVec`` has no space overhead in its memory
    layout, although the size of the vec should not be relied upon. (See `#114`_)
  - ``ArrayString`` updated to not use uninitialized memory, it instead zeros its
    backing array. This will be refined in the next version, since we
    need to make changes to the user visible API.
  - The ``use_union`` feature now does nothing (like its documentation foretold).

.. _`#114`: https://github.com/bluss/arrayvec/pull/114

- 0.4.8

  - Implement Clone and Debug for ``IntoIter`` by @clarcharr
  - Add more array sizes under crate features. These cover all in the range
    up to 128 and 129 to 255 respectively (we have a few of those by default):

    - ``array-size-33-128``
    - ``array-size-129-255``

- 0.4.7

  - Fix future compat warning about raw pointer casts
  - Use ``drop_in_place`` when dropping the arrayvec by-value iterator
  - Decrease mininum Rust version (see docs) by @jeehoonkang

- 0.3.25

  - Fix future compat warning about raw pointer casts

- 0.4.6

  - Fix compilation on 16-bit targets. This means, the 65536 array size is not
    included on these targets.

- 0.3.24

  - Fix compilation on 16-bit targets. This means, the 65536 array size is not
    included on these targets.
  - Fix license files so that they are both included (was fixed in 0.4 before)

- 0.4.5

  - Add methods to ``ArrayString`` by @DenialAdams:

    - ``.pop() -> Option<char>``
    - ``.truncate(new_len)``
    - ``.remove(index) -> char``

  - Remove dependency on crate odds
  - Document debug assertions in unsafe methods better

- 0.4.4

  - Add method ``ArrayVec::truncate()`` by @niklasf

- 0.4.3

  - Improve performance for ``ArrayVec::extend`` with a lower level
    implementation (#74)
  - Small cleanup in dependencies (use no std for crates where we don't need more)

- 0.4.2

  - Add constructor method ``new`` to ``CapacityError``.

- 0.4.1

  - Add ``Default`` impl to ``ArrayString`` by @tbu-

- 0.4.0

  - Reformed signatures and error handling by @bluss and @tbu-:

    - ``ArrayVec``'s ``push, insert, remove, swap_remove`` now match ``Vec``'s
      corresponding signature and panic on capacity errors where applicable.
    - Add fallible methods ``try_push, insert`` and checked methods
      ``pop_at, swap_pop``.
    - Similar changes to ``ArrayString``'s push methods.

  - Use a local version of the ``RangeArgument`` trait
  - Add array sizes 50, 150, 200 by @daboross
  - Support serde 1.0 by @daboross
  - New method ``.push_unchecked()`` by @niklasf
  - ``ArrayString`` implements ``PartialOrd, Ord`` by @tbu-
  - Require Rust 1.14
  - crate feature ``use_generic_array`` was dropped.

- 0.3.23

  - Implement ``PartialOrd, Ord`` as well as ``PartialOrd<str>`` for
    ``ArrayString``.

- 0.3.22

  - Implement ``Array`` for the 65536 size

- 0.3.21

  - Use ``encode_utf8`` from crate odds
  - Add constructor ``ArrayString::from_byte_string``

- 0.3.20

  - Simplify and speed up ``ArrayString``’s ``.push(char)``-

- 0.3.19

  - Add new crate feature ``use_generic_array`` which allows using their
    ``GenericArray`` just like a regular fixed size array for the storage
    of an ``ArrayVec``.

- 0.3.18

  - Fix bounds check in ``ArrayVec::insert``!
    It would be buggy if ``self.len() < index < self.capacity()``. Take note of
    the push out behavior specified in the docs.

- 0.3.17

  - Added crate feature ``use_union`` which forwards to the nodrop crate feature
  - Added methods ``.is_full()`` to ``ArrayVec`` and ``ArrayString``.

- 0.3.16

  - Added method ``.retain()`` to ``ArrayVec``.
  - Added methods ``.as_slice(), .as_mut_slice()`` to ``ArrayVec`` and ``.as_str()``
    to ``ArrayString``.

- 0.3.15

  - Add feature std, which you can opt out of to use ``no_std`` (requires Rust 1.6
    to opt out).
  - Implement ``Clone::clone_from`` for ArrayVec and ArrayString

- 0.3.14

  - Add ``ArrayString::from(&str)``

- 0.3.13

  - Added ``DerefMut`` impl for ``ArrayString``.
  - Added method ``.simplify()`` to drop the element for ``CapacityError``.
  - Added method ``.dispose()`` to ``ArrayVec``

- 0.3.12

  - Added ArrayString, a fixed capacity analogy of String

- 0.3.11

  - Added trait impls Default, PartialOrd, Ord, Write for ArrayVec

- 0.3.10

  - Go back to using external NoDrop, fixing a panic safety bug (issue #3)

- 0.3.8

  - Inline the non-dropping logic to remove one drop flag in the
    ArrayVec representation.

- 0.3.7

  - Added method .into_inner()
  - Added unsafe method .set_len()


License
=======

Dual-licensed to be compatible with the Rust project.

Licensed under the Apache License, Version 2.0
http://www.apache.org/licenses/LICENSE-2.0 or the MIT license
http://opensource.org/licenses/MIT, at your
option. This file may not be copied, modified, or distributed
except according to those terms.


