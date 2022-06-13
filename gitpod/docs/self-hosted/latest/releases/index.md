---
section: self-hosted/latest
title: Self-Hosted Gitpod Releases and Versioning
---

<script context="module">
  export const prerender = true;
</script>

# Self-Hosted Gitpod Releases and Versioning

### Release Policy

Github Self-Hosted is released once a month, usually at the end of each month. The versioning schema is: `YYYY.MM.V`. Each new version of Self-Hosted Gitpod includes all of the changes made to Gitpod up to the release date. This means that the Self-Hosted version of Gitpod is at most one month behind the SaaS version. Outside of the regular monthly releases, hot-fix releases are possible to add functionality or fix bugs. Hot fix releases increment the `V` in the aforementioned versioning schema.

### Support Policy

Gitpod is committed to supporting the last 2 versions of Self-Hosted Gitpod with patches and security updates.

### Roll out Policy

New versions are rolled first to all users that are on the `community` license, i.e. are on the stable branch. Customers with a paid license receive the newest version one week later.

### Testing Policy

For each release, we validate that core Gitpod workflows function as expected via a series of automated and manual tests. These tests are performed on Gitpod installations running on our reference architectures This should help ensure that Gitpod works for you, assuming your architecture is close to the reference architecture.

## Upgrade Guides

<!-- ToDo: This should live on it's own page once we have three levels of hierarchy in the nav bar - https://github.com/gitpod-io/website/issues/2221-->

This section informs you if there are specific considerations to take into account when upgrading to a specific version.

### Upgrading to 2022.05

No breaking changes and thus specific recommendations when updating. Please follow the normal upgrade procedure mentioned on the [Updating your Gitpod Installation](../updating) page.
