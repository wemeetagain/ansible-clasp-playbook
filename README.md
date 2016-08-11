# Clasp Playbook

This playbook builds clasp and all its dependencies. It is meant
mainly for CI purposes, and has not been optimized for incremental usage.
Currently this has only been tested on Archlinux and Ubuntu 16.04.

#### How to use on you local machine (if you are unfamiliar with ansible):

- install ansible
- run `ansible-galaxy install -fr requirements.yml` to install role dependencies
- tweak variables to your liking by editing group_vars/all.yml
- run `ansible-playbook playbook -i "localhost," -c local --ask-sudo-pass`, optionally setting
  [tags](https://docs.ansible.com/ansible/playbooks_tags.html), to begin

## Roles

- [swapfile](https://github.com/kamaln7/ansible-swapfile)
- [clasp-externals](https://github.com/wemeetagain/ansible-clasp-externals)
- [clasp-build](https://github.com/wemeetagain/ansible-clasp-build)

## Variables

- `swapfile_use_dd` - if set to False, fallocate is used to create
  the swapfile, otherwise, dd is used. You may need to set this to
  True if your filesystem does not support fallocate, default: False
- `swapfile_size` - the size of the swapfile to create in the
  format that fallocate expects, default: 512MB
- `swapfile_location` - the location of where the swapfile will be
  created, default: /swapfile

- `externals_clasp_dir` - where to clone the externals-clasp to
locally, default: /home/vagrant/externals-clasp
- `externals_clasp_repo_url` - git repo url of externals-clasp,
default: https://github.com/drmeister/externals-clasp
- `externals_clasp_version` - the branch, commit, tag to use, default:
  master
- `externals_clasp_target` - the make target to invoke, default: all
- `pjobs` - the number of parallel jobs to use for C/++ compilation,
default: 2
- `gcc_toolchain` - defines the root directory where gcc binaries can be found (gcc_toolchain/bin/gcc exists), default: /usr
- `gcc_executable` - default: "{{ gcc_toolchain }}/bin/gcc"
- `gxx_executable` - default: "{{ gcc_toolchain }}/bin/g++"

- `clasp_version` - which branch/commit/tag of code to build from, default: testing
- `clasp_src_dir` - directory to download clasp source, default: ~/clasp
- `clasp_repo_url` - url of remote clasp repo, default: https://github.com/drmeister/clasp
- `clasp_force_checkout` - whether to force a recheckout, default: true
- `externals_clasp_dir` - directory which contains the externals-clasp
  repo, default: /home/vagrant/externals-clasp

## [Tags](https://docs.ansible.com/ansible/playbooks_tags.html)

- `swapfile` - run swapfile role
- `clasp-externals` - run clasp-externals role
  - `clasp-externals-checkout` - (re)checkout clasp-externals
  - `clasp-externals-make` - make clasp-externals
  - `boost-from-src` - checkout and install boost from source, only
    applies to Ubuntu (for now)
- `clasp-build` - run clasp-build role
  - `clasp-checkout` - (re)checkout clasp from source
  - `clasp-make` - make clasp from source

## License

MIT
