
{: style="text-align: center; margin-bottom: 1em;"}
# Specification (draft)


## Global Configuration

When a git repository is set up to contain Open Pull Requests, it needs to
create the next git reference:

- `refs/pull-requests/meta`

This git reference SHOULD be the top of an orphan branch (totally unrelated to
the rest of the regular branches in the repository). Other git references that
will be described later MAY branch off from this one.

When checked out, this git reference will contain a tree with the next text
file:

- `/version`: contains the version of the Open Pull Request specification used
  by this repository.


## Pull Requests

Every pull request is represented by a git reference inside the repository:

- `refs/pull-requests/:ID/meta`

Where `:ID` is an arbitrary identifier defined by the user who created the pull
request. It MAY be a sequential number, or an ISO-formatted date/time, or any
other alphanumerical string. In any case, the resulting reference name MUST be
[well formed](https://git-scm.com/docs/git-check-ref-format).

This git reference MAY be a child of the previous one containing the global
configuration.

When checked out, this git reference will contain a tree with the next text
files:

- `/version`: contains the version of the Open Pull Request specification used
  by this specific pull request.
- `/status`: contains the status of this pull request. It MUST be one of these:
  - `TENTATIVE`
  - `OPEN`
  - `DECLINED`
  - `MERGED`
  - `SUPERSEDED`
- `/title`: contains a short summary describing this pull request.
- `/description`: contains a long description explaining the purpose of this
  pull request.
- `/git-request-pull`: contains a summary of pending changes generated by
  [git-request-pull](https://git-scm.com/docs/git-request-pull).
- `/source-repository`: identifies the source repository of the Pull Request. It
  SHOULD be a [git URL](https://git-scm.com/docs/git-clone#URLS).
- `/source-branch`: identifies the source branch of the Pull Request.
- `/source-commit`: identifies the source commit of the Pull Request.
- `/destination-repository`: identifies the destination repository of the Pull
  Request. It SHOULD be a [git URL](https://git-scm.com/docs/git-clone#URLS).
- `/destination-branch`: identifies the destination branch of the Pull Request.
- `/destination-commit`: identifies the destination commit of the Pull Request.

Apart from the previous `/meta` reference, each pull request can also be
associated to the next git references:

- `refs/pull-requests/:ID/source`: points to the head of the source branch of
  the Pull Request.
- `refs/pull-requests/:ID/destination`: points to the head of the destination
  branch of the Pull Request.
- `refs/pull-requests/:ID/merge`: points to a branch that integrates
  proposed changes into the destination branch.
- `refs/pull-requests/:ID/merge-clean`: symlink reference to previous `/merge`
  reference if there are no conflicts.
- `refs/pull-requests/:ID/merge-conflicted`: symlink reference to previous
  `/merge` reference if there are any conflicts.