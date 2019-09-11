# Notes on the API and ABI stability checking

Outcome is a header only library with a very simple Application Binary
Interface (ABI). Therefore, your shared library with functions which consume
and return Outcomes ought to be perfectly binary stable in the pure sense,
no matter what changes occur in Outcome in the future.

That is, however, in the pure sense, and it only applies to shared libraries
which do not export any Outcome implementation (i.e. `-fvisibility=hidden` etc
on ELF). Where implementation is exported e.g. in a static library, one may
also wish to ensure that the API as well as ABI is compatible. Another example
of use case are precompiled C++ Modules.

This directory contains dumps of both Outcome's API and ABI for all versions
released, which can be useful for verifying stability. Note that no ABI
guarantees will be made for the first year after Outcome enters Boost,
however as the presence of this directory shows, we shall be attempting
to implement such a guarantee anyway.

Currently stored ABI versions:

<dl>
  <dt>2.1</dt>
  <dd>This is the ABI of the post-second-peer-review Outcome i.e.
  the Outcome with the changes recommended by the acceptance peer
  review. This is the version of Outcome which entered Boost.</dd>
</dl>

## Prerequisites for using this directory:

1. GCC 6.3 with libstdc++.
2. A recent Linux.

### Prerequisites if you want to check only ABI:

1. Some edition of `abi-dumper` (one usually comes in your Linux
package repos).
2. Some edition of `abi-compliance-checker` (one usually comes in your Linux
package repos).

Run `./check-abi.sh abi_dumps/Outcome/<ABIVER>/binary_only.dump` to check that
the current Outcome's ABI is compatible with the stored ABI. A HTML report
will be generated by the `abi-compliance-checker` tool useful for displaying
in CI servers and/or emailing to team members.

### Prerequisites if you want to check ABI and API:

1. A new enough ABI compliance checker to work with GCC 6 (https://github.com/lvc/abi-compliance-checker)
and all its prerequisites.

Run `./check-api-abi.sh abi_dumps/Outcome/<ABIVER>/ABI.dump` to check that the
current Outcome's ABI and API is compatible with the stored ABI. A HTML report
will be generated by the `abi-compliance-checker` tool useful for displaying
in CI servers and/or emailing to team members.

As users familiar with this tool will know, this form of the tool is
somewhat prone to false positives. Every failure needs to be carefully
studied to determine if it is actually a problem or not. If so, please
report such ABI breakages to https://github.com/ned14/outcome/issues.