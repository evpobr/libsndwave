# Contibuting

Fist, read GitHub's [Collaborating with issues and pull requests](https://docs.github.com/en/free-pro-team@latest/github/collaborating-with-issues-and-pull-requests) atricle.

## Submitting Issues

* If your issue is that libsndwave is not able to or is incorrectly reading one
  of your files, please include the output of the `sndwave-info` program run
  against the file.
* If you are writing a program that uses libsndwave and you think there is a bug
  in libsndwave, reduce your program to the minimal example, make sure you compile
  it with warnings on (for GCC I would recommend at least `-Wall -Wextra`) and that
  your program is warning free, and that is is error free when run under Valgrind
  or compiled with AddressSanitizer.

## Submitting Patches

* For commits, follow [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/) guide
* Patches should pass all existing tests
* Patches should not change existing coding style
* Patches for new features should include tests and documentation.
* Patches to fix bugs should either pass all tests, or modify the tests in some
  sane way
