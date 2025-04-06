# Welcome boost::unordered

This repo contains history linkages for Mike Popoloski's [fork](https://github.com/MikePopoloski/boost_unordered) of the [boost::unordered](https://github.com/boostorg/unordered) library.

## Organization

This repository is organized in the following independed branches:

  - `Welcome` - this branch, holds only this README file
  - [`fork/MikePopoloski-unordered`](https://github.com/drok/boost-unordered/tree/fork/MikePopoloski-unordered) - contains Mike Popoloski's standalone `boost::unordered` code, and commit history documenting the relation to the upstream Boost repository.
    - The trees in this branch are exactly those that Mike created (exact SHA1 matches of the tree objects), nothing added, nor taken away.
  - [`autotools`](https://github.com/drok/boost-unordered/tree/autotools) - contains the same boost library, with added Autotools (automake + autoconf) build tooling.


## Motivation

The Popoloski project creates standalone version of the Boost' library. It is a closely-related derivative work, but its commit history is disconnected from the boost project.

This repo provides the glue merge commits that explain precisely how the two projects relate. It allows, for example to query what boost changes/bugfixes have been made in the boost repo since the last Popoloski release, and allows merging those changes into your project.

I (Radu Hociung/[@drok](https://github.com/drok)) maintain this repo using [Mergesium git](https://github.com/Mergesium/git), a git fork which implements the "History-Link" feature, maintains the "HLink:" trailer lines in the commits, and maintains the refs/replace/* refs that allow those placeholder commits to be grafted/replaced with the respective to complete the view of separate histories, without joining them into one unified (and large) commit history.

This repo contains only 337 objects as of `v1.87.1`, adding up to a 512KB pack file, compared with the upstream, which is some 28MB of history. This repo only contains three trees/versions (Mike's `v1.0`, `v1.85.1` and `v1.87.1`).

By comparison, Mike's repo contains 2590 objects 84 commits/trees and 2.4MB of packs, since he started with a copy of the boost::unordered release `boost-1.82.0`, and contains all intermediate commits where he works on making the library standalone.

This repo allows me (and you're welcome to use it too) to integrate Mike's standalone version of the library into my projects without growing my repo size with historical objects/commits I don't need, and also without losing the ability to track upstream changes or explore their history when neeeded.

## Rehydrating the history

In order to navigate the full histories of the two repos, [git graft linkages](https://git-scm.com/docs/git-replace) need to be made.

The "History-Link" feature of my git fork (which I expect to soon propose to the git project for adoption in the mainstream) performs exactly the following steps to rehydrate this repo with the rest of the commit histories from the Boost and Popoloski repos (as of Mike's v1.87.1 release), using the HLink trailer metadata:

```sh
git remote add MikePopoloski https://github.com/MikePopoloski/boost_unordered.git
git remote add boost-unordered https://github.com/boostorg/unordered.git

git fetch MikePopoloski ea557f788d66730ee743ca2b40cc2f6d83e82af0
git fetch boost-unordered b41c054c66e7c71d6c6105a1e9da096f6731434d

# Boost links
git replace 7153424 b41c054c6
git replace cd249de 5e6b9291d
git replace cf4ef96 9aedb9529

# Popoloski links
git replace 6346931 ea557f788
git replace fae04e1 0c3583130
git replace 7e8f4ce fe64be037

```

These commands, run locally have the same effect as the Mergesium git.

Then, you can list the full graph with `git log --graph --oneline`, or `git fetch boost-unordered` and `git fetch MikePopoloski` and query the history as if both Mike's and Boost's commits were in the same repo.

Hydrating defeats the purpose of keeping the local disk-space usage low, but other people who clone your project fetch only 337 objects (512KB) for the standalone boost::unordered feature, without the in-depth history that they don't need.
