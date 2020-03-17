
{: style="text-align: center; margin-bottom: 1em;"}
# Project Description


## Summary

This specification will describe a convention on top of a git repository that
makes it easier for developers to collaborate.

Open Pull Requests will provide a standard way for reviewing and discussing
proposed changes before integrating them into a target branch.

They will be stored inside the repository itself as git references, yet they
will not interfere with the rest of the branches.


## Goals

- To communicate the intent of integrating changes from one branch into another.
- To facilitate reviewing, discussing, accepting and rejecting proposed changes.
- To allow collaboration from developers who don't necessarily have write access
  to the target git repository.
- To make the mechanism as compatible as possible with any existing git
  repository hosting services available.
- To make it usable when no hosting service is being used (i.e. a regular git
  repository shared over SSH), or even when a developer is offline.
- To support pull requests over a distributed network of repositories.


## Prior Art

There are several well-known git repository hosting services that provide Pull
Requests as an additional functionality through their web application.

Our goal here is to look at how do they store internally (in the git repository)
information about the pull requests, so that this specification can integrate
better with them if possible.


### GitHub

For each Pull Request, GitHub exposes the next internal, read-only git
references:

- `refs/pull/:ID/head`: points to the head of the source branch of the
  Pull Request.
- `refs/pull/:ID/merge`: points to a branch that integrates proposed changes
  into the destination branch (if there are no conflicts).

In addition, other metadata is exposed through the REST API:
<https://api.github.com/repos/octocat/Hello-World/pulls/32>


### BitBucket

For each Pull Request, BitBucket exposes the next internal, read-only git
references:

- `refs/pull-requests/:ID/from`: points to the head of the source branch of the
  Pull Request.
- `refs/pull-requests/:ID/to`: points to the head of the destination branch of
  the Pull Request.
- `refs/pull-requests/:ID/merge`: points to a branch that integrates
  proposed changes into the destination branch.
- `refs/pull-requests/:ID/merge-clean`: symlink reference to previous `/merge`
  reference if there are no conflicts.
- `refs/pull-requests/:ID/merge-conflicted`: symlink reference to previous
  `/merge` reference if there are any conflicts.
- `refs/notes/pull-requests/:ID/merge`: contains the list of conflicted files
  for previous `/merge-conflicted` reference.

In addition, other metadata is exposed through the REST API:
<https://api.bitbucket.org/2.0/repositories/tutorials/tutorials.bitbucket.org/pullrequests/4199>


### GitLab

For each Merge Request, GitLab exposes the next internal, read-only git
references:

- `refs/merge-requests/:ID/head`: points to the head of the source branch of the
  Pull Request.
- `refs/merge-requests/:ID/merge`: points to a branch that integrates proposed
  changes into the destination branch (if there are no conflicts).

In addition, other metadata is exposed through the REST API:
<https://gitlab.com/api/v4/projects/8377576/merge_requests/1>


## References

- [Git references](https://git-scm.com/book/en/v2/Git-Internals-Git-References)
- [A better pull request](https://blog.developer.atlassian.com/a-better-pull-request/)


### GitHub

- [About pull requests](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests)
- [Reviewing changes in pull requests](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/reviewing-changes-in-pull-requests)
- [Merging a pull request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/merging-a-pull-request)
- [Checking out GitHub pull requests locally](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/checking-out-pull-requests-locally)
- [Using Git refs to check out GitHub Pull Requests, from your local repo](https://www.jvt.me/posts/2019/01/19/git-ref-github-pull-requests/)
- [GitHub REST API - Pull Requests](https://developer.github.com/v3/pulls/)


### BitBucket

- [Create a pull request for review](https://confluence.atlassian.com/get-started-with-bitbucket/create-a-pull-request-for-review-862720862.html)
- [Review a pull request](https://confluence.atlassian.com/get-started-with-bitbucket/review-a-pull-request-862720867.html)
- [Merge a pull request](https://confluence.atlassian.com/get-started-with-bitbucket/merge-a-pull-request-862720864.html)
- [Purpose of `refs/pull-requests/<ID>/merge-clean` and other internal references](https://community.atlassian.com/t5/Bitbucket-questions/What-purpose/qaq-p/352791)
- [Difference of `refs/pull-requests/<ID>/merge` and `refs/pull-requests/<ID>/from`](https://community.atlassian.com/t5/Bitbucket-questions/Difference-of-refs/qaq-p/772142)
- [About BitBucket pull-request internal references](https://stackoverflow.com/a/53889183)
- [Bitbucket Server REST API - Pull Requests](https://docs.atlassian.com/bitbucket-server/rest/7.0.1/bitbucket-rest.html#idp283)
- [Bitbucket Server 7.0.1 Javadoc - Pull Requests](https://docs.atlassian.com/bitbucket-server/javadoc/7.0.1/api/com/atlassian/bitbucket/pull/PullRequest.html)
- [Stash 3.11.3 Javadoc - Pull Requests](https://docs.atlassian.com/DAC/javadoc/stash/3.11.3/api/reference/com/atlassian/stash/pull/PullRequest.html)


### GitLab

- [Merge requests](https://docs.gitlab.com/ee/user/project/merge_requests/)
- [Reviewing and managing merge requests](https://docs.gitlab.com/ee/user/project/merge_requests/reviewing_and_managing_merge_requests.html)
- [Merge when pipeline succeeds](https://docs.gitlab.com/ee/user/project/merge_requests/merge_when_pipeline_succeeds.html)
- [Checkout Merge requests locally by adding a Git alias](https://docs.gitlab.com/ee/user/project/merge_requests/reviewing_and_managing_merge_requests.html#checkout-locally-by-adding-a-git-alias)
- [Code Owners](https://gitlab.com/help/user/project/code_owners)
- [Using Git refs to check out GitLab Merge Requests, from your local repo](https://www.jvt.me/posts/2019/01/19/git-ref-gitlab-merge-requests/)
- [GitLab REST API - Merge Requests](https://docs.gitlab.com/ee/api/merge_requests.html)
