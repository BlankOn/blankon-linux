# Glossary

This page explains the names and terms that BlankOn uses in this wiki. If a word on another page is new to you, look for it here first. The page has two parts: the list of terms in alphabetical order, and the rules for adding and changing entries.

## Terms

### Arsip

The main package repository of BlankOn, at `arsip.blankonlinux.id`. Packages here are meant to be stable, and a normal BlankOn system gets its updates from here. "Arsip" is Indonesian for "archive". See [Managing Repository](https://github.com/BlankOn/blankon-linux/blob/main/UserGuides/ManagingRepository.md).

### Arsip-dev

The development package repository, at `arsip-dev.blankonlinux.id`. It is synced with [Debian Sid](#debian-sid) from time to time, so it can break your system. The development team uses it to test packages before they go to [Arsip](#arsip). See [Managing Repository](https://github.com/BlankOn/blankon-linux/blob/main/UserGuides/ManagingRepository.md).

### Debian Sid

The unstable development branch of Debian, also called "unstable". [Sinambung](#sinambung) is based on Sid. See the [Debian wiki](https://wiki.debian.org/DebianUnstable).

### IRGSH

The system that builds BlankOn packages and adds them to the repositories. Source code: https://github.com/BlankOn/irgsh-go. See [Repository Setup Guide with IRGSH](https://github.com/BlankOn/blankon-linux/blob/main/Infrastructure/RepositorySetupGuideWithIRGSH.md).

### Jahitan

The server that publishes BlankOn ISO images, at `jahitan.blankonlinux.id`. See [Zsync](https://github.com/BlankOn/blankon-linux/blob/main/QualityAssurance/Zsync.md).

### Praya

A GNOME Shell extension made by BlankOn. It is part of the BlankOn desktop. See [Praya](https://github.com/BlankOn/blankon-linux/blob/main/UserGuides/Praya.md).

### Sinambung

The current BlankOn distribution. It is a rolling release based on [Debian Sid](#debian-sid): it gets new packages in tested batches, without large version upgrades. BlankOn aims to test updates before they reach users. It is also the suite name in the APT sources of a BlankOn system. See [Goals](https://github.com/BlankOn/blankon-linux/blob/main/Goals.md) and [Managing Repository](https://github.com/BlankOn/blankon-linux/blob/main/UserGuides/ManagingRepository.md).

### Verbeek

The code name of the BlankOn ISO image. Before Sinambung, Verbeek was also a release (BlankOn Linux 12.0) with its own repositories. Those repositories are discontinued. If you installed Verbeek in the first half of 2026, move to [Sinambung](#sinambung) as described in [Managing Repository](https://github.com/BlankOn/blankon-linux/blob/main/UserGuides/ManagingRepository.md).

## Editing this page

1. **Add a term when a reader may not know it.** This includes BlankOn names (for example Sinambung or Jahitan) and technical words that a new user may not know.
2. **Keep each definition to one short paragraph.** Say what the thing is first, then why it matters to the reader.
3. **Link to the full page.** If another wiki page explains the term in detail, link to it. The glossary does not repeat that page.
4. **Do not translate names.** Write each name as the project uses it. If the name is an Indonesian word, you may give its meaning.
5. **Keep old meanings.** If a term changes meaning or is no longer used, update the entry and keep the old meaning. Readers still find the old meaning on older pages.
6. **Link from other pages.** When a page uses a glossary term for the first time, link the term to its entry here, with the full URL: `[Sinambung](https://github.com/BlankOn/blankon-linux/blob/main/Glossary.md#sinambung)`.
